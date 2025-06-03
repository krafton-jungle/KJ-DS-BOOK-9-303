# 트라이(Trie) 완벽 가이드

## 📚 목차

1. [트라이(Trie)란 무엇인가?](#1-트라이trie란-무엇인가)
2. [트라이 구조](#2-트라이-구조)
3. [구현 방법](#3-구현-방법)
4. [장단점 분석](#4-장단점-분석)
5. [시간복잡도 분석](#5-시간복잡도-분석)
6. [결론](#6-결론)
7. [참고](#7-참고)


## 1. 트라이(Trie)란 무엇인가?

트라이는 문자열을 저장하고 탐색하는 데 특화된 **트리 기반 자료구조**이다.

트라이는 문자열의 길이를 $L$이라 할 때, 탐색 및 삽입 연산을 $O(L)$ 시간에 수행할 수 있다.
이는 해시 테이블과 유사한 성능을 가지며, 접두사 기반 검색이 자연스럽게 가능하다는 점에서 유리하다.

## 2. 트라이 구조

트라이는 루트 노드에서 시작하여 각 문자마다 분기하는 구조를 가진다.

각 노드는 다음과 같은 속성을 가진다:

* `children`: 자식 노드를 저장하는 딕셔너리 (dict)
* `is_end`: 단어의 끝을 나타내는 불리언 값 (bool)

### 트라이 구조 예시

다음과 같은 단어들을 저장한다고 가정한다:

```
apple, april, bus, busy, beer, best
```

해당 단어들을 삽입한 트라이 구조는 다음과 같다:

![trie\_example](/docs/assets/ch15_trie/trie_example.png)

파란색으로 표시된 노드는 각 단어의 마지막 문자에 해당하며, `is_end = True`로 표시된다.

### 트라이 구조 특징

* 루트 노드는 항상 비어 있다.
* 루트의 자식 노드들은 각 단어의 첫 글자에 해당한다.
* 중복되는 접두사는 노드를 공유함으로써 메모리를 절약한다.

## 3. 구현 방법
구현은 Node 및 Trie 클래스로 나뉜다.

Node 클래스는 자식 노드들을 담을 dict, 마지막 여부를 확인 할 boolean is_end를 가진다

### Node 클래스

```python
class Node:
    def __init__(self):
        self.children = {} #자식 노드 저장하는 딕셔너리
        self.is_end = False # 단어의 끝 표시
```

---

### Trie 클래스
Trie 클래스는 root 노드를 필드로 갖는다
또한, 생성할 때 Root에 새 Node 객체를 만들어준다

```python
class Trie:
    def __init__(self):
        self.root = Node() # 루트 노드 초기홧

    def insert(self, word):  # 삽입 
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = Node()
            node = node.children[char]
        node.is_end = True

    def search(self, word):  # 탐색 
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end

    def starts_with(self, prefix):  # 접두사 검사 
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True

```

### 테스트 코드

```python
trie = Trie()
trie.insert("cat")
trie.insert("cap")
trie.insert("can")

assert trie.search("cat") == True
assert trie.search("car") == False
assert trie.starts_with("ca") == True
assert trie.starts_with("dog") == False
```

## 4. 장단점 분석

### 장점

* 문자열 삽입과 검색의 시간복잡도는 $O(L)$로 빠르다.
* 접두사 검색 기능에 유리하다.

### 단점

* 모든 문자에 대해 노드를 생성하므로 메모리 사용량이 크다.
* 유니코드, 대소문자 구분 등 복잡한 문자 체계에 대해 구현 부담이 존재한다.

### 해시테이블 vs 트라이
- 장점
    - 트라이에서는 키 충돌 일어나지 않음
    - 모든 항목이 키에 따라 사전순 정렬
    - 해시 함수 사용x
    - 최악의 경우에도 안정적인 시간 복잡도
        - 탐색 시간 문자열 길이 L에 비례하여 $O(L)$
        - 반면, 해시 테이블은 평균 $O(1)$이나 충돌이 많으면 최악의 경우 $O(N)$

- 단점
    - 조회는 해시테이블보다 느리다
    - 불필요한 메모리 낭비
## 5. 시간복잡도 분석

| 연산          | 시간복잡도  |
| ----------- | ------ |
| 삽입 (insert) | $O(L)$ |
| 탐색 (search) | $O(L)$ |
| 접두사 검사      | $O(L)$ |

여기서 $L$은 문자열의 길이를 의미한다. 트라이는 문자열 길이에 비례하여 작업이 수행되므로, 긴 문자열보다는 짧고 반복되는 접두사가 많은 경우에 더 효율적이다.

## 6. 결론

트라이는 문자열 기반 데이터를 처리하는 데 유용한 자료구조이다.

접두사 기반 검색이 필요한 상황에서는 해시 테이블보다 유리한 선택이 될 수 있다.

검색 엔진 자동완성, 금지어 필터링, 사전 구현 등 다양한 분야에서 활용된다.

트라이의 기본 구조는 단순하지만 강력하며, 다양한 응용으로 확장 가능하다.

## 7. 참고
- GeeksforGeeks: Trie Data Structure (https://www.geeksforgeeks.org/trie-insert-and-search/)
- Wikipedia: Trie (https://en.wikipedia.org/wiki/Trie)