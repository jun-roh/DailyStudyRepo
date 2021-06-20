## 07. Application
#### 1. 애플리케이션 계층
* 애플리케이션 계층은 TCP/IP 모델에서 최상위 계층으로 사용자와 가장 가까운 인터페이스제공

#### 2. DNS
* 도메인 주소를 IP 로 변화해주는 서비스이며 계층적 구조
* Recursive Query 는 Local DNS 서버가 재귀적으로 여러 서버에게 질의하며 응답받는 과정
* DNS 서버의 정보 타입으로 총 4가지 A, NS, CNAME, MX 가 있음
* DNS 메시지는 쿼리와 응답으로 구분되며 쿼리 전에 hosts.txt & DNS 캐시 테이블을 참조

