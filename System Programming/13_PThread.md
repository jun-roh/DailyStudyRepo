## 13. PThread
#### 1. Pthread
* thread 표준 API
    * POSIX Thread 또는 Pthread 라고 부름
    
* Pthread API
    * 저수준 API 로 100여개의 함수 제공
    * 복잡하지만 유닉스 시스템 핵심 스레딩 라이브러리
    * 다른 스레딩 솔루션도 결국 Pthread를 기반으로 구현되어 있으므로 익혀둘 가치가 있음
    
#### 2. Pthread 라이브러리
* <pthread.h> 헤더 파일에 정의
* 모든 함수는 pthread_로 시작
* 크게 두가지 그룹
    * 스레드 관리 : 생성, 종료, 조인, 디태치 함수 등
    * 동기화 : 뮤택스 등 동기화 관련 함수
* 기본 라이브러리 (glibc) 와 분리된 libpthread 라이브러리에 pthread 구현되어 있으므로 컴파일시 명시적으로 -pthread 옵션 필요
```
gcc -pthread test.c -o test
```

#### 3. Thread 생성
```
// thread : 생성된 스레드 식별자
// attr : 스레드 특정 설정(기본 null)
// start_routine : 스레드 함수(스레드로 분기해서 실행할 함수)
// arg: 스레드 함수 인자
int pthread_create(pthread_t * thread, const pthread_attr_t *attr, void *(*start_routine)(void *), void *arg);
```
```
pthread_t thread1;
void *thread_function(void *ptr);
ret = pthread_create(&thread1, NULL, thread_function, (void*)message1);
```

#### 4. Thread 종료
```
// exit 와 유사, NULL 또는 0 은 정상 종료
void pthread_exit(void *retval);
```
```
pthread_exit(NULL);
```

#### 5. Thread 조인
```
// thread: 기다릴 스레드 식별자
// thread_return: 스레드의 리턴 값을 가져올 수 있는 포인터
int pthread_join(pthread_t thread, void ** thread_return);
```
* p_thread 식별자를 가진 스레드의 종료를 기다리고 status 포인터로 종료값을 가져옴
```
pthread_join(p_thread, (void *) &status);
printf("thread join : %d\n", status);
```

#### 6. Thread Detach
* 해당 스레드가 종료될 경우 즉시 관련 리소스를 해제 한다
    * pthread_join 을 기다리지 않고 종료 즉시 리소스를 해제
    
```
// thread: detach 할 스레드 식별자
int pthread_detach(pthread_t thread);
```

