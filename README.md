# CVFX-Team8-HW4

# HW4-feature detectors

## 1. Sequence of moving-forward images in NTHU campus.

![](https://imgur.com/Na6aOIk.png)  
![](https://imgur.com/IJmp4T2.png)
攝影處：清大旺宏館五樓
## 2. Show feature extraction and matching results between two images.
#### Feature Extraction method1 : ORB(ORiented Brief)
原理簡介：
  ORB 基本上是方法 FAST 與 BRIEF 的結合，再加上許多的修改來增加效能，一開始先使用 FAST 來搜尋 keypoint，再將所搜尋出來的結果，用 Harris corner 來取出前 N 個最有可能是 corner 的 keypoint。 
  對每個 keypoint 附近區域的像素計算其 weighted centroid(以各像素的灰階值為weight)，從 keypoint 到其 weighted centroid 的方向即為該 keypoint 的 orientation。 

*  Defines the moments of a patch as :
     - ![](https://imgur.com/v60wlBJ.png)
*  The centroid :
     - ![](https://imgur.com/vD2vfeG.png)
*  The orientation of the patch then simply is: 
     - ![](https://imgur.com/a91ybCI.png)
#### Ｍatching results：ORB
![](https://imgur.com/lz8eGO8.png)
## 3. Perform image alignment and generate infinite zooming effect.

* method1： [ORB](https://youtu.be/oIadADF9t1k)
* method2： [SIFT](https://youtu.be/vhURFM1Pxig)
* method3： [SURF](https://youtu.be/fE8_7UKjBx8)
* **效果： SIFT ＝ SURF > ORB**
> ORB在最後幾張圖片的效果上有變形，而 SIFT、SURF則還好

## 4. Implement different feature extrators.
#### Feature Extraction method2 : SIFT(Scale Invariant Feature Transform)
#### 可以拆解為四個步驟：
*  Detection of scale-space extrema :
    搜索所有大小上的圖像位置。通過高斯微分函數來辨別可能的對於大小和旋轉不變的候選位置。
*  Accurate keypoint localization :
    在每個候選的位置上，通過一個fitting精細的模型來確定位置和大小。keypoint的選擇依據於它們的穩定程度。
*  Orientation assignment：
    基於圖像局部的梯度方向，assign給每個keypoint位置一個或多個Orientation。所有後面的對圖像數據的操作都相對於keypoint的方向、大小和位置進行變換，從而提供對於這些變換的不變性。
*  Local image descriptor：
    在每個keypoint周圍的鄰近之內，在選定的尺度上測量圖像局部的梯度。這些梯度被變換成一種descriptor，這種descriptor允許比較大的局部形狀的變形和光照變化。
#### Ｍatching results：SIFT
![](https://imgur.com/I0HKimU.png)

#### Feature Extraction method3 : SURF(Speeded Up Robust Features)
* Surf 是對 SIFT 的一種改進，主要是在算法的執行效率上，比 SIFT 算法運行更快
* 從中使用「**積分圖像**」、「**Hession矩陣探測器**」（利用二階微分來進行斑點檢測）
> 斑點：與周圍顏色和灰度差別的區域

#### 可以拆解為五個步驟：
*  Hessian 矩陣 :
    - 構建矩陣
    - ![](https://i.imgur.com/Z6caokO.png)
    - H矩陣判別式為
    - ![](https://i.imgur.com/zRuOD4L.png)
    - 判別式的值是H矩陣的特徵值，可以利用判定結果的符號將所有點分類，根據判別式取值正負，來判別該點是或不是極值點。在SURF算法中，用圖像像素l(x，y)代替函數值f(x，y)，選用二階標準高斯函數作為濾波器，通過特定核間的捲積計算二階偏導數，這樣便能計算出H矩陣的三個矩陣元素，從而計算出H矩陣：
    - ![](https://i.imgur.com/KkN15u2.png)

*  構建高斯金字塔 :
    **圖像的尺度空間是這幅圖像在不同解析度下的表示**，由上式知，一幅圖像j(X)在不同解析度下的表示可以利用高斯核G(￡)的捲積來實現，**圖像的尺度大小一般用高斯標準差來表示**。在計算視覺領域，尺度空間被象徵性的表述為一個圖像金字塔，其中，輸入圖像函數反覆與高斯函數的核卷積並反復對其進行二次抽樣，這種方法主要用於Sift算法的實現，但每層圖像依賴於前一層圖像，並且圖像需要重設尺寸，因此，這種計算方法運算量較大，而**SURF算法申請增加圖像核的尺寸，這也是SIFT算法與SURF算法在使用金字塔原理方面的不同。算法允許尺度空間多層圖像同時被處理，不需對圖像進行二次抽樣，從而提高算法性能。** 圖1(a)是傳統方式建立一個如圖所示的金字塔結構，圖像的尺寸是變化的，並且運算會反複使用高斯函數對子層進行平滑處理，圖1(b)說明Surf算法使原始圖像保持不變而只改變濾波器大小。
    
![](https://i.imgur.com/tvOR0TZ.png)

*  精確定位特徵點：
    所有小於預設極值的取值都被丟棄，增加極值使檢測到的特徵點數量減少，最終只有幾個特徵最強點會被檢測出來。檢測過程中使用與該尺度層圖像解析度相對應大小的濾波器進行檢測，以3×3的濾波器為例，該尺度層圖像中9個像素點之一檢測特徵點與自身尺度層中其餘8個點和在其之上及之下的兩個尺度層9個點進行比較，共26個點，圖中標記'x'的像素點的特徵值若大於周圍像素則可確定該點為該區域的特徵點。

![](https://i.imgur.com/ynGjDpE.png)

*  主方向確定：
    為保證旋轉不變性，首先以特徵點為中心，計算半徑為6s(S為特徵點所在的尺度值)的鄰域內的點在z、y 方向的Haar小波(Haar小波邊長取4s)響應，並給這些響應值賦高斯權重係數，使得**靠近特徵點的響應貢獻大，而遠離特徵點的響應貢獻小**。範圍內的響應相加以形成新的矢量，遍歷整個圓形區域，**選擇最長矢量的方向為該特徵點的主方向**。這樣，**通過特徵點逐個進行計算，得到每一個特徵點的主方向。**

![](https://i.imgur.com/DwMbc6q.png)

*  特徵點描述子生成：
    首先將坐標軸旋轉為關鍵點的方向，以確保旋轉不變性。

![](https://i.imgur.com/RHPwIwq.png)

接下來以關鍵點為中心取8×8的窗口。圖左部分的中央黑點為當前關鍵點的位置，每個小格代表關鍵點鄰域所在尺度空間的一個像素，利用公式求得每個像素的梯度幅值與梯度方向，箭頭方向代表該像素的梯度方向，箭頭長度代表梯度模值，然後用高斯窗口對其進行加權運算,每個像素對應一個向量，長度為為該像素點的高斯權值，方向為圖中藍色的圈代表高斯加權的範圍（越靠近關鍵點的像素梯度方向信息貢獻越大）。然後在每4×4的小塊上計算8個方向的梯度方向直方圖，繪製每個梯度方向的累加值，即可形成一個種子點，如圖右部分示。此圖中一個關鍵點由2×2共4個種子點組成，每個種子點有8個方向向量信息。**這種鄰域方向性信息聯合的思想增強了算法抗噪聲的能力，同時對於含有定位誤差的特徵匹配也提供了較好的容錯性。**

SIFT 跟 SURF 採用Henssian矩陣獲取圖像局部最值還是十分穩定的，但是在求主方向階段太過於依賴局部區域像素的梯度方向，有可能使得找到的主方向不准確，後面的特徵向量提取以及匹配都嚴重依賴於主方向，即使不大偏差角度也可以造成後面特徵匹配的放大誤差，從而匹配不成功；另外圖像金字塔的層取得不足夠緊密也會使得尺度有誤差，後面的特徵向量提取同樣依賴相應的尺度，發明者在這個問題上的折中解決方法是取適量的層然後進行插值。 SIFT是一種只利用到灰度性質的算法，忽略了色彩信息

#### Ｍatching results：SURF
![](/photo/SURF_matched_1.jpg)

## 5. Exploit creativity to add some image processing to enhance effect. 
* method1： [ORB](https://youtu.be/ZsT8M1qJw-4)
* method2： [SIFT](https://youtu.be/fduYaQfQGHY)
* method3： [SURF](https://youtu.be/rTayIhhzbMQ)

> 使用Mac軟體iMovie將Ken Burns特效套用到照片上，在剪輯片段的開頭、結尾設定裁切選取開始及結束影格，然後拖移並調整大小。

* **效果： SURF > SIFT > ORB**

---
參考資料：
* [SURF](https://blog.csdn.net/shenziheng1/article/details/72579635)
