# Array List

## Java에서의 Array List란?

Java에서 Array List는 List 인터페이스의 구현 클래스이다.

<img src = "https://user-images.githubusercontent.com/50768959/161384499-b1dcf5ee-ccc4-4945-b3c6-7ac4c254d8e6.jpg" width="500">

### Java의 ArrayList 특징

- **객체가 인덱스로 관리된다.**
- 배열과 달리 저장 용량을 초과한 객체들이 들어오면 자동적으로 저장 용량이 늘어난다.
- 배열과 동일하게 삽입, 삭제 시 shift가 발생한다.
- 배열과 동일하게 인덱스를 통해 random access가 가능하다.

### Java에서의 ArrayList 사용 방법

```java
//1.선언 방법
List<E> list = new ArrayList<E>();
List<E> list = new ArrayList<E>(저장용량);

//2.삽입
list.add(Object); //마지막에 데이터 추가
list.add(index,Object);

//3.값 변경
list.set(index,Object);

//4.삭제
list.remove(Object);
list.remove(index);
list.clear();
```
