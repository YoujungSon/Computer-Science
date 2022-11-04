### 큐(queue)

FIFO(First In First Out) 형태로 양쪽 끝에서 데이터의 삽입과 삭제가 각각 이루어지는 자료구조

- `rear` : 데이터가 삽입되는 곳
- `front` : 데이터가 제거되는 곳
- 데이터를 삭제하기 전에는 큐가 `empty`한지, 큐에 데이터를 추가하려 할 때는 큐가 `full`인지 확인 후 진행해야 한다.

```javascript
class Queue {
  constructor() {
    this._arr = [];
  }
  enqueue(item) {
    this._arr.push(item);
  }
  dequeue() {
    return this._arr.shift();
  }
}

const queue = new Queue();
queue.enqueue(1);
queue.enqueue(2);
queue.enqueue(3);
queue.dequeue(); // 1
```

- **선형 큐 Linear Queue**
    
    
    ![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f8565c0a-8708-4823-83a5-de8c5018531d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221029T103057Z&X-Amz-Expires=86400&X-Amz-Signature=fae5ac80bf940ae75556122dc690f4e3b205ef6b087989c0de4dd39468c1feb8&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)
    
    - 선형 배열을 사용하여 구현된 큐
    - 삽입을 위해서는 계속해서 요소들을 이동시켜야 함
    - `front`, `rear` 는 증가만 하는 방식, 실제로는 `front` 앞쪽에 공간이 있더라도 삽입할 수 없는 경우가 발생할 수 있음
- **원형 큐 Circular Queue**
    
    ![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0cbc87fb-b883-4813-bf22-9306d74be66a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221029T103144Z&X-Amz-Expires=86400&X-Amz-Signature=9cb3d6790809bc47d3c27627bfb8fafe309809ff77e898b9632bbed0dfa4e924&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)
    
    - 선형 큐의 단점을 보완
    - `front` = 맨 첫번째 요소 바로 앞을 가리킴
    - `rear` = 마지막 요소 가리킴
    - 큐 empty 상태 : `front == rear`
    - 큐 full 상태 : `front == (rear+1) % MAX_QUEUE_SIZE`
    - 공백 상태와 포화 상태를 구분하기 위해 하나의공간을 비워둠

**큐 주요 동작**

- enqueue : 큐에 아이템을 추가한다. (JS에서 push)
- dequeue : 큐 맨 앞의 아이템을 제거하고 및 그 아이템을 반환한다. (JS에서 shift)
- contains : 해당 아이템이 큐에 존재하는지 확인한다.
- size : 현재 큐에 있는 아이템의 총 개수를 반환한다.

**큐 활용 예시**

- 우선순위가 같은 작업 예약 (프린터의 인쇄 대기열)
- 은행 업무
- 콜센터 고객 대기시간
- 프로세스 관리
- 너비 우선 탐색(BFS, Breadth-First Search) 구현
- 캐시(Cache) 구현
- 데이터를 입력된 순서대로 처리해야 할 때
- BFS 알고리즘
