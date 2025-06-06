# 이중 연결 리스트 (Doubly Linked List)

> 이중 연결 리스트(Doubly Linked List)는 각 노드가 앞뒤 노드에 대한 포인터를 모두 가지고 있어 양방향 순회가 가능한 연결 리스트이다.

## 핵심 키워드

- **노드(node)**: 데이터를 저장하고 이전 및 다음 노드에 대한 포인터를 포함하는 단위  
- **헤드(head)**: 리스트의 첫 번째 노드를 가리키는 포인터  
- **테일(tail)**: 리스트의 마지막 노드를 가리키는 포인터  
- **prev**: 현재 노드에서 앞쪽 노드를 가리키는 포인터  
- **next**: 현재 노드에서 뒤쪽 노드를 가리키는 포인터  

## 핵심 개념

단일 연결 리스트는 다음 노드로만 이동할 수 있기 때문에, 뒤쪽 노드를 찾기는 쉽지만 앞쪽 노드를 찾기는 어렵다는 단점이 존재한다.

이러한 구조적 제약을 보완하기 위해 등장한 자료구조가 이중 연결 리스트이다.

이중 연결 리스트는 각 노드가 다음 노드뿐만 아니라 이전 노드에 대한 포인터(`prev`)도 함께 보유한다. 이를 통해 리스트를 양방향으로 순회할 수 있으며, 특정 노드를 기준으로 앞뒤 방향 모두에서 삽입과 삭제 연산을 유연하게 수행할 수 있다.

이중 연결 리스트는 **양방향 리스트(bidirectional linked list)**라고도 불린다.

## 구조 및 시각화

이중 연결 리스트는 각 노드가 앞 노드와 뒤 노드에 대한 포인터를 모두 포함하는 방식으로 구성된다.

각 노드는 세 개의 필드로 구성된 클래스 형태로 구현된다.

- `data`: 데이터에 대한 참조  
- `prev`: 앞쪽 노드에 대한 포인터  
- `next`: 뒤쪽 노드에 대한 포인터  

리스트는 양 끝이 `None`으로 닫혀 있으며, 각 노드는 `prev`, `next`를 통해 서로 연결되어 있다.

```
None ← [Data|Prev|Next] ⇄ [Data|Prev|Next] ⇄ [Data|Prev|None]
```

### 예시

```
Head → [10|None|●] ⇄ [20|●|●] ⇄ [30|●|None] ← Tail
```

- **Head**는 첫 번째 노드를 가리킨다.  
- **Tail**은 마지막 노드를 가리킨다.  
- 각 노드는 이전/다음 노드에 대한 포인터(`prev`, `next`)를 함께 보유한다.  

## 기본 연산

### 삽입 (Insertion)

#### 맨 앞에 삽입

- 시간복잡도: `O(1)`
- 과정:
  1. 새 노드를 생성한다.
  2. 새 노드의 `next`를 현재 헤드로 연결한다.
  3. 현재 헤드의 `prev`를 새 노드로 설정한다.
  4. 헤드를 새 노드로 변경한다.

#### 맨 뒤에 삽입

- 시간복잡도: `O(n)`
- 과정:
  1. 마지막 노드까지 순회한다.
  2. 마지막 노드의 `next`를 새 노드로 연결한다.
  3. 새 노드의 `prev`를 마지막 노드로 설정한다.

#### 중간에 삽입

- 시간복잡도: `O(n)`
- 과정:
  1. 삽입할 위치의 앞 노드까지 순회한다.
  2. 새 노드의 `prev`를 앞 노드로, `next`를 다음 노드로 설정한다.
  3. 앞 노드의 `next`를 새 노드로, 다음 노드의 `prev`를 새 노드로 연결한다.

### 삭제 (Deletion)

#### 맨 앞 노드 삭제

- 시간복잡도: `O(1)`
- 과정:
  1. 헤드를 다음 노드로 변경한다.
  2. 새 헤드의 `prev`를 `None`으로 설정한다.

#### 특정 값 노드 삭제

- 시간복잡도: `O(n)`
- 과정:
  1. 삭제할 노드를 탐색한다.
  2. 이전 노드의 `next`를 삭제할 노드의 다음 노드로 연결한다.
  3. 다음 노드의 `prev`를 삭제할 노드의 이전 노드로 설정한다.

### 탐색 (Search)

- 시간복잡도: `O(n)`
- 과정:
  1. 헤드부터 순차적으로 노드를 탐색하여 원하는 데이터를 찾는다.

## 장단점

### 장점

- 양방향 순회가 가능하다.
- 중간 삽입 및 삭제가 포인터 조작만으로 이루어져 효율적이다.

### 단점

- 포인터가 두 개 필요하여 메모리 오버헤드가 크다.
- 구현 시 포인터 관리가 복잡하다.

## 단일 연결 리스트와의 비교

| 항목 | 단일 연결 리스트 | 이중 연결 리스트 |
|------|------------------|------------------|
| 연결 방향 | 한 방향 (앞 → 뒤) | 양 방향 (앞 ⇄ 뒤) |
| prev 포인터 존재 여부 | 없음 | 있음 |
| 삽입/삭제 효율성 | 맨 앞은 O(1), 중간은 O(n) | 맨 앞, 뒤, 중간 모두 포인터만 조작하면 되므로 효율적 |
| 메모리 사용량 | 낮음 (포인터 1개) | 높음 (포인터 2개) |
| 구현 복잡도 | 간단 | 포인터 조작이 더 복잡함 |
| 양방향 순회 | 불가능 | 가능 |

## 구현 예시 (Python)

```python
# 노드 클래스 정의: 데이터와 이전/다음 노드를 가리키는 포인터를 포함
class Node:
    def __init__(self, data):
        self.data = data        # 노드가 저장하는 데이터
        self.prev = None        # 이전 노드를 가리키는 포인터
        self.next = None        # 다음 노드를 가리키는 포인터

# 이중 연결 리스트 클래스 정의
class DoublyLinkedList:
    def __init__(self):
        self.head = None        # 리스트의 첫 번째 노드를 가리키는 포인터

    # 리스트 맨 앞에 노드를 삽입
    def insert_front(self, data):
        new_node = Node(data)             # 새 노드 생성
        new_node.next = self.head         # 새 노드의 next를 현재 헤드로 설정
        if self.head:
            self.head.prev = new_node     # 기존 헤드의 prev를 새 노드로 설정
        self.head = new_node              # 헤드를 새 노드로 갱신

    # 리스트 맨 뒤에 노드를 삽입
    def insert_end(self, data):
        new_node = Node(data)             # 새 노드 생성
        if not self.head:
            self.head = new_node          # 리스트가 비어 있으면 새 노드를 헤드로 설정
            return
        current = self.head
        while current.next:               # 마지막 노드까지 순회
            current = current.next
        current.next = new_node           # 마지막 노드의 next를 새 노드로 연결
        new_node.prev = current           # 새 노드의 prev를 마지막 노드로 설정

    # 특정 값을 가진 노드를 삭제
    def delete_value(self, key):
        current = self.head
        while current:
            if current.data == key:
                if current.prev:
                    current.prev.next = current.next  # 이전 노드의 next를 현재 노드의 next로 연결
                else:
                    self.head = current.next          # 현재 노드가 헤드일 경우, 헤드를 다음 노드로 변경
                if current.next:
                    current.next.prev = current.prev  # 다음 노드의 prev를 현재 노드의 prev로 설정
                return True
            current = current.next
        return False   # 해당 값을 찾지 못한 경우

    # 특정 값을 가진 노드가 있는지 탐색
    def search(self, key):
        current = self.head
        while current:
            if current.data == key:
                return True
            current = current.next
        return False

    # 리스트 전체 데이터를 출력
    def display(self):
        elements = []
        current = self.head
        while current:
            elements.append(str(current.data))  # 각 노드의 데이터를 문자열로 변환하여 저장
            current = current.next
        print(' ⇄ '.join(elements) + ' ⇄ None')  # 양방향 화살표로 연결하여 출력
```