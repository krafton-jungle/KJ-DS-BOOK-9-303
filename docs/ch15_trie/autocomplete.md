# 트라이를 활용한 자동완성 (Auto Complete)

## 📚 목차

1. [Auto Complete란?](#1-auto-complete란)
2. [자동완성의 작동방식](#2-자동완성의-작동방식)
3. [구현 예시 (Python)](#3-구현-예시-python)
4. [사용 예](#4-사용-예)
5. [시간복잡도 분석](#5-시간복잡도-분석)
6. [공간복잡도](#6-공간복잡도)
7. [자동완성에서 트라이가 유리한 이유](#7-자동완성에서-트라이가-유리한-이유)
8. [참고](#8-참고)

---

## 1. Auto Complete란?

입력한 **접두사(prefix)** 를 기반으로 가능한 모든 문자열을 자동으로 제안하는 기능이다.  
우리가 구글을 사용할 때, 검색어를 하나만 입력해도 연관된 검색어가 자동으로 제안되는 것과 같다.

검색창 외에도 IDE 자동완성, 코드 에디터, 검색어 추천기능 등에 빠르게 응답하고 정확도를 보장하기 위해 널리 사용된다.

![trie\autocomplete](/docs/assets/ch15_trie/autocomplete.png)
출처: Google 검색창

## 2. 자동완성의 작동방식

자동완성 기능은 기본적으로 다음과 같은 흐름으로 작동한다:

1. 사용자가 일부 단어( 접두사 - prefix )를 입력한다.
2. 트라이 자료구조에서 해당 접두사를 따라 내려가며 노드를 탐색한다.
3. 접두사 노드 이후로 연결된 모든 단어를 수집한다.
4. 수집된 단어들을 추천 리스트로 보여준다.

---

## 3. 구현 예시 (Python)

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    # insert 는 앞의 트라이에서 설명되었기 때문에 생략합니다.
    def insert(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                node.children[ch] = TrieNode()
            node = node.children[ch]
        node.is_end_of_word = True

    def auto_complete(self, prefix):
        node = self.root
        for ch in prefix:              # 접두사를 따라 트라이를 탐색
            if ch not in node.children:
                return []              # 접두사 불일치 시 빈 리스트 반환
            node = node.children[ch]

        result = []
        self._dfs(node, prefix, result)  # DFS로 후보 단어 탐색
        return result

    def _dfs(self, node, path, result):
        if node.is_end_of_word:
            result.append(path)        # 단어의 끝이면 결과 리스트에 추가
        for ch, child_node in node.children.items():
            self._dfs(child_node, path + ch, result)  # 자식 노드로 재귀 탐색
```

위 구현 예시틑 트라이 트리구조를 만들고 삽입하고 삭제하는 내용을 포함하고 있다.

조금 더 이해하기 쉽게 그림으로 보겠겠다.

![trie\trie](/docs/assets/ch15_trie/trie.png)

최초 진입점을 루트로 설정하고 단어가 알맞게 추가되고 있다.

사용자가 'ca'만 입력해도 트라이는 ca 까지 탐색후 'n','t','p'를 검색해서 자동완성 기능을 제안할 수 있다.

---

## 4. 사용 예

```python
trie = Trie()
words = ["cat", "car", "cap", "dog"]
for word in words:
    trie.insert(word) # 트라이에 단어를 삽입합니다.

print(trie.auto_complete("ca"))  # 출력: ['cat', 'car', 'cap']
print(trie.auto_complete("do"))  # 출력: ['dog']
print(trie.auto_complete("z"))   # 출력: []
```

---

## 5. 시간복잡도 분석

| 연산                            | 시간복잡도   | 설명                                                 |
| ------------------------------- | ------------ | ---------------------------------------------------- |
| 자동완성 검색 (`auto_complete`) | O(P + N × L) | P: 접두사 길이, N: 결과 단어 개수, L: 평균 단어 길이 |

- 접두사 탐색은 **O(P)**
- 접두사 이후 DFS를 통해 후보 단어들을 수집하는 데는 **O(N × L)**

제가 'search' 를 검색한다고 가정한다.

'se' 까지만 검색했다고 하면 se까지 탐색에는 문자열 길이 P만큼의 시간이 걸린다.

그 이후 DFS 를 통해 se 다음에 오는 단어들을 수집하는데에는  
se 하위의 단어 N 개 \* N개의 단어들의 평균길이 만큼의 시간복잡도가 소요된다.

## 6. 공간복잡도

- 전체 공간복잡도: **O(N × L)**
  - N: 단어 개수
  - L: 평균 단어 길이

트라이는 분명 문자열 검색에 최적화된 자료구조지만 단점도 존재한다.

- 공간복잡도가 크다.

문자열 하나하나를 모두 노드를 생성해서 저장하다보니, 메모리 사용량이 엄청나다.

구현도 단순히 배열을 만들어서 저장하거나  
해시맵 기반 ( python - dic ) 자료구조에 비하면 복잡한 편이다.

---

## 7. 자동완성에서 트라이가 유리한 이유

- ✅ **접두사 검색 속도**: O(P), P는 입력된 접두사의 길이
- ✅ **빠른 후보 수집**: DFS 또는 BFS를 통해 빠르게 가능
- ✅ **공간 효율성**: 공통 접두사 공유로 메모리 절약
- ✅ **정확한 추천**: 접두사 기반의 정밀한 단어 예측 가능

---

그래도 공간복잡도가 크다는 단점을 제외하면

**검색창, 코드 에디터, 스마트 키보드, 검색어 추천 시스템** 등에서 아주 강력하다.

혹시라도 검색창을 만들어야 될 일이 있다면 트라이를 사용해 자동완성 기능을 구현해 보는걸 추천한다

## 8. 참고

- Tistory : 자료구조-트라이-Trie (https://velog.io/@kimdukbae/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%ED%8A%B8%EB%9D%BC%EC%9D%B4-Trie)
