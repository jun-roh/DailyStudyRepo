## 02. Queue

#### 1. Queue?
- FIFO (First in First Out)
- Enqueue : 큐에 데이터를 넣는것
- Dequeue : 큐에서 데이터를 꺼내는 기능
- 스케줄링 구현을 할 때 많이 사용함

ex)

```
Queue<Integer> queue = new LinkedList<Integer>();
Queue<String> queue_str = new LinkedList<String>();

queue.add(1);   // return true
queue.add(2);
queue.poll();   // return 1
queue.remove(); // return 2
```

- 
- 