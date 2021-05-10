## 08. 컨텍스트 스위칭

> PC(Program Counter) + SP(Stack Pointer) 에 주목
> 
> Stack, Heap, Data(BSS, DATA), Text(CODE) 에서의 동작 주목

1. PCB (Process Control Block)
    * PC, SP, 스케줄링 정보 등 저장
    * Process Context Block 이라고도 함
        1. Process ID
        2. Register 값(PC, SP 등)
        3. Scheduling Info(Process State)
        4. Memory Info (메모리 사이즈 limit)
        5. ...
    * 프로세스가 실행중인 상태를 캡쳐 / 구조화 해서 저장
    
2. Context Switching (문맥 교환)
    * CPU 에 실행할 프로세스를 교체하는 기술
    1) 실행 중지할 프로세스 정보를 해당 프로세스의 PCB 에 업데이트해서, 메인 메모리에 저장
    2) 다음 실행할 프로세스 정보를 메인 메모리에 있는 해당 PCB 정보(PC, SP)를 CPU 의 레지스터에 넣고 실행
    > dispatch : ready 상태의 프로세스를 running 상태로 바꾸는 것

    ![Alt text](./images/context_switching.png "Context Switching")
    
    * Context Switching 은 매우 짧은 시간 (m/s) 동작이 자주 일어난다.
    * 어떻게 하면 시간을 줄일 수 있을까?
        * 컴파일 언어가 아니라 어셈블리어로 컨텍스트 스위칭 코드를 작성하............