---
title:  "가우시안 혼합 모형(Gaussian Mixture Model) a.k.a GMM"
tags:
  - GMM
  - EM
use_math: true
comments: true
---

## 사건의 시작
어떤 학교에서 마음먹고 학생들의 발 사이즈를 모조리 전수조사 했다고 합시다. 그 데이터는 어떤 모양일까요?

일반적으로 남자는 여자보다 발이 큽니다. 그리고 발 사이즈는 우리가 잘 알고 있는 정규분포의 모양을 띄게 될 것입니다.

아래는 평균이 각각 270과 240, 표준편차가 각각 14와 12인 정규분포를 합한 결과입니다. 남자를 300명, 여자를 200명으로 가정하여 각각의 값에 300과 200을 곱하였습니다.



```python
import numpy as np
import matplotlib.pyplot as plt

x=np.arange(210,310,0.01)
def gaussian(x,mean,sigma):
  return (1/np.sqrt(2*np.pi*sigma**2)) * np.exp( -(x-mean)**2 / (2*sigma**2))

legend=[]
legend.append('N(270,14)')
legend.append('N(245,12)')

plt.plot(x,gaussian(x,270,14)*300)
plt.plot(x,gaussian(x,240,12)*200)
plt.plot(x,gaussian(x,270,14)*300+gaussian(x,240,12)*200)
plt.xlabel('x')
plt.ylabel('density')
plt.legend(legend)
plt.show()
```


    
![png](https://raw.githubusercontent.com/hi-math/hi-math.github.io/master/images/2022-03-27-Gausian%20mm.md/output_2_0.png)
    


두 이질 집단이 섞여있다고 가정하면 분포는 녹색 그래프처럼 보아뱀 모양을 나타내게 될 것입니다. 가우시안 혼합 모형(이하 GMM)은 하위집단의 개수를 파악하여 분류하는 분류 모형입니다.

## 수학적 배경

하위집단이 정규분포를 따른다고 가정합시다. 그러면 각각의 하위집단은 평균과 표준편차를 파라미터로 가집니다. 그리고 남자 300명, 여자 200명 처럼 전체에서 그 집단이 차지하는 비율이 있습니다. 이렇게 각각의 하위집단은 평균, 표준편차, 비율이라는 세개의 파라미터를 가집니다. 각각을 𝜇, 𝜎, 𝜋 라고 둡시다. 
위의 예처럼 두 개의 하위집단인 경우, 두 집단의 파라미터는 각각 

$( 𝜇_1, 𝜎_1, 𝜋 _1)= (270, 14, \frac{3}{5} )$


$(𝜇_2, 𝜎_2, 𝜋 _2)= (240, 12, \frac{2}{5} )$
입니다.


이를 바탕으로 확률변수 X의 확률밀도함수를 만들면 다음과 같습니다.


$f(x)=\sum_c \pi_c N(x|\mu_c,\sigma_c)$

위 식에서 c는 집단의 인덱스를 의미하며 처음에 들었던 예에서는 남자와 여자 두개의 집단이 있으므로 1,2로 쓸 수 있습니다.

## 최대우도법을 이용하여 추정하기
분포를 알면 우도(likelihood)를 구할 수 있습니다. 그림처럼 발 사이즈의 분포가 주어졌다고 합시다. 그러면 여자의 발 사이즈의 분포와 남자의 발 사이즈 분포를 이용하여 정규분포를 그릴 수 있습니다.

랜덤으로 여자 5명, 남자 5명의 발 사이즈를 추출하고 이를 그래프로 나타내봅시다.


```python
f_fsize= np.random.normal(size=5)*12+240
m_fsize= np.random.normal(size=5)*14+270
zero=np.zeros(5)

x=np.arange(210,310,0.01)

plt.plot(x,gaussian(x,f_fsize.mean(),f_fsize.std()))
plt.plot(x,gaussian(x,m_fsize.mean(),m_fsize.std()))
plt.scatter(f_fsize,zero)
plt.scatter(m_fsize,zero)
plt.xlabel('x')
plt.ylabel('density')
plt.show()
```


    
![png](https://raw.githubusercontent.com/hi-math/hi-math.github.io/master/images/2022-03-27-Gausian%20mm.md/output_7_0.png)
    


위 그래프처럼 어떤 데이터가 어떤 집단에 속하는지 알때, 즉 라벨링이 되어있을 때는 이를 이용하여 분포를 예측할 수 있습니다. 

만약 라벨링이 되어있지 않다면, 주어진 분포에 맞게 우도를 구하여 각 자료가 어떤 분포에 속할 확률을 구할 수 있습니다. 아래그림처럼 말이죠.

<img src = "https://drive.google.com/uc?id=1hPDG-WnJAwpADQNfOsBhNbpEdgmcYXPd" height = 300 width = 400>


256이라는 발 사이즈는 남자일까요? 여자일까요? 파란색 분포는 여자의 발 사이즈를 알려주는 분포입니다. 주황색은 남자의 발 사이즈이죠. 즉 likilihood를 계산하면 256이라는 사이즈는 여자의 발 크기였을 확률이 더 높습니다.

여태까지 우리의 걸음을 되돌아봅시다. 만약 라벨링이 된 데이터를 알고있다면 분포를 알아낼 수 있습니다. 그리고 라벨링이 되지 않은 데이터를 알고있다면 분포를 이용하여 데이터를 라벨링 할 수 있습니다. 최대우도법(MLE)을 이용해서 말이죠.

하지만 문제는 라벨링이 되어있지 않은, 그리고 분포도 알 수 없는 데이터를 알았을 때는 이것을 어떻게 분류해야 할지 알 수 없다는 것 입니다.

아래 코드는 여자 5명과 남자 5명의 발 사이즈를 합쳐 놓은 자료입니다. 어디까지가 남자의 발인지, 여자의 발인지 구분할 수있을까요?


```python
from numpy.ma.core import concatenate
f_fsize= np.random.normal(size=5)*12+240
m_fsize= np.random.normal(size=5)*14+270
fsize = np.concatenate((f_fsize, m_fsize), axis=0)

plt.ylim(-0.01,0.2)
zero=np.zeros(10)
plt.scatter(fsize,zero)
plt.show()
```


    
![png](https://raw.githubusercontent.com/hi-math/hi-math.github.io/master/images/2022-03-27-Gausian%20mm.md/output_11_0.png)
    



# 내이름은 탐정, 코난이죠!
<img src = "https://drive.google.com/uc?id=1CGWi_xI8IzqNxXCg3dQAPq-3XfriY2hV" height = 300 width = 300>

우리의 목적은 이처럼 자료의 분포도 알 수 없고 라벨링도 되어있지 않은 데이터를 이용해 원래 분포를 예측하고 이를 바탕으로 각 데이터를 클러스터링(clustering)하는 것입니다. 이때 데이터들은 정규분포를 이루고 있다고 가정합니다. 즉 정규분포를 이루고 있는 n개의 집단이 섞여있고 이것들을 분류하는데 있어 GMM은 좋은 성능을 발휘할 수 있습니다. 그러면 이제부터 GMM을 이해하는데 필요한 수학적인 배경에 대해 알아봅시다.
                                                  

##닭이 먼저? 달걀이 먼저?
라벨링이 되어있으면 분포를 구할 수 있습니다. 분포를 알면 라벨링을 할 수 있죠. 그런데 라벨링도 되어있지 않고 분포도 알 수 없으면 어떻게 해야 할까요?

그래서 임의의 분포를 먼저 가정하고 이 분포를 바탕으로 라벨링을 한 후 다시 라벨링을 바탕으로 분포를 구합니다. 이 방법을 EM알고리즘이라고 합니다. (사실 정확한 EM알고리즘과는 다릅니다. 아직 각 분포의 비율은 다루지 않았으니까요.)

그렇기 때문에 초기에 어떤 분포를 가정하고 시작하도록 하겠습니다.

아래는 여자 10명(파랑), 남자 10명(주황)의 데이터를 랜덤추출하여 다음 과정을 거친 것입니다.

1. 임의의 정규분포를 가정
2. 정규분포에서 likelihood를 계산
3. likelihood가 큰 것으로 라벨링
4. 라벨링 한것을 바탕으로 다시 정규분포
5. 정규분포를 이용하여 다시 2번과정부터 반복

epoch를 10으로 주었는데 쉬운과제이다보니 3이후에는 변화가 없는 것을 관찰할 수 있습니다.

<img src = "https://drive.google.com/uc?id=1diKwPIdp6AHzRDKJOkSFzgNUeAO2phkH" height = 300 width = 500>



## EM algorithm
하지만 남자와 여자의 수가 같지는 않겠죠? 우리가 처음 세개의 파라미터를 고려하기로 했는데 이제 그걸 알아내봅시다.

드디어 EM algorithm이 필요하게 되었군요.



<b>E-step</b>

일단은 random intialization으로 시작합니다. expaction-step에서는 
$\omega^{(i)}_j = p(z^{(i)} = j | x^{(i)};𝜙,𝜇,𝛴)$ 라는 식으로 쓰여지고 파라미터로 𝜙,𝜇,𝛴 를 사용하게 됩니다.

𝜙는 집단의 비율입니다. 여자와 남자의 성비가 4:6이라면 [0.4, 0.6]을 갖죠. 스텝을 거쳐 업데이트 될 것이므로 초기에는 [0.5, 0.5]로 시작하겠습니다.

𝜇,𝛴는 각각 집단의 평균과 표준편차입니다. 여기서 𝛴는 공분산을 뜻하는데 데이터가 1차원이 아닌 경우 공분산이 표준편차의 역할을 대신합니다. 여기서는 1차원 변수를 다루므로 일단 표준편차로 생각할게요.

만약 i번째 데이터가 256mm라고 합시다. 그렇다면 이 데이터가 여자 혹은 남자집단에 속할 확률 $\omega^{(i)}_여, \omega^{(i)}_남$ 을 합하면 1인 각각의 확률값이 됩니다. 

$$\omega^{(i)}_여 = p(z^{(i)} = 여 | x^{(i)}=256)$$

$$\omega^{(i)}_남 = p(z^{(i)} = 남 | x^{(i)}=256)$$

그리고 당연히 두 확률의 합은 1이죠.

좀 더 정확한 확률을 구하기 위해 베이즈 정리를 사용하면

$$p(z^{(i)} = j | x^{(i)};\phi,\mu,\Sigma)  = \frac {p(x^{(i)}|z^{(i)} = j ; \mu,\sigma)p(z^{(i)} = j; \phi)}{p(x^{(i)};\phi,\mu,\Sigma)}$$

$$=\frac {p(x^{(i)}|z^{(i)} = j ; \mu,\Sigma)p(z^{(i)} = j; \phi)}{\sum^l_{k=1} p(x^{(i)}|z^{(i)} = k ; \mu,\Sigma) p(z^{(i)} = k ; \phi)}$$



입니다.


<b>M-step</b>

E-step를 이용하면 된다. 확률을 통해 집단을 다시 정렬하고 라벨링을 새로 합니다.
새로 구한 그룹의 평균과 표준편차(공분산), 그리고 비율을 결정합니다.


#코딩을 해봅시다.

여자는 평균 240, 표준편차 7에서 700개의 샘플을 추출합니다. 

남자는 평균 270, 표준편차 6에서 300개의 샘플을 추출합니다.

초기 평균은 210, 290 초기 분산은 12, 15, 초기 샘플은 5:5로 가정합니다.


```python
f=np.random.normal(size=700)*7+240
m=np.random.normal(size=300)*6+270

data = np.concatenate((f,m), axis=0)
data.sort()

data1,data2 =[],[]
for i in data:
  if np.random.rand()>0.5 :
    data1.append(i)
  else:
    data2.append(i)

data1 = np.array(data1)
data2 = np.array(data2)

mu=[210,290]
std=[12,15]
phi=[0.5,0.5]
```



```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import norm
import matplotlib.animation as animation

epoch=10

for aa in range(epoch):

  ep = "epochs={}".format(aa)

  label1='N({:.2f}, {:.2f})'.format(mu[0],std[0])
  label2='N({:.2f}, {:.2f})'.format(mu[1],std[1])
  plt.xlabel('x')
  plt.ylabel('density')
  plt.hist(data, color='green', alpha=0.4, bins=50, density=True)
  plt.plot(x,norm(mu[0], std[0]).pdf(x)*phi[0])
  plt.plot(x,norm(mu[1], std[1]).pdf(x)*phi[1])
  plt.scatter(data1,np.zeros(len(data1)))
  plt.scatter(data2,np.zeros(len(data2)))

  plt.title(ep)

  plt.legend((label1, label2), loc='upper right')
  plt.show()


#e-step

  data1_tmp, data2_tmp=[], []
  normal1 = np.array(norm(mu[0],std[0]).pdf(data))
  normal2 = np.array(norm(mu[1],std[1]).pdf(data))

  p_w1 = (normal1*phi[0])/(normal1*phi[0] + normal2*phi[1])
  p_w2 = (normal2*phi[1])/(normal1*phi[0] + normal2*phi[1])


  for i in range(len(data)):
    if p_w1[i]>p_w2[i]:
      data1_tmp.append(data[i])
    else :
      data2_tmp.append(data[i])

  data1 = np.array(data1_tmp)
  data2 = np.array(data2_tmp)

#m-step
  mu=[data1.mean(),data2.mean()]
  std=[data1.std(),data2.std()]
  phi=[len(data1)/(len(data1)+len(data2)),len(data2)/(len(data1)+len(data2)) ]
```

## 결과적으로..

<img src = "https://drive.google.com/uc?id=1Xk4LK5TpC1mGt_G1iEnIehUrX1HWFOz-" height = 300 width = 480>

구분이 잘 되는 집단이라 그런지 3회정도 반복하니까 거의 정확하게 맞았습니다.

음 생각해보니 이정도 라벨링은 그림보면 바로 할 수 있는게 아닐까요?!

내일은 좀 더 실용적인 예를 찾아봐야겠습니다.
