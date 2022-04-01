# Linked List

## Linked List란?

Array의 삭제, 삽입의 시간 복잡도 한계를 해결하기 위한 선형 자료구조이다.

노드들의 연결로 이루어져 있으며, 각 노드는 데이터 필드와 다음 노드에 대한 참조를 포함하고 있다.

따라서 **논리적 저장순서와 물리적 저장 순서가 일치하지 않는다.**

<img src = "https://user-images.githubusercontent.com/50768959/161087245-ea4dba1b-0e75-46f8-8925-36f8a53c3a6e.png" width="700">


## Linked List 종류

1. Singly Linked List (단일 연결 리스트)
    - 단일 연결 리스트는 위에서 설명한 가장 기본적인 연결 리스트이다.
    - 각 노드들은 데이터 값과 다음 노드에 대한 참조만을 가지고 있다.
    <img src = "https://user-images.githubusercontent.com/50768959/161087631-1968bdbb-0e2e-4db8-baf6-f48b50c1ef6b.png" width="700">
    
2. Doubly Linked List (이중 연결 리스트)
    - 이중 연결 리스트는 단일 연결 리스트와 노드들이 가지고 있는 데이터가 다르다.
    - 각 노드들은 데이터 값과 이전 노드에 대한 참조, 다음 노드에 대한 참조를 가지고 있다.
    - 따라서, 단일 연결 리스트는 다음 노드에만 접근할 수 있지만, 이중 연결 리스트는 이전 노드와 다음 노드 모두에 접근할 수 있다.
    <img src = "https://user-images.githubusercontent.com/50768959/161087769-ebf99819-9429-4b9c-90e5-708fb65e0857.png" width="700">

    

## Linked List 특징

- 동적 크기
    - Linked List는 배열과 다르게 길이가 정해져 있지 않다.
- Random Access 불가능
    - Linked List는 배열과 다르게 random access가 불가능하다.
    - 따라서 탐색을 위해서는 첫 번째 노드부터 순차적으로 원소에 접근해야 한다.
    - **이중 연결 리스트**의 경우에는 첫 번째 노드 또는 끝 노드 부터 순차적으로 접근하기 때문에, 탐색해야 하는 엘리먼트가 반으로 줄어든다.
    - 탐색시 시간복잡도 : O(n)
- 삽입, 삭제 용이
    - Linked List는 배열과 다르게 삽입,삭제시 shift가 필요 없다.
    - 원하는 위치 앞의 노드의 다음 노드 참조 값만 바꿔주면 된다.
    - 하지만 **단일 연결 리스트**의 경우, 삭제 또는 삽입할 위치 직전의 노드를 찾기 위해서는 탐색을 해야 하기 때문에, O(n)이다.
    - **이중 연결 리스트**의 경우, 삭제 또는 삽입할 위치의 노드의 참조를 현재 알고 있다면, O(1)이다.
    

## Java에서의 Linked List

Java에서 Linked List는 List 구현 클래스로 제공된다.

이는 이중 연결 리스트로, 이전 노드와 다음 노드를 모두 참조할 수 있다.

### Linked List의 선언 및 사용

```java
List<E> list = new LinkedList<E>();

//삽입
list.add(Something);
list.addFirst(Something);
list.addLast(Something);
list.add(index,Something);

//삭제
list.removeFirst();
list.removeLast();
list.remove();
list.remove(index);
list.clear(); 모든 값 제거
```
