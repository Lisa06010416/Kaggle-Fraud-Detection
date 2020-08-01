# Kaggle-Fraud-Detection
https://www.kaggle.com/c/ieee-fraud-detection/submit
>在survey了一些現有的方法後，發現比較沒有方法著重在考慮同一個使用者購物紀錄之間的關係，因此決定嘗試一下時續性的模型，看看當模型有一個使用者之前交易紀錄的資訊是否能夠增加模型預測Fraud的能力。

##  Feature Selection (in featureSelect.ipynb )
- 使用kernel https://www.kaggle.com/cdeotte/xgb-fraud-with-magic-0-9600
> - 用 card1_addr1_D1n 三個feature來標示user
> - 最後一筆交易紀錄有263個feature

## Data Preprocess (in seqDataPreprocess.ipynb)
- 將資料normalization
- 因為我發現大部分的使用者(>80%)交易紀錄都在3筆以下，LSTM模型的timestap=3
- 根據uid將資料處理成LSTM模型的輸入

## Model One (in modelOne.ipynb)
-	使用一個timestamp=3的LSTM模型
-	每個input(Transaction)是前面處理好的交易紀錄的vector，有263維
-	在LSTM上的每個timestamp輸出的hidden vector後接上Dense Layer 
-	最終output取得一個該筆交易紀錄是否為Fraud的機率值
-	設定一個threshold去另機率高於threshold的為1(Fraud)、低於threshold的為0(Normal)
![image](https://github.com/Lisa06010416/Kaggle-Fraud-Detection/blob/master/image/Model%201.png)

### Problem: data unbalance
-	發現當threshold越小準確率越高 
-	推測是因為訓練資料不平均的關係造成
<img width="150" height="300" src="https://github.com/Lisa06010416/Kaggle-Fraud-Detection/blob/master/image/Threshold1.png"/>

-	把loss改成一個帶有權重的mse loss
<img width="150" height="300" src="https://github.com/Lisa06010416/Kaggle-Fraud-Detection/blob/master/image/Threshold2.png"/>

## Model Two (in modelTwo.ipynb)
-	只有很少部分的Fraud User(0.2%)會有 Normal和Fraud 的交易紀錄參雜的情況
- 因此將同一個使用者的交易一起判斷是否有Fraud 也許是個可行的方法
- 使用Attention將多筆交易資訊一起參考
![image](https://github.com/Lisa06010416/Kaggle-Fraud-Detection/blob/master/image/Model%202.png)

# Predict Fraud User
