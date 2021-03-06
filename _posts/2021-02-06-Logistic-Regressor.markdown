# 로지스틱 회귀

# Logistic Regression

## 이론

로지스틱 회귀는 선형 회귀처럼 종속 변수와 독립 변수간의 관계를 성립하여 특정 데이터의 값을 예측한다. 하지만 로지스틱 회귀는 선형 회귀와 다르게 일정 범위 내에서 추론해내고 특정 데이터가 입력되었다면 그 데이터가 어느 분류에 속하는지를 반환하기 때문에 일종의 분류(Classification) 기법이라고 할 수 있다. 다음은 위키백과의 내용이다 : 

<aside>
📌 로지스틱 회귀의 목적은 일반적인 [**회귀 분석**](https://ko.wikipedia.org/wiki/%ED%9A%8C%EA%B7%80_%EB%B6%84%EC%84%9D)의 목표와 동일하게 **[종속 변수](https://ko.wikipedia.org/wiki/%EB%8F%85%EB%A6%BD_%EB%B3%80%EC%88%98%EC%99%80_%EC%A2%85%EC%86%8D_%EB%B3%80%EC%88%98)**와 독립 변수간의 관계를 구체적인 함수로 나타내어 향후 예측 모델에 사용하는 것이다. 이는 독립 변수의 선형 결합으로 **종속 변수**를 설명한다는 관점에서는 [선형 회귀](https://ko.wikipedia.org/wiki/%EC%84%A0%ED%98%95_%ED%9A%8C%EA%B7%80) 분석과 유사하다. 하지만 로지스틱 회귀는 [선형 회귀](https://ko.wikipedia.org/wiki/%EC%84%A0%ED%98%95_%ED%9A%8C%EA%B7%80) 분석과는 다르게 종속 변수가 범주형 데이터를 대상으로 하며 입력 데이터가 주어졌을 때 해당 데이터의 결과가 특정 분류로 나뉘기 때문에 일종의 **분류([classification](https://en.wikipedia.org/wiki/classification)) 기법**으로도 볼 수 있다.

</aside>

어떤 데이터가 주어졌을때, 그 데이터가 분류 A, 분류 B, 분류 C ....의 확률을 구하는 것이다.

K-NN 회귀를 이용해서 구할수도 있고, 로지스틱 회귀를 이용해서 구할 수 있다. 하지만 K-NN회귀는 최근접 이웃의 개수(n-neighbors)를 분모로 하여 확률을 계산한다. 때문에 모델마다 다른 확률이 나올 수 있고 부정확하다.

하지만 로지스틱 회귀는 다항 회귀와 같이 계수와 절편을 구해 알맞은 방정식을 세우고 그 방정식을 통과해 각 데이터들을 방정식에 대입했을 때 나오는 값 z을 **시그모이드 함수 혹은 소프트맥스 함수**에 통과시켜 확률을 도출해 낸다.

---

시그모이드 함수란 z값에 따라 0과 1사이의 결과값(Φ)를 반환하는 함수이다. z값이 작을수록 Φ는 0에 수렴하고, z값이 클수록 Φ는 1에 수렴하는 다음과 같은 함수이다.

![Untitled](%E1%84%85%E1%85%A9%E1%84%8C%E1%85%B5%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%A8%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%2016265685b00c476aa50007c3e939fc9d/Untitled.png)

시그모이드 함수의 식은 다음과 같다.

$$
⁍
$$

---

### 코드

**K-NN 회귀**

```python
from sklearn.neighbors import KNeighborsClassifier
kn = KNeighborsClassifier(n_neighbors = 3)
kn.fit(train_scaled, train_target)
print('\n Train & Test score : ')
print("train_scaled : " + str(kn.score(train_scaled, train_target)))
print("test_scaled : " + str(kn.score(test_scaled, test_target)))

print('\n KN Classes : ')
print(kn.classes_)
```

![Untitled](%E1%84%85%E1%85%A9%E1%84%8C%E1%85%B5%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%A8%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%2016265685b00c476aa50007c3e939fc9d/Untitled%201.png)

kn.classes_ 에는 데이터들의 특성이 담겨있다.

테스트 세트의 처음 다섯 개 샘플들을 가져와 예측해보자.

K-NN회귀의 predict()함수를 사용하면 예측 결과를 얻을 수 있다.

또 predict_proba()함수를 사용하면 각 예측 결과의 확률을 얻을 수 있다.

```python
# 처음 5개의 샘플 타깃 출력
print('\n 5 predicts of test_scaled : ')
print(kn.predict(test_scaled[:5]))
# ['Perch' 'Smelt' 'Pike' 'Perch' 'Perch'] 로 예측중. 확률을 구해보자

# 각각의 확률 출력
import numpy as np
proba = kn.predict_proba(test_scaled[:5])
print('\n Each probablity : ')
print(np.round(proba, decimals = 4))
```

![Untitled](%E1%84%85%E1%85%A9%E1%84%8C%E1%85%B5%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%A8%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%2016265685b00c476aa50007c3e939fc9d/Untitled%202.png)

예측 결과를 보면 1/3, 2/3, 3/3 으로 분모가 3인 분수로만 이루어져 있다. 이는 K-NN 회귀 모델의 n_neighbors (n개의 최근접 이웃)이 3으로 정해져 있기 때문이다.

---

**로지스틱 회귀 (Logistic Regression)**

로지스틱 회귀를 이용해보자.

---

먼저 **이진분류**. 정답은 양성 클래스 (1) 과 음성 클래스(0)만 존재하는 경우이다.

```python
bream_smelt_indexes = (train_target == "Bream") | (train_target == "Smelt")
train_bream_smelt = train_scaled[bream_smelt_indexes]
target_bream_smelt = train_target[bream_smelt_indexes]

# 로지스틱 회귀 (Logistic Regression)을 이용해 학습하고 확률을 구할 것.
# 학습하는 방법 : 1. 예측을 해서(fit) 방정식의 계수(coefficient)를 정의한다.
from sklearn.linear_model import LogisticRegression
lr = LogisticRegression()
lr.fit(train_bream_smelt, target_bream_smelt)
```

여기서 바로 lr.predict()와 lr.predict_proba()를 이용해 확률을 구할 수 있다.

```python
print("lr predict , predict_proba : ")
print(lr.predict(train_bream_smelt[:5]))
print(lr.predict_proba(train_bream_smelt[:5]))
```

![Untitled](%E1%84%85%E1%85%A9%E1%84%8C%E1%85%B5%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%A8%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%2016265685b00c476aa50007c3e939fc9d/Untitled%203.png)

하지만 계수를 직접 구해서 z값을 구한 후 시그모이드 함수에 넣어 결과값을 도출해 볼 수도 있다.

```python
print("\nCoefficients of Logistic Regression for Binary Classification :")
print("coef : " + str(lr.coef_))
print("intercept : " + str(lr.intercept_))

decisions = lr.decision_function(train_bream_smelt[:5])
print("Z(decisions) : " + str(decisions))
```

![Untitled](%E1%84%85%E1%85%A9%E1%84%8C%E1%85%B5%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%A8%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%2016265685b00c476aa50007c3e939fc9d/Untitled%204.png)

이는 다음과 같은 식이 성립함을 알 수 있다.

$$
z = -0.404 * (특성1) -0.576 * (특성2) -0.663 * (특성3) -1.013 * (특성4) - 0.732 * (특성5) -2.162
$$

그리고 Z(decisions) 는 train_bream_smelt의 0번째 부터 4번째 까지의 샘플들을 위 방정식에 넣었을 때 나오는 값이다.

이제 이 z를 시그모이드 함수에 넣어 확률을 구해보자.

```python
from scipy.special import expit
print("\nResult of sigmoid function with decisions by expit function")
print(expit(decisions))
```

![Untitled](%E1%84%85%E1%85%A9%E1%84%8C%E1%85%B5%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%A8%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%2016265685b00c476aa50007c3e939fc9d/Untitled%205.png)

그 결과는 위 lr.predict_proba()의 2열 내용과 같음을 알 수 있다.

**LogisticRegression**의 predict_proba() 메서드는 음성/양성 클래스의 확률을 모두 보여주고, z값을 시그모이드 함수에 넣어 나온 값은 양성 클래스의 확률 값만 도출한다.

---

**다중분류**

다중분류는 이진분류와 큰 차이가 없다. 다만, 시그모이드 함수를 사용하지 않고 소프트맥스 함수를 사용해 decisions값을 0~1사이의 값으로 변환한다.

```python
lr2 = LogisticRegression(C=20, max_iter=1000) 
# C는 1/alpha 즉 C가 작을수록 규제가 큼, max_iter는 충분히 훈련시키기 위한 반복횟수
lr2.fit(train_scaled, train_target)
print("\nTrain & Test score")
print("Train Score : " +  str(lr2.score(train_scaled, train_target)))
print("Test Score : " + str(lr2.score(test_scaled, test_target)))

# 점수가 괜찮으니 처음 5개 샘플에 대한 예측
print("\n Predict for 5 test_scaled")
print(lr2.predict(test_scaled[:5]))

#  5개 샘플에 대한 예측 확률 ?
print("\n Percentage of 5 test_scaled") 
proba = lr2.predict_proba(test_scaled[:5])   
print(np.round(proba, decimals = 3))
```

여기서 C는  $1/alpha$ 값이다. 로지스틱 회귀는 릿지 회귀와 같이 계수의 제곱을 규제하고, 릿지 회귀에 alpha 매개변수가 있듯이 로지스틱 회귀에서도 비슷한 역할을 하는 (하지만 alpha의 역수인) C 매개변수가 존재한다.

![Untitled](%E1%84%85%E1%85%A9%E1%84%8C%E1%85%B5%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%A8%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%2016265685b00c476aa50007c3e939fc9d/Untitled%206.png)

이번엔 decisions값을 이용해 소프트맥스 함수에 넣어 나온 값으로 확률을 구해보자.

```python
decisions2 = lr2.decision_function(test_scaled[:5])
print("\nDecisions2 (Z) of 7 Species : ")
print(np.round(decisions2, decimals = 2))

# z값 결과를 softmax에 의뢰
from scipy.special import softmax
print("\n PREDICT OF 5 decisions2 using Softmax")
proba2 = softmax(decisions2, axis = 1) # axis=1이면 각 샘플에 대한 소프트맥스를 계산함.
print(np.round(proba2, decimals = 3))
```

![Untitled](%E1%84%85%E1%85%A9%E1%84%8C%E1%85%B5%E1%84%89%E1%85%B3%E1%84%90%E1%85%B5%E1%86%A8%20%E1%84%92%E1%85%AC%E1%84%80%E1%85%B1%2016265685b00c476aa50007c3e939fc9d/Untitled%207.png)

위의 predict_proba() 메서드를 통한 결과와 동일한다는 것을 알 수 있다.
