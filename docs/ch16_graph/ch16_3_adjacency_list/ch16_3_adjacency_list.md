# 인접 리스트(Adjacency List)

## 인접 리스트란?

그래프를 구현하는 대표적인 방법은 인접 행렬과 인접 리스트 두 가지이다.

하지만 인접 행렬은 간선의 개수가 적은 희소 그래프(Sparse Graph)에서 메모리 낭비가 심하다는 단점이 있다.

이에 따라, 필요한 간선 정보만 저장하는 방식으로 인접 리스트(Adjacency List)가 도입되었다.

인접 행렬이 메모리 낭비가 심한 이유?

그래프의 정점 수가 $n$개일 때, 인접 행렬은 항상 $n \times n$ 크기의 2차원 배열을 사용한다.

즉, 실제 간선의 유무와 관계없이 **모든 정점 쌍 $(i, j)$에 대해 공간을 예약**한다.

- 이로 인해, **간선 수 $m$이 적은 희소 그래프에서는 대부분의 공간이 0으로 채워져 낭비**된다.
- 공간 복잡도는 항상 $O(n^2)$로 고정되며, 실제 간선 수 $m$에 비례하지 않는다.

예시:

- 정점 1,000개에 간선이 5,000개뿐이라면, 인접 행렬은 1,000,000개의 공간 중 **99.5%가 0**이다.

이런 경우에는 **간선 중심으로 저장하는 인접 리스트 방식이 훨씬 효율적**이다.

밀집 그래프, 희소 그래프란?

**희소 그래프 (Sparse Graph)**

- 간선 수 $m$이 정점 수 $n$에 비해 매우 적다. (보통 $m \ll n^2$)
- 예: 트리 구조, 소셜 네트워크 초기 상태 등
- 적은 메모리 사용이 중요하기 때문에, **인접 리스트**를 선호한다

**밀집 그래프 (Dense Graph)**

- 간선 수 $m$이 정점 수 $n^2$에 가까운 수준이다. (보통 $m \approx n^2$)
- 예: 모든 정점이 서로 연결된 네트워크 등
- **빠른 간선 접근**이 중요하기 때문에, **인접 행렬**이 유리하다

**정량적 기준:**

 *(엄격하진 않지만 일반적으로)*

- $m = O(n)$ → 희소 그래프
- $m = \Theta(n^2)$ → 밀집 그래프

### 인접 리스트의 개념 설명

인접 리스트는 각 정점(Vertex)마다 연결된 간선(Edge)들을 연결 리스트로 저장하는 방식이다.

정점이 `n`개 있을 경우, 배열 또는 해시맵 등으로 `n`개의 리스트를 관리하며, 각 리스트에는 해당 정점에서 직접 연결된 이웃 정점의 정보를 저장한다.

예시: 정점 A에 연결된 정점이 B, C라면

`adj[A] = [B, C]`

- 메모리 사용량은 `O(n + m)`으로, 간선 수가 적을수록 효율적이다.
- 무방향 그래프일 경우 양쪽 정점의 리스트에 모두 추가된다.

수식 버전

그래프 $G = (V, E)$가 주어졌을 때, 인접 리스트는 각 정점 $v \in V$에 대해 해당 정점과 직접 연결된 이웃 정점들의 집합을 리스트로 저장하는 방식이다.

즉, 정점 $v_i$에 대한 인접 리스트는 다음과 같이 정의된다:

$$
\text{adj}[v_i] = \{ v_j \mid (v_i, v_j) \in E \}
$$

전체 인접 리스트는 배열 또는 해시맵 구조로 관리되며, 메모리 사용량은 다음과 같다:

- 정점 수: $n = |V|$
- 간선 수: $m = |E|$
- 메모리 복잡도: $O(n + m)$

무방향 그래프인 경우, 각 간선 $(u, v)$에 대해 $\text{adj}[u]$와 $\text{adj}[v]$ 양쪽에 모두 추가한다.


## 파이썬 코드로 구현

- 전체 그래프는 `Graph`라는 클래스로 표현하며, 이 안에는 정점 수(`numVertices`)와 각 정점의 인접 리스트 배열(`adjList`)이 포함된다.
- 각 리스트의 원소는 `Node` 클래스로 표현되며, 이 안에 다음 노드에 대한 참조가 저장된다.

```python
# 최대 정점 수 정의
MAX_VERTICES = 100

# 인접 리스트의 각 노드를 정의하는 클래스
class Node:
    def __init__(self, vertex):
        self.vertex = vertex        # 연결된 정점 번호
        self.next = None           # 다음 노드를 가리키는 포인터

# 그래프 클래스 정의
class Graph:
    def __init__(self, numVertices):
        self.numVertices = numVertices                 # 그래프 내 정점 수
        self.adjList = [None] * MAX_VERTICES           # 각 정점에 대한 인접 리스트 배열

# 새로운 노드 생성 함수
def createNode(v):
    newNode = Node(v)           # 노드에 정점 번호 저장
    return newNode              # 기본적으로 다음 노드는 없음

# 새로운 그래프 생성 함수
def createGraph(vertices):
    graph = Graph(vertices)     # 그래프 객체 생성
    return graph

# 간선 추가 함수 (무방향 그래프 기준)
def addEdge(graph, src, dest):
    # src → dest 방향 간선 추가
    newNode = createNode(dest)                     # dest 정점에 해당하는 노드 생성
    newNode.next = graph.adjList[src]              # 새 노드를 기존 리스트 앞에 연결
    graph.adjList[src] = newNode                   # 새 노드를 리스트의 첫 노드로 설정

    # dest → src 방향 간선 추가 (무방향이므로 반대 방향도 연결)
    newNode = createNode(src)                      # src 정점에 해당하는 노드 생성
    newNode.next = graph.adjList[dest]             # 기존 리스트 앞에 연결
    graph.adjList[dest] = newNode                  # 리스트 갱신

# 그래프 출력 함수
def printGraph(graph):
    for v in range(graph.numVertices):
        temp = graph.adjList[v]
        print(f"{v}:", end=" ")       # 현재 정점 번호 출력

        # 인접 리스트 순회하면서 연결된 정점 출력
        while temp:
            print(f"-> {temp.vertex}", end=" ")
            temp = temp.next
        print()

# 예제 실행
if __name__ == "__main__":
    g = createGraph(5)  # 5개의 정점을 가진 그래프 생성

    # 간선 추가
    addEdge(g, 0, 1)
    addEdge(g, 0, 4)
    addEdge(g, 1, 2)
    addEdge(g, 1, 3)
    addEdge(g, 1, 4)
    addEdge(g, 2, 3)
    addEdge(g, 3, 4)

    # 그래프 출력
    printGraph(g)
```

## 시간복잡도와 공간복잡도

### 시간복잡도

인접 행렬에서는 `adj[u][v]`에 접근을 한 번 수행하기 때문에 시간복잡도가 O(1)이지만, 인접 리스트에서는 `adj[u]`의 연결 리스트를 순회하기 때문에 최대 O(degree(u))의 시간복잡도가 발생한다.

즉, 리스트에서는 해당 정점에 연결된 간선 수만큼 탐색해야 한다.

| Operation | 설명 | Adj. List |
| --- | --- | --- |
| `vertex_count()` | 노드 개수 | O(1) |
| `edge_count()` | 간선 개수 | O(1) (추적시) |
| `vertices()` | 모든 노드 순회 | O(n) |
| `edges()` | 모든 간선 순회 | O(m) |
| `get_edge(u,v)` | 특정 간선(u,v)에 접근 | O(min(dᵤ, dᵥ)) |
| `degree(v)` | 특정 노드v에서 간선 개수 | O(1) |
| `incident_edges(v)` | 해당 노드의 인접 간선 순회 | O(dᵥ) |
| `insert_vertex(x)` | 노드 삽입 | O(1) |
| `remove_vertex(v)` | 노드 삭제 | O(dᵥ) |
| `insert_edge(u,v)` | 간선 삽입 | O(1) |
| `remove_edge(e)` | 간선 삭제 | O(dᵤ) |

※ `dᵥ`는 정점 v의 차수 (degree)

※ `m`은 간선 개수, `n`은 정점 개수

항목별 상세 설명 추가

- **`get_edge(u, v)`**:
    
    간선 존재 여부를 확인하려면 연결된 노드들의 리스트를 순회해야 하므로 O(min(dᵤ, dᵥ))이다.
    
- **`degree(v)`**:
    
    노드 v의 연결 리스트 길이를 반환하면 되므로 O(1)이다.
    
- **`incident_edges(v)`**:
    
    노드 v의 인접 리스트를 순회하면 되므로 O(dᵥ)이다.
    
- **`insert_vertex(x)`**:
    
    새로운 리스트 항목을 하나 추가하면 되므로 O(1)이다.
    
- **`remove_vertex(v)`**:
    
    노드 v의 연결 리스트를 제거하고, 다른 리스트에서 v를 제거하는 데 O(dᵥ)가 소요된다.
    
- **`insert_edge(u, v)`**:
    
    노드 u와 v의 인접 리스트에 간선을 하나씩 추가하면 되므로 O(1)이다.
    
- **`remove_edge(e)`**:
    
    노드 u의 인접 리스트에서 v를 탐색하여 제거해야 하므로 O(dᵤ)이다.
    

### 공간복잡도

인접 행렬은 간선이 거의 없는 경우에도 n² 공간을 낭비할 수 있는 반면, 인접 리스트는 연결된 간선만 저장하므로 희소 그래프(sparse graph)에서 유리하다.

| 표현 방식 | 공간복잡도 |
| --- | --- |
| 인접 행렬 | O(n²) |
| 인접 리스트 | O(n + m) |

## 인접리스트의 장・단점

### 장점

- 메모리 효율적: 간선 수가 적은 희소 그래프에서 O(n²) 메모리 낭비를 피할 수 있음
- 간선 삽입/삭제가 빠름: 연결 리스트의 앞쪽에 삽입하면 O(1) 가능
- 간선 순회에 유리: 어떤 정점에서 출발하는 간선을 빠르게 조회 가능

### 단점

- 특정 간선 존재 여부 확인이 느림: O(dᵥ) 소요 → 인접 행렬의 O(1)보다 느림
- 구현 복잡도 상승: 메모리 동적 할당이 필수이며 디버깅 어려움
- 무방향 그래프에서 간선 중복 삽입 주의 필요

## 활용 예시

### 📌 인접 리스트가 유리한 경우

| 상황 | 설명 |
| --- | --- |
| 희소 그래프 | 간선 수가 n²보다 훨씬 작을 때 메모리 절약 |
| DFS/BFS | 인접 정점들을 순차적으로 탐색하는 경우 |
| 알고리즘 경진대회 | 빠른 구현과 공간 효율성이 중요한 상황 |
| 트리/산술 그래프 | 간선이 거의 정점 수에 비례하는 구조 |

### 예시 문제 (백준)

- [11724번 연결 요소의 개수](https://www.acmicpc.net/problem/11724)
    
    방향 없는 그래프가 주어졌을 때, 연결되어있는 요소(Connected Component)의 개수를 구하는 문제이다.
    
    정점의 개수 N과 간선의 개수 M, 각 간선의 양 끝점 u와 v가 주어진다. 같은 간선은 한 번만 주어진다.
    
    (1 ≤ N ≤ 1,000, 0 ≤ M ≤ N×(N-1)/2, 1 ≤ u, v ≤ N, u ≠ v)
    
    문제 선택 적절한지
    
    주석 말투 괜찮은지
    
    설명 수준이 적절한지
    
    ```python
    N, M = map(int, input().split())  # 정점의 개수 N과 간선의 개수 M 입력
    
    # 1. 인접 리스트를 초기화 한다: 정점 번호가 1부터 시작하므로 N+1 크기로 생성
    graph = [[] for _ in range(N+1)]
    
    # 2. 간선 정보를 입력 받는다: 무방향 그래프이므로 양쪽에 모두 추가한다
    for _ in range(M):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)
        
    visited = [False] * (N+1)  # 방문 여부를 저장하는 리스트
    count = 0  # 연결 요소 개수를 저장하는 변수
    
    # DFS 탐색을 활용하여 연결된 정점들을 모두 방문한다
    def dfs(v, graph, visited):
        visited[v] = True
        for neighbor in graph[v]:
            if not visited[neighbor]:
                dfs(neighbor, graph, visited)
    
    # 모든 정점에 대해 DFS를 시작한다
    for i in range(1, N+1):
        if not visited[i]:  # 방문하지 않은 노드이면,
            dfs(i, graph, visited)  # 연결 요소를 탐색한다
            count += 1
    
    print(count)
    ```
    
    - 제공된 간선 정보를 활용하여 인접 리스트를 생성한다
        - 무방향 그래프이므로, 양쪽 노드에 모두 간선 정보를 추가한다
    - 탐색을 통해 연결 요소(Connected Component)를 찾는다

- [2606번 바이러스](https://www.acmicpc.net/problem/2606)
- [1260번 DFS와 BFS](https://www.acmicpc.net/problem/1260)

연결 요소(Connected Component)란?

그래프 이론에서 무방향 그래프의 컴포넌트(component)란, 연결된 정점들의 집합이며, 더 이상 확장할 수 없는 최대 단위를 의미한다. 즉, 그래프의 전체가 연결되어 있다면, 그 그래프는 **하나의 컴포넌트만** 가지며, 그 컴포넌트는 그래프 전체와 동일하다. 컴포넌트는 connected components(연결 요소)라고도 불린다.

그래프에서 컴포넌트의 수는 중요한 그래프 불변량(graph invariant) 중 하나이며, 매트로이드(matroid), 위상 공간(topological space), 행렬(matrix) 등의 다양한 분야와 밀접하게 연관된다.

컴포넌트는 DFS 또는 BFS로 선형 시간 안에 구할 수 있다.

→ 여기까지는 안 가도 될 것 같기도?

불변량 설명하지 않으려면 그냥 ‘특성’이라는 단어로 치환하면 됨

그래프 불변량(graph invariant)이란?

그래프의 **표현 방식이나 정점/간선 이름이 바뀌어도 변하지 않는 값**이나 **성질**을 말한다. 

그래프의 모양이나 구조는 그대로인데, 정점 이름이나 순서를 바꾸더라도 **변하지 않는 특성**들이 있다. 이런 특성들을 **그래프 불변량**이라고한다.

그래프 불변량은 알고리즘 설계나 그래프 비교에서 아주 중요한 개념이다.

| 그래프 불변량 | 설명 |
| --- | --- |
| 정점 수 | V |
| 간선 수 | E |
| **연결 요소 개수** | 연결된 컴포넌트의 수 |
| **그래프의 지름(diameter)** | 가장 멀리 떨어진 두 정점 사이의 거리 |
| **최대 차수(max degree)** | 가장 많이 연결된 정점이 가진 간선 수 |
| **그래프가 연결되어 있는지 여부** | 모든 정점이 경로로 이어져 있는지 |
| **사이클 존재 여부** | 그래프에 사이클(돌아오는 경로)이 있는지 |

+

날개 개념은 `:::tip`, `:::note`, `:::warning` 같은 GitBook의 블록 스타일 활용

![스크린샷 2025-06-09 오후 4.05.10.png](attachment:b450ea7a-37bb-4b6e-8349-bcd803c0dff9:스크린샷_2025-06-09_오후_4.05.10.png)

### **삽입/삭제 시 비용의 차이**

- `insert_vertex(x)`나 `remove_vertex(v)`는 인접 행렬에서 매우 비싸다:
    
    → 2차원 배열 크기 조정이 필요 (C에서는 재할당이 어려워 더 큰 문제)
    
- 반면 인접 리스트는 연결 리스트 구조이기 때문에 정점 수를 늘리는 것이 유연하다

### **실전에서 어떤 게 더 많이 쓰이는가?**

- 대부분의 알고리즘 문제(백준, 프로그래머스, 대회)는 인접 리스트 사용을 전제로 한다
- 인접 행렬은 그래프의 크기가 작거나, 간선 존재 여부를 자주 확인해야 하는 특수 문제에서 사용한다
