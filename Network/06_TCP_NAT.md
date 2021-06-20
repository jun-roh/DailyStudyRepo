## 6. TCP 와 NAT
#### 1. Transport 계층
1. 역할
* End to End 서비스, 커넥션(연결)을 관리
* Connection - oriented, Reliability, Flow control, Multiplexing
* TCP & UDP 소켓을 통한 프로세스 별 통신
* 5 tuple = Source IP, Source Port, Dest IP, Dest Port, Protocol

2. 포트
* 전송 계층에서 사용되며 특정 프로세스를 구분하는 단위

#### 2. TCP
1. 정의
* TCP(Transmission Control 전송 제어 프로토콜)
    * 인터넷을 구성하는 핵심 프로토콜
    * 신뢰성을 기반으로 데이터를 에러 없이 전송 1:1 통신
    * 연결 지향, connection-oriented 패킷의 상태 정보를 확인하고 유지
    * 에러 발생시 재전송을 요청하고 에러를 복구
    
2. 헤더 포맷
   
   ![Alt text](./images/06_network_01.png "IP")
   
3. TCP 제어 플래그

    ![Alt text](./images/06_network_02.png "IP")

#### 3. UDP
1. 정의
* UDP(User Datagram Protocol)
* 신뢰성은 낮으나 데이터 전송이 빠르다
* 송신 측은 일반적으로 데이터를 보내고 확인 안함. 1:n 통신 가능
* Connectionless, 재전송 불가, 실시간 데이터 전송에 적합
* 스트리밍 서비스의 경우 전송 문제가 발생해도 재전송보다 실시간 데이터 전송이 중요

2. 헤더 포맷
   
   ![Alt text](./images/06_network_03.png "IP")

#### 4. TCP UDP 비교

![Alt text](./images/06_network_04.png "IP")

#### 5. TCP 통신과정
1. 3 way Handshake
* TCP 는 연결 지향 프로토콜로 두 호스트가 통신하기 전에 연결을 위한 관계 수립

![Alt text](./images/06_network_05.png "IP")

2. 4 way Handshake
* 정상적인 연결을 종료

![Alt text](./images/06_network_06.png "IP")

3. TCP 상태 전이도 - client

![Alt text](./images/06_network_07.png "IP")

4. TCP 상태 전이도 - Server

![Alt text](./images/06_network_08.png "IP")

5. TCP 타이머 - Retransmission
* 송신 측이 패킷을 매번 전송할 때 카운트

6. TCP 타이머 - Persistence
* 윈도우 사이즈 관련 타이머
* 수신측에서 용량 부족으로 윈도우 사이즈 없음을 보내고 다시 용량에 여유가 생기면 송신 측에 요청
* 중간에서 윈도우 사이즈 > 0 을 보내는 ACK 이 유실되면 서로 통신 간 문제 발생
* 수신 측 윈도우 사이즈 = 0 을 보낼 경우 Persistence 타이머 가동 - RTO
* Persistence 타이머가 종료되면 Probe(ACK 재전송 요청)를 보내고 타이머 재가동
* 다시 타이머가 종료되기 전에 ACK 을 수신 못하면 시간을 2배로 늘리고 Probe 재전송
* 타이머의 임계치는 60초

7. TCP 타이머 - Time waited
* TCP 연결 종료 후에 특정 시간만 연결 유지

8. TCP 타이머 - Keepalive
* TCP 연결 유지 타이머

#### 6. 흐름 제어
1. Flow Control
* 송신과 수신측의 데이터 처리 속도 차이를 해결

2. Congestion Control
* 수신측으로 유입되는 트래픽의 양이 정해진 대역폭을 넘어가지 않도록 제어

#### 7. 공인 IP & 사설 IP
1. 공인 IP
* ICANN(Internet Corporation for Assigned Names and Numbers)
* 공인기관에서 인정하는 IP 주소이며 인터넷을 통한 외부망에서 식별되고 통신 가능한 IP

2. 사설 IP
* 내부망에서 사용 및 식별 가능한 IP, IPv4 개수의 한계

3. IP 정보 확인
* 자식의 PC 가 외부 인터넷으로 통신 시 사용하는 공인 IP 정보 확인

#### 8. NAT
1. NAT(Network Address Translation)
* 네트워크 주소 변환
* 사설 IP 네트워크을 인터넷으로 연결 -> 라우팅 가능한 공인 IP 로 변환
* 보안: 내부 IP 주소를 외부에 공개하지 않음
* 유연성: 공인 IP 대역은 영향을 주지 않고 내부 네트워크 구성 변경이 가능, 기존 사용하던 외부에 공개된 공인 IP 주소는 변경되지 않으나 내부 IP 만 변경
* 비용: 공인 IP 할당 비용 감소
* L3 이상의 장비 또는 방화벽에서 NAT 가능

2. Static NAT
* 1:1 NAT, 정적 NAT
* 사설 IP 1개를 공인 IP 1개로 맵핑하며 주로 외부 공개형 서버에 구성

3. Dynamic NAT
* 내부 IP 주소와 외부 IP 주소가 범위 내에서 맵핑
* 내부 PC 들은 외부로 통신 시 공인 IP 대역 Pool 에서 할당 받음

4. PAT(Port Address Translation)
* 1:N NAT, 여러개의 내부 사설 IP 들이 1개의 공인 IP 로 변환
* 공개형 서버가 아닌 내부 -> 외부로 접속이 필요한 PC 들이 사용
* IP가 중복되기 때문 Port 로 세션 구분

5. Port Forwarding
* 공인 IP 1개로 여러대의 사설 IP를 Port 로 구분하여 연결
* 공인 IP 1개로 여러대의 공개형 서비스를 구축할 때 사용

#### 9. TELNET & SSH
1. TELNET
* 원격지 호스트 컴퓨터에 접속하기 위해 사용되는 프로토콜
* 장비 관리 또는 서버 접속시 사용 -shell - CLI
* 클라이언트 소프트웨어인 경우, 포트 테스트 용도로 많이 사용

2. TELNET 기능
* NVT(Network Virtual Terminals) 지원 : 데이터 변환 가상 장치
* 협상 가능한 옵션
* 프로세스와 터미널의 1:1 symmetric 관계

3. SSH
* Secure Shell
* TELNET 을 대체하기 위해 개발
* 원격지에 있는 컴퓨터를 명령어를 통해서 제어
* 강력한 인증 방법 및 암호화 통신을 제공

4. SSH 특징
* 인증(Authentication)
    * 사용자가 서버 접속시 패스워드 또는 공개키 기반의 인증 방식을 지원
* 암호화 (Encryption)
    * 대칭키 방식 사용
* 무결성 (Integrity)
    * 데이터 위변조 방지
* 압축 (Compression)
    * 다중화 통신
    
* 대칭키 : 동일한 키로 암호화를 동시에 할 수 있는 방식
* 공개키(공개키 + 개인키) 방식
* 공개키 암호화 -> 데이터 보안, 서버의 공개키로 데이터를 암호화 -> 서버의 개인키로 복호화
* 개인키 암호화 -> 인증보안, 개인키 소유자가 개인키로 암호화 하고 공개키를 함께 전달 -> 암호화 데이터 + 공개키로 신원 확인 -> 전자서명 방법

