# 재귀(Recursion)

- 재귀란 무엇인가?
- 호출 스택
- 재귀 함수의 두 가지 핵심 요소
- 하노이 탑

---

# 재귀란 무엇인가?

a process (a function in our case) that calls itself

자기 자신을 스스로 호출하는 절차(함수)

## 재귀의 예시

- JSON.parse, JSON.stringify
- document.getElementById, DOM 순회 알고리즘(traversal algorithm)
  - 돔은 모든 요소가 중첩된 트리 구조로 되어있다. 만약 그 모든 걸 살펴보고자 한다면, 흔한 접근법 중 하나가 재귀적으로 작동하는 코드를 작성하는 것이다.
- Object traversal(객체 순회)
- 때로는 반복(iteration)을 사용하는 것보다 재귀를 사용하는 게 깔끔할 때가 있다.

<br>

# 호출 스택

함수가 호출되면 자바스크립트의 경우 호출스택(call stack)에 함수가 쌓인다.

return 키워드나 함수 안에 더이상 실행할 코드가 없다면 컴파일러가 스택 제일 위에 있는 항목을 제거한다.

```jsx
function takeShower() {
  return 'Showering!';
}

function eatBreakfast() {
  let meal = cookFood();
  return `Eating ${meal}`;
}

function cookFood() {
  let items = ['Oatmeal', 'Eggs', 'Protein Shake'];
  return items[Math.floor(Math.random() * items.length)];
}
function wakeUp() {
  takeShower();
  eatBreakfast();
  console.log('Ok ready to go to work!');
}

wakeUp();
```

- 재귀함수를 사용할 땐, 호출 스택에 계속해서 새로운 함수를 push 한다.
- 하지만 어떤 지점에서 꼭 종료되어야 한다.
- 언제 종료되는지 어떻게 알 수 있을까?? ⇒ 종료 조건(base case)

<br>

# 재귀의 필수 요소

- 종료 조건 (base case) : 종료되는 시점이 있어야 한다.
- 다른 입력값 (different input) : 매번 다른 데이터를 가지고 함수를 호출해야 한다.

```jsx
function countDown(num) {
  if (num <= 0) {
    console.log('all done!');
    return;
  }
  console.log(num);
  num--;
  countDown(num);
}

countDown(5);
```

# 하노이 탑

3개의 기둥과 n 개의 원반이 있을 때, 목적 기둥으로 원반을 모두 옮기는 게임. 단, 한 번에 하나의 원반만 움직일 수 있고, 작은 원반 위에 큰 원반이 올 수 없다.

문제 : https://www.acmicpc.net/problem/11729

<br>

애니메이션 : https://codesandbox.io/s/tower-of-hanoi-visualization-z02l2

<img src="https://shoark7.github.io/assets/img/algorithm/hanoi-tower-process.png" width="900px"/>

<br>

가장 큰 원반이 목적지의 맨 아래에 놓여야하고, 이게 가능하려면 나머지 원반들이 크기 순서대로 보조 기둥에 놓여있어야 한다.
내가 옮기고 싶은 가장 큰 원반이 가장 아래에 오기 위해서는, 그 위의 원반들은 보조 기둥에 순서대로 꽂혀있어야 한다.

<img src="https://t3.daumcdn.net/thumb/R720x0/?fname=http://t1.daumcdn.net/brunch/service/user/cBY/image/A4BfDcwC2-9lpT5UO_Q0ThhC_-w.png" width="600px"/>

- n-1 개를 보조기둥으로 옮긴 후, 1개를 목적기둥으로 옮기고, n-1개를 보조기둥에서 목적기둥으로 옮기면 된다. 
- => n개를 옮기는 함수와 1개를 옮기는 함수가 필요
- hanoi(n) : 원반 개수 n개를 입력 받아 모든 원반을 특정 막대에 옮기는 움직임을 나타내는 함수
- move(1) : 가장 큰 원반 1개를 목적기둥으로 옮기는 움직임을 나타내는 함수

```
hanoi(n, start, to, via)
  - hanoi(n-1, start, via, to)
  - move(1, start, to)
  - hanoi(n-1, via, to, start)
  - if(n === 1) : move(1, start, to)
```

<br >

```js
let answer = [];
let count = 0;

function solution(input) {
  const move = (start, to) => {
    answer.push([start, to]);
    count += 1;
  };

  const hanoi = (n, start, to, via) => {
    if (n === 1) {
      move(start, to);
      return;
    }
    hanoi(n - 1, start, via, to);
    move(start, to);
    hanoi(n - 1, via, to, start);
  };

  hanoi(input, 1, 3, 2);
  console.log(count);
  return answer.map((i) => i.join(' ')).join('\n');
}

console.log(solution(3));
```

```
hanoi(3, 1, 3, 2) //hanoi(3) 원반 3개를 1번 기둥에서 목적기둥인 3번 기둥으로 옮기기
  hanoi(2, 1, 2, 3) //hanoi(2) 3번째 원반을 가장 아래로 옮기기 위해서 2번째 원반까지를 보조기둥인 2번 기둥으로 옮겨야 한다
    hanoi(1, 1, 3, 2) //hanoi(1) 2번째 원반을 가장 아래로 옮기기 위해서 1번째 원반을 목적 기둥인 3번으로 옮겨야 한다.
      move(1,3) // ✨1번 원반을 3번 기둥으로 이동
    move(1,2) // ✨ 2번 원반을 2번 기둥으로 이동!!
    hanoi(1, 3, 2, 1) // hanoi(1)
      move(3, 2) // ✨1번 원반을 2번 기둥으로 이동!!
  move(1,3) // ✨3번째 원반을 목적 기둥인 3번으로 이동!!
  hanoi(2, 2, 3, 1) // hanoi(2) 3번째 원반이 가장 아래에 두어졌으니 나머지 원반을 목적 기둥으로 옮겨야 한다.
    hanoi(1, 2, 1, 3) //hanoi(1) 2번째 원반을 목적 기둥으로 옮기기 위해서 1번째 원반을 1번 기둥으로 옮긴다.
      move(2,1) // ✨1번째 원반을 1번 기둥으로 이동!!!
    move(2, 3) //✨ 2번째 원반을 목적 기둥인 3번으로 이동!!
    hanoi(1, 1, 3, 2) // 1번째 원반을 목적 기둥인 3번으로 옮긴다.
      move(1, 3) //✨1번째 기둥을 목적 기둥인 3번으로 이동!!

```
