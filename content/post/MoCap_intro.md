---
title: "Motion capture introdution"
date: 2026-05-15T05:16:15Z
draft: false
tags: 
    - MoCap
cover: "images/ui_bird.png"
banner: "images/ui_bird.png"
description: Motion Capture introdution
---


# 動態捕捉(Motion Capture)
![image](https://d26oc3sg82pgk3.cloudfront.net/files/media/edit/image/29681/large_thumb%403x.jpg)
## 介紹
動作捕捉(Motion Capture)，也稱作為動態捕捉或動捕(MoCap)
- 將人體或物體的高解析度運動記錄到計算機系統內的的一項技術
- 常用在...
    - 軍事，娛樂，運動
    - 醫療應用
    - 計算機視覺及機器人的驗證

動捕是指紀錄演員的運動過程，並使用記錄到的資料套用到2D或3D的數位模型做動畫。如果它有包含到細緻的手部動作或面部表情，我們也稱它為**表演捕捉**(performence capture)。在一般領域中，動態捕捉通常是進行**運動捕捉**，但在電影或遊戲製作中，它比較傾向於[**匹配移動**](https://hackmd.io/XMCBn3x3SU21dUc0IOHg6w?stext=5953%3A22%3A1%3A1747100118%3ADGPzS2&both=)。

**Note :** 動態捕捉只紀錄**演員的動作**，而不是他們的**視覺外觀** 

## 歷史
最初的動畫是由畫家創作出一堆具有細微差異的圖像，再透過視覺暫留的方式來給我們一種圖片在動的錯覺。1914年，[動態摳像(Rotoscoping)](https://hackmd.io/XMCBn3x3SU21dUc0IOHg6w?both=&stext=6245%3A21%3A1%3A1747101739%3AS4Z2Lo)技術出現，動畫的流暢度以及細節得到了很大進步。但在此之後，動畫就很難再有什麼創新，直到電腦科技開始成熟普及之後，[關鍵幀](https://hackmd.io/XMCBn3x3SU21dUc0IOHg6w?both=&stext=6775%3A79%3A1%3A1747103476%3AI-6afi)的技術被發明出來，動畫家的工作量大幅度減少。然而，一些複雜的人體結構以及物理現象還是難以被創作出來。1983年，西門菲莎(Simon Fraser)大學在物理機械捕捉服上取的重大的突破，因此人們見識到了最早的機械式動態捕捉。在同一時期，麻省理工推出了一套基於LED燈的[圖像木偶化(graphical marionette)](https://hackmd.io/XMCBn3x3SU21dUc0IOHg6w?both=&stext=7141%3A32%3A1%3A1747103492%3AFDNe1q)系統，它就是早期光學式動態捕捉的雛形。

## 種類

### Marker-based Motion Capture
- Acoustical Systems(30 - 60(Hz/fps))
    - **原理** : 將一組聲波發射器貼在主要關節上，由三個接收器去收集發射器射出了聲波，用三角測量來去計算發射器的3D位置
    - **缺點** : 難以獲取某個瞬間的資料(你可能會動的比音速快でしょう)，所以無法應付高速或複雜的動作。對於環境的要求也是非常重要的(周圍物品對於雜訊的反射)。另外，過多得電纜也造成動作的自由度降低
    - 因此，聲學系統MoCap通常用於靜態(比較不流暢)捕捉，像是手部細節動作或小範圍的姿勢紀錄

**使用情境** : 教學用、簡單動捕、低成本的研究
- [Mechanical Systems](https://p3-sdbk2-media.byteimg.com/tos-cn-i-xv4ileqgde/d3e5c6eb832e43619197c6fe09d7711f~tplv-xv4ileqgde-resize-w:750.image)(60 - 180(Hz/fps))
    - **原理** : 將旋轉編碼器(Encoder)或電位器(Potentiometer)貼在關節處，並用一些桿桿連接，之後再以關節的運動角度及機械桿的長度去計算3D位置
    - **優點** : 相較於聲學式MoCap
        - 不需要額外的電纜或接收器(自由度upup)
        - 動作精度大幅提升，動作實時更新(真的好快...)
        - 可以多人連動
    - **缺點** : 
        - 穿戴物件笨重
        - 機械桿限制自然動作
        - 維護成本高

**使用情境** : 早期動畫製作、機械臂工程學模擬
- Magnetic Systems (60 - 240(Hz/fps))
    - **原理** : 在關節放置接收器，測量與天線的距離跟方向
    - **優點** : 
        - 便宜(買到賺到)
        - 採樣率高(不是真的高)，適合簡單的動作捕捉，cp值突破天際
    - **缺點** : 
        - 環境影響極大，任何東西都可能影響到磁力(超重的東西、磁鐵...)
        - 電纜影響自由度

**使用情境** : [醫療手術研究](https://www.ndigital.com/electromagnetic-tracking-technology/aurora/)(微創手術...等)、VR研究室內追蹤

- Optical Systems
    - **原理** : 在演員身上貼上反射器，然後用相機為每個反射器生成2D座標，然後用專有的軟體分析相機的數據生成3D座標。
    - **優點** : 
        - 超高採樣率(取決於相機的品質)，可以用來捕捉武術、體操等快速動作
        - 沒有電纜之類物件的影響(這就是自由阿)
        - 反射器在捕獲時幾乎沒有數量限制，理論上，光學式MoCap的動作細節是非常高的
    - **缺點** : 
        - 超貴(超多高解析度攝影機，高技術性專用軟體)
        - 發射器之間的遮擋(牽手動作或緊密交戶的動作)。如果這些被遮擋的動作無法被修復 = 白拍(增加反射器來解決，CPU處理時間更長)
        - 及時交互性不足(收集到的數據往往需要進行過濾跟除噪)，時間成本非常的昂貴
    - **Note**: 無止盡的增加反射器並不是一種好選擇，通常增加到一定數量就會出現「trackingconfusion」的問題。相機難以識別靠太近的反射器，所以需要解析度更高的攝影機($$)
    - [Active Markers](https://www.researchgate.net/profile/Ronan-Boulic/publication/43651884/figure/fig3/AS:667677609693191@1536198155518/Setup-of-active-markers.jpg) (100 - 360(Hz/fps))
        - 用LED燈或其他能發送光訊號發射器發射光線(通常是紅外線)，可以發射出不同的編碼光脈衝來區別不同角色身分。
        - 精度高，不需要後製來辨識每個點，不易混淆
        - 設備較重，穿戴成本高，需要電源或電池
        - **使用情境** : 實驗室高階人體運動分析、虛擬製作現場
    {{< youtube vgwBfT_IFMw >}}
    - [Passive Markers](https://p.turbosquid.com/ts-thumb/Am/l2eHJn/sj/screenshot006/png/1625335051/600x600/fit_q87/1de6c1c6074b0c91b5346ce7b31f55b2f5f6799d/screenshot006.jpg) (120 - 500(Hz/fps))
        - 用反射器(反光球)反射光源，利用軟體計算反射光源座標，整合成3D骨架
        - 設備輕巧，不需要電源，可以大量部屬(成本低)
        - 需要系統辨認(匹配)標記點，容易出錯，可能受阻擋
        - **使用情境** : 電影動畫、遊戲角色動作捕捉

[曦曦鱼SAKANA](https://www.youtube.com/watch?v=BlrRFiLCph4)

- [Inertial Systems](https://d10lvax23vl53t.cloudfront.net/images/Article_Images/ImageForArticle_43_15816813570943783.png)(60 - 240(Hz/fps))
    - **原理** : 利用穿戴在身上的慣性測量單元(IMU, Inertial Measurement Unit)，測量各部位得加速度跟角動量，之後經由[運動學演算法](https://web.ntnu.edu.tw/~algo/Motion.html)來推估出人體姿勢。
    **補充** : IMU裡通常包含 加速度計、陀螺儀、有些還有磁力計
    - **優點** : 
        - 不需要攝影機
        - 可以在戶外使用
        - 穿戴裝置輕便靈活
        - 成本比光學式低
    - **缺點** : 
        - 會有累積誤差，用久了需要校正
        - 只會有人體姿態，空間位置無法準確取得
        - 如果有磁力計，磁力式有的問題它也會有

**使用情境** : VR遊戲或虛擬體驗、戶外運動分析、動作紀錄、工業工程學(分析勞工姿勢與安全)

**介紹** : [Sony mcocpi 3D Motion Capture](https://electronics.sony.com/more/mocopi/all-mocopi/p/qmss1-uscx?srsltid=AfmBOoqvU7G9b9TteY-Dzengr-zu5_zgUBcbgIAYq7le1JQ_7w6RcD6U)([規格](https://www.sony.com/electronics/support/other-products-motion-capture-systems/qm-ss1/specifications))

{{< youtube 2eHjC7daOWs >}}
### [Markerless Motion Capture](https://news.miami.edu/edu/_assets/images/images-stories/2020/09/kinatrax940x529.jpg)
在計算機視覺領域不斷的進步下，無標記光學式動態捕捉蓬勃發展，在這個系統下，複雜的特殊設備不再被需要。演員的動作被記錄在多個stream中，再用特殊的方法(CNN...之類的)分析這些stream來辨識人體型態，並且分解成各自獨立的部分以方便進行追蹤。
所以，無標記式動態捕捉的所有過程都透過軟體進行，消除了物理限制，並提高了計算成本。在這個系統下最廣為人知的就是Microsoft的[Kinect](https://en.wikipedia.org/wiki/Kinect#)(目前已停產)，它向大眾提供了低成本的動態捕捉方案。

**Comparison :** Comparison of $100 Markerless MoCap and $25k Optical Mocap{%youtube 3WZSCVeGblU %}
## Acquired Data Process(採集數據處理)

### Direct acquisition
在直接採集的系統下，數據不需要任何形式的後製(data processing cycle)，直接取得資料後就可以使用，例如 : 聲學式、磁力式、機械式...等，雖然這是直接採集系統的一個優點，但同時它也擁有硬體設備上的硬傷，採樣率也較低。
雖說直接採集系統不需要進行任何後製，但因物理法則的影響，後製在某些情況下還是必須的。
### Indirect acquisition
間接採集系統，通常指光學式系統，提供了演員更多的自由度以及更高的採樣率。系統將採集到的數據送到數據集中進行分組，並透過專業軟體進行後驗處理(posteriori)產生最終數據集。因此，若在錄製流程中需要實時交互或立刻確認捕捉情形的話，建接採集系統就不是一個好選擇。它的天職就是精確且完美的捕捉複雜而快速的動作。


##### 最後對數據進行清理過濾，及映射到骨架上後，就可以將標記綁定到骨架的關節上隨著動畫的每一幀進行移動了

---

## 補充
如果你想知道更多動態捕捉底層的知識，請你參考(我都不會)

**運動學演算法(Kinetics algorithm)**

- [Orientation estimation and movement recognition](https://www.diva-portal.org/smash/get/diva2:1127455/FULLTEXT02.pdf)(姿態估計)
- [Forward kinetics](https://en.wikipedia.org/wiki/Forward_kinematics)(正向運動學)
- [Rigid Body Kinematics](https://ocw.nthu.edu.tw/ocw/index.php?page=chapter&cid=75&chid=887)(剛體運動學)

## Reference

- [Motion Capture](https://en.wikipedia.org/wiki/Motion_capture)
- [Motion Capture Fundamental](https://paginas.fe.up.pt/~prodei/dsie12/papers/paper_7.pdf)
- [Rotoscoping](https://d1wqtxts1xzle7.cloudfront.net/91480629/1547-libre.pdf?1664027373=&response-content-disposition=inline%3B+filename%3DExploring_3D_Playblast_To_2D_Animation_R.pdf&Expires=1747107593&Signature=ZJWa3ldWc~RXWD6Xn-tgYNKl8J0jGU8zSt5xPFDEAH6-yAm8JEwZuTjPskeEm~uXvbkI6uIrmSLMPFg3wtJaEOgvM7acHch52-FGOjgCIm5hpjbRnpCiJ2I-rEuMNdPRSy7t~S8~lqY~NUE6mAFLiG6VbtvaYPGiJ1Y-G3aGxyKPTM~cBO3ED8CJzebVvkamLlc5~yxuXK-gT0nHrBJBOGR~2gx4MwQRe6ME4a10GRGH3NSsZLKjSeRYh~3Kd8iirspVSVPv5h-gPY6wWE8M2ZG0FWX4kSxKImbfKccf3bFoJQWN3ZwOdbR5YoRGLOD29G4nPVRNV9aJ3BlmxHlmyA__&Key-Pair-Id=APKAJLOHF5GGSLRBV4ZA)
- [Match Moving-Wekipedia](https://en.wikipedia.org/wiki/Match_moving)
- [Key Frame-Wekipedia](https://zh.wikipedia.org/wiki/%E9%97%9C%E9%8D%B5%E6%A0%BC)


## 實作
- [IMU_ring]({{< ref "/post/IMU_ring.md" >}})

## 名詞解釋

### 匹配移動(Match Moving)
匹配移動是一種基於軟體的技術，它允許一些2D元素、實況動作元素或CG插入到實景鏡頭內，並相對於鏡頭內物件正確的位置、方向以及比例進行移動。匹配移動跟動態摳像和攝影測量有關。同時他也常稱作動態追蹤(Motion Tracking)，與動態捕捉不同的地方是，動態捕捉是透過精密的攝影機及感測器等設備進行對人體運動資料的紀錄，匹配移動則不需要這樣的硬體設備來操作。

### 動態摳像(Rotoscoping)
動態摳像是一種可以讓動畫師逐幀追蹤影片素材，以產生逼真動作的技術。最一開始的動態摳像是將影片圖像投影到玻璃面板上，並描在紙上，這個過程被稱作為轉描(由波蘭裔美國動畫師 Max Fleischer 開發)。雖然他已經被電腦取代，但這個過程依然叫做動態摳像，具體來說是怎麼操作呢? 製作人會主動對畫面上的物件進行遮罩(matte)，這樣就可以在畫面上提取物件並在不同畫面上使用，ex : 《星際大戰》中的光劍。而推動這個方法的著名工作室就是Ufotable。
**遮罩**
![image](https://upload.wikimedia.org/wikipedia/commons/thumb/7/7b/Travelling_matte.svg/500px-Travelling_matte.svg.png)

### [關鍵幀(Key Frame)](https://zh.wikipedia.org/wiki/%E9%97%9C%E9%8D%B5%E6%A0%BC)
平滑過渡的起點(幀)跟終點(幀)的繪畫或鏡頭。簡單來說就是作家想讓你在畫面上那些動作，而你在動畫中看到的東西通常都是關鍵幀。
![image](https://i0.wp.com/wherecreativityworks.com/wp-content/uploads/2023/01/walk-cycle-featured-image-1.jpg?fit=2830%2C1380&ssl=1)

### 圖像木偶化(Graphical Marionetter)
就如同字面上意思所述，木偶圖像化就是將圖像轉變成像牽線木偶一樣，可以對它不同的部位進行移動或其他操作。動畫師可以透過GUI、滑鼠等工具來操作人物的骨架及關節。
{{< youtube 77RvfjaWvRQ >}}