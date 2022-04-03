---
title:  "연습삼아 해봅시다."
tags:
  - GMM
  - scikit-learn
use_math: true
comments: true
---

## scikit-learn 만세
이 업계가 감동적인 이유가 있다면, 
혼자 알고 끝나는 것이 아니라 굳이굳이 그걸 다른사람도 알도록 코드를 공유하거나 라이브러리로
만든다는 것입니다.
게다가 컴퓨터를 잘하는 사람이다보니 전세계 어디서나 볼 수 있는 포맷으로 만드는 것이 익숙하죠.
생각해보니 그렇네요. 지금 쓰고 있는 이 글도 언젠가 그 사람들에게 그렇게 보일 수도 있겠군요.

`scikit-learn`은 머신러닝을 학습할때 빠지지 않는 라이브러리입니다.
각종 도구들과 데이터를 가지고 있으니 아직 실전에 뛰어들기전 끝없는 수련이 필요한 저에게 가장 필요한 사이트일 것입니다.

분류를 위해 먼저 2차원 데이터를 만들어보겠습니다.

```python
import matplotlib.pyplot as plt
from sklearn.datasets import make_blobs
from sklearn.mixture import GaussianMixture
import numpy as np
import pandas as pd
%matplotlib inline


X, y = make_blobs(
    n_samples=200,
    n_features=2,
    centers=3,
    cluster_std=0.5,
    shuffle=True,
    random_state=0
)

plt.scatter(X[:, 0],X[:, 1],c='g', marker='o',  s=20)

plt.show()
```
![생성된 데이터](https://raw.githubusercontent.com/hi-math/hi-math.github.io/master/images/2022-04-03-SampleTest.md/cluster.png)

이 데이터를 분류해봅시다. 누가봐도 세개의 그룹으로 이루어져있군요. 컴포넌트를 3으로 지정하고 결과값을  gmm_labels에 저장합니다.
각각 색깔을 주어 분류결과를 봅시다.

![생성된 데이터](https://raw.githubusercontent.com/hi-math/hi-math.github.io/master/images/2022-04-03-SampleTest.md/classify.png)

눈으로 보기엔 잘 분류된 것 같은데 진짜 그럴까요? 개수를 세어봅시다. 

```python
df = pd.DataFrame({"target":y, "pred":z})
print(df.groupby('target')['pred'].value_counts())
```

![결과](https://raw.githubusercontent.com/hi-math/hi-math.github.io/master/images/2022-04-03-SampleTest.md/result.PNG)

이름은 다르지만 아주 정확하게 분류된 것을 확인했습니다. 2차원에서 잘 작동하는 것을 보았으니 이제 3차원에서 시도해보겠습니다.


## 3차원에 도전

역시 데이터를 만드는 것부터 시작하겠습니다. 3차원데이터를 만들기 때문에 3차원 그래프로 그려야 합니다.
```python
from mpl_toolkits.mplot3d import Axes3D

X, y = make_blobs(
    n_samples=300,
    n_features=3,
    centers=3,
    cluster_std=0.5,
    shuffle=True,
    random_state=0
)

fig = plt.figure(figsize=(6, 6))
ax = fig.add_subplot(111, projection='3d')
ax.scatter(X[:, 0], X[:, 1],X[:, 2], c="b", marker='o', s=15)
plt.show()
```


![결과](https://raw.githubusercontent.com/hi-math/hi-math.github.io/master/images/2022-04-03-SampleTest.md/3ddata.png)


3차원 그래프로 확인한 결과 노란색과 초록색이 좀 가깝습니다. 이를 x-z축으로 표현하면 거의 구분이 안되는 것을 확인할 수 있습니다.

![결과](https://raw.githubusercontent.com/hi-math/hi-math.github.io/master/images/2022-04-03-SampleTest.md/2ddata.png)

2차원으로 분류하면 거의 분류가 되지 않겠지만 다른 차원을 이용하면 잘 분류할 수 있을 것입니다.

`scikit-learn`을 이용하여 분류해봅시다.

```python
gmm = GaussianMixture(n_components=3, random_state=12)
gmm_labels = gmm.fit_predict(X)

z=gmm_labels

df = pd.DataFrame({"target":y, "pred":z})
print(df.groupby('target')['pred'].value_counts())
```

![결과](https://raw.githubusercontent.com/hi-math/hi-math.github.io/master/images/2022-04-03-SampleTest.md/result2.PNG)

각각 100개로 잘 분류했습니다. 비지도학습이다보니 타겟의 이름을 맞추지는 못하는군요.


## GMM 괴롭히기
하지만 이렇게 잘 분류하면 재미가 없으니 이 친구에게 좀 더 고약한 일을 시켜보겠습니다.
표준편차가 크게 데이터를 생성하면 아마 좀 겹치는 녀석들이 생길 것입니다. 표준편차를 좀 크게 해서 섞이는 데이터를 만들어봅시다.

```python
from mpl_toolkits.mplot3d import Axes3D

X, y = make_blobs(
    n_samples=300,
    n_features=3,
    centers=3,
    cluster_std=2,
    shuffle=True,
    random_state=0
)

fig = plt.figure(figsize=(6, 6))
ax = fig.add_subplot(111, projection='3d')
ax.scatter(X[y==0,0], X[y==0, 1], X[y==0, 2], c="y", marker='o', s=15, alpha=0.40)
ax.scatter(X[y==1,0], X[y==1, 1], X[y==1, 2], c="g", marker='o', s=15, alpha=0.40)
ax.scatter(X[y==2,0], X[y==2, 1], X[y==2, 2], c="b", marker='o', s=15, alpha=0.40)

plt.show()
```
![결과](https://raw.githubusercontent.com/hi-math/hi-math.github.io/master/images/2022-04-03-SampleTest.md/3-1.png)

얼핏보아도 군데군데 섞인 녀석들이 보입니다. 노란색과 녹색은 꽤 많이 섞여있군요.


```python
fig = plt.figure(figsize=(6, 6))
ax = fig.add_subplot(111, projection='3d')
color=["r","g","b"]
marker=["o","^","*"]
for i in range(len(y)):
  ax.scatter(X[i,0], X[i,1], X[i,2], c=color[y[i]], marker=marker[z[i]], s=20, alpha=0.80)

plt.show()
```
y는 생성당시에 만들어진 타겟번호입니다. 즉 정답이라고 할 수 있습니다. 0,1,2는 각각 빨강, 녹색, 파란색의 마커를 만들어줍니다.
z는 GMM의 결과 분류된 예측번호입니다. 즉 분류값이라고 할 수 있습니다. 0,1,2는 각각 원, 삼각형, 별모양의 마커를 만들어줍니다.

구분이 완벽하게 이루어졌다면 같은 색은 같은 모양의 마커여야 합니다. 결과를 볼까요?

![결과](https://raw.githubusercontent.com/hi-math/hi-math.github.io/master/images/2022-04-03-SampleTest.md/3-3.png)

군데군데 섞인 것이 보입니다. 파란색 마커는 대체로 삼각형인데 빨간색중에도 삼각형이 있습니다. 자세히 보면 녹색중에도 별모양이 있습니다.
결과를 수치로 확인하면 다음과 같습니다.


![결과](https://raw.githubusercontent.com/hi-math/hi-math.github.io/master/images/2022-04-03-SampleTest.md/3-4.png)

300개의 데이터에서 13개를 잘못 분류했습니다. 하지만 이건 잘못분류했다고 하긴 좀 힘들겁니다. 왜냐면 인간도 분류하기 힘든 영역이니까요

값만보면 충분히 잘못분류할 수 있을 것입니다. 아마 인간도 맥락없이 보기만 했다면 제대로 분류하기 힘들겁니다. 
갑자기 머리 긴 사람을 보고 여자인줄알고 따라다니는 광고가 생각나네요.

<iframe width="560" height="315" src="https://www.youtube.com/embed/krOMSqcJBrI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

이 광고가 벌써 13년 전이네요..