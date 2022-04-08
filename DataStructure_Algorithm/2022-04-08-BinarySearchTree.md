# 이진탐색트리(Binary Search Tree)

## 이진 트리 (Binary Tree)

- 각 노드의 자식 노드가 최대 2개인 트리

## 이진 탐색 (Binary Search)

- 배열 자료구조를 사용한다.
    - 배열은 조회에 강점이 있는 자료구조, 검색만 한다고 가정할 때는 트리구조가 필요없다.

## 이진탐색트리 (Binary Search Tree)

**이진 탐색 + 트리 (연결리스트)**

- 이진탐색 : 탐색에 소요되는 시간복잡도 O(logN)
- 트리 (연결리스ㅛ트) : 삽입, 삭제의 시간복잡도는 O(1)
- 두가지를 합하여 장점을 모두 얻는 것이 “이진탐색트리”
    - 효율적인 탐색 능력을 가지고, 자료의 삽입 삭제도 가능하다.

## 이진탐색트리 조건

- 각 노드에 중복되지 않는 키(Key)가 있다.
- 루트노드의 왼쪽 서브 트리는 해당 노드의 키보다 작은 키를 갖는 노드들로 이루어져 있다.
- 루트노드의 오른쪽 서브 트리는 해당 노드의 키보다 큰 키를 갖는 노드들로 이루어져 있다.
- 좌우 서브 트리도 모두 이진 탐색 트리여야 한다.

![이진탐색트리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbCe3QD%2Fbtq2ytHuN1Z%2FAi82KHYBlgY01j9hbwjOO1%2Fimg.png)

## 이진 탐색 트리 특징

1. BST의 Inorder Traveral을 수행하여 모든 키를 **정렬된 순서**로 가져올 수 있다.

⇒ 트리의 inorder traversal 결과 : 7, 11, 15, 50, 54, 62, 80

> 중위순회(inorder) 방식 (왼쪽 - 루트 - 오른쪽)


![이진탐색트리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcg5ezX%2Fbtq2y4AZEqe%2FBIKAzOf74qQmD4vqEN6lP1%2Fimg.png)

1. BST의 검색에 대한 시간복잡도는 **균형 상태**이면 **O(logN)**의 시간이 걸리고 **불균형 상태**라면 **최대 O(N)** 시간이 걸린다.
- 삽입, 검색, 삭제 시간복잡도는 모두 트리의 Depth에 비례한다.

![이진탐색트리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb5AX6Z%2Fbtq2BDbA4Sw%2FCDSV3lITsUrDIiss7ODMLk%2Fimg.png)

> 편향된 트리는 시간복잡도가 O(N)이므로 트리를 사용할 이유가 사라진다.
→ 이를 바로 잡도록 도와주는 개선된 트리가 AVL Tree이다.
>

## 이진탐색트리 연산

### 생성

> 50, 15, 62, 70, 7, 54, 11 이진탐색트리 생성
>
1. 50을 트리의 루트로 트리에 삽입한다.
2. 다음 요소를 읽고 루트 노드 요소보다 작으면 왼쪽 하위 트리의 루트로 삽입한다.
3. 그렇지 않으면 오른쪽 하위 트리 오른쪽 루트로 삽입한다.

![이진탐색트리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcexmJD%2Fbtq2z1DLANG%2FZFiFmM5657r46N4hKKzv91%2Fimg.png)

### 탐색 (Search)

1. **루트 노드의 키와 찾고자 하는 값을 비교**한다. 찾고자 하는 값이라면 탐색을 종료한다.
2. 찾고자 하는 값이 루트 노드의 키보다 작다면 왼쪽 서브 트리로 탐색을 진행한다.
3. 찾고자 하는 값이 루트노드의 키보다 크다면 오른쪽 서브트리로 탐색을 진행한다.
4. 위 과정을 찾고자 하는 값을 찾을 때까지 반복해서 진행한다. 검색 값이 없으면 null을 반환

> key = 60 탐색
>

![이진탐색트리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbDH5Xq%2Fbtq2D4F3nRp%2FUFt8cFNCzqfeytVgtvVLuk%2Fimg.png)

### 삽입 (Insert)

1. Root에서 시작
2. 삽입 값을 루트와 비교한다. 루트보다 작으면 왼쪽으로 재귀하고 크다면 오른쪽으로 재귀한다.
3. 리프 노드에 도달한 후 노드보다 크다면 오른쪽에 작다면 왼쪽에 삽입한다.

> key = 10 삽입
>

![이진탐색트리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmiB91%2Fbtq2DsUG16h%2FZBlqV9bktKWCSDfIfQYxT1%2Fimg.png)

### 삭제 (Delete)

- 이진 검색 트리에서 노드를 삭제하는 세 가지 상황이 있다.

### 1. 삭제할 노드가 리프노드인 경우

- 노드를 삭제하기만 하면 된다.

> key = 1 삭제
>

![이진탐색트리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeudyFG%2Fbtq2GXflqdC%2FTvIXkjTgEWoVoyvOv4xQN1%2Fimg.png)

### 2. 삭제할 노드에 자식이 하나만 있는 경우

- 노드를 삭제하고 자식 노드를 삭제된 노드의 부모에 직접 연결한다.

> key = 30 삭제
>

![이진탐색트리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fd9YABr%2Fbtq2y4HJBqp%2FDbafbadT1SL5WSnKO6AFLK%2Fimg.png)

### 3. 삭제할 노드에 자식이 둘 있는 경우

- 자식이 둘 있는 경우 successor 노드를 찾는 과정이 추가됩니다.

> 참고 :
> successor 노드란?
> 1. right subtree에 최소값
> 2. inorder 순회에서 다음 노드


1. 삭제할 노드를 찾는다.
2. 삭제할 노드의 successor 노드를 찾는다.
3. 삭제할 노드와 successor 노드의 값을 바꾼다.
4. successor 노드를 삭제

> 이진탐색트리 key = 50 삭제


![이진탐색트리](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FkYDgz%2Fbtq2BCDKWPR%2FT5wAjm1PwyAAKq9NNYctV0%2Fimg.png)

## 이진 탐색 트리 시간복잡도

### 검색/삽입 시간복잡도

- 모든 원소를 탐색하지 않는다.
- 최악의 경우 트리의 높이 만큼 탐색한다.
    - 높이가 H인 경우 검색/삽입의 시간 복잡도는 O(H)
- 원소의 개수로 표기
    - 원소의 개수가 N 이면 O(logN) (이진검색트리 구조가 균형 이진 트리인 경우)

### 삭제 시간 복잡도

삭제할 노드에 자식이 둘 있는 경우

- 삭제 노드 탐색 O(H)
- 대체 노드를 찾기 위한 서브 트리를 탐색 O(H)
- 참조값 변경 O(1)

삭제 연산의 시간 복잡도는 O(H + H + 1) → O(2H) → O(H) , O(logN)

## 질문

### 1. BST와 Binary Tree에 대해서 설명하세요.

이진탐색트리(Binary Search Tree)는 **이진 탐색과 연결 리스트**를 결합한 자료구조이다.

이진 탐색의 **효율적인 탐색 능력을 유지**하면서, 빈번한 **자료 입력과 삭제**가 가능하다는 장점이 있다.

이진 탐색 트리는 왼쪽 트리의 모든 값이 반드시 부모 노드보다 작아야 하고, 반대로 오른쪽 트리의 모든 값이 부모 노드보다 커야 하는 **특징**을 가지고 있어야 한다.

이진 탐색 트리의 **탐색, 삽입, 삭제의 시간복잡도는 O(H)**이다. 트리의 높이에 영향을 받는데, 트리가 균형이 맞지 않으면 워스트 케이스가 나올 수 있다. (그래서 이 균형을 맞춘 구조가 AVL Tree이다)

### 2. 힙과 이진 탐색 트리의 공통점과 차이점

- 공통점 : 힙과 이진 탐색 트리는 모두 이진 트리이다.
- 차이점
    - 힙은 각 노드의 값이 자식 노드보다 크거나 같다. (최대 힙의 경우)
    - 이진 탐색 트리는 왼쪽 자식 노드의 값이 가장 작고, 그 다음 부모 노드, 그 다음 오른쪽 자식 노드 값이 가장 크다.
- 이진 탐색 트리는 탐색을 위한 구조, 힙은 최대/최소값 검색을 위한 구조로 이해하면 된다.

![이진탐색트리](https://media.vlpt.us/images/nagosooo/post/0958aa5e-780e-4ec9-bcee-55ef1994acb9/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202021-10-27%20%EC%98%A4%ED%9B%84%202.27.28.png)

## 참고

[https://yoongrammer.tistory.com/71](https://yoongrammer.tistory.com/71)

[https://mommoo.tistory.com/101](https://mommoo.tistory.com/101)