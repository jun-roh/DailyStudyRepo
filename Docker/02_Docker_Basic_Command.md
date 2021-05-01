## 1. Docker 기본 명령어 사용 (run, ps)
#### 1) Docker 내부 파일 시스템 구조
```
# docker run 이미지이름 ls
```
* docker : 클라이언트 언급
* run : 컨테이너 생성 및 실행
* 이미지이름 : 실행할 컨테이너의 이미지
* ls : ls 커맨드는 현재 디렉토리 파일 리스트 표출. 원래 이 자리는 이미지가 가지고 있는 시작 명령어를 무시하고 커맨드를 실행하게 한다

#### 2) Container 의 리스트 반환
```
docker ps
```
* docker : 클라이언트 언급
* ps : process status

[실습]
1) 터미널 2개 실행 (A, B로 명명)
2) A 터미널에 명령어
    ```
    # docker run alpine ping localhost
    ```
3) B 터미널에 명령어
    ```
    # docker ps
   CONTAINER ID   IMAGE     COMMAND            CREATED         STATUS         PORTS     NAMES
   7f2c5b2b47f5   alpine    "ping localhost"   7 seconds ago   Up 6 seconds             ecstatic_raman
    ```
   - CONTAINER ID : Container 고유 hash 값
    - Image : 이미지 이름
    - COMMAND : Container 시작시 실행된 명령어
    - CREATED : 생성 시간
    - STATUS : Container 상태 
        - UP : 실행중
        - Exited : 종료
        - Pause : 일시정지
    - PORT : Container 가 개방한 포트와 호스트에 연결한 포트
    - NAMES : Container 고유한 이름. 컨테이너 생성시 --name 옵션으로 설정. default 로 임의로 지정해줌.
    
####* 원하는 항목만 보고 싶을 경우
```
# docker ps --format 'table{{.Names}}\ttable{{.Image}}'
NAMES            tableIMAGE
ecstatic_raman   tablealpine
```
####*모든 Container 보고 싶을 경우
```
# docker ps -a 
CONTAINER ID   IMAGE         COMMAND            CREATED          STATUS                   PORTS     NAMES
7f2c5b2b47f5   alpine        "ping localhost"   11 minutes ago   Up 11 minutes                      ecstatic_raman
0f6dc69936aa   hello-world   "/hello"           12 days ago      Exited (0) 12 days ago             awesome_fermi
```