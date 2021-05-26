## 11. Signal (시그널)
#### 1. 시그널
* 유닉스에서 30년 이상 사용된 전통적인 기법
* 커널 또는 프로세스에서 다른 프로세스에 어떤 이벤트가 발생되었는지를 알려주는 기법

#### 2. 주요 시그널
* 시그널 종류와 각 시그널에 따른 기본 동작이 미리 정해져 있음
    * SIGKILL
      * 프로세스 죽여라 (슈퍼 관리자가 사용하는 시그널로 프로세스는 어떤 경우든 죽도록 되어 있음)
    * SIGALARM
      * 알람을 발생
    * SIGSTP
      * 프로세스 멈춰라 (ctrl+z)
    * SIGCONT
      * 멈춰진 프로세스를 실행
    * SIGINT
      * 프로세스에 인터럽트를 보내서 프로세스를 죽여라(ctrl+c)
    * SIGSEGV
      * 프로세스가 다른 메모리 영역을 침범했다
    
#### 3. signal 동작
* 프로그램에서 특정 시그널의 기본 동작 대신 다른 동작을 하도록 구현 가능
* 각 프로세스에서 시그널 처리에 대해 다음과 같은 동작 설정 가능
    * 시그널 무시
    * 시그널 블록 ( 블록을 푸는 순간, 해당 프로세스에서 시그널 처리)
    * 프로그램 안에 등록된 시그널 핸들러로 재정의 특정 동작 수행
    * 등록된 시그널 핸들러가 없다면 커널에서 기본 동작 수행
    
1. 시그널 보내기
```
#include <sys/type.h>
#include <signal.h>
// pid : 프로세스 PID
// sig : 시그널 번호
int kill (pid_t pid, int sig);
```
* ex
```
./loop &
./sigkill 1806 2
ps
```

2. 받은 시그널에 따른 동작 정의
```
#include <signal.h>
void (*siganl (int signum, void (*handler)(int)))(int);

siganl(SIGINT, SIG_IGN);
// SIGINT 시그널 수신 시, signal_handler 함수를 호출
siganl(SIGINT, (void *)signal_handler);
```

#### 4. 시그널과 프로세스
* PCB 에 해당 프로세스가 블록 또는 처리해야하는 시그널 관련 정보 관리
* 커널 모드에서 사용자 모드 전환시 시그널 정보 확인해서 해당 처리
