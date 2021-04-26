---
title: "[TIL] BinarySearchTree 재귀 함수 반환 오류"
date: 2021-04-26T16:59:57+09:00
---

### 이진탐색트리 자료구조 Binary Search Tree

 ![img](https://miro.medium.com/max/315/1*QbBvIgb_WS08Nm17kLGSrQ.png)

- 각 노드는 유니크한 값을 갖는다.
- 최상위에는 루트 노드가 존재한다.
- 다음에 오는 노드는 상위 노드와의 값 비교를 통해 왼쪽 또는 오른쪽에 위치한다.
- 쓰는 이유: 데이터를 항상 제자리에 두기 위하여
- 변화가 많은 동적인 데이터셋에서 정렬 가능한 형태를 유지하기 쉬움

### 알고리즘 형태

1. `search` 함수 호출
2. `self.root`가 없다면 노드 생성하여 전달
   1. 있다면 재귀함수 `_recursive_search` 에 `self.root`를 인자로 함수 호출
3. 탐색값과 인자로 받은 노드(`self.root`)의 값을 비교
   1. 동일하면 노드 리턴
   2. 노드의 값보다 작다면 루트의 왼쪽 노드를 인자로 다시 `_recursive_search` 호출
      1. 동일한 값을 찾을 때까지 재귀
   3. 노드의 값보다 크다면 루트의 오른쪽 노드를 인자로 다시 `_recursive_search` 호출
      1. 동일한 값을 찾을 때까지 재귀
   4. 찾는 값이 존재하지 않으면 Attribute 에러 raise

### 코드

```python
    def _search_recursive(self, id, node):
        try:
            if id == node.data["id"]:
                print('find')
                return node.data # None 값만 반환
            elif id < node.data["id"]:
                print('left')
                self._search_recursive(id, node.left) 
                # return을 주지 않아 새로 호출된 재귀함수의 값이 search 함수에 반환되지 않음
            else:
                print('right')
                self._search_recursive(id, node.right)
        except AttributeError as e:
            return False
            
    def search(self, blog_post_id):
        blog_post_id = int(blog_post_id)
        if self.root is None:
            raise AttributeError('no root')
        else:
            return self._search_recursive(blog_post_id, self.root)
```

### 오류

- 오류 발생: line20에서 재귀함수 `_recursive_search`의 리턴값이 항상 None임.
- 오류 원인: 재귀 함수를 호출은 했지만 리턴 값을 전달하지 않음
- 오류 해결: 재귀 함수를 호출하는 라인 8과, 라인12에 return 문 추가
  `return self._search_recursive(id, node.left)`

### reference

- [stackoverflow: recursive function w/o return logic](https://stackoverflow.com/questions/54812654/python-recursive-function-without-return-logic)
- [why use binary search tree](https://www.nickang.com/2017-12-10-why-use-binary-search-tree/)
