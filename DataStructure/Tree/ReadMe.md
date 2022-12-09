# 트리

발표자: 손유정
스터디 날짜: 2022년 12월 9일
주차: 6주차
학습범위: 트리

## 1. 트리 소개

- 일반 트리, 이진 트리, 이진 탐색 트리(Binary Search Trees) 등 종류가 있다
- 이진 탐색 트리가 핵심
- 노드로 이루어지며 parent-child 관계를 갖는다
  - 부모 노드는 자식 노드 만을 가리킬 수 있다 (동일 수준의 노드 가리킬 수 없음)
- 비선형(non-linear) 자료구조 (선형 자료구조 : 리스트)
- 시작지점(entry point)이 단 하나여야 한다 -> 루트

### 1-1. 용어 정리

![Untitled](%E1%84%90%E1%85%B3%E1%84%85%E1%85%B5%2069ff0a06bc714b789d91ac056fa9b39d/Untitled.png)

- Root : 최상위 노드
- Child : Root에서 내려와 다른 노드로부터 연결된 노드
- Parent : Child의 상위 노드
- Siblings : 같은 Parent를 가진 노드
- Leaf : Child가 없는 노드
- Edge (간선) : 한 노드와 다른 노드 간의 연결

### 1-2. 트리 활용

- HTML DOM : DOM Tree 구조를 가짐
- 네트워크 라우팅
- 추상구문트리 (AST: Abstract Syntax Tree) : 프로그래밍 언어의 구문을 설명

![Untitled](%E1%84%90%E1%85%B3%E1%84%85%E1%85%B5%2069ff0a06bc714b789d91ac056fa9b39d/Untitled%201.png)

![Untitled](%E1%84%90%E1%85%B3%E1%84%85%E1%85%B5%2069ff0a06bc714b789d91ac056fa9b39d/Untitled%202.png)

- 인공지능 (ex. 최소 최대 트리, 결정 트리)

![Untitled](%E1%84%90%E1%85%B3%E1%84%85%E1%85%B5%2069ff0a06bc714b789d91ac056fa9b39d/Untitled%203.png)

- 파일 시스템의 디렉토리 구조

## 2. 이진 트리 (Binary Tree) 와 이진 탐색 트리(BST)

- 이진 트리 : 부모 노드가 최대 2개까지만 자식 노드를 가지는 트리 구조
- 이진 탐색 트리 : 탐색에 강점을 보이는 이진 트리로, 데이터를 비교해서 정렬 가능하게 저장
  - 값들은 모두 unique 해야 한다 (중복 금지)
  - 부모 노드의 왼쪽 자식 노드는 부모 노드보다 작고
  - 부모 노드의 오른쪽 자식 노드는 부모 노드보다 크다

![Untitled](%E1%84%90%E1%85%B3%E1%84%85%E1%85%B5%2069ff0a06bc714b789d91ac056fa9b39d/Untitled%204.png)

### 2-1. 이진 탐색 트리의 탐색 과정

아래 이진탐색트리에서 30를 찾아나가는 과정은 어떻게 될까?

![Untitled](%E1%84%90%E1%85%B3%E1%84%85%E1%85%B5%2069ff0a06bc714b789d91ac056fa9b39d/Untitled%205.png)

1. 30는 20보다 큰가? (O) -> 우측 자식 노드(28)로 이동 (좌측 절반 날아감) -> 이진 탐색과 유사
2. 30는 28보다 큰가? (O) -> 우측 자식 노드(33)로 이동
3. 30는 33보다 큰가> (X) -> 좌측 자식 노드(30)로 이동 -> 소요된 연산 : 3회

## 3. 이진 탐색 트리 구현

```jsx
class Node {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}
class BinarySearchTree {
  constructor() {
    this.root = null;
  }
}
var tree = new BinarySearchTree();
tree.root = new Node(10);
tree.root.right = new Node(15);
tree.root.left = new Node(7);
tree.root.left.right = new Node(9);
```

### insert : 새 노드를 생성하고 트리에 추가하고 트리를 반환

```jsx
class Node {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}
class BinarySearchTree {
  constructor() {
    this.root = null;
  }
  insert(value) {
    var newNode = new Node(value);
    if (this.root === null) {
      this.root = newNode;
      return this;
    } else {
      var current = this.root;
      while (true) {
        if (value === current.value) {
          return undefined;
        }
        if (value < current.value) {
          if (current.left === null) {
            current.left = newNode;
            return this;
          } else {
            current = current.left;
          }
        } else if (value > current.value) {
          if (current.right === null) {
            current.right = newNode;
            return this;
          } else {
            current = current.right;
          }
        }
      }
    }
  }
}

//          10
//     5         13
//  2    7    11     16

var tree = new BinarySearchTree();
tree.insert(10); // root
tree.insert(5);
tree.insert(13);
tree.insert(11);
tree.insert(2);
tree.insert(16);
tree.insert(7);
```

반복 or 재귀 중에 한 방법을 사용할 수 있다

- 노드를 생성한다
  - root 노드가 없으면, 새 노드가 root가 된다
  - root 노드가 있으면, 새 노드 값과 대소 비교
    - 새 노드가 root 노드보다 크면, root 우측 노드가 있는지 확인하고
      - 없으면 우측 노드에 위치
      - 있으면 대소 비교를 실시
    - 새 노드가 root 노드보다 작으면, root 좌측 노드가 있는지 확인하고
      - 없으면 좌측 노드에 위치
      - 있으면 대소 비교를 실시

### find : value값을 입력 받아 해당 값과 노드를 반환

```jsx
class Node {
  constructor(value) {
    this.value= value;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }
  insert(value){
    var newNode = new Node(value);
    if(this.root === null){
      this.root = newNode;
      return this;
    } else {
      var current = this.root;
      while(true){
        if(value === current.value){
        return undefined;
        }
        if(value < current.value){
          if(current.left === null){
            current.left = newNode;
            return this;
          } else {
            current = current.left;
          }
        }else if(value > current.value){
					  if(current.right === null){
            current.right = newNode;
            return this;
          } else {
            current = current.right;
          }
        }
      }
    }
  }
  find(value) {
	    if (this.root === null) return undefined;
	    let current = this.root;
	    let found = false;
	    while (current && !found) {
	      if (value > current.value) {
	        current = current.right;
	      } else if (value < current.value) {
	        current = current.left;
	      } else {
	        found = true;
	      }
	    }
	    if (!found) return undefined;
	    return current;
	  }
  }
}
//          10
//     5         13
//  2    7    11     16

var tree = new BinarySearchTree();
tree.insert(10) // root
tree.insert(5)
tree.insert(13)
tree.insert(11)
tree.insert(2)
tree.insert(16)
tree.insert(7)
tree.find(1);
```

### contain : value값을 입력 받아 해당 값이 트리에 있으면 true, 없으면 false를 반환

```jsx
class Node {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }
  insert(value) {
    var newNode = new Node(value);
    if (this.root === null) {
      this.root = newNode;
      return this;
    } else {
      var current = this.root;
      while (true) {
        if (value === current.value) {
          return undefined;
        }
        if (value < current.value) {
          if (current.left === null) {
            current.left = newNode;
            return this;
          } else {
            current = current.left;
          }
        } else if (value > current.value) {
          if (current.right === null) {
            current.right = newNode;
            return this;
          } else {
            current = current.right;
          }
        }
      }
    }
  }

  contain(value) {
    if (this.root === null) return false;
    var current = this.root;
    let found = false;
    while (current && !found) {
      if (value > current.value) {
        current = current.right;
      } else if (value < current.value) {
        current = current.left;
      } else {
        return true;
      }
    }
    return false;
  }
}
//          10
//     5         13
//  2    7    11     16

var tree = new BinarySearchTree();
tree.insert(10); // root
tree.insert(5);
tree.insert(13);
tree.insert(11);
tree.insert(2);
tree.insert(16);
tree.insert(7);
tree.find(1);
```

```jsx
class Node {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }
  insert(value){
    var newNode = new Node(value);
    if(this.root === null){
      this.root = newNode;
      return this;
    } else {
      var current = this.root;
      while(true){
        if(value === current.value){
        return undefined;
        }
        if(value < current.value){
          if(current.left === null){
            current.left = newNode;
            return this;
          } else {
            current = current.left;
          }
        }else if(value > current.value){
					  if(current.right === null){
            current.right = newNode;
            return this;
          } else {
            current = current.right;
          }
        }
      }
    }
  }
  find(val) {
    if (this.root === null) return undefined;
    let current = this.root;
    let found = false;
    while (current && !found) {
      if (value > current.value) {
        current = current.right;
      } else if (value < current.value) {
        current = current.left;
      } else {
        found = true;
      }
    }
    if (!found) return undefined;
    return current;
  }
}
  contain(val) {
    if (this.root === null) return false;
    var current = this.root;
    let found = false;
    while (current && !found) {
      if (value > current.value) {
        current = current.right;
      } else if (value< current.value) {
        current = current.left;
      } else {
        return true;
      }
    }
    return false;
  }
}
//          10
//     5         13
//  2    7    11     16

var tree = new BinarySearchTree();
tree.insert(10) // root
tree.insert(5)
tree.insert(13)
tree.insert(11)
tree.insert(2)
tree.insert(16)
tree.insert(7)
tree.find(1);
```

반복 or 재귀 중에 한 방법을 사용할 수 있다

- root가 없으면 탐색 종료
  - 루트가 있으면 값을 비교
    - 찾던 값이면 탐색 종료
    - 아니면 대소 비교
      - 새 노드 값이 루트 값보다 크면 우측 노드 있는지 확인
        - 우측 노드 없으면 탐색 종료
        - 우측 노드 있으면 우측 노드로 이동해 대소 비교
      - 새 노드 값이 루트 값보다 작으면 좌측 노드 있는지 확인
        - 좌측 노드 없으면 탐색 종료
        - 좌측 노드 있으면 좌측 노드로 이동해 대소 비교

## 4. 이진 탐색 트리의 시간 복잡도

- 삽입 : O(log n) -> best, average
- 탐색 : O(log n)
- log n 의 시간 복잡도는 매우 우수한 편이다!
- 하지만 worst case도 존재한다
  - 예) 한 방향으로만 늘어지는 형태
  ![Untitled](%E1%84%90%E1%85%B3%E1%84%85%E1%85%B5%2069ff0a06bc714b789d91ac056fa9b39d/Untitled%206.png)
  - 복잡도가 깊이만큼 증가 : O(N)

![Untitled](%E1%84%90%E1%85%B3%E1%84%85%E1%85%B5%2069ff0a06bc714b789d91ac056fa9b39d/Untitled%207.png)

![Untitled](%E1%84%90%E1%85%B3%E1%84%85%E1%85%B5%2069ff0a06bc714b789d91ac056fa9b39d/Untitled%208.png)

# 트리 순회 (tree traversal)

- 어떤 트리에서든지 사용할 수 있다 (굳이 이진 트리가 아니어도 상관없음)
- 모든 트리의 노드를 한번 씩 방문하는 방법은? -> 2가지 방법
  - Breadth-first-Search
  - Depth-first-Search

```
//        10
//      6    15
//   3     8     20

```

- 방법 1. BFS (너비 우선 탐색)
  - 트리를 가로질러서 동작이 진행 (형제 노드끼리 수평으로 움직임)
- 방법2. DFS (깊이 우선 탐색)
  - 트리를 수직으로 내려갔다가 다시 올라옴
  - 3가지 방법이 있음

## 1. BFS 너비 우선 탐색

```jsx
BFS() {
  var node = this.root,
          data = [],
          queue = [];
      queue.push(node);

      while(queue.length){
         node = queue.shift();
         data.push(node.value);
         if(node.left) queue.push(node.left);
         if(node.right) queue.push(node.right);
      }
      return data;
  }
```

```jsx
//        10
//      6    15
//   3     8     20
```

10 -> 6 -> 15 -> 3 -> 8 -> 20 순으로 나아감

### 1-1. BFS 구현 (반복문 사용) : 방문한 노드가 담긴 배열 리턴

- 큐 생성 (배열도 가능) 및 방문한 노드 저장할 변수 생성
- 루트 노드를 큐에 배치한다
- 큐에 값이 있을 동안 아래를 반복한다
  - 큐에서 노드를 dequeue하고 노드의 값을 마지막에 출력할 변수(배열)에 저장
  - dequeue된 노드에 왼쪽 값이 있는지 확인하고 있으면 큐에 넣는다
  - dequeue된 노드에 오른쪽 값이 있는지 확인하고 있으면 큐에 넣는다
- 루프가 끝나면 모든 값을 저장한 변수를 리턴

## 2. DFS 깊이 우선 탐색

```
//        10
//      6    15
//   3     8     20

```

- 3가지 탐색으로 나눌 수 있음
  - 전위 탐색 : [10, 6, 3, 8, 15, 20]
  - 후위 탐색 : [3, 8, 6, 20, 15, 10]
  - 중위 탐색 : [3, 6, 8, 10, 15, 20]

### 2-1. 전위 탐색 - pre order (재귀함수 사용) : 방문한 노드가 담긴 배열 리턴

```jsx
DFSPreOrder(){
        var data = [];
        function traverse(node){
            data.push(node.value);
            if(node.left) traverse(node.left);
            if(node.right) traverse(node.right);
        }
        traverse(this.root);
        return data;
    }
```

- 깊이 상관없이 제일 왼쪽 먼저, 오른쪽은 그 다음
- 방문할 노드 저장할 변수 생성
- 루트 노드를 current 변수에 할당
- 노드를 인수로 받는 helper 함수(traverse) 생성
  - 노드들을 저장할 변수(배열)를 만들고 노드를 push한다
  - 노드에 왼쪽 값이 있으면 헬퍼함수를 재귀호출(왼쪽 노드를 인수로 받음)
  - 노드에 오른쪽 값이 있으면 헬퍼함수를 재귀호출(오른쪽 노드를 인수로 받음)
- data를 리턴
- left를 먼저 leaf까지 순회하며 내려갔다가 right를 순회하며 root로 올라옴

### 2-2. 후위 탐색 - post order

```jsx
DFSPostOrder(){
        var data = [];
        function traverse(node){
            if(node.left) traverse(node.left);
            if(node.right) traverse(node.right);
            data.push(node.value);
        }
        traverse(this.root);
        return data;
    }
```

- 깊이 상관없이 제일 왼쪽 먼저, 오른쪽은 그 다음
- 루트에서 시작은 하지만, 자식 노드가 있으면 leaf까지 쭉 내려감
- 자식 노드를 다 돌면 부모 노드로 올라오는데 부모 노드를 바로 확인하지 않고 오른쪽 노드로 내려감
- 오른쪽 노드까지 마저 다 보면, 부모 노드로 올라옴
- 그래서 루트 노드를 가장 마지막에 방문
- 구현 (pre order 에서 helper 함수 내부 data push 순서만 다름)
- 방문할 노드 저장할 변수(data) 생성
- 루트 노드를 current 변수에 할당
- 노드를 인수로 받는 helper 함수(traverse) 생성
  - 노드에 왼쪽 값이 있으면 헬퍼함수를 재귀호출(왼쪽 노드를 인수로 받음)
  - 노드에 오른쪽 값이 있으면 헬퍼함수를 재귀호출(오른쪽 노드를 인수로 받음)
  - 노드값을 data에 push
- current를 인수로 traverse 재귀 호출

### 2-3. 중위 탐색 - in order

```jsx
DFSInOrder(){
        var data = [];
        function traverse(node){
            if(node.left) traverse(node.left);
            data.push(node.value);
            if(node.right) traverse(node.right);
        }
        traverse(this.root);
        return data;
    }
```

- 깊이 상관없이 제일 왼쪽 먼저, 오른쪽은 그 다음
- 루트에서 시작하지만, 자식 노드가 있으면 leaf까지 쭉 내려감
- 제일 왼쪽 leaf 방문 후 부모 노드 방문, 그 다음 우측 노드 있으면 우측 노드 방문
- 후위 탐색과 비슷하지만 우측 노드로 넘어가기 전에 부모 노드 방문하는 점이 다름

### 2-4. 전위 vs 후위 vs 중위

- 3가지 DFS는 helper 함수의 일부분만 다르고 나머지는 똑같다
  - 전위 : 방문한 노드를 data 배열에 저장하는 구문이 left 노드 확인, right 노드 확인 앞에 위치
  - 후위 : left, right 노드 뒤에 위치
  - 중위 : left 다음, right 전에 위치

## BFS vs DFS 언제 쓰는가?

- BFS는 큐를 사용하게 된다 (자식 노드를 먼저 방문하지 않음)
- 그래서 트리의 Depth가 깊어지면 큐에 필요한 메모리가 증가하게 된다 (공간 복잡도 증가)

### 한 쪽으로 치우친 트리

- BFS를 사용하면 큐에 저장할 데이터가 적어서 부담이 덜하다

### 노드가 수평으로 길게 구성된 트리

- BFS를 사용하면 큐에 엄청난 데이터가 들어가야 함
- DFS를 사용하면 leaf까지 한번씩 쓱 훑으면 되기 때문에 유리

### 같은점

- 시간 복잡도는 동일 (모든 노드 방문)

### DFS 내부에서 구분

- 용도에 따라 어떤 알고리즘을 쓰라고 규정된 경우는 거의 없다
- 중위 탐색의 경우 오름차순이라는 강점이 있다
