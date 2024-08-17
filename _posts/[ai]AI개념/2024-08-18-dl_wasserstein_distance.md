---
title: "[딥러닝] Wasserstein 거리(Wasserstein Distance(Earth-Mover Distance))"
excerpt: "생성 모델에서 사용되는 Wasserstein 거리 개념 정리"

categories:
  - ai-dl
tags:
  - deep learning
  - generative model

date: 2024-08-18
last_modified_at: 2024-08-18
---

거리를 재는 방식은 유클리드 거리(Euclidean Distance), 맨하튼 거리(Manhattan Distance) 등 많습니다.

거리를 잰다는 것은 두 점 사이의 거리를 재는 것을 말하기도 하지만, 두 확률 분포 사이의 거리를 재는 것을 말하기도 합니다.

**Wasserstein Distance** 또한 거리를 나타내는 것 중 한 가지 방식입니다. 

본 내용에서는 Wasserstein Distance를 이용하여 두 확률 분포(Probability Distribution)의 거리를 재보겠습니다.


## Distances(Metric) vs Divergence


P와 Q를 서로 다른 확률분포라고 둘 때, 확률분포에서의 Distance 또는 Metric은 아래의 세가지 조건을 모두 만족합니다.

Divergence는 Distance보다 약한 개념이기는 하지만, 이 또한 거리의 개념(거리의 대략적인 추세?)를 나타내기에 많이 사용됩니다.

<p align="center"><img src="https://github.com/user-attachments/assets/f4959cd8-0a87-4d10-8089-c4dd2d63e60a" width="600"></p>


우선 위 세가지 조건을 설명하겠습니다.

**(1) 첫 번째 조건은 이 두 확률분포 사이의 거리는 0보다 크거나 같아야 한다는 것입니다.**

- 이건 어떻게 보면 당연한 말이죠. 두 분포 사이의 거리가 0보다 작은 경우가 생기면 이것은 거리 개념으로서는 전혀 사용할 수가 없습니다. 또 하나 여기에 추가되는 조건은 이 두 분포 사이의 거리가 0인 경우는 이 두 분포가 "같은 경우"밖에 없다. 이런 뜻입니다.

**(2) 두번째 조건은 대칭성(Symmetric)을 만족해야 한다는 것입니다.**

- 두 분포를 바꿔도 거리가 같아야 한다는 뜻입니다.

**(3) 세번째 조건은 삼각부등식(triangle inequality)을 만족해야 한다는 것입니다.**

- 점의 거리에서도 그러하듯이, 세개의 분포 사이에서 삼각 부등식을 만족해야 합니다.

<br>

**이 세가지 조건을 모두 만족하면 Distance라고 부르고,**

**세가지 조건 중 1번 조건만 만족하면 Divergence라고 부릅니다.**

---

자주 사용하는 예시를 보겠습니다. 본 내용에서 따로 증명은 보이지 않겠습니다.

<p align="center"><img src="https://github.com/user-attachments/assets/5ce2b745-e07e-4179-8a15-0930d6a77ef2" width="800"></p>

Total Variation Distance(TV distance)는 위 세가지 조건을 모두 만족하므로 distance입니다.<br>

<p align="center"><img src="https://github.com/user-attachments/assets/203ea18d-4b93-4595-9e7c-776c8945fa61" width="800"></p>

Kullback-Leibler divergence는 직관적으로도 대칭성을 만족하고 있지 않죠. 2번과 3번 조건을 만족하고 있지 못하므로 distance가 아니라 divergence입니다.<br>

<p align="center"><img src="https://github.com/user-attachments/assets/0e1cd2e1-7d09-4a25-896c-7093de4debea" width="800"></p>

Jensen-Shannon divergence는 직관적으로 대칭성을 만족하고 있습니다. 실제로도 2번 대칭성 조건은 만족하지만, 3번 삼각 부등식 조건을 만족하지 못해 distance가 아니라 divergence입니다.
<br>

### Infimum과 Supremum 개념


Infimum 개념은 다음과 같습니다. 본 내용에서는 깊게 다루지 않으므로, 이해를 위해선 rough하게 Minimum으로 생각하셔도 좋습니다.

<p align="center"><img src="https://github.com/user-attachments/assets/652ea815-e5dd-40bc-a592-0fb898c2041e" width="500"></p>


위 TV distance에 사용되기도 한 Supremum 개념은 다음과 같습니다. 이또한 이해를 위해선 rough하게 Maximum으로 생각하셔도 좋습니다.

<p align="center"><img src="https://github.com/user-attachments/assets/6348a04e-2033-49b0-9455-09b25ab504a2" width="500"></p>

<br>


### Wasserstein Distance(Earth-Mover Distance)의 원리
이제 Earth-Mover Distance라고도 불리는 Wasserstein Distance의 원리에 대해서 보겠습니다.

왜 Earth-Mover Distance(ER distance) 이라고 불리는지 원리를 이해하면서 살펴보겠습니다.



Wasserstein Distance의 공식은 다음과 같습니다.

<p align="center"><img src="https://github.com/user-attachments/assets/ec75c036-5477-48f3-901f-8fef9702edd4" width="400"></p>

<p align="center"><img src="https://github.com/user-attachments/assets/d8027220-748d-41be-95eb-ba5c94cb5fa5" width="800"></p>

공식을 보시면, **joint distribution** 인 감마의 모든 집합인 ∏(P,Q)에서 infimum(minimum) 값을 뽑습니다.





이 joint distribution 감마에 대해서 보겠습니다.

다음과 같은 파란색과 빨간색의 연속확률분포가 있다고 생각해보겠습니다. 파란색 분포를 P, 빨간색 분포를 Q라고 부르겠습니다.

여기서 파란색, 빨간색 박스는 이 연속확률분포를 이해를 위해 이산적으로 생각한 것입니다.

<p align="center"><img src="https://github.com/user-attachments/assets/2a8e7716-625b-40b7-8b12-3db651abef4b" width="400"></p>

x는 확률 분포 P를 따르는 변수, y는 확률 변수 Q를 따르는 변수라고 두면, 아래와 같은 감마 joint distribution을 만들 수 있을 것입니다.

<p align="center"><img src="https://github.com/user-attachments/assets/4875fccc-b95c-4be5-af64-020a23803393" width="200"></p>

<p align="center"><img src="https://github.com/user-attachments/assets/e8044a62-9be4-45c5-b701-41098088392a" width="700"></p>

표를 자세히 분석해 보면 각 원소의 행/열을 더한 값이 분포에 잘 나타나 있는 것을 볼 수 있습니다.



그럼 이렇게 잘 나타낸 joint distribution을 가지고 이 확률 분포의 빈 박스에 숫자를 매겨보겠습니다. 한 개 있는 것은 숫자 하나씩, 두개 있는 것을 숫자 두 개씩 번호를 줬습니다.

<p align="center"><img src="https://github.com/user-attachments/assets/38a35699-c4a8-4d08-8235-c52c3fe6550d" width="700"></p>

그것을 왼쪽 분포에 표시해보면, 예를 들어 새로 매긴 숫자 1은 x에서는 1에, y에서는 3에 위치해 있습니다. 분포에 잘 들어가 있죠. 이번엔 숫자 3을 보면, x에는 2에 y에는 5에 잘 들어가 있죠. 이런 식으로 모두 표기하면 왼쪽과 같습니다.



그럼 이제 P분포를 Q분포로 이동해보겠습니다. Joint distribution에 나와있는 짝에 맞춰서, P의 4을 Q의 4으로 이동해보겠습니다. 이 경우에는 오른쪽으로 세 칸을 이동하죠? 이것을 모든 분포에 대해 적용하여 행으로 움직인 양을 계산하면 아래와 같습니다. 총 19이죠.

<p align="center"><img src="https://github.com/user-attachments/assets/268cc7a7-4fb0-4ef8-b01c-52bd71b3eb38" width="400"></p>




하지만, 이 joint distribution은 하나만 만들어지는 것이 아니라, 어떻게 설정하냐에 따라서 더 만들어 질 수 있습니다.

아래는 같은 distribution에서 추출한 x와 y의 다른 두번째 감마 joint distribution 조합을 표로 나타낸 것입니다.

<p align="center"><img src="https://github.com/user-attachments/assets/92010ae8-8d99-4ff5-b5e1-5f14030cf618" width="200"></p>

똑같은 과정을 거쳐, 새로운 joint distribution에 또 다시 번호를 매기고

<p align="center"><img src="https://github.com/user-attachments/assets/94efbef5-a4e1-4886-a958-e5d0f63a6d43" width="700"></p>


총 행 이동량을 계산하면 21입니다.

<p align="center"><img src="https://github.com/user-attachments/assets/325946dc-f3c9-45cc-a0e2-c6996542c21d" width="400"></p>


똑같은 분포에서 joint distribution을 뽑아 이동량을 똑같이 계산했는데 하나는 19이고, 하나는 21입니다.

왜 이런차이가 발생하냐면,

- 첫번째 감마에서는 모든 원소가 아래의 4와 5처럼 (파란색 -> 빨간색)오른쪽으로만 이동했는데
- 두번째 감마에서는 7의 예시처럼 (파란색 -> 빨간색) 역순으로 이동하는 경우가 존재합니다.

<p align="center"><img src="https://github.com/user-attachments/assets/97186918-4bda-4f1b-8371-ce9357a671a6" width="800"></p>

이렇게 역순으로 가는 곳에서 비효율이 발생해서, 총 이동량이 증가한 것입니다.

---
<br>
<p align="center"><img src="https://github.com/user-attachments/assets/c1e3363c-f055-4a82-8af6-b7962ad23959" width="400"></p>

이제 다시 정의를 자세히 읽어보면, 결국 Wasserstein distance는

1. x에서 y까지의 "이동량"의 Expectation을 구해서
2. 그것의 Infimum (쉽게 생각해 Minimum)을 구하는 것이었습니다.

(따라서, 앞선 예시들의 경우 블록이 총 7종류였기 때문에 19/7과 21/7 중, 더 작은 값인 19/7이 Wasserstein 값에 가깝겠죠.)



이 Wasserstein 공식을 쉽게 말로 풀어서 이해해보면 결국: "P분포를 Q분포로 이동을 시키는데, 가장 효율적으로 이동하는 방법"입니다.

P분포 모양의 흙더미를 Q분포 모양의 흙더미로 옮기는 것이라고 해서 "Earth-Mover distance"라고도 부르는 것입니다.



가장 효율적으로 옮기려면, 직관적으로 생각해도 P분포의 가장 왼쪽에 있는 것을 Q분포의 가장 왼쪽으로 옮기는 방식으로,아래 그림과 같이 차곡차곡 채워가는 식으로 분포를 이동하는 것이 가장 효율적인 방법일 것입니다.

<p align="center"><img src="https://github.com/user-attachments/assets/38328d5c-5e49-487e-b325-fc4a8fb9da51" width="400"></p>
<br>

## Wasserstein GAN
결국, Wasserstein distance은 **"좌우방향"** 의 거리를 이용하여 distance를 정의내립니다.

위에서 간단히 보인, 우리가 가장 자주 사용하는 거리 개념인 TV distance, KLD, JSD는 모두 "상하 방향"의 거리를 이용하여 distance를 정의합니다. 모두 적분을 이용하기 때문에 세로 방향의 차를 사용하기 때문이죠.

<p align="center"><img src="https://github.com/user-attachments/assets/9c5d932d-8dcc-4f57-95b5-228e7a69be0d" width="250"></p>


만약에 P와 Q가 disjoint support를 갖는다고 하겠습니다. 확률에서 support라는 말은 확률값이 0보다 큰 x값을 말하는데요. 분포의 support 두개가 서로 겹치지 않는 상황을 disjoint support라고 합니다. 이 상황에서 여기있는 divergence 값들을 계산해보겠습니다.

- TV distance부터 하면, 아까 P가 Q보다 더 큰 쪽에서의 면적이므로 1이 나옵니다.
- KLD를 보면, 아까 Q가 P에서 멀리 떨어진 상황에서 무한대가 나온다고 했습니다. 이것도 같은 상황이므로 무한대의 값을 가집니다.
- JSD는 log2의 값이 나옵니다.

이제 Q가 움직이는 상황을 가정하겠습니다. 그런데 Q가 움직인다고 하더라도 P와 support를 계속해서 겹치지만 않는다면, 이 밑의 거리 값들은 바뀌지 않는다는 것입니다. 멀리 움직여도 어차피 support가 안 겹쳐서 위 값들을 갖고, 가까이 가더라도 support가 안 겹치기만 하면 움직였음에도 불구하고 이 값들은 같은 값을 갖는다는 것입니다.

이걸 반대로 말하면, P와 Q가 disjoint supports를 갖는 상황이면, Divergence를 이용해서 Q 분포를 마음대로 옮길 수가 없게 되는 것입니다.
<br>

<p align="center"><img src="https://github.com/user-attachments/assets/0c20b26e-3578-4526-adc8-327c64a5f7e0" width="300"></p>

그래서 이 세가지 divergence는 두 분포가 어느정도 겹쳐있고 근접해 있을 때는 좋은 성능을 내는데, 떨어져 있는 경우에는 divergence를 사용해서 두 분포의 거리를 좁히는 것이 어렵습니다. 그런데 좌우 방향의 거리를 이용하는 Wasserstein distance는 이 문제를 잘 해결합니다.

<p align="center"><img src="https://github.com/user-attachments/assets/e52a6ed3-e68d-4186-a5b8-0da871a82967" width="500"></p>

이제 disjoint supports를 다시 보면, Wasserstein distance를 사용한 경우, disjoint support라고 하더라도 Q가 멀리 가면 멀리 갈수록 값이 커지게 됩니다. 반대로 가까이 가면 작아지겠죠. 아까 본 세개의 divergence에서는 거의 불가능했던 내용입니다. Wasserstein distance의 이러한 특징이 GAN을 학습하는 데 아주 중요한 역할을 합니다. GAN이 학습할 때 초기 단계에서 Generator가 만든 fake data는 초기에는 성능이 많이 떨어져서 real data와 거의 전혀 다른 모습입니다. 그럼 두 분포가 아주 많이 떨어져 있을 텐데 그런 경우에 KL divergence나 JSD 이런 것들은 실제로 이 간격을 줄이는 것을 잘 할 수가 없습니다. 그런데 Wasserstein distance는 이 두 분포가 많이 떨어져 있다 하더라도 이 두 분포의 거리를 잘 줄일 수 있습니다.





본 내용은 [고려대학교 오승상 교수님의 강의 영상](https://www.youtube.com/watchv=Fsv57RcHRWQ&ab_channel=%EC%98%A4%EC%8A%B9%EC%83%81Gmail)을 토대로 만들어졌습니다.


