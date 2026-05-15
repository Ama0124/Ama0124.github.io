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
---

## Motivation
因為滑鼠通常需要放在平滑的平面上，限制較多，也不利於攜帶便利性，如果有一個設備是戒指造型且只要戴在手指上操控，那對於便利性會有極大的好處。所以我們利用IMU作為蒐集資料的設備，利用EKF來進行濾波，藉此來操控電腦的鼠標。

## Gimbal lock
Gimbal lock(萬向鎖)就是陀螺儀在特定軸向時，會失去一個自由度。
以三軸陀螺儀為例，若以Y軸做旋轉軸將X軸向上旋轉90度，那X軸跟Z軸將會重疊，導致我們無法判斷接下來是X軸或Z軸在做旋轉。
![image](https://upload.wikimedia.org/wikipedia/commons/4/49/Gimbal_Lock_Plane.gif?utm_source=en.wikipedia.org&utm_campaign=parser&utm_content=thumbnail_unscaled)


在應用數學中，歐拉角(Euler angles)就會有gimbal lock的產生，因此我們會選擇四元數(Quaternions)來做運算。

## Quaternions
通常複數是由實數加上虛數單位\(i\), 其中
\[i^{2} = -1\]

而四元數的定義則是，實數加上三個虛數單位\(i, j, k\), 並且有下列關係
\[i^{2} = j^{2} = k^{2} = ijk = -1\]