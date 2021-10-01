## 03. Stack

#### 1. Stack?
- LIFO (Last in First Out)
- 가장 나중에 쌓은 데이터가 가장 먼저 빼낼 수 있는 데이터 구조
- push()
- pop()

#### 2. 장단점
- 장점
  - 구조가 단순해서 구현쉬움
  - 데이터 저장 읽기 속도가 빠름
- 단점
  - 데이터 최대 갯수를 미리 정해야함
  - 저장공간 낭비가 발생할 수도 있음

ex)
```
Stack<Integer> stack = new Stack<Integer>();
stack.push(1);
stack.pop();
```