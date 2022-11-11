# 해시 테이블(Hash Table)

![https://velog.velcdn.com/images%2Fjangws%2Fpost%2Ff2303335-5f1c-4889-8dfb-819b78d4ca9d%2F1200px-Hash_table_3_1_1_0_1_0_0_SP.svg.png](https://velog.velcdn.com/images%2Fjangws%2Fpost%2Ff2303335-5f1c-4889-8dfb-819b78d4ca9d%2F1200px-Hash_table_3_1_1_0_1_0_0_SP.svg.png)

- 목표
  - 해시 알고리즘에 대한 정의
  - 좋은 해시 알고리즘을 만드는 방법
  - 해시 테이블에서 충돌이 발생하는 경우 이해
  - 개별 체이닝(separte chaining)과 선형 조사법(linear probing)을 이용한 충돌 해결
- 해시 테이블이란
  - 해시 테이블은 key-value 쌍을 저장하는 데 사용한다.
  - 해시 테이블의 key는 순서를 갖지 않는다.(배열과 달리)
  - 해시 테이블은 값을 찾거나 추가하거나 제거하는 데 빠르다.(배열과 달리)
- 해시 테이블은 이처럼 속도가 빠르기 때문에 자주 사용되며, 대부분의 프로그래밍 언어에서는 다음과 같이 해시 자료 구조를 갖고 있다. 이들은 모두 key-value 쌍 데이터를 저장하고 해시 테이블을 사용한다.
  - Python: Dictionary
  - Javascript :Map, Object(객체에서는 key로 문자열만 사용 가능하다)
  - Java, Go, Scala : Map
  - Ruby : Hash

## 해시 함수 초안

- 만약 이와 같은 내장 자료구조가 없다고 가정하고, 자바스크립트로 해시 테이블을 직접 만들어보자.
  - key를 갖고 value를 찾으려고 하면, key를 배열의 유효한 인덱스로 변환하는 방법이 필요하다. 이러한 기능을 하는 함수를 `해시 함수(Hash fucntion)` 라고 부른다.
    - key를 해시 함수에 입력하면, key들은 각각 항상 같은 인덱스(숫자)들로 변환돼야 한다.
    - 하지만 반대로는 변환이 불가하다. 즉, 해시된 인덱스를 key로 변환할 수는 없다. 해시 함수는 단방향 함수이다.
- 좋은 해시 함수의 조건 (보안 목적은 고려하지 않음)
  1. 속도가 빠르다(ex, O(1))
  2. 해시된 인덱스가 한 곳으로 뭉치지 않고, 골고루 분배돼야 한다.
  3. 결정론적이다(=특정 입력값을 입력할 때마다 같은 출력값이 나와야 한다).
- 문자열에만 작동하는 해시 함수를 만들어보자.
  - `"a".charCodeAt(0) - 96`
  - `Stirng.prototpye.charCodeAt(인덱스)` 는 주어진 인덱스에 대한 UTF-16 코드를 반환한다. 알파벳 A의 UTF-16 코드는 97부터 시작되므로, `"a".charCodeAt(0) - 96` 는 1을 의미한다. 이를 이용해서 아래와 같은 해시 함수를 만든다.

```jsx
  _hash(key) {
    let total = 0;
// key에 있는 글자들에 대해 아래와 같은 루프를 돌린다.
// 각각의 글자에 대한 코드값(value)과 total 더하고, 이를 배열 길이로 나눈 나머지를 total에 저장한다.
    for (let i = 0; i < key.length; i++) {
      const char = key[i];
      const value = char.charCodeAt(0) - 96;
      total = (total + value) % this.keyMap.length;
    }
    return total;
  }
```

- 위와 같은 해시 함수의 문제점
  1. 오직 문자열에 대해서만 해시할 수 있다(하지만 이는 의도한 것이므로 일단 논외)
  2. 함수 실행에 걸리는 시간이 O(1)이 아니다. key의 길이만큼 O(N)의 시간이 소요된다.
  3. 무작위성이 떨어지고, 해시된 인덱스가 충돌할 수 있다.

## 해시 함수 개선하기

- 먼저 속도를 조금 더 빠르게 해보자.
  - 예를 들어, 해시하려는 key 문자열이 수백만 글자라고 하면, for문을 수백만번 돌려야 한다. 이러한 비효율을 방지하기 위해, for문의 반복횟수인 key.length을 Math.min(key.length, 100)으로 수정하여 수 백만 번의 루프 대신 앞 글자 100개에 대해서만 루프를 돌리도록 지정한다.
    - 이를 통해 `거의` O(1)의 시간복잡도를 갖게 됐다.
    - 하지만, 어떤 데이터 key들은 언제나 맨 앞의 100 글자가 같고 나머지 글자들이 다른 경우가 있다면, 알고리즘을 약간 바꿔서 맨 뒤에서 시작한다거나 약간의 조정이 필요하다.
- 해시된 인덱스의 충돌을 줄이자.충돌의 횟수를 줄이고 데이터가 더 퍼지고 무작위성을 가지도록 하기 위해 `소수`를 추가한다. `소수` 를 추가하는 데에는 수학적인 이유가 있으나 복잡하므로 일단 넘어가고, 실제로 효과가 있음을 다음과 같은 표를 통해 확인한다.테이블 저장하는 배열의 길이가 소수이면, 충돌이 일어날 확률이 더 적어지기 때문에 해시 함수 코드 내에 소수를 추가한다.일반적으로 추가하는 소수가 더 클수록 충돌이 일어날 확률이 적어진다.[Does making array size a prime number help in hash table implementation? Why?](https://www.quora.com/Does-making-array-size-a-prime-number-help-in-hash-table-implementation-Why)
  ![https://velog.velcdn.com/images%2Fjangws%2Fpost%2F1307a9a9-d469-402b-bf42-7138f4975376%2FUntitled%20(19).png](<https://velog.velcdn.com/images%2Fjangws%2Fpost%2F1307a9a9-d469-402b-bf42-7138f4975376%2FUntitled%20(19).png>)

```jsx
  _hash(key) {
    let total = 0;
    const WEIRD_PRIME = 31;  // 소수 추가
		// for 문의 최대 횟수를 Math.min을 통해 100 이하로 제한
    for (let i = 0; i < Math.min(key.length, 100); i++) {
      const char = key[i];
      const value = char.charCodeAt(0) - 96;
			// 소수 반영해서 모듈러 연산
      total = (total * WEIRD_PRIME + value) % this.keyMap.length;
    }
    return total;
  }
```

## 해시함수에서 충돌 처리

- 해시 함수를 사용할 때, 데이터가 아주 많이 있는 경우라면 충돌이 어느 정도 일어날 가능성이 높다. 또는 데이터 저장 공간이 아주 크고 해시 함수가 아주 좋은 것일지라도 여전히 충돌은 발생할 확률이 있다.
- 아까 만든 조악하나 해시 함수와 아주 작은 해시 테이블을 갖고 작업을 하게 되면, 충돌이 확실히 일어나게 된다.
- 충돌 해결 방법
  1. Separte Chaining(개별 체이닝)
  2. Linear Probing(선형 탐색법)

### 방법1. Separte Chaining(개별 체이닝)

- 개별 체이닝은 같은 장소에 여러 데이터를 저장할 때 배열이나 연결 리스트 등과 같은 것을 활용하여 `중첩 데이터 구조`를 쓰는 것이다. 즉, 충돌하는 데이터를 같은 인덱스에 중첩해서 저장하는 것이며, 배열을 쓰면 아래 이미지와 같이 중첩 배열 구조가 나오게 된다.
- 개별 체이닝을 사용하면, 테이블의 길이보다 더 많은 데이터를 저장할 수 있다.

![https://velog.velcdn.com/images%2Fjangws%2Fpost%2F1a58d7fd-9901-4c97-96c1-d44ff5ba3bcd%2FUntitled%20(20).png](<https://velog.velcdn.com/images%2Fjangws%2Fpost%2F1a58d7fd-9901-4c97-96c1-d44ff5ba3bcd%2FUntitled%20(20).png>)

### 방법2. Linear Probing(선형 탐색법)

- 선형 탐색법은 각 위치에 하나의 데이터만 저장한다는 규칙을 그대로 살려서 지킨다.
  - 다만, 충돌이 발생하면 다음 빈 인덱스가 어디인지 확인하고 빈 인덱스 공간에 저장한다.
- 선형 탐색법은 최대 테이블의 길이만큼만 저장할 수 있다.

![https://velog.velcdn.com/images%2Fjangws%2Fpost%2Ffa864e48-8496-4f45-a8b1-c9aaa1245909%2FUntitled%20(21).png](<https://velog.velcdn.com/images%2Fjangws%2Fpost%2Ffa864e48-8496-4f45-a8b1-c9aaa1245909%2FUntitled%20(21).png>)

## 해시 테이블 기본 구조(Class)

```jsx
class HashTable {
  // 전달된 size 매개변수 값이 없으면 테이블 길이 기본값을 53을 설정한다.
  constructor(size = 53) {
    this.keyMap = new Array(size); // 배열 사용
  }

  _hash(key) {
    let total = 0;
    const WEIRD_PRIME = 31;
    for (let i = 0; i < Math.min(key.length, 100); i++) {
      const char = key[i];
      const value = char.charCodeAt(0) - 96;
      total = (total * WEIRD_PRIME + value) % this.keyMap.length;
    }
    return total;
  }
}
```

## set 메서드와 get 메서드

- 이번에는 위에서 살펴본 충돌 해결 방법 중 Separate Chaining(개별 체이닝)을 활용하여 Set과 Get 메서드를 만들어 본다.
- set 메서드

```jsx
  set(key, value) {
// 입력된 key를 해시하여 index 를 구한다.
    const index = this._hash(key);
// 만약 충돌하지 않는다면 일단 해당 인덱스에 빈 배열을 할당해주고
    if (!this.keyMap[index]) {
      this.keyMap[index] = [];
    }
// 그 인덱스에 있는 배열에 [key, value] 를 push한다.
    this.keyMap[index].push([key, value]);
  }
```

- get 메서드

```jsx
  get(key) {
// 입력된 key를 해시하여 index 를 구한다.
    const index = this._hash(key);
// 만약 해당 인덱스에 값이 있다면
    if (this.keyMap[index]) {
// 그 인덱스 배열을 순회하여 찾는 것과 동일한 key를 찾고 그것의 value를 return 한다.
      for (let i = 0; i < this.keyMap[index].length; i++) {
        if (this.keyMap[index][i][0] === key) {
          return this.keyMap[index][i][1];
        }
      }
    }
// 만약 해당 인덱스에 값이 전혀 없다면 undefined를 return한다.
    return undefined;
  }
```

## keys 메서드와 values 메서드

- keys 메서드는 테이블에 있는 모든 key들의 목록(중복되는 것은 한 개씩)을 반환한다.

```
  keys() {
// 마지막에 return할 빈 배열을 정의한다.
    const keysArr = [];
// 해시 테이블 배열을 순회하면서 값이 있는 인덱스에서만
    for (let i = 0; i < this.keyMap.length; i++) {
      if (this.keyMap[i]) {
// 그 인덱스에 할당된 배열을 순회하고
        for (let j = 0; j < this.keyMap[i].length; j++) {
// 만약 keysArr에 이미 있는 key가 아닌 경우에만 그 key를 keysArr에 push한다
          if (!keysArr.includes(this.keyMap[i][j][0])) {
            keysArr.push(this.keyMap[i][j][0]);
          }
        }
      }
    }
// 반복문이 다 종료된 후 keysArr을 return한다.
    return keysArr;
  }

```

- values 메서드는 테이블에 있는 모든 value들의 목록을 반환한다.
  - keys메서드와 로직이 동일하며, value는 배열의 1 인덱스에 위치하므로 `this.keyMap[i][j][1]` 부분만 수정됐다.

```
values() {
// 마지막에 return할 빈 배열을 정의한다.
    const valuesArr = [];
// 해시 테이블 배열을 순회하면서 값이 있는 인덱스에서만
    for (let i = 0; i < this.keyMap.length; i++) {
      if (this.keyMap[i]) {
// 그 인덱스에 할당된 배열을 순회하고
        for (let j = 0; j < this.keyMap[i].length; j++) {
// 만약 valuesArr에 이미 있는 value가 아닌 경우에만 그 value를 valuesArr에 push한다
          if (!valuesArr.includes(this.keyMap[i][j][1])) {
            valuesArr.push(this.keyMap[i][j][1]);
          }
        }
      }
    }
// 반복문이 다 종료된 후 valuesArr을 return한다.
    return valuesArr;
  }
```

## 해시 테이블의 성능

- 시간복잡도(평균)
  - 삽입 : O(1)
  - 삭제 : O(1)
  - 접근 : O(1)
- 이러한 시간복잡도는 실제로 해시 함수의 성능에 달려 있다. 해시 함수가 얼마나 빠른지, 그리고 얼마나 고르게 데이터를 분배해서 충돌의 확률을 줄이는지 말이다.
  - 위에서 작성한 코드들은 유용성을 염두하여 만든 것이 아니라 교육용 체험판에 불과하다고 하다.
  - 따라서 좋은 해시 함수를 사용하여 성능을 높이기 위해서는, 직접 만든 해시 함수를 사용하기보다는 유명한 코드나 라이브러리들을 활용하자.
