---
title: "[기계학습] 선형회귀 Lasso, Ridge 정규화기법"
excerpt: "선형회귀에서 사용되는 Lasso, Ridge 정규화기법 비교분석"

categories:
  - ai-ml
tags:
  - machine learning
  - linear regression
  - regularization
  - lasso
  - ridge

date: 2025-03-19
last_modified_at: 2025-03-19
---

# Lasso와 Ridge 정규화 기법: 개념과 차이점

기계학습 및 통계 모델에서 회귀분석은 독립 변수와 종속 변수 간의 관계를 설명하거나 예측하는 데에 유용한 도구입니다. 하지만 모델이 너무 복잡해지거나 **과적합(Overfitting)**이 발생할 경우 예측력이 떨어지게 됩니다. 이 문제를 해결하기 위한 방법 중 하나가 **정규화(Regularization)** 기법입니다. 정규화는 모델의 복잡도를 제어하여 **일반화(Generalization)** 성능을 향상시키는데, 그 대표적인 방법이 **Ridge 회귀** 와 **Lasso 회귀**입니다.



## 1. 회귀 계수란?
**회귀 계수(Regression Coefficient)**는 선형 회귀 모델에서 각 독립 변수(\\(x\\))가 종속 변수(\\(y\\))에 미치는 영향력의 크기와 방향을 나타내는 값입니다. 수식으로 보면 다음과 같습니다:

$$
\hat{y} = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \dots + \beta_p x_p
$$

- \\(\beta_0\\) (절편, intercept): 모든 독립 변수 값이 0일 때 예측값(\\(\hat{y}\\))
- \\(\beta_j\\) (회귀 계수): \\(x_j\\)가 한 단위 증가할 때 \\(y\\)가 평균적으로 \\(\beta_j\\)만큼 증가(양수) 또는 감소(음수) 함을 의미

이러한 회귀계수의 **부호(Sign)**가 양수(+)이면 독립변수와 종속변수는 정(+)의 관계임을 나타내고, 음수(-)이면 반(-)의 관계임을 나타냅니다. 또한 회귀 계수의 절댓값이 클수록 해당 변수가 예측에 끼치는 영향이 크다는 의미입니다.



## 2. 정규화의 필요성

아래는 일반적인 선형 회귀의 **Ordinary Least Squares(OLS)** 비용 함수입니다.
이 비용 함수는 모델이 관측된 데이터를 얼마나 잘 설명하는지(즉, 오차가 얼마나 작은지)를 평가합니다. 

$$
J(\beta) = \sum_{i=1}^{n} \left(y_i - \beta_0 - \sum_{j=1}^{p} \beta_j x_{ij}\right)^2
$$

- \\(y_i\\) : \\(i\\) 번째 관측치의 실제 종속 변수 값
- \\(\beta_0\\) : 상수항(절편, bias)
- \\(\beta_j\\) : \\(j\\) 번째 독립 변수에 대한 회귀 계수
- \\(x_{ij}\\) : \\(i\\) 번째 관측치의 \\(j\\) 번째 독립 변수 값
- \\(n\\) : 총 관측치의 수
- \\(p\\) : 독립 변수(특성)의 개수

\\(J(\beta)\\)는 쉽게 말해 모델이 얼마나 **나쁜지** 측정하는 비용 함수(cost function) 입니다. 우리가 목표하는 것은 이 \\(J(\beta)\\)를 **최소화** 하는 \\(\beta\\)를 찾는 것입니다. 위 OLS 함수(\\(J(\beta)\\))를 최소화하면 **잔차 제곱합(RSS)**이 작아지는, 즉 데이터에 가장 잘 맞는 \\(\beta\\)가 선택됩니다.

회귀 모델은 이런 식으로 잔차 제곱합을 최소화하는 방식으로 파라미터를 추정합니다. 하지만 독립 변수의 개수가 많거나 서로 강하게 상관되어 있을 때, 회귀 계수가 불안정해지거나 과도하게 커질 수 있습니다. 이 경우 모델은 훈련 데이터에는 잘 맞지만 새로운 데이터에 대해서는 성능이 저하되는 **과적합** 문제가 발생합니다.

$$
J(\beta) = \sum_{i=1}^{n} \left(y_i - \beta_0 - \sum_{j=1}^{p} \beta_j x_{ij}\right)^2 + Penalty(\beta_j)
$$

그런데 만약 이와같이 \\(J(\beta)\\)에 패널티 항이 추가된다면, 해당 패널티 항을 통해 \\(J(\beta)\\)의 최소화를 조절할 수 있게 될 것입니다. 만약 어떤 \\(\beta_j\\)가 너무 큰 경우에 \\(Penalty(\beta_j)\\) 항이 전체 비용 함수를 크게 늘려버리도록 한다면, \\(J(\beta)\\)를 최소화하는 \\(\beta\\)를 찾으려 할 때 전체 함수의 최종 최소값을 위해서는 해당 \\(\beta_j\\)를 작게 만들어야 할 것입니다.

이런 식으로 정규화 기법은 모델 학습 과정에서 비용 함수(Cost function)에 제약 조건을 추가하여 계수의 크기를 제한합니다. 이렇게 하면 모델이 불필요하게 큰 회귀 계수를 갖지 않도록 조정하여 과적합을 방지합니다.


## 3. Ridge 회귀

**Ridge 회귀**는 **L2 정규화(L2 regularization)**를 사용하는 방법입니다. 모델의 비용 함수에 계수들의 제곱합(즉, L2 norm)을 추가함으로써 회귀 계수의 크기를 줄입니다.

$$
J_{\text{ridge}}(\beta) = \sum_{i=1}^{n} \left(y_i - \beta_0 - \sum_{j=1}^{p} \beta_j x_{ij}\right)^2 + \lambda \sum_{j=1}^{p} \beta_j^2
$$

- \\(\lambda\\) : 정규화 강도를 조절하는 하이퍼파라미터.
- \\(\lambda\\) 값이 크면 회귀 계수의 크기를 더 강하게 축소시키고, \\(\lambda\\) 값이 작으면 원래 OLS 모델에 가까운 형태가 됩니다.

Ridge 회귀는 앞서 설명한대로 모델이 훈련 데이터에 과도하게 맞춰지지 않도록 회귀 계수의 크기를 ‘작게’ 유지합니다. 수식에서 추가되는 L2 페널티(계수 제곱의 합)는 모든 변수의 계수를 동시에 축소하지만, 특정 계수를 완전히 0으로 만들지는 않습니다. 덕분에 독립 변수 간에 높은 상관관계(다중공선성)가 있을 때도 계수 추정이 안정적이며, 모델이 지나치게 복잡해지는 것을 막아 과적합 위험을 낮춥니다.

## 4. Lasso 회귀

Lasso 회귀는 **L1 정규화(L1 regularization)**를 사용하는 방법입니다. 비용 함수에 계수들의 절댓값 합(L1 norm)을 추가함으로써 회귀 계수의 크기를 제한하고, 일부 계수는 0으로 만들어 변수 선택 효과를 얻습니다.

Lasso 회귀의 비용 함수는 다음과 같이 정의됩니다. 마찬가지로 \\(\lambda\\)는 정규화의 강도를 조절하는 하이퍼파라미터입니다.

$$
J_{\text{lasso}}(\beta) = \sum_{i=1}^{n} \left(y_i - \beta_0 - \sum_{j=1}^{p} \beta_j x_{ij}\right)^2 + \lambda \sum_{j=1}^{p} |\beta_j|
$$

Lasso 회귀는 L1 페널티(계수 절댓값의 합)를 사용해 불필요한 변수의 계수를 **‘0’**으로 만들어 버리는 특징이 있습니다. 결과적으로 모델에 실제로 기여하지 않는 특성은 제거되고, 남은 변수만으로 예측이 이루어지므로 해석이 쉽고 계산량도 줄어듭니다. 이처럼 변수 선택 기능이 자연스럽게 내장되어 있어, 많은 특성 중 핵심 변수를 골라내야 할 때 특히 유용합니다.


## 5. Lasso와 Ridge의 차이점
두 방법 모두 모델의 일반화 능력을 향상시키기 위한 정규화 기법이지만, 패널티 항의 형태에 따른 차이를 가집니다.

패널티 항의 형태:
- Ridge: \\(\lambda \sum_{j=1}^{p} \beta_j^2\\) 를 사용하여 모든 계수를 작게 만드는 데 초점을 맞춥니다.
- Lasso: \\(​\lambda \sum_{j=1}^{p} |\beta_j|\\) 를 사용하여 일부 계수를 0으로 만들 수 있습니다.

두 기법을 비교해 보면, Ridge는 모든 변수를 유지하면서 계수 값을 작게 만들어 안정성을 높이고 다중공선성을 완화하는 데 강점을 가지며, Lasso는 변수 선택을 통해 모델을 단순화하고 중요하지 않은 특성을 제거함으로써 해석성을 높입니다.

결론적으로 **Ridge 회귀는 변수 수가 많고 서로 연관성이 높을 때 신뢰할 수 있는 계수 추정을 원할 때 선택**하고, **Lasso 회귀는 모델을 간단하게 만들고 변수 선택이 필요할 때 선택**합니다. 

필요에 따라 두 방법의 장점을 결합한 **Elastic Net**을 사용하면, 변수 선택과 계수 축소를 동시에 수행하여 보다 견고한 예측 모델을 만들 수도 있습니다.


## 6. Ridge, Lasso 비교 시각화

아래의 코드를 사용하여 Ridge, Lasso 회귀의 비교를 시각화하겠습니다.



### 계수 시각화
<img src="https://github.com/user-attachments/assets/e17854e3-c1a4-4c04-9015-6004139d54c3" width="800">
위 그림은 데이터 생성 과정에서의 실제 계수를 보여줍니다.

<p align="center"><img src="https://github.com/user-attachments/assets/c56883b3-6ec9-4b2b-838d-e16c72fecd4f" width="1000"></p>
위 그림은 다양한 알파 값에서의 **Ridge** 계수를 보여줍니다. 알파가 증가함에 따라 Ridge 계수는 0을 향해 줄어들지만 거의 정확히 0이 되지는 않는 것을 보여줍니다.

<p align="center"><img src="https://github.com/user-attachments/assets/91128eed-6ad1-48b8-96db-4d2d667efed0" width="1000"></p>
위 그림은 다양한 알파 값에서의 **Lasso** 계수를 보여줍니다. Lasso 계수는 알파가 증가함에 따라 종종 정확히 0이 되며, 이는 Lasso의 특성 선택 능력을 보여줍니다.

### 0계수 개수
<p align="center"><img src="https://github.com/user-attachments/assets/6dc00ace-046f-4d46-9f51-2fe2e60ae9c9" width="400"></p>
위 그림은 각 알파 값에 대해 Lasso가 정확히 0으로 설정하는 계수 수를 보여줍니다.

알파가 증가함에 따라 더 많은 계수가 0이 되어, Lasso가 관련 없는 특성을 완전히 제거하여 특성 선택을 수행하는 능력을 확인할 수 있습니다.

### 정규화 경로
<p align="center"><img src="https://github.com/user-attachments/assets/e8d9261d-e64d-4d56-a5b3-c58b270bd71b" width="800"></p>
알파가 증가함에 따라 Ridge 계수(왼쪽)와 Lasso 계수(오른쪽)가 어떻게 변하는지 보여줍니다.

Ridge 계수는 알파가 증가함에 따라 0을 향해 부드럽게 감소합니다. Lasso 계수는 종종 특정 알파 값에서 0에 도달하고 그 이후로는 0을 유지하며, 더 급격한 전환을 보여줍니다.


### 성능 지표
<p align="center"><img src="https://github.com/user-attachments/assets/5a1b746e-6d66-417f-8bf8-5cc53a84fc86" width="300"></p>

MSE(평균 제곱 오차) 출력은 다양한 알파 값에서 각 모델의 성능을 보여줍니다.

- 알파 값이 매우 작을 때, 두 모델은 선형 회귀와 유사하게 작동합니다.
- 알파 값이 매우 클 때, Ridge는 과도하게 패널티를 부여하고, Lasso는 너무 많은 특성을 제거할 수 있습니다.
편향과 분산의 균형을 맞추는 최적의 알파 값을 찾아 적절한 정규화를 적용할 수 있습니다.


아래는 위 시각화에 사용한 소스코드입니다.
```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import Ridge, Lasso, LinearRegression
from sklearn.preprocessing import StandardScaler
from sklearn.datasets import make_regression
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error

# Set random seed for reproducibility
np.random.seed(42)

# 1. Generate synthetic data
# 20 features, but only 10 are informative (actually affect the target)
X, y, coef = make_regression(n_samples=100, n_features=20, n_informative=10, 
                            noise=10, coef=True, random_state=42)

# 2. Split data into train and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# 3. Scale the data (important for regularization techniques)
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# 4. Train models with various alpha (regularization strength) values
alphas = [0.01, 0.1, 1.0, 10.0, 100.0]
ridge_models = []
lasso_models = []

for alpha in alphas:
    # Train Ridge model
    ridge = Ridge(alpha=alpha, random_state=42)
    ridge.fit(X_train_scaled, y_train)
    ridge_models.append(ridge)
    
    # Train Lasso model
    lasso = Lasso(alpha=alpha, random_state=42, max_iter=10000)
    lasso.fit(X_train_scaled, y_train)
    lasso_models.append(lasso)

# Train regular linear regression model
lr = LinearRegression()
lr.fit(X_train_scaled, y_train)

# 5. Evaluate models - Performance Metrics
print("="*50)
print("Model Performance Comparison (MSE)")
print("="*50)
print(f"Linear Regression MSE: {mean_squared_error(y_test, lr.predict(X_test_scaled)):.4f}")

for i, alpha in enumerate(alphas):
    ridge_pred = ridge_models[i].predict(X_test_scaled)
    lasso_pred = lasso_models[i].predict(X_test_scaled)
    
    ridge_mse = mean_squared_error(y_test, ridge_pred)
    lasso_mse = mean_squared_error(y_test, lasso_pred)
    
    print(f"Alpha={alpha}:")
    print(f"  Ridge MSE: {ridge_mse:.4f}")
    print(f"  Lasso MSE: {lasso_mse:.4f}")

# 6. Visualize coefficients - key difference between Ridge and Lasso
plt.figure(figsize=(18, 10))

# True coefficients
plt.subplot(3, 1, 1)
plt.stem(range(len(coef)), coef)
plt.title('True Coefficients', fontsize=15)
plt.xlabel('Coefficient Index')
plt.ylabel('Coefficient Value')

# Ridge coefficients (various alpha values)
plt.subplot(3, 1, 2)
for i, alpha in enumerate(alphas):
    plt.plot(ridge_models[i].coef_, 'o-', label=f'Ridge (alpha={alpha})')
plt.plot(lr.coef_, 'o-', label='Linear Regression')
plt.title('Ridge Regression Coefficients', fontsize=15)
plt.xlabel('Coefficient Index')
plt.ylabel('Coefficient Value')
plt.legend(bbox_to_anchor=(1.05, 1), loc='upper left')

# Lasso coefficients (various alpha values)
plt.subplot(3, 1, 3)
for i, alpha in enumerate(alphas):
    plt.plot(lasso_models[i].coef_, 'o-', label=f'Lasso (alpha={alpha})')
plt.plot(lr.coef_, 'o-', label='Linear Regression')
plt.title('Lasso Regression Coefficients', fontsize=15)
plt.xlabel('Coefficient Index')
plt.ylabel('Coefficient Value')
plt.legend(bbox_to_anchor=(1.05, 1), loc='upper left')

plt.tight_layout()
plt.show()

# 7. Visualize number of zero coefficients (Lasso's feature selection property)
zero_coef_count = []
for model in lasso_models:
    # Consider very small values (< 1e-10) as zero
    # Use int() to ensure the count is an integer
    zero_coef_count.append(int(np.sum(np.abs(model.coef_) < 1e-10)))

plt.figure(figsize=(10, 6))
plt.bar(range(len(alphas)), zero_coef_count)
plt.xticks(range(len(alphas)), [str(alpha) for alpha in alphas])
plt.title('Lasso: Number of Zero Coefficients by Alpha Value', fontsize=15)
plt.xlabel('Alpha Value')
plt.ylabel('Number of Zero Coefficients')
plt.show()

# 8. Regularization paths for Ridge and Lasso (how weights change with alpha)
plt.figure(figsize=(15, 6))

# Ridge regularization path
plt.subplot(1, 2, 1)
for i in range(X.shape[1]):
    plt.plot([np.log10(alpha) for alpha in alphas], 
             [model.coef_[i] for model in ridge_models], '-')
plt.title('Ridge Regularization Path', fontsize=15)
plt.xlabel('log(Alpha)')
plt.ylabel('Coefficient Value')

# Lasso regularization path
plt.subplot(1, 2, 2)
for i in range(X.shape[1]):
    plt.plot([np.log10(alpha) for alpha in alphas], 
             [model.coef_[i] for model in lasso_models], '-')
plt.title('Lasso Regularization Path', fontsize=15)
plt.xlabel('log(Alpha)')
plt.ylabel('Coefficient Value')

plt.tight_layout()
plt.show()
```