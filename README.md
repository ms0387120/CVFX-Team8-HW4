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


## 3. Perform image alignment and generate infinite zooming effect.


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
## 5. Exploit creativity to add some image processing to enhance effect. 





 
