# 리뷰수를 기준으로 한 유의미한 데이터

## 접근방법

서울에 있는 대학교 근처 모든 가게들의 데이터를 뽑은 후 리뷰 갯수를 100개씩 독립변수 10개로 잘라 그룹화 하였고(data[9])  **배달료**, **최소 주문 금액**, **가격**을 종속변수로 하여  두 변수간의 상관관계를 분석할 것이다. 

## 변수

- ~~delivery_fee~~

  > **무의미한 이유** : 대부분의 가게들의 배달료는 2000원~3000원으로 고정되어 있어서  10개의 독립 변수 대부분이 저 구간에 머물러 있어서 무의미하다.
  >
  > <img src="C:\Users\hyun\AppData\Roaming\Typora\typora-user-images\image-20201031202356062.png" width="80%" height="50%"></img>
  >
  > 

- min_order_amount

  > **유의미한 이유** :  특정 부분에서 급격한 변화를 보이며 유의미한 지표를 준다. (색칠 : 분산)
  >
  > <img src="C:\Users\hyun\AppData\Roaming\Typora\typora-user-images\image-20201031203926877.png" width="90%" height="30%"></img>
  >
  > 5000 ~ 7000구간에서 상승폭을 보이며 7000원 중 후반 구간에서 평균 리뷰수가 제일 높으며 분산도 윗구간이 더 높다 -> 7000후반 대의 최소 주문 가격대가 가장 효율적이다. 

- price

  > **무의미한 이유 **: price의 종속 변수는 review_count보단 min_order_amount와 더 알맞다고 생각하였다. 리뷰수와 상관없이 최소금액부분에 밀집되어 있는것을 알 수 있다. 
  >
  > <img src="C:\Users\hyun\AppData\Roaming\Typora\typora-user-images\image-20201031213132374.png" width="80%" height="30%"></img>

- min_price_amount & price

  > 최소주문가격은 대부분 비슷한것을 보아 메뉴 가격과의 상관관계가 없다는 것을 알 수 있다. 
  >
  > 그리고 리뷰수가 많으면 음식 메뉴의 가격이 마냥 싸지만은 않다는 것을 알 수 있다.
  >
  > <img src="C:\Users\hyun\AppData\Roaming\Typora\typora-user-images\image-20201031221201646.png" width="90%" height="30%"></img>

## 그래프 코드

> 모든 결과를 보고 시각화가 잘 되는 것을 골랐다. 이미지 파일이 많으므로 코드로 대체.

공통 코드

```sns.boxplot( x = ' ', y = ' ', data = ndf)
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline

df = pd.read_csv('/app/DB/code/All_colum.csv') #데이터 불러오기
ndf = df.set_index(['id']) #열로 id 넣기
ndf = ndf.sort_values(by=['review_count']) #리뷰수기준 오름차순
plt.rcParams['figure.figsize'] = [15,10]

n1 = ndf[0:100]		#0 0->리뷰수가 0개라 쓰레기 데이터
n2 = ndf[100:200]
n3 = ndf[200:300]
n4 = ndf[300:400]   #20
n5 = ndf[400:500]   #20 ~ 40
n6 = ndf[500:600]   #40 ~ 80
n7 = ndf[600:700]   #80 ~ 150
n8 = ndf[700:800]   #152 ~ 315
n9 = ndf[800:900]   #326 ~ 767
n10 = ndf[900:1000] #767 ~ 8161

n_n1 = n8.loc[:,('min_order_amount','delivery_fee','review_count','price1','price2','price3')]
#분석에 필요한 행 추출
```

- boxplot

  `sns.boxplot( x = ' ', y = 'review_count', data = ndf)`

- histogram

  ``sns.histplot( x = ' ', y = 'review_count' , palette = 'light:m_r', edgecolor = '.3', linewidth = .5, log_scale = True)``

- scatter

  `sns.scatterplot(x=' ', y = 'review_count', data = ndf, ax = ax)`

- lineplot

  `sns.lineplot(x = ' ', y = 'review_count', data = n_n1)`

- distplot

  `sns.displot( data, x = ' ', y = 'review_count')`



