## 7. Process ID

#### 1. 프로그램, 프로세스, 스레드
* 프로그램
    * 바이너리, 코드 이미지, 응용 프로그램, Application 또는 실행파일
    
* 프로세스 
    * 실행중인 프로그램(메모리 적재 + 프로세스 상태 정보 포함)
    
* 스레드
    * 리눅스 프로세스는 기본 스레드 포함
    * 싱글스레드 프로세스 : 기본 프로세스
    * 멀티스레드 프로세스 : 여러 스레드 존재
    
#### 2. 프로세스 ID
* 프로세스 ID
    * pid, 각 프로세스는 해당 시점에 unique 한 pid 가짐
    * pid 최대값은 32768
    * 부호형(signed) 16 비트 정수값 사용
    * 2^15 = 32768
    ```
    # sudo vi /proc/sys/kernel/pid_max 
    ```   
    * 최근 할당된 pid 가 200 이라면, 그 이후는 201,202.. 식으로 할당
    
#### 3. 프로세스 계층
* 최초 프로세스 : init 프로세스, pid 1
* init 프로세스는 운영체제가 생성
* 다른 프로세스는 또 다른 프로세스로부터 생성
    * 부모 프로세스, 자식 프로세스
* ppid 값이 부모 프로세스의 pid 를 뜻함
* ppid 확인
    ```
    # ps -ef
    -e : 시스템상의 모든 프로세스 출력
    -f : 다음 목록 출력 (UID, PID, PPID, CPU%, STIME, TTY, TIME, CMD)
    ```
  
#### 4. 프로세스와 소유자(Owner) 관리
* 리눅스 내부에서는 프로세스의 소유자(사용자)와 그룹을 UID/GID (정수) 로 관리
* 사용자에 보여줄때에만 UID 와 사용자 이름 매핑 정보를 기반으로 사용자 이름으로 제공
```
# ps -ef
# sudo vi /etc/passwd
# sudo vi /etc/shadow
```
* /etc/passwd 확인하기

|항목|예1|예2|
|---|---|---|
|사용자명(아이디)|root|ubuntu|
|패스워드|x|x|
|사용자 ID(UID)|0|1000|
|그룹 ID(GID)|0|1000|
|사용자 정보|root|Ubuntu|
|홈디렉토리|/root|/home/ubuntu|
|쉘환경|/bin/bash|/bin/bash|

#### 5. 프로세스 관리 관련 시스템콜
* 사전작업
  * ubuntu linux 에 gcc 설치
  ```
  # sudo apt-get intall gcc
  # gcc --version
  # gcc -o test.c test
  ```

* getpid() 와 getppid()
  * 함수 원형
  ```
  #include <sys/types.h>
  #include <unistd.h>
  pid_t getpid (void);
  pid_t getppid (void);
  ```  
  * 실습 코드
  ```
  #include <sys/types.h>
  #include <unistd.h>
  #include <stdio.h>
  int main(){
    printf("pid=%d\n", getpid());
    printf("ppid=%d\n", getppid());
    return 0;
  }
  ```
#### 6. 부모 프로세스와 자식 프로세스
* bash 프로세스가 실행 파일의 부모 프로세스인 예
```
# ./pidinfo
pid=3671
ppid=2868
```

```
# ps
PID   TTY     TIME    CMD
2868  pts/0  00:00:00 bash
3669  pts/0  00:00:00 ps
```