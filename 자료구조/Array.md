# Array (배열)

## 정의 (Array)
배열은 **선형 구조**의 자료형으로, **동일한 데이터 타입**을 가진 요소들을 순차적으로 저장하며, 각 요소는 **인덱스**를 통해 식별된다.  
요소들이 메모리상에 **연속적으로** 배치된다는 특성 때문에, 임의의 인덱스에 접근하는 데 걸리는 시간 복잡도는 **$O(1)$**이다.  

- 인덱스로 접근 가능 → O(1)
- 삽입/삭제는 위치에 따라 O(n)

## 특징
- 고정 크기 배열(Fixed-size)과 동적 배열(Dynamic Array) 존재
- 메모리 연속성을 가짐
- 빠른 접근 속도

## 예제 코드
```javascript
const arr:number[] = [1, 2, 3];

/* 삽입 (push, splice) */
arr.push(4); // 1, 2, 3, 4
arr.splice(2, 0, 2) // 1, 2, 2, 3, 4
arr.unshift(1); // 1, 1, 2, 2, 3, 4

/* 삭제 (pop, splice) */
arr.pop(); // 1, 1, 2, 2, 3
arr.splice(2, 1); // 1, 1, 2, 3
arr.shift(); // 1, 2, 3

/* 인덱스 */
/* arr = 1, 2, 3 */
arr.indexOf(3) // 2
arr.indexOf(4) // -1

/* 순회 */
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]); // 1 2 3
}

/* 배열 메서드 */
arr.forEach((value) => value * 2); // 1 2 3
arr.map((value) => value * 2);
arr.reduce((acc, cur) => acc + cur, 0); // 6
// filter, find, some, every, entries from ... 등
```