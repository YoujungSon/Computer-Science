
# 그래프

## 1. 그래프란?

- 유한하고 변할 수 있는 정점 또는 노드, 점들의 집합으로 구성된 자료구조
- 무방향 그래프 (꼭지점들의 집합에 순서가 없는 경우)
- 방향 그래프 (꼭지점들의 집합에 순서가 있는 경우)

### 1-1. 그래프 소개

- SNS에서 친구 추천 기능 구현을 위해 그래프가 쓰임
- 노드나 노드들의 연결(커넥션)들의 모임
- 트리(Tree)는 최소 2개의 정점이 하나의 경로로 연결된 그래프

### 1-2. 그래프 활용 예시

- 소셜 네트워크
- 지도 기능, 길 찾기
- 추천 알고리즘
- 라우팅 알고리즘 (네트워크)
- 파일 시스템 최적화

### 1-3. 그래프 용어 정리

1. 정점 (Vertex) : node와 같은 의미
2. 간선 (Edge) : 노드간 연결
3. 방향 그래프 / 무방향 그래프
    
    
    | 방향 그래프 (Directed Graph) | 무방향 그래프 (Undirected Graph) |
    | --- | --- |
    | 간선이 방향을 가진다 | 방향이 없기 때문에 가령 A와 B의 연결이 있을 때, A->B, A<-B 둘 다 가능하다 (A<->B) |
    | 인스타그램 팔로우 관계 | 페이스북 친구 관계 |
    
    ![image](https://user-images.githubusercontent.com/88040809/209317700-d2d72b90-9ee6-4479-930b-26754e3479d1.png)
    
4. 가중 그래프(Weighted Graph)
    
    ![image](https://user-images.githubusercontent.com/88040809/209317760-4d5bc5d0-a9b1-4067-9ecf-a8a41d66da57.png)
    
    - 간선에 숫자 값이 있음
    - 정점이 아닌 간선 그 자체에 정보가 생김
    - 지도 정점 사이 km (무방향)
    - 인스타그램 팔로우 관계를 예를 들면, A가 B 관련 게시물에 댓글이나 좋아요를 많이 할수록 A->B 가중치가 높아진다
5. 비가중 그래프(Unweighted Graph)
    - 간선에 숫자 값이 없음

## 2. 그래프 표현 방법

### 2-1. 인접 행렬(Adjacency Matrix) 방법

```jsx
//   A B C D E F
// A 0 1 0 0 0 1
// B 1 0 1 0 0 0
// C 0 1 0 1 0 0
// D 0 0 1 0 1 0
// E 0 0 0 1 0 1
// F 1 0 0 0 1 0

// A~B 정점 간의 연결이 있으면 1, 없으면 0
```

- 그래프 관계를 행렬로 나타낼 수 있다.
    - boolean 기반 (0, 1)으로 연결 관계를 2차원 배열 형태로 표시

### 2-2. 인접 리스트(Adjacency List) 방법

```jsx
[
  [1, 5], // 정점 0과 연결된 정점들
  [0, 2], // 정점 1과 연결된 정점들
  [1, 3],
  [2, 4],
  [3, 5],
  [0, 4]
]
```

- 그래프 관계를 이차원 배열로 나타낼 수 있다.
    - 인덱스 0에는 정점 0과 연결된 정점들이 배열에 들어가 있음
- 위 그래프처럼 정점들이 숫자가 아닌 **문자열**이라면 해시 테이블을 이용한 인접 리스트로 저장할 수 있다.
    - 만약 아래 예시에서 C노드와 다른 노드들 간 간선을 확인하려면, 인접리스트의 ‘C’ 키에 있는 `['B', 'D']` 밸류를 통해 C 노드는 B노드 및 D노드와 연결되어 있다.
    
    ![image](https://user-images.githubusercontent.com/88040809/209317804-6d87e360-299f-4899-8de7-8afaa0ccb1c6.png)
    

### 2-3. 인접 리스트의 시간 복잡도

- V : 정점(Vertex)의 갯수
- E : 간선(Edge)의 갯수

| 작업 | 인접 리스트 | 인접 행렬 |
| --- | --- | --- |
| 정점 추가 | O(1) | O(V^2) |
| 간선 추가 | O(1) | O(1) |
| 정점 제거 | O(V+E) | O(V^2) |
| 간선 제거 | O(E) | O(1) |
| 쿼리(탐색) | O(V+E) | O(1) |
| 저장공간 | O(V+E) | O(V^2) |

| 인접 리스트 | 인접 행렬 |
| --- | --- |
| 공간을 덜 차지한다. | 공간을 더 많이 차지한다. |
| 빠르게 모든 간선을 순회할 수 있다. | 간선을 확인하려면 모든 간선 순회 필요 (느림) |
| 특정 간선을 찾는 것은 느림 | 특정 간선을 찾는 것은 빠르다. |
| 밀집도가 낮은 데이터를 다룰 때 | 밀집한 데이터를 다룰 때 좋다. |

## 3. 인접 리스트 그래프 구현

무방향 그래프를 다룬다 (방향 그래프로 바꾸는 것 용이함)

### 3-1. 메서드

- 전체 코드
    
    ```jsx
    class Graph {
      constructor() {
        this.adjacencyList = {}
      }
    
      addVertex(vertex) {
        if (!this.adjacencyList[vertex]) this.adjacencyList[vertex] = [];
      }
    
    	addEdge(vertex1, vertex2) {
    	  this.adjacencyList[vertex1].push(vertex2);
    	  this.adjacencyList[vertex2].push(vertex1);
    	}
    
      removeEdge(vertex1, vertex2) {
        this.adjacencyList[vertex1] = this.adjacencyList[vertex1].filter(v => v !== vertex2);
        this.adjacencyList[vertex2] = this.adjacencyList[vertex2].filter(v => v !== vertex1);
      }
    
      // removeVertex(name) {
      //   for (let v of this.adjacencyList[name]) {
      //     this.removeEdge(v, name);
      //   }
      //   delete this.adjacencyList[name];
      // }
    
    	removeVertex(vertex) {
    	// 정점과 연결된 모든 간선을 제거하기 위해 해당 정점의 배열에 대해 루프를 돌면서,
    	    while (this.adjacencyList[vertex].length) {
    	// 삭제하려는 정점의 간선들을 removeEdge 메서드를 사용해서 모두 삭제한다.
    	      const adjacentVertex = this.adjacencyList[vertex].pop();
    	      this.removeEdge(vertex, adjacentVertex);
    	    }
    	// 정점도 삭제한다.
    	    delete this.adjacencyList[vertex];
    	  }
    
    // 도시를 정점으로, 간선을 직항편으로
    const g = new Graph();
    g.addVertex('Dallas');
    g.addVertex('Seoul');
    g.addVertex('Chicago');
    ```
    

### addVertex : 정점의 이름을 인수로 받아 정점을 생성한다.

```jsx
  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) this.adjacencyList[vertex] = [];
  }
```

- vertex 인수를 받는다 (정점의 이름)
- 인접 리스트에 key: vertex, value: 빈 배열 형태로 추가한다

![image](https://user-images.githubusercontent.com/88040809/209317860-82759ff6-114a-4c41-a54a-dc45adc89ccb.png)

### addEdge : 정점 2개를 입력받아 간선을 생성한다

```jsx
addEdge(vertex1, vertex2) {
	  this.adjacencyList[vertex1].push(vertex2);
	  this.adjacencyList[vertex2].push(vertex1);
	}
```

- vertex1, vertex2를 인수로 받는다
- 인접 리스트에서 vertex1 키의 밸류(배열)에 vertex2를 추가한다.
- 인접 리스트에서 vertex2 키의 밸류(배열)에 vertex1를 추가한다.

![image](https://user-images.githubusercontent.com/88040809/209317903-5facf7e4-d6b8-43e8-bd7c-9db7cd86de95.png)

### removeEdge : 정점 2개를 입력 받아 그 간선을 제거한다

```jsx
removeEdge(vertex1, vertex2) {
    this.adjacencyList[vertex1] = this.adjacencyList[vertex1].filter(v => v !== vertex2);
    this.adjacencyList[vertex2] = this.adjacencyList[vertex2].filter(v => v !== vertex1);
  }
```

- vertex1, vertex2를 인수로 받는다.
- vertex1 키의 밸류값을 vertex2가 존재하지 않는 배열로 재할당한다.
- vertex2 키의 밸류값을 vertex1가 존재하지 않는 배열로 재할당한다.

![image](https://user-images.githubusercontent.com/88040809/209317924-f2e505f8-e07c-439b-a700-010045aef064.png)

### removeVertex : 제거할 정점을 입력받고 정점을 제거한다.

```jsx
removeVertex(vertex) {
	// 정점과 연결된 모든 간선을 제거하기 위해 해당 정점의 배열에 대해 루프를 돌면서,
	    while (this.adjacencyList[vertex].length) {
	// 삭제하려는 정점의 간선들을 removeEdge 메서드를 사용해서 모두 삭제한다.
	      const adjacentVertex = this.adjacencyList[vertex].pop();
	      this.removeEdge(vertex, adjacentVertex);
	    }
	// 정점도 삭제한다.
	    delete this.adjacencyList[vertex];
	  }
```

- 제거할 정점 이름을 입력 받는다.
- 정점만 제거하는 걸로 끝나지 않고, 정점과 관련된 모든 간선도 제거해야 한다.
    - 인접 리스트의 모든 정점을 순회하며 간선이 있는 경우 removeEdge 함수를 실행한다.

![image](https://user-images.githubusercontent.com/88040809/209317951-7d8c83fd-0a57-4d1d-b18e-5a2a21ea89ee.png)
