## 14. Virtual Machine(가상 머신)

#### 1. 가상 머신
* 하나의 하드웨어 (CPU, Memory 등) 에 다수의 운영체제를 설치하고 개별 컴퓨터처럼 동작하도록 하는 프로그램

#### 2. Virtual Machine Type 1(native 또는 bare metal)
* 하이퍼 바이저(또는 VMM) : 운영 체제와 응용프로그램을 물리적 하드웨어에서 분리하는 프로세스
* 하이퍼 바이저 또는 버추얼 머신 모니터 (VMM) 라고 하는 소프트웨어가 Hardware 에서 직접 구동
    * Xen, KVM

#### 3. Virtual Machine Type 2
*  하이퍼바이저 또는 버추어 머신 모니터 (VMM) 라고 하는 소프트웨어가 Host OS 상위에 설치
    * VMWare, Parallels
    
#### 4. Full Virtualization(전가상화) VS Virtualization(반가상화)
* 전가상화 : 각 가상머신이 하이퍼 바이저를 통해서 하드웨어와 통신
    * 하이퍼바이저가 마치 하드웨어인 것ㄹ처럼 동작하므로 가상머신의 OS 는 자신이 가상 머신인 상태인지를 모름
    * VMM (통역)
    
* 반가상화 : 각 가상머신에서 직접 하드웨어와 통신
    * 각 가상머신에 설치되는 OS는 가상 머신인 경우 이를 인지하고 각 명령에 하이퍼바이저 명령을 추가해서 하드웨어와 통신
    * VMM (리소스 관리)
    

