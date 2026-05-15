---
title: "IMU_ring"
date: 2026-05-15T05:30:37Z
draft: false
tags: 
    - MoCap
cover: "images/ui_bird.png"
banner: "images/ui_bird.png"
description: >
    Motion Capture Implementation : IMU_ring
math: true
---

## Motivation
因為滑鼠通常需要放在平滑的平面上，限制較多，也不利於攜帶便利性，如果有一個設備是戒指造型且只要戴在手指上操控，那對於便利性會有極大的好處。所以我們利用IMU作為蒐集資料的設備，利用EKF來進行濾波，藉此來操控電腦的鼠標。

## Gimbal lock
Gimbal lock(萬向鎖)就是陀螺儀在特定軸向時，會失去一個自由度。
以三軸陀螺儀為例，若以Y軸做旋轉軸將X軸向上旋轉90度，那X軸跟Z軸將會重疊，導致我們無法判斷接下來是X軸或Z軸在做旋轉。
![image](https://upload.wikimedia.org/wikipedia/commons/4/49/Gimbal_Lock_Plane.gif?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail_unscaled)


在應用數學中，歐拉角(Euler angles)就會有gimbal lock的產生，因此我們會選擇四元數(Quaternions)來做運算。

## Quaternions
通常複數是由實數加上虛數單位\\(i\\), 其中
\\[i^{2} = -1\\]

而四元數的定義則是，實數加上三個虛數單位\\(i, j, k\\), 並且有下列關係
\\[i^{2} = j^{2} = k^{2} = ijk = -1\\]


## 開始實作

### 硬體設備
- Raspberry Pi 4
- MPU-9250 9-DOF IMU

九軸IMU中包含
- 陀螺儀
- 加速度計
- 磁力計

這三個感測器可以提供姿態估計所需要的資訊

### 流程
1. IMU 蒐集資料
2. 樹莓派接受感測器數據
3. EKF進行姿態估計
4. 計算roll/pitch
5. 映射成滑鼠移動
6. 傳送到電腦中操控


### Extended Kalman Filter
為什麼需要EKF? 因為IMU 的資料通常會包含很多雜訊, 其中包含
- 陀螺儀長時間積分產生飄移
- 加速度計容移受到震動干擾
- 磁力計容易受到外部磁場干擾
所以我們需要用sensor fusion來將多種感測器資料結合, 而 EKF(Extended Kalman Filter) 就是一種非常常見的非線性狀態估測方法。

### Prediction state
在預測階段，我們要利用陀螺儀的角速度來進行姿態的更新，假設角速度為
\[\omega = (\omega_{x}, \omega_{y}, \omega_{z})\]
則四元數更新可以表示為
\[\dot{q}(t) = \frac{1}{2}  \Omega(\omega) \cdot q(t)\] (解微分方程)
我們可以得到
\[\hat{x}_{k|k-1} = (I + \frac{1}{2}  \Omega(\omega_k)\Delta t)\hat{x}_{k-1|k-1}\]

### Update state
在上一個階段我們依靠陀螺儀做預測，久了會有drift(飄移), 所以我們就要用
- 加速度計(提供重力方向)
- 磁力計(提供地磁北方向)
來修正姿態誤差


### Kalman gain
\[K = PH^T(HPH^T + R)^{-1}\]
其中
- P : Covariance matrix
- R : Measurement noice covariance
- H : Jacobian matrix
又
\[h(x) = R(q)^{T} \cdot v_{ref}\]
- for Accelerator : \(v_{ref} = [0, 0, 1]^{T}\)
- for Magnetometer :  : \(v_{ref} = [1, 0, 0]^{T}\)

我們可以透過調整R去決定我們要多相信加速度計跟磁力計


### Magnetometer Calibration
因為磁力計會受周圍磁場影響，所以我們需要進行calibration。主要會有兩種誤差
- Hard iron effect
    - 會造成固定偏移量，所以量測值會整體偏向某個地方
- Soft iron effect
    - 會造成磁場變形。
    - 理想情況下磁力計資料應該要為球形，但受到soft iron effect影響之後可能會變成橢球
所以需要用calibration進行修正
\textbf{Correction formula}
\[Mag_{cal} = (Mag_{raw} - bias_{hard}) \times scale_{soft}\]

### 滑鼠控制邏輯
當 EKF 得到 quaternion 後，我們會將 quaternion 轉換成 Euler angles：
\[(q_w, q_x, q_y, q_z) \rightarrow (\phi, \theta, \psi)\]
其中
- Roll \(\phi\)控制X軸移動
- Pitch \(\theta\)控制Y軸移動
因此只要手部改變傾斜方向，就可以控制滑鼠

### Deadzone
如果感測器有微小震動，滑鼠可能會一直抖動。因此我們設定 deadzone，當角度小於某個 threshold 時，滑鼠不會移動。
這樣可以有效降低 jitter。

### 點擊偵測
點擊功能則是利用 jerk detection。當加速度突然產生大變化時，我們就判定為一次 click。
