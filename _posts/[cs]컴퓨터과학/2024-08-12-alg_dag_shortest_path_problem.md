---
title: "[알고리즘] DAG 최단 경로 문제 (DAG Shortest Path Problem)"
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

# Directed Acyclic Graph(DAG)
DAG란 그래프 연결에 방향이 있고(Directed), 한 곳에서 순환이 발생하지 않는(Acyclic) 그래프(Graph)를 의미합니다.

<p align="center"><img src="https://github.com/user-attachments/assets/93b63517-aeeb-457c-ae86-f87f4ebe2a10" width="300"></p>

# Edge Relaxation
DAG Shortest Path 문제를 풀기 위해 Edge Relaxation 개념이 필요합니다. Edge Relaxation이란, 가중치가 있는 그래프(Weighted Graph)에서, 각 edge에 배정되어 있는 만큼의 가중치를 더해가며 탐색할 때, 더 나은(작은) 경로의 가중치가 있다면 그 값으로 update하는 것을 의미합니다. 

말이 어렵게 느껴지는데 굉장히 쉬운 개념입니다. 결국 더 나은 대안으로 수정한다는 의미입니다. 그리고 앞으로 그래프에서 이 작업을 Relax라고 부릅니다.

그림으로 보면 다음과 같습니다.

<p align="center"><img src="https://github.com/user-attachments/assets/dd1d4883-f82e-4e82-a2c9-f4014bb00106" width="500"></p>

- (a): 기존에 어떠한 다른 경로에 의해 v가 9의 값을 가지게 되었습니다. 하지만, v까지 가는 경로 중 u에서 v로 가는 경로를 선택한다면 5+2=7의 값으로 v의 값을 수정해줄 수 있습니다. 9보다 7이 더 나은(작은) 경로이므로, Relax를 거치면 v의 값은 7로 수정됩니다.

- (b): 기존에 어떠한 다른 경로에 의해 v가 6의 값을 가지게 되었습니다. 이때 u를 거쳐 v로 가게 방식은 5+2=7로, 기존의 6보다 나은(작은) 대안이 아닙니다. 따라서 Relax를 거쳐도 v는 그대로 6의 값을 갖습니다.


Edge Relaxation의 코드는 다음과 같습니다. 여기서 ".d"는 시작점(root node)로부터의 distance를 말합니다.

<p align="center"><img src="https://github.com/user-attachments/assets/dd3919a0-88f8-4ac9-b938-35b56233922c" width="250"></p>

- 만약 v까지의 거리가 u까지의 거리에 (u,v) edge의 가중치를 더한 것보다 크면,
- v.d를 더 작은 값으로 업데이트한다.


# DAG Shortest Path
DAG 그래프의 최단경로를 찾는 문제입니다. DAG Shortest Path는 Topological Sort와 Edge Relaxation을 이용하여 구할 수 있습니다. Topological Sort에 대한 자세한 내용은 [여기](https://stevenhskim.github.io/cs-alg/alg_topological_sort/)에서 확인하실 수 있습니다.

DAG Shortest Path를 찾는 Pseudo Code는 아래와 같습니다.

<p align="center"><img src="https://github.com/user-attachments/assets/74369473-8328-42ba-9fe5-31dbea8dbd9f" width="450"></p>

- 우선 DAG에서 Topological Sort를 수행한 후,
- 모든 vertex에 대해 Relaxation을 수행해준다.

이 코드를 통해 DAG의 Shortest Path를 구할 수 있습니다.


## Visualization
DAG Shortest Path의 과정을 시각화하면 다음과 같습니다.

<p align="center"><img src="https://github.com/user-attachments/assets/be6e44a4-9db3-4eb0-a470-edd175155b73"></p>

- 우선 왼쪽에서 오른쪽 방향으로 Topological Sort를 수행합니다. 이 정렬된 방향(왼→오)으로 순회를 수행합니다.

- (a): 시작 노드가 s이므로, s는 0으로, 나머지는 ∞로 초기화합니다. 

- (b): r은 무한대의 값을 가지므로, 연결된 vertices에 5나 3을 더해도 무한대이다. 아주 큰 값을 가지므로 relax를 거쳐도 연결된 vertices들의 값이 업데이트 되지 않습니다.

- (c): s는 0이므로 무한대의 값을 가지고 있는 t와 x의 값을 업데이트 해줄 수 있습니다. 0+2<∞ 이고 0+6<∞ 이므로, t와 x의 값을 각각 2와 6으로 relax 해줍니다.

- (d): 같은 방식으로 relax 해줍니다.

- (e): 이때, 6의 값을 가지고 있던 기존의 y보다 x(6)에서 -1을 더한 값인 5가 더 작으므로 y의 값을 relax해줍니다.

이런 방식으로 Topological 방향으로 Relaxation을 수행하면, 시작점(s)에서부터의 DAG 최단 경로를 찾을 수 있습니다.



# Running Time
- 코드의 1번 줄의 Topological Sort에서 O(V+E)가 소요되고,
- 2번 줄의 Initialization에서 O(V)가 소요됩니다.
- 3번 줄 for 문에서 O(E)가 소요됩니다.

즉, DAG의 Shortest Path를 찾는 알고리즘의 수행시간은 여느 그래프 알고리즘과 마찬가지로 **O(V+E)** 를 갖습니다.

---
참고 자료:
<https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-spring-2020/>