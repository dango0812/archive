# Stack

## 정의 (Stack)
스택은 **선형 구조**의 자료형으로, 나중에 들어온 데이터가 먼저 나가는 **(LIFO, Last In First Out)** 특성을 가진다.
데이터의 **삽입(push)** 와 **삭제(pop)** 은 모두 **같은 쪽(top)**에서 이루어진다.

- 마지막에 들어온 요소가 먼저 처리됨 (LIFO)
- top → 삽입 / 삭제

## 특징
- 가장 최근 데이터에 빠르게 접근 가능
- 삽입과 삭제의 위치가 하나로 제한됨
- 배열 또는 연결 리스트로 구현 가능

## 시간 복잡도
- 삽입 (push): **O(1)**
- 삭제 (pop): **O(1)**
- 탐색 / 접근: **O(n)**

## 사용 사례
- 실행 취소 / 다시 실행 (Undo / Redo)
- 함수 호출 스택 (Call Stack)
- 괄호 검사, 수식 계산
- DFS (깊이 우선 탐색)

## 예제 코드
```javascript
class Stack {
    constructor() {
        this.items = [];
    }

    /* 삽입 */
    push(value) {
        this.items.push(value);
    }

    /* 삭제 */
    pop() {
        return this.items.pop();
    }
    /* 마지막 요소 확인 */
    peek() {
        return this.items[this.items.length - 1];
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

const stack = new Stack();
console.log(stack.items); // []

stack.push(0); // [0]
stack.push(1); // [0, 1]
stack.push(2); // [0, 1, 2]

stack.pop(); // [0, 1]
```