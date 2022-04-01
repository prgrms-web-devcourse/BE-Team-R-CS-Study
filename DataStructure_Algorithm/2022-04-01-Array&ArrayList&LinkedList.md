# Array vs ArrayList vs LinkedList

이 중, 두 개 또는 세 개 모두를 짝지어서 비교하는 질문이 자주 나옵니다.

***

### Array

<img src = "https://user-images.githubusercontent.com/50768959/161098465-958ab248-929e-4d8f-a96d-37f86d53037f.png" width="700">

- Index로 접근 가능함
- 삭제,삽입 시 shift 발생
- 고정 크기
- 논리적 저장 순서와 물리적 저장 순서가 일치함

***

### Array List

<img src = "https://user-images.githubusercontent.com/50768959/161098528-8ea1f8f8-fa04-42b7-b672-f95a5a90dd02.png" width="700">

- Index로 접근 가능함
- 삭제,삽입 시 shift 발생
- 가변 크기 : 꽉 차면 새로 더 큰 배열을 할당한다
- 내부적으로 배열로 구현되어있다.

***

### Linked List

<img src = "https://user-images.githubusercontent.com/50768959/161098583-66db65a3-7de1-4714-8d63-2cd3cd81eb8f.png" width="700">

- Index로 접근 불가능
- 탐색이 비교적 느리지만, 삭제, 삽입 시 shift 발생하지 않음
- 가변 크기 : 새로운 요소가 추가될 때마다 메모리를 할당한다 (동적 메모리 할당)
- 논리적 저장 순서와 물리적 저장 순서가 일치하지 않음
