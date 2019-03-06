# Computer-Vision-for-Visual-Effects-
HW1
# Homework 1 Report
第3組
## Training CycleGAN
### 訓練進度條
```
python train.py --dataroot datasets/apple2orange/ --cuda
```
我們將 epoch 設為 default 的 200，所以跑了一天也只到60%左右的進度，但 model 在此時已經訓練得差不多了，測試出來的結果都還不錯。
![](https://i.imgur.com/H7R6V7N.png)


### testing 圖片結果
```
python test.py --dataroot datasets/apple2orange/ --cuda
```
以下是拿 apple2orange test dataset 做的測試結果，雖然畫質都比原圖還要差，但是在顏色轉換的部分都表現得很好，訓練的成果還算不錯。
* apple to orange
![](https://i.imgur.com/o7ainHZ.png)
![](https://i.imgur.com/Eobk6eq.png)
* orange to apple
![](https://i.imgur.com/Vgy2oNJ.png)
![](https://i.imgur.com/WQEOMg1.png)
### pth file
經過約 120 個 epoch 之後，訓練出來的 model： [Google Drive](https://drive.google.com/open?id=1ikeGdd7-ss3GfUz7rUtvgN64n2CAAsmn)

## Testing in personal images
以下是我們測試自己圖片的結果，在物品顏色的轉換都還不錯，雖然在第一張的轉換，物品周圍出現了些微的瑕疵，但不影響整體圖片的效果。
* apple to orange
![](https://i.imgur.com/eT5SpG3.png)
* orange to apple
![](https://i.imgur.com/EUSliEI.png)
![](https://i.imgur.com/lQwewWl.png)


## Compare with another method
使用方法:Super fast color transfer between images(https://github.com/jrosebr1/color_transfer)
這個方法並沒有使用機器學習，只是單純的透過數學計算得到轉換後的結果，我們透過這個方法把中間的target透過左邊的source染色後，得到我們transfer後的圖片。
以下面4張圖做示範，皆是把蘋果變成橘子:

1. 色彩飽和度上升了，但並不類似於Source的橘子色
![](https://i.imgur.com/Twdwm29.png)
2. transfer色彩並沒有完全與source相似，大概是介於source與target之間
![](https://i.imgur.com/srfgM02.png)
3. 由於Source的橘子背景色是黑的，所以導致transfer的蘋果顏色偏黑
![](https://i.imgur.com/PHxxPIU.png)
4. 由於target背景不是全白的，導致背景也被染色了
![](https://i.imgur.com/7RrFJS8.png)

而這些蘋果照片若用本次作業的方法做轉換，可以得到以下的結果：
1. 與上面的4相比，只有蘋果皮本身被染色
![](https://i.imgur.com/7H1hH5K.png)
2. 與上面的2和3相比，顏色更偏向橘子的顏色
![](https://i.imgur.com/B0OYnU1.png)

由上我們可以發現，使用本次作業的方法比較能知道想要做色彩轉換的區域在哪，並且顏色的轉換程度也比 Super fast color transfer between images 還要高，整體的轉換較為合理，不會將不相干的背景轉換成奇怪的顏色。

## Conclusion

### Super fast color transfer between images
這個方法是利用L * a * b *顏色空間以及每個L *，a *和b *通道的平均值和標準偏差來證明顏色可以在兩個圖像之間傳遞。優點是可以快速的轉換圖像的顏色，缺點則是如果照片中含有其他不想轉換的顏色(例如：背景)，則轉換效果和顏色會有偏差。
改善方式可以先找出具有相似質地的區域，然後在每個單獨的區域內進行顏色轉移。這樣可以使顏色的平均和標準差限定在一定的範圍內，使顏色轉移更具真實感。

### CycleGAN
CycleGAN是利用Image-to-Image Translation的技術之外，再重複利用自己產生的圖像進行轉換。在許多情況下，要產生配對好的圖像通常不太容易，但是CycleGAN打破了這個限制，可以在非配對好的圖像集之間的轉換。以我們實驗出的結果來看，CycleGAN不會只是單純的改變蘋果的顏色，而會去辨認只有蘋果皮的部分要改成橘子的顏色，果肉的部分依然保持著該有的顏色色調。

所以，如果以兩種類似的domain去做轉換時，CycleGAN的效能和真實性會比Super fast color transfer between images還要好上不少。
