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

scikit-learn은 머신러닝을 학습할때 빠지지 않는 라이브러리입니다.
각종 도구들과 데이터를 가지고 있으니 아직 실전에 뛰어들기전 끝없는 수련이 필요한 저에게 가장 필요한 사이트일 것입니다.

분류를 위해 먼저 2차원 데이터를 만들어보겠습니다.

```python
import matplotlib.pyplot as plt
from sklearn.datasets import make_blobs
from sklearn.mixture import GaussianMixture
import numpy as np
import pandas as pd
%matplotlib inline


n = 200
xmin, xmax, ymin, ymax= 0, 20, 0, 20
X, y = make_blobs(
    n_samples=n,
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

