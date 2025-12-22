# Queue

## 정의 (Queue)
큐는 **선형 구조**의 자료형으로, 먼저 들어온 데이터가 먼저 나가는 **(FIFO, First In First Out)** 특성을 가진다.  
데이터의 **삽입(enqueue)** 은 **뒤(rear)** 에서, **삭제(dequeue)** 는 **앞(front)** 에서 이루어진다.  

- 먼저 들어온 요소가 먼저 처리됨 (FIFO)
- front → 삭제, rear → 삽입

## 특징
- 순서가 중요한 처리에 적합
- 삽입과 삭제의 위치가 제한 됨
- 배열 또는 연결 리스트로 구현 가능

## 시간 복잡도
- 삽입 (enqueue): **O(1)**
- 삭제 (dequeue): **O(1)**
- 탐색 / 접근: **O(n)**

## 사용 사례
- 작업 대기열 (Task Queue)
- 프린터 출력 대기
- 이벤트 루프, 메시지 큐
- BFS (너비 우선 탐색)

## 예제 코드
```javascript
class Queue {
    constructor() {
        this.items = [];
    }

    /* 삽입 */
    enqueue(value) {
        this.items.push(value);
    }

    /* 삭제 */
    dequeue() {
        // shift: 배열의 첫 번째 요소를 제거하고 해당 요소를 반환
        return this.items.shift();
    }

    /* 맨 앞 요소 확인 */
    peek() {
        return this.items[0];
    }

    /* 큐가 비어있는지 확인 */
    isEmpty() {
        return this.items.length === 0;
    }

    /* 큐 크기 */
    size() {
        return this.items.length;
    }
}

const queue = new Queue();
console.log(queue.items); // []

queue.enqueue(0); // [0]
queue.enqueue(1); // [0, 1]
queue.enqueue(2); // [0, 1, 2]

queue.peek();    // 0
queue.dequeue(); // 0 → [1, 2]
