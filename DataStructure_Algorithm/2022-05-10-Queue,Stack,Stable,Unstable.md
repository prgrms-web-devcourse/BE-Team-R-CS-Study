# < Stack & Queue>

## 1. Stack

<figure>
  <img src = "https://user-images.githubusercontent.com/50768959/167532233-8bc1efad-23ec-4a55-88a7-d7d8a0033f50.png" width="700">
  <figcaption>
    출처 : [https://zoosso.tistory.com/868](https://zoosso.tistory.com/868)
  </figcaption>
</figure>

<br><br>

Stack은 말 그대로 데이터를 쌓아 올린다는 의미를 가진 자료구조로, 입력과 출력이 한 곳으로 제한되어있다.

Stack은 LIFO(Last-In-First-Out)에 따라 자료를 배열한다. 따라서 Stack은 배열과 달리 index를 통해 특정 항목에 접근할 수 없다. 하지만 데이터를 추가하거나 삭제할 때, 배열처럼 원소들을 하나씩 옆으로 밀어 줄 필요가 없기 때문에, O(1) 시간에 가능하다. (배열은 O(n)임)

<br>

- pop : Stack에서 가장 위에 있는 항목을 제거한다.
- push : 데이터 하나를 Stack의 가장 윗 부분에 추가한다.
- peek : Stack의 가장 위에 있는 항목을 반환한다.
- isEmpty : Stack이 비어 있을 때에 true를 반환한다.

<br>

```java
public class MyStack<T>{
    private static class StackNode<T> {
        private T data;
        private StackNode<T> next;
        public StackNode(T data){
            this.data= data;
        }
    }
    
    private StackNode<T> top;
    
    public T Pop(){
        if(top == null) throw new EmptyStackException();
        T item = top.data;
        top = top.next;
        return item;
    }
    
    public void push(T item){
        StackNode t = new StackNode(item);
        t.next = top;
        top = t;
    }
    
    public T peek(){
        if (top == null) throw new EmptyStackException();
        return top.data;
    }
    
    public boolean isEmpty(){
        return top == null;
    }
}
```

<br>

## 2. Queue

<figure>
  <img src = "https://user-images.githubusercontent.com/50768959/167532323-5107321e-7baf-4bae-832d-d486c0513d69.png" width="700">
  <figcaption>
    출처 : [https://www.fun-coding.org/Chapter05-queue-live.html](https://www.fun-coding.org/Chapter05-queue-live.html)
  </figcaption>
</figure>

<br><br>

Queue는 Stack과 달리 입력과 출력에 각각의 한 쪽 끝을 제한한다. (한 쪽으로는 입력만, 한 쪽으로는 출력만 가능하다)

Queue는 FIFO (First-In-First-Out) 순서를 따른다. 즉, 큐에 저장되는 항목들은 큐에 추가되는 순서로 제거된다.

<br>

- enQueue() : 큐에 데이터 넣기
- deQueue() : 큐에서 데이터 빼기
- isEmpty()
- isFull()

<br>

- front : deQueue 할 위치
- rear : enQueue 할 위치 (구현에 따라 다음 빈 칸을 나타내는 경우도 있고, 마지막 입력된 칸을 나타내는 경우도 있음)

<br>

- 연결리스트로 구현한 큐 : front, rear 대신 first, last를 사용함. 연결리스트라 full 개념이 없음

```java
public class MyQueue<T>{
    private static class QueueNode<T>{
        private T data;
        private QueueNode next;
        public QueueNode(T data){
            this.data = data;
        }
    }

    private QueueNode<T> first;
    private QueueNode<T> last;

    public void add(T item){
        QueueNode t = new QueueNode(item);
        if (last != null){
            last.next = t;
        }
        last = t;
        
        if (first == null){
            first = last;
        }
    }
    
    public T remove(){
        if (first == null) throw new NoSuchElementException();
        T data = first.data;
        first = first.next;
        if (first == null){
            last = null;
        }
        return data;
    }
    
    public T peek(){
        if (first == null) throw new NoSuchElementException();
        return first.data;
    }
    
    public boolean isEmpty(){
        return first == null;
    }
}
```

<br>


## 3. 원형큐

### (1) 배열을 사용한 Queue의 단점

배열을 사용한 Queue의 경우, 배열의 가장 앞에 있는 데이터를 꺼낸 후, 그 다음 인덱스의 데이터들을 한 칸씩 모두 이동해야 하는 단점이 있다.

따라서, 데이터 하나를 꺼낼 때 마다, O(n)만큼의 시간 복잡도를 요구하므로 비효율적이다.

### (2) 배열 Queue의 단점을 보완한 원형 큐

원형 큐는 다음 그림처럼 front는 첫번째 데이터를 가리키고 rear은 마지막 데이터를 가리키는 형태를 가지고 있다. 이 때문에, 기존의 배열 큐처럼 데이터를 매번 한 칸씩 이동시킬 필요가 없어서 데이터를 빠르게 처리할 수 있다.

<img src = "https://user-images.githubusercontent.com/50768959/167579721-fd867745-1add-4f0b-8806-7d00d73e61a5.png" width="250">

원형 큐에 최초로 데이터를 추가한 후의 모습은 다음과 같다.

<img src = "https://user-images.githubusercontent.com/50768959/167579910-e1e65515-d392-431e-9fa0-94b5caf42d21.png" width="250">

다음과 같은 원형 큐에서 데이터를 삭제해보자.

<img src = "https://user-images.githubusercontent.com/50768959/167580099-faeb3f20-10f0-460c-a3a3-d535e85deb16.png" width="300">

front에 있는 어피치를 삭제하면 frotn가 그 다음 데이터인 프로도를 가리키게 된다. 그리도 데이터들의 이동은 나타나지 않는다.

<img src = "https://user-images.githubusercontent.com/50768959/167580155-60892f88-73ef-42bc-8d36-2b57f9848493.png" width="300">

원형 큐에서 데이터가 최대로 저장된 경우는 다음과 같다.

<img src = "https://user-images.githubusercontent.com/50768959/167580231-1175d9de-de11-4225-9025-055248b75b8a.png" width="300">

데이터가 꽉 찬 경우는 위처럼 front와 rear의 인덱스 차이가 한 칸 만큼 날 때이다. 

front와 rear이 같은 경우는 큐가 빈 경우인데, 꽉 찬 경우도 front와 rear이 같도록 만들면 큐가 빈 경우와 구분할 수 없기 때문에 위처럼 구현한다.

## 4. Stack 두 개로 Queue 구현하기

```java
class MyQueue<T>{
    private Stack<T> realStack = new Stack<>();
    private Stack<T> tempStack = new Stack<>();

    public T push(T data){
        while(!realStack.isEmpty())
            tempStack.push(realStack.pop());

        realStack.push(data);

        while(!tempStack.isEmpty())
            realStack.push(tempStack.pop());

        return data;
    }

    public T pop(){
        return realStack.pop();
    }
}
```

<br>

# <Stable Sort & Unstable Sort>

## 1. Stable Sort

Stable Sort란 정렬을 진행할 때, 같은 값의 숫자더라도 그 상대적인 위치가 유지되는 sorting 방식을 말한다.

다음과 같은 배열을 정렬할 때,

| 3 (1) | 3 (2) | 4 | 2 | 1 | 5 | 3 (3) |
| --- | --- | --- | --- | --- | --- | --- |

다음과 같이 3의 순서가 입력된 순서와 동일한 경우가 Stable Sort이다.

| 1 | 2 | 3 (1) | 3 (2) | 3 (3) | 4 | 5 |
| --- | --- | --- | --- | --- | --- | --- |

<br>

### (1) Stable Sort 종류

- Insertion sort
- Merge sort
- Bubble sort
- Selection sort : 구현 방식에 따라 Stable 할 수 있다.

<br>

## 2. Unstable Sort

Unstable Sort란 정렬을 진행할 때, Stable Sort와 달리 상대적인 위치가 유지되지 않는 sorting 방식이다.

다음과 같은 배열을 정렬할 때,

| 3 (1) | 3 (2) | 4 | 2 | 1 | 5 | 3 (3) |
| --- | --- | --- | --- | --- | --- | --- |

다음과 같이 3의 순서가 입력된 순서와 달라질 가능성이 있는 정렬이 unstable sort 방식이다.

| 1 | 2 | 3 (2) | 3 (1) | 3 (3) | 4 | 5 |
| --- | --- | --- | --- | --- | --- | --- |

<br>

### (1) Unstable Sort 종류

- Quick Sort
- Heap Sort
- Selection Sort : 구현 방식에 따라 Unstable 할 수 있다.
