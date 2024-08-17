---
title: "[알고리즘] 벨만-포드 알고리즘(Bellman-Ford Algorithm)"
excerpt: ""

categories:
  - cs-alg

tags:
  - algorithm
  - shortest path

use_math: true
classes: wide

date: 2024-08-12
last_modified_at: 2024-08-12
---

# 단일 출발지 최단 경로 Single Source Shortest Path
Single Source Shortest Path란, 말 그대로 하나의 출발지(Single Source)에서 시작한 최단 경로(Shortest Path)를 말합니다.



# 벨만-포드 알고리즘 Bellman-Ford Algorithm
Bellman-Ford 알고리즘은, Dijkstra 알고리즘과 함께 가중치가 있고(Weighted) 방향이 있는(Directed) 그래프에서 Single Source Shortest Path(SSSP)을 구하는 알고리즘입니다. **가중치(Weight)가 0보다 크거나 같은 그래프만 풀 수 있는 Dijkstra 알고리즘과는 다르게, Bellman-Ford 알고리즘은 Negative Weighted Edge가 존재하는 경우에도 SSSP 문제를 풀 수 있습니다.** 

Bellman-Ford 알고리즘은 두가지의 주요 기능을 가지고 있습니다.

1. Shortest Path (with Negative edge)를 찾는다.
2. Negative Cylcle을 감지한다.

여기서 Negative Cycle이란, 다음 그림과 같이 어떠한 한 순환(cycle)을 순환하면 순환할수록 점점 음수값이 커져 결국 -∞로 발산하는 불필요한 순환을 말합니다. Bellman-Ford 알고리즘은 이러한 Negative Cycle을 감지하는 기능을 가집니다.

<p align="center"><img src="https://github.com/user-attachments/assets/c61ab9cd-be59-4cf0-abb4-fa8803bd9f22" width="300"></p>

참고로 Bellman-Ford 알고리즘에는 Relaxation 개념이 사용됩니다. Relaxation에 대한 설명글은 [여기](https://stevenhskim.github.io/cs-alg/alg_dag_shortest_path_problem/)에 있습니다.

## Bellman-Ford 알고리즘 Pseudo Code
Bellman-Ford 알고리즘의 Pseudo Code는 다음과 같습니다. 

<p align="center"><img src="https://github.com/user-attachments/assets/bf69003f-5df4-4bb8-a4ed-89c733508fe4" width="400"></p>

- 위 코드의 오른쪽에 표시되어 있는 것 처럼 윗 부분의 for문은 Shortest Path를 찾기 위해 Relaxation을 수행하는 부분이고,
- 아래의 for문은 Negative Cycle이 존재하는지 판별하는 부분입니다.


## Bellman-Ford 알고리즘의 원리
위 Pseudo Code를 이용하여 Bellman-Ford 알고리즘의 원리에 대해 알아보겠습니다. 아래 그래프에 Bellman-Ford 알고리즘을 적용하겠습니다.

<p align="center"><img src="https://github.com/user-attachments/assets/7fe4f298-469b-420a-ac5a-5dac3a5bec65" width="300"></p>


### 1. Shortest Path 찾는 과정

<p align="center"><img src="https://github.com/user-attachments/assets/8d4c935d-923f-40a3-a4c3-038670b7daa6" width="600"></p>

- 우선 시작하고자 하는 노드(s)만을 0으로 초기화하고, 나머지 노드는 모두 무한대로 초기화합니다. 노드의 값을 무한대로 초기화 하는 이유는, 이후 취할 relaxation이 비교 후 작은 수로 업데이트 해주는 특성을 가지고 있기 때문입니다.

---

<p align="center"><img src="https://github.com/user-attachments/assets/3dcb7875-12bc-4319-8d07-53f2c9e743e4" width="600"></p>

- 그래프의 어느 vertex든지 시작 지점으로 선택할 수 있지만, 본 내용에서는 위와 같은 순서로 순회하는 예시를 보이겠습니다. 순서는 임의로 지정된 것으로 얼마든지 바뀔 수 있습니다.
- 현재 위 edge의 나열을 보면, t → x → y → z → s 노드의 순서로 진행됨을 얼추 알 수 있습니다.

---

<p align="center"><img src="https://github.com/user-attachments/assets/27838ee8-ba60-4d29-858e-3cdeef58959a" width="600"></p>

- 먼저 t 노드의 인접 vertices에 대해 Relax를 진행합니다. 하지만, t의 값이 무한대이므로, relax 이후의 값이 그대로입니다.

---

<p align="center"><img src="https://github.com/user-attachments/assets/4151a588-48b9-4ab8-9100-a9f9352157ad" width="600"></p>

- 그 다음으로 x 노드에 대해 인접 노드에 대한 Relax를 진행합니다. 하지만 t와 마찬가지로 현재 x의 값이 무한대이기 때문에 relaxation 이후에도 그래프의 값의 변화가 없습니다.

---

<p align="center"><img src="https://github.com/user-attachments/assets/7413abcf-3e4d-4079-92dd-40ed052a2f54" width="600"></p>

- y 노드와 z 노드에 대해서도 Relax를 수행합니다. 마찬가지로 모두 무한대의 값을 가지므로 변화가 없습니다.

---

- <p align="center"><img src="https://github.com/user-attachments/assets/e12809cc-a281-4769-bb53-d67d53c06254" width="600"></p>

- 하지만 그 다음 차례인 s 노드에 대해서는 Relaxation을 수행했을 때, 0+6<∞ 이고 0+7<∞ 이므로 t와 y노드의 값이 업데이트 됩니다.

--- 

<p align="center"><img src="https://github.com/user-attachments/assets/58f9b66f-6ac0-45cd-bc62-c0c7764ceac2" width="600"></p>

- (a) ~ (j)까지 해당 edge 순서를 모두 돌았으므로, 다시 (t,x)부터 relaxation을 수행합니다.

---

<p align="center"><img src="https://github.com/user-attachments/assets/5396a72e-7ace-42cf-b5f7-fd5cd28ab1e1" width="600"></p>

- 이 다음번 trial 부터는 노드에 무한대의 값이 점점 사라져 가게 되므로, 눈에 띄게 Relax가 수행됩니다.

---

위와 같은 방식으로 "모든 edge에 대해 Relaxation을 취할 때까지" 즉, "모든 edge에 대해 (Vertex 개수)-1번의 Relaxation을 취할 때까지" 이 과정을 반복합니다. 이에 대한 그림은 아래와 같습니다.

<p align="center"><img src="https://github.com/user-attachments/assets/25fc105f-c7f4-41d0-bce7-1df840ef42aa" width="900"></p>


### 2. Negatice Cycle 감지하는 과정
이번에는 Negatice Cycle을 감지하는 과정입니다. 위와 같은 방식으로 그래프를 순회하면서, **v.d > u.d + w(y,v)** 가 한 번 더 발생하는지의 여부를 감지합니다.

아래는 Edge Relaxation의 과정에 대한 설명입니다. 

<p align="center"><img src="https://github.com/user-attachments/assets/f9af0e81-f05c-4329-b41c-4bd631ef93c8" width="600"></p>

Edge relaxation은 만약 v.d > u.d + w(y,v)가 존재한다면 더 작은 쪽으로 업데이트합니다.

<p align="center"><img src="https://github.com/user-attachments/assets/c055ed24-e2de-454d-9f86-91b1b5cd9f4f" width="300"></p>


- 그런데 다음 Bellman-Ford 코드에서 위 for loop을 돌며 Relaxation 작업이 모두 끝났음에도 불구하고, 만약 Relaxation이 될 만한 여지가 한 번 더 감지된다면,

- 그것은 Negative Cylce이 그래프 내에 존재한다는 의미입니다.


# Running Time

<p align="center"><img src="https://github.com/user-attachments/assets/7b6be912-2286-4ad9-aa13-7361a9f4fa2a" width="400"></p>

Bellman-Ford 알고리즘의 Running Time은 O(VE) + O(V) + O(E) 즉, **O(VE)** 입니다.

---

Negative Edge가 없는 그래프에서 SSSP 문제를 푸는 알고리즘인 Dijkstra 알고리즘에 대한 게시물은 [여기](https://stevenhskim.github.io/cs-alg/alg_dijkstra/)에 있습니다.


참고 자료:

<https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-spring-2020/>

<https://ocw.snu.ac.kr/node/32373>