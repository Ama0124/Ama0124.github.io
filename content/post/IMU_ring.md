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
\[\dot{q} = \frac{1}{2}  q \otimes \omega\]