
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

# 그래프 순회(Graph Traversal)

### 그래프

![image](https://user-images.githubusercontent.com/88040809/210062722-b68f2741-2076-4c3b-a235-28f11e2bbdec.png)

1. 순회를 하면 그래프에 있는 모든 노드(정점)을 방문하지만 보통 실생활에서 적용을 하는 경우에는 다른 특이한 경우들이 더 많다. 
2. 그래프를 순회하는 코드를 짤 때는 시작점을 정해주어야 한다. 
3. 그래프의 한 노드에서 다른 노드로 갈 때에는 유일한 하나의 길만이 있다는 보장은 없다.
여러 가지 순서가 있을 수도 있고 아니면 뒤로 다시 돌아가거나 이미 방문한 노드를 다시 방문해야 할 수도 있다.
4. 루트가 없기 때문에 순회하기 전 어디서 시작해야 하는지 모른다는 문제가 생긴다. 
기준은 다를 수 있지만 어느 정점에서든 시작할 수 있다.

### 그래프 사용 예시

- 네트워크
- 토렌트
- 웹 크롤러 (모든 링크를 하나에서 시작해서 따라감)
    - 일단 한 사이트에 있는 모든 링크를 따라가서 계속 내보낸다.
- 추천 알고리즘
- 인공지능
- 틱택토, 스크래블(게임을 이기기 위한 가장 짧은 경로)
- GPS 네비게이션

### **순회하는 방법 예시**

- 가장 가까운, 또는 비슷한 노드를 찾는 것
- 한 정점에서 다른 정점으로 가는 최단 경로를 찾는 것. 예) GPS 안내
- 트리 (한 노드에서 다른 노드로 가는 경로가 하나만 있는 그래프)
    - 트리
        
        ![image](https://user-images.githubusercontent.com/88040809/210062737-19d4a479-5ad4-482c-ad30-be4aabd7b569.png)
        
    - 모든 트리는 그래프지만 트리가 아닌 그래프는 아주 많다.
    - 가끔 루트부터 위로 올라오는 방식으로 트리를 그리기도 하지만 그리는 방식에 관계 없이 트리에는 확실한 경로가 존재한다.
    - 루트가 있기 때문에 루트에서부터 오르거나 내리거나 하는 수 밖에 없고 무엇을 하든 해당 루트를 사용하게 된다.
- 기본 순회 접근법에서 비롯된 더 어려운 알고리즘
    - 방문, 최신화, 확인, 탐색, 출력 등을 그래프의 모든 정점에 대해 수행하는 것
- 방향 그래프
    
    ![image](https://user-images.githubusercontent.com/88040809/210062764-ca82baf2-8b57-4612-877a-cee1a385a404.png)
    
    - 간선(연결)들 각각에 결부된 방향(단방향)이 있다.
    

![image](https://user-images.githubusercontent.com/88040809/210062791-c5322d92-5726-4d76-9247-e64db9d0ede6.png)

트리에서 이진 탐색 트리나 이진 트리 일반에 대해 많은 시간을 쓴 부분이 있었는데 트리를 순회하는 방법에는 너비와 길이 두 가지 선택지가 있습니다.

그리고 깊이 우선 탐색에는 전위, 후위, 정위가 있습니다.

## 깊이 우선 탐색(DFS)

1. DFS(깊이우선탐색)은 다른 형제를 방문(backtracking)하기 전에 한 브랜치에서 가능한 가장 아래까지 깊이 탐색한다.
2. DFS와 BFS 모두, 매개변수로 시작점을 받고 순회하면서 방문한 정점을 추적한다는 점들은 같다.
3. 형제 트리를 방문하기 전에 자식 노드를 먼저 방문하는 것
4. 탐색의 범위를 옆으로 늘리기 전에 아래로 먼저 늘린다.
5. 한 인접점을 찾고 이어서 인접점을 또 찾는다.
6. 결론적으로 깊이 우선은 루트에서 멀어진다는 뜻이다.

### 예시 1) 방향 그래프

![image](https://user-images.githubusercontent.com/88040809/210062813-e10e8db8-3e12-4e9a-b018-0b0216df5e80.png)

### 깊이 우선 (0을 루트로 하고 가장 작은 값 기준)

0 - 1 - 5 - 1 - 6 - 7 - 9

여기서 형제라고 하면 트리와는 달리 반드시 같은 레벨에 있을 필요가 없다.

### 예시 2) 무방향 그래프

![image](https://user-images.githubusercontent.com/88040809/210062830-f6996ec2-161b-4733-b470-08cdb8cec6ea.png)

기본 값으로 시작하도록 정해져 있는 루트 노드는 없지만 여기서는 A에서 시작하는 깊이 우선 탐색을 실행해보자.
다시 원래로 돌아가기 전에 가능한 끝까지 이어서 가 보고 싶다고 했을 때, 먼저 요소를 방문할 순서를 정해야 한다.

### 깊이 우선 ( 루트를 A로 하고 알파벳 순서를 기준)

A - B - C - D(이미 방문한 B를 기억, 그렇지 않으면 다시 B로 감) - E- F

알파벳 순서처럼 딱 떨어지는 기준이 있는 것이 좋다.

방문했던 곳을 기억하는 것은 정말 중요하다. 방문했던 사실을 기억해두지 않는다면 A에서 B로 갔다가 다시 A로 가게 된다. 알파벳 순서로 선택을 하고 가장 빠른 알파벳으로 간다면 A와 B에 계속 멈춰 있을 것이다.

### 너비 우선

A - B - A - E 

### 예시 3) 무방향 그래프

![image](https://user-images.githubusercontent.com/88040809/210062861-94d78ce2-120c-4169-8bba-cf72f28c7f55.png)

### 깊이 우선 (A에서 시작 알파벳 순서)

A - B - D - E - C - E - F

정점은 다 그대로지만 간선들이 다른 그래프가 있다. 
세 개의 간선이 있는데 이미 D에 다녀왔다는 것을 알고 있다. 
그다음 알파벳 순서로 하려면 C와 F중 C로 가야 하고 C를 보면 다시 막다른 길이다. 
A도 다녀왔고 E도 다녀왔으면 다시 E로 가서 E는 이미 끝냈으니까 F로 간다.

### 예시 4) 무방향 그래프 + 연결 리스트

![image](https://user-images.githubusercontent.com/88040809/210062888-e1f4d342-3f32-4108-8b58-49cf37ca54ec.png)

```jsx
{
"A":["B","C"],
"B":["A","D"],
"C":["A","E"],
"D":["B","E","F"],
"E":["C","D","F"],
"F":["D","E"],
}
```

같은 그림이지만 인접리스트로 A가 B, C하고 연결되어 있음을 볼 수 있게 했다.

리스트는 알파벳 순서로 정리되어 있다. 그러므로  A와 간선 중 B가 C보다 먼저이다.

```jsx
{
"A":["B","C"],
"B":[~~"A"~~,"D"],
"C":[~~"A"~~,"E"],
"D":["B","E","F"],
"E":["C","D","F"],
"F":["D","E"],
}
```

A에서 시작을 해서 A를 방문한다. 표시를 하고 A가 인접점으로 있을 때는 그걸 무시해야 한다.

A를 보면 두 개의 간선, 즉 두 개의 인접 정점이 있다. 그러면 B나 C로 갈 수가 있는데 첫 번째 것인 B를 먼저 선택한다. 

```jsx
{
"A":[~~"B"~~,"C"],
"B":[~~"A"~~,"D"],
"C":[~~"A"~~,"E"],
"D":[~~"B"~~,"E","F"],
"E":["C","D","F"],
"F":["D","E"],
}
```

그리고 B를 방문한 다음, B에도 표시를 한다. 그렇다고 삭제를 하는 것은 아니다. 
방문했다는 사실을 의미한다. 
그러니까 다음에 보게되면 무시하면 된다. 

이제 B를 보면 두 개의 간선이 있다. A와 D이다 그렇지만 A는 이미 다녀와서 표시가 되어 있다.

```jsx
{
"A":[~~"B"~~,"C"],
"B":[~~"A"~~,~~"D"~~],
"C":[~~"A"~~,"E"],
"D":[~~"B"~~,"E","F"],
"E":["C",~~"D"~~,"F"],
"F":[~~"D"~~,"E"],
}
```

그럼 이제 D로 가서 D는 방문을 했으니 다시 표시한다. 세 개의 간선이 있으니까 세 개 다 D에 표시를 한다.

이제 D를 보면 세 개의 선택지가 있다. 

```jsx
{
"A":[~~"B"~~,"C"],
"B":[~~"A"~~,~~"D"~~],
"C":[~~"A"~~,~~"E"~~],
"D":[~~"B"~~,~~"E"~~,"F"],
"E":["C",~~"D"~~,"F"],
"F":[~~"D"~~,~~"E"~~],
}
```

그렇지만 B에는 이미 갔기 때문에 다음 E로 간다. 그리고 모든 E에 표시를 한다.

```jsx
{
"A":[~~"B"~~,~~"C"~~],
"B":[~~"A"~~,~~"D"~~],
"C":[~~"A"~~,~~"E"~~],
"D":[~~"B"~~,~~"E"~~,"F"],
"E":[~~"C"~~,~~"D"~~,"F"],
"F":[~~"D"~~,~~"E"~~],
}
```

E에서 이번에는 C로 간다. 그리고 모든 C에 표시를 한다. 그리고 마지막으로 있던 곳으로 간다. 
C는 이미 방문했고, D도 이미 방문했고 이제 F를 방문하면 끝이다.

이 것이 깊이 우선 탐색의 기본이다.아래로 내려간다고 말하기가 어려운데 그래프에서는 깊이가 의미하는 것이 무엇인지 정확히 눈으로 보기가 어렵다.

트리에서는 그냥 쉽게 루트에서 멀어지는 방식으로 움직였지만 이 경우에는 그냥 루프로 보인다.

그래서 깊이라는 것을 설명하기 어려웠던 것 같다.

실제 의미는 인접점을 따라가고 따라가서 길이 막힐 때까지 가는 것이다.

탐색이라고 했지만 실제로는 순회를 하게 된다. 기본적으로 같은 말이지만 무언가를 찾는지 여부가 다르다. 결국 모든 노드(정점)을 다 방문해야 한다.

```jsx
DFS(정점) : 

if(정점이 없으면)
  return (기본 케이스)

결과에 정점 추가

방문했다는 표시

정점의 인접점의 각 인접점에 대해:

  인접점을 방문하지 않은 경우:

    인접점에서 DFS를 재귀적으로 호출
```

일단 함수를 정의하고 트리처럼 자동으로 시작하는 지점이 있는 것이 아니기 때문에 시작할 때는 정점 하나를 입력한다. 재귀 방식으로 코딩을 할 때는 기반 조건이 필요하다.

첫번째의 경우에는 기반조건이 충족되지 않는다. 시작점을 입력할 것이 때문이다. 
기반 조건이 충족되면 null 같은 것을 return한다.

조건이 충족되지 않으면 결과로 돌려준 리스트가 나온다.

정점을 방문할 때마다 해당 정점을 results 리스트에 추가하면서 맨 마지막에 돌려줄 배열을 만든다.

```jsx
{
  "A":true,
  "B":true,
  "D":true,
}
```

그리고 이런 식으로 키-값 쌍을 가지고 해당 정점을 방문했다고 표시해야 한다. 

그 다음에는 해당 정점에 있는 인접점들을 본다.

A의 모든 인접점에 대해 해당 인접점을 방문하지 않았다면 재귀 방식으로 DFS를 해당 인접점에 대해 호출한다.

그러면 DFS를 B와 D 두 인접점에 대해 호출하게 된다. 그 다음 각각에 대해 똑같이 해주면 된다.

루프에 걸리지 않도록 results에 리스트에 추가하고 방문했다고 표시한다.  

그리고 각 인접점을 방문한다.

이걸 재귀 방식으로 하게되면 호출 스택에 호출들이 쌓이게 된다.

![image](https://user-images.githubusercontent.com/88040809/210062919-68ea261f-ecde-4d98-8678-5ab481cc4fcb.png)

```jsx
{
"A":["B","C"],
"B":["A","D"],
"C":["A","E"],
"D":["B","E","F"],
"E":["C","D","F"],
"F":["D","E"],
}
```

```jsx
{
  "A":true,
}
```

정점 A를 방문하고 객체(키-값)에 추가한다.

그 다음은 A의 인접점을 확인하고 이미 방문했는지 확인한다.

방문한적이 없다면 재귀형으로 깊이 우선 탐색을 호출한다.

B의 인접점을 모두 확인한 다음 C로 돌아간다.

C는 호출 스택에서 기다리고 있다.

B를 방문한 다음 리스트와 방문 정점 객체에 추가해 준다.

B는 두개의 인접점이 있다. 각각에 대해 이미 방문을 했는지 확인한다.

A는 이미 방문을 했고 재귀형으로 호출하지는 않는다. D는 방문하지 않았다.

이미 방문한 정점들을 추적하는 것이 핵심이다.

각 회차마다 정점이 가지고 있는 각 인접점마다 이미 방문을 했었는지 확인을 한다.

아직 방문을 하지 않았다면 재귀식으로 깊이 우선 탐색을 호출해준다.

```jsx
g.addVertex("A")
g.addVertex("B")
g.addVertex("C")
g.addVertex("D")
g.addVertex("E")
g.addVertex("F")

g.addEdge("A","B")
g.addEdge("A","C")
g.addEdge("B","D")
g.addEdge("C","E")
g.addEdge("D","E")
g.addEdge("D","F")
g.addEdge("E","F")
```

재귀함수는 자기 자신을 다시 호출하고 메소드 재귀함수는 데이터가 일부 유지되어야 하는 경우 사용한다.

헬퍼함수는 정점을 입력하게 되어 있다. 정점이 비어있으면 끝이다.

헬퍼 함수는 입력한 정점을 방문 객체에 넣어야 한다.

방문했다는 표시를 하고 결과 배열에 push해준다.

이 결봐 배열은 마지막에 출력한다. 그리고 나면 해당 정점의 모든 인접점에 대해 루프를 돌린다.

### DFS - 재귀형 코드

- depthFirstRecursive 메서드 코드

```jsx
// 순회를 시작할 정점 노드를 인수로 받는다.
depthFirstRecursive(start) {
  // 결과를 저장할 빈 배열 result를 만든다(마지막에 이것을 return할 것이다)
  const result = [];
  // 방문한 정점들를 기록할 빈 객체 visted를 만든다.
  const visited = {};
  // const adjacencyList = this.adjacencyList;
  const { adjacencyList } = this;
  // 재귀를 도울 헬퍼 함수를 정의하며, start를 인수로 받아 즉시 실행한다.
  (function dfs(vertex) {
    // 만약 정점이 비었다면 return 한다.
    if (!vertex) return null;
    // 방문한 정점을 visited 객체에 true로 기록한다.
    visited[vertex] = true;
    // 방문한 정점을 result 배열에 추가한다.
    result.push(vertex);
    // 방금 방문한 정점과 간선으로 이어진 노드 중에서 아직 방문하지 않은 노드들에 대하여
    // 본 헬퍼 함수를 재귀한다.
    adjacencyList[vertex].forEach(neighbor => {
      if (!visited[neighbor]) {
        return dfs(neighbor);
      }
    });
  })(start);

  // 순회한 정점이 순서대로 담긴 배열 result를 return한다.
  return result;
}
```

- 메서드 실행 결과 확인

```jsx
const g = new Graph();

g.addVertex("A");
g.addVertex("B");
g.addVertex("C");
g.addVertex("D");
g.addVertex("E");
g.addVertex("F");

g.addEdge("A", "B");
g.addEdge("A", "C");
g.addEdge("B", "D");
g.addEdge("C", "E");
g.addEdge("D", "E");
g.addEdge("D", "F");
g.addEdge("E", "F");

// 위와 같은 그래프에서 A부터 순회를 시작하도록 depthFirstRecursive('A') 메서드를 실행하면, 마지막 줄과 같이 예상한대로 순회했음을 확인할 수 있다.
console.log(g.depthFirstRecursive("A"));

// [ 'A', 'B', 'D', 'E', 'C', 'F' ]
```

### DFS - 순환형 코드

- depthFirstIterative 메서드 코드

```jsx
depthFirstIterative(start) {
  // stack 배열을 정의하고, start 노드를 추가한다.stack은 반복문에서 조건으로 사용할 것이다.
  const stack = [start];
  // 결과를 저장할 빈 배열 result를 만든다(마지막에 이것을 return할 것이다)
  const result = [];
  // 방문한 정점들를 기록할 빈 객체 visted를 만든다.
  const visited = {};
  let currentVertex;
  // visited 객체에 start 노드는 방문했음을 기록한다.
  visited[start] = true;

  // stack이 비어 있지 않은 한, 무한 루프를 돌린다.
  while (stack.length) {
    console.log("stack", stack);  // 이해하기 위해 콘솔
    // 여기서는 pop을 이용하여 간선 배열의 끝에서부터 정점을 꺼내어 방문한다.
    // stack에서 가장 최근에 들어온 노드를 하나 꺼내어(pop), currentVertex 변수에 할당한다.
    currentVertex = stack.pop();
    // 지금 방문한 정점을 result 배열의 마지막에 추가(push)한다.
    result.push(currentVertex);
    // 지금 방문한 정점과 간선으로 이어진 정점 중
    this.adjacencyList[currentVertex].forEach(neighbor => {
      // 아직 방문하지 않은 인접점에 대하여
      if (!visited[neighbor]) {
				console.log("neighbor", neighbor);  // 이해하기 위해 콘솔
        // visited 객체에 이제 방문했음을 기록하고
        visited[neighbor] = true;
        // stack 배열(while문 종료 조건)에 push한다.
        // stack에는 이제 start 정점은 없고 그 다음에 방문한 정점 하나가 있게 되며, 다시 while문을 돈다.
        stack.push(neighbor);
      }
    });
  }
  // 반복문이 다 종료된 후 result를 반환한다.
  return result;
}
```

- 코드 실행 결과
    - 코드의 작동 방식 때문에, 재귀형 코드와 달리 순회 순서가 달라지긴 했으나, 두 방법 모두 DFS 방식인 것은 동일하다.

```jsx
const g = new Graph();

g.addVertex("A");
g.addVertex("B");
g.addVertex("C");
g.addVertex("D");
g.addVertex("E");
g.addVertex("F");

g.addEdge("A", "B");
g.addEdge("A", "C");
g.addEdge("B", "D");
g.addEdge("C", "E");
g.addEdge("D", "E");
g.addEdge("D", "F");
g.addEdge("E", "F");

// 위와 같은 그래프에서 A부터 순회를 시작하도록 depthFirstIterative('A') 메서드를 실행하면, 마지막 줄과 같이 순회했음을 확인할 수 있다.

console.log(g.depthFirstIterative("A"));

// [ 'A', 'C', 'E', 'F', 'D', 'B' ]

```

```jsx
stack [~~'A'~~]
neighbor B
neighbor C
stack ['B',~~'C'~~]
neighbor E
stack ['B',~~'E'~~]
neighbor D
neighbor F
stack ['B','D',~~'F'~~]
stack ['B',~~'D'~~]
stack [~~'B'~~]
['A','C','E','F','D','B']
```

## BFS(너비우선탐색)

- BFS(너비우선탐색)은 현재 같은 높이(height)의 인접점들을 먼저 방문하는 알고리즘이다.
- 아래 코드에 주석을 달긴 했지만, BFS 순환형 코드는 DFS 순환형 코드와 거의 비슷한다.
    - 다른 점은 while 조건문에서 순회하는 개념으로 stack 대신 queue를 사용하고 이를 위해 pop 메서드 대신 shift 메서드를 쓴다는 것이다.

```jsx
breadthFirst(start) {
  const queue = [start];
  const result = [];
  const visited = {};
  let currentVertex;
  visited[start] = true;

  while (queue.length) {
  console.log("queue", queue);  // 이해하기 위해 콘솔
    // 여기서는 shift를 이용하여 간선 배열의 처음부터 정점을 꺼내어 방문한다.
    // queue에서 줄 제일 앞에 서 있는 노드를 하나 꺼내어(shift), currentVertex 변수에 할당한다.
    currentVertex = queue.shift();
    result.push(currentVertex);

    this.adjacencyList[currentVertex].forEach(neighbor => {
      if (!visited[neighbor]) {
       console.log("neighbor", neighbor);  // 이해하기 위해 콘솔
        visited[neighbor] = true;
        queue.push(neighbor);
      }
    });
  }
  return result;
}
```

- 코드 실행 결과

```jsx
const g = new Graph();

g.addVertex("A");
g.addVertex("B");
g.addVertex("C");
g.addVertex("D");
g.addVertex("E");
g.addVertex("F");

g.addEdge("A", "B");
g.addEdge("A", "C");
g.addEdge("B", "D");
g.addEdge("C", "E");
g.addEdge("D", "E");
g.addEdge("D", "F");
g.addEdge("E", "F");

// QUEUE: []
console.log(g.breadthFirst("A"));
// RESULT: [A, B, C, D, E, F]
```

메서드 내부에서 실행된 콘솔로그까지 포함한 코드 실행 결과는 아래 이미지와 같다.

queue에서 제일 앞 정점부터 초록선으로 shift하며 순회하는 모습이다. 지금 방문한 정점과 간선으로 이어진 정점들을 돌리는 forEach문에서 queue에 push되는 정점들이 neighbor 옆에 찍혔다.

```jsx
queue [~~'A'~~]
neighbor B
neighbor C
queue [~~'B'~~,'C']
neighbor D
queue [~~'C'~~,'D']
neighbor E
queue [~~'D'~~,'E']
neighbor F
queue [~~'E'~~,'F']
queue [~~'F'~~]
['A','B','C','D','E','F']
```
