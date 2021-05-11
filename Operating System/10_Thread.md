## 10. Thread
#### 1. Thread 란?
* Light Weight Process
* 하나의 프로세스에 여러개 Thread 생성 가능
* Thread 들을 동시에 실행 가능
* Process 안에 있으므로, 프로세스의 데이터를 모두 접근 가능
* Thread 는 각기 실행이 가능한 Stack 존재
  ![Alt text](./images/process_thread.png "Process Thread")

#### 2. Multi Thread(멀티 스레드)
* 소프트웨어 병행 작업 처리를 위해서 multi thread 사용
    ![Alt text](./images/process_thread.png "Process Thread")

#### 3. 멀티 프로세싱과 Thread
* 멀티 태스킹과 멀티 프로세싱
    * 멀티 태스킹 : 하나의 CPU 에 여러 프로세스
    * 멀티 프로세싱 : 하나의 프로세스를 여러개의 CPU 를 사용해서 나눠서 병렬 실행
* 최근 CPU 는 멀티 코어를 가지고 있기 때문에 Thread 을 여러 개 만들어서 멀티 코어 활용도를 높임

#### 4. Thread 장점, 단점
[장점]
1. 사용자에 대한 응답성 향상
   ![Alt text](./images/thread_adv1.png "Thread 장점 1")
    
2. 자원 공유 효율
    * IPC 기법과 같이 프로세스 간 자원 공유를 위해 번거로운 작업 필요 없음
    * 프로세스 안에 있으므로 프로세스의 데이터를 모두 접근 가능

3. 작업이 분리되어 코드가 간결
    * 간결하게 보일수도 있지만 하기 나름
    
[단점]
1. Thread 중 한 스레드만 문제 있어도 전체 프로세스가 영향을 받음
2. Thread 를 많이 생성하면 Context Switching 이 많이 일어나 성능 저하
    * 리눅스 에서는 Thread 를 Process 와 같이 다룸
    * Thread 를 많이 생성하면 모든 Thread 를 스케쥴링해야 하므로 Context Switching 이 빈번할 수 밖에 없음

#### 5. Process VS Thread
* 프로세스는 독립적, 스레드는 프로세스의 서브셋
* 프로세스는 각각 독립적인 자원을 갖음, 스레드는 프로세스 자원 공유
* 프로세스는 자신만의 주소 영역을 가짐, 스레드는 주소 영역 공유
* 프로세스 간에는 IPC 기법 통신, 스레드는 필요 없음


#### 6. PThread
* POSIX 스레드 (POSIX Threads 약어 PThread)
    * Thread 관련 표준 API
    