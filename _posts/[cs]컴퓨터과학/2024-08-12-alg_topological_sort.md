---
title: "[알고리즘] 위상 정렬 (Topological Sort)"
excerpt: ""

categories:
  - cs-alg

tags:
  - algorithm
  - sort

use_math: true
classes: wide

date: 2024-08-12
last_modified_at: 2024-08-12
---

Topological Sort는 기본적으로 DFS를 통해 구현합니다. DFS에 대한 자세한 내용은 [여기](https://stevenhskim.github.io/cs-alg/alg_dfs/)에 있습니다.

## Topological Sort
Topological Sort란 그래프를 표현하는 한 가지 방식입니다. 그래프를 수평적으로, 즉 모든 노드를 한 줄로 나열하여 정렬하는 방식입니다. Topological Sort는 Directed Acyclic Graph(DAG)에서 수행됩니다.

- Directed Acyclic Graph(DAG): 그래프 연결에 방향이 있고(Directed), 한 곳에서 순환이 발생하지 않는(Acyclic) 그래프(Graph)를 의미합니다.

<p align="center"><img src="https://github.com/user-attachments/assets/09bd215b-6606-424f-aae1-d98ddf3fb123" width="300"></p>

다음과 같이 옷을 입은 순서를 나타낸 그래프가 있다고 하겠습니다. 이때 이 아래의 그래프를 DFS로 탐색했고, 각 노드 옆의 숫자는 (discovered time / finish time)을 의미합니다.

<p align="center"><img src="https://github.com/user-attachments/assets/49dd2010-d106-4415-a446-83a6aa05c919" width="450"></p>

위 그래프를 수평적으로 눌러 나탄내면 아래와 같습니다. DFS의 방문 종료 시점(finish time)을 기준으로 내림차순 정렬합니다. 현재 slash의 오른쪽에 18,16,15,14,10,8,7,5,4 순으로 노드가 정렬된 것을 볼 수 있습니다.

<p align="center"><img src="https://github.com/user-attachments/assets/92b437ee-2f22-4b19-aec8-c9b882fa413e" width="700"></p>

이 Topological 개념이 다른 그래프 알고리즘에서 주요하게 사용됩니다.

## Running Time
DFS를 통해 탐색하기 때문에, DFS의 수행 시간과 동일합니다: **O(V+E)**

---

이 글은 다음의 자료를 참고하여 만들어졌습니다.
<https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-spring-2020/>
