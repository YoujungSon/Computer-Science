# List

리스트는 자료(Data)들을 일렬로 나열하여 다루는 것이다.



## Index

연속하여 존재하는 데이터들의 "**위치**"를 말한다.


Index는 0부터 시작한다.

# 리스트 종류

리스트는 ArrayList, LikedList로 나누어진다.

## ArrayList

ArrayList는 실제 물리메모리에 연속된 위치에 데이터들을 저장하는 기법이다.

![image](https://user-images.githubusercontent.com/79268661/198816956-58fc4bf9-0974-469c-a850-cf9cd5d04e7c.png)


![images_foeverna_post_983d9265-61d0-45b4-b81d-7059cad32201_skjdflas2](https://user-images.githubusercontent.com/79268661/198816979-ca9ebcbe-037a-477e-acdc-95ffa5c4c6df.png)


위 사진과 같이 실제 물리메모리에 연속해서 저장된다.

## LikedList

ListedList는 ArrayList와는 다르게 실제 물리메모리의 연속된 위치에 데이터들을 저장하지 않는다.

다음 그림을 보면서 이해해보자.

![image](https://user-images.githubusercontent.com/79268661/198817059-ae43c3c2-3a09-4ff5-bb0b-a4c39079b392.png)

위 사진과 같이 _**500**_ 번 주소에 Data가 저장되고, 다음 Index의 주소값(1004)을 저장하고있다.

# ArrayList 장단점

```java
int[] arr = new int[];
```

위와같이 자바코드에 작성하면 컴파일이 되지않는다.

이유는 메모리에 배열을 할당해야하는데 배열의 크기를 얼만큼 할당받아야하는지 모르기때문이다.

```java
int[] arr = new int[10];
```

위와같이 처음에 할당할 크기(reservation)를 정해주어야 비로소 배열을 생성할 수 있다.

![image](https://user-images.githubusercontent.com/79268661/198817488-758531ac-fe2b-48d7-9e97-dc1432cf338f.png)

위와 같이 int의 크기인 "4byte"만큰의 공간을 총 8개 할당받는다.

## 장점

> 정답부터 말하면, 검색이 빠르다.

ArrayList는 가장 처음에 지정해준 타입의 크기만한 공간을 reservation만큼 할당받는다.

그렇기 떄문에 컴퓨터 입장에서 특정 index에 진입할 때, 엄청나게 빠르다.

다음의 그림을 보자.


위 그림에서 Index 5에 값을 가져오라고하면 여러분들은 어떻게 할 것인가?
너무나 쉬울 것이다.

index 5가 적혀있는 부분의 값을 말할 것이다.

컴퓨터는 다르다.

컴퓨터 입장에서는 배열 0번의 주소인 "0x200"에 Int 크기인 4byte를 인덱스만큼 곱해주고 더하여 해당 Index 5번을 찾는다.

다음과 같이 접근한다. 200 + 4 * 5 = 220   -> 0x220 주소에 바로 접근

> 사실 상, for문을 돌려 앞에부터 count를 세면서 5에 접근하는 것이랑 속도가 비슷하다. 하지만 index 10000000번으로 접근하라고 하면? 너무나 오래걸린다.

## 단점

> 정답부터 말하면, 삽입과 삭제가 상당히 느리다. 특수한 상황에서는 더 느리다.

위에서 설명했던 것 처럼, 배열은 가장 처음에 메모리의 일정 공간을 먼저 할당받고 시작한다.

### 삽입

아래의 그림처럼 특정 Index에 값을 넣어야하는 경우에는 어떻게 해야하는가?


먼저 모든 값들을 1칸씩 다음 Index로 밀어낸다음 특정 Index에 값을 삽입해야한다.

> Index가 1000000번까지 존재하는 배열의 0 Index에 삽일할 경우에 999999개를 한 칸씩 옆으로 밀어주어야 한다. 상당히 느리다.


### 삭제

삭제 또한 마찬가지이다. 

아래의 그림처럼 특정 Index를 삭제하면 뒤에 존재하는 Index 값들을 앞으로 1칸씩 당겨주어야하는 작업이 필요하다.

> Index가 1000000번까지 존재하는 배열의 0 Index를 삭제할 경우에 뒤에 존재하는 999999개를 한 칸씩 옆으로 밀어주어야 한다. 상당히 느리다.

# LikedList 장단점

LikedList는 ArrayList에서의 단점을 보완하기 위해 생겨난 자료구조이다.

LikedList는 다음 그림처럼 어떠한 Index가 다음 Index의 주소를 가지고 있다.

이게 어떻게 가능한가?
다음의 코드를 보고 이해해보자.

```java
public class Node{
  Node next;
  int value;
}
```

위와 같이 만들면 연결리스트를 만든 것이다.

사용해보자.

```java

Node root = new Node();
root.value = 10;
root.next = new Node();
root.next.value = 11;
```

위와 같이 구성하면 다음 그림과 같이 구성된다.

그림에도 나와있듯이, root가 0번 Index가 된다. root.next가 1번 Index가 된다.

## LikedList 단점

> 정답부터 말하면, 검색속도가 상당히 느리다.

위의 ArrayList는 처음주소 + 자료형의 크기 * 구하려는 Index를 하여 연산 1번에 바로 접근하려는 Index의 주소를 찾을 수 있다.

하지만 LikedList 같은 경우는 어떤 주소에 Index가 존재하는지 예측할 수 없다.

그래서 root부터 next, next, next 하여 원하는 Index에 도착하여야 한다.

하나씩 직접 찾아가야하기 때문에 검색에서 상당히 느리다.

## LinkedList 장점

> 정답부터 말하면, 삽입과 삭제가 상당히 빠르다.

다음 그림을 보자.

0 Index와 1 Index에 새로운 값을 삽입하려고 한다.

이때 LinkedList는 아주 빠르게 삽입이 가능하다.

다음의 그림을 보자.

위 그림처럼 0 Index에 존재하는 "next"값을 삽입하려는 요소로 변경한다.
그리고 새롭게 삽입하는 요소의 "next"값을 0x320으로 바꾸어준다.

> ArrayList와는 다르게 별도로 나머지 요소들을 옆으로 밀어주거나 당겨주는 작업이 필요없다.


삭제 또한 마찬가지이다. 특정 값이 빠져나가게 되면 특정 값을 가리키던 "next"값을 특정 값이 가리키던 "next"로 바꾸어주면 된다.










