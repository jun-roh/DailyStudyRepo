## 4. Docker exec 명령어 전달 과 컨테이너에서 shell 환경 사용
#### 1) docker exec <컨테이너 아이디>

이미 실행중인 컨테이너에 명령어를 전달하고 싶다면 docker exec 명령어를 사용해서 전달하자.

실습을 통해서 한번 전달 테스트 해보도록 하자

[실습]

1) 우선 터미널을 2개 실행한다 (A, B 터미널)
2) A 터미널에 docker run alpine ping localhost 명령어로 컨테이너를 실행한다.
3) B 터미널에서 docker ps 로 동작하고 있는 터미널을 확인한다.
4) B 터미널에서 동작하는 컨테이너에 docker exec <컨테이너 아이디> ls 명령어를 전달합니다.
5) B 터미널에서 ls 명령어를  받았는지 확인한다.

* docker run 과 docker exec 차이점
    - 명령어를 사용했을 경우에 같은 결과를 보여준다.
    - 하지만 차이점은 
    - docker run 은 컨테이너를 만들어서 실행
    - docker exec 는 이미 실행중인 컨테이너에 명령어를 전달

#### 2) Redis 서버를 활용해서 컨테이너 이해하기 (exec 명령어 예)

1. Redis 동작 방식
  
    1. Redis Server 와 Redis Client 두개가 필요하다.
    2. Redis Server 가 우선적으로 작동 해야한다.
    3. 그 후에 Redis Client 실행 후 명령어를 Redis Server 로 전달한다.
  

2. 실습
    1. 우선 터미널을 2개 실행한다 (A, B 터미널)
    2. A 터미널에 docker run redis 실행
    3. B 터미널에 docker run redis-cli 실행
   -> 에러 :
   Redis Client 가 Redis Server 가 있는 컨테이너 밖에서 실행하려 해서 Redis Server 접근을 할 수 없다.
       

=> Redis Client 도 컨테이너 안에서 실행해야되는데! 여기서 exec  명령어를 사용해 보자.

  1. 우선 터미널을 2개 실행한다 (A, B 터미널)
  2. A 터미널에 docker run redis 실행
  3. B 터미널에서 docker exec -it <컨테이너 아이디> redis-cli 실행한다.
  4. B 터미널에서 redis-cli 에 접속한 화면이 나온다.
  5. redis client 에서 set key1 hello 명령어를 적어본다.
  6. get key1 로 내용이 들어갔는지 확인 해본다.

```
# docker exec -it 11afef348688 redis-cli
127.0.0.1:6379> set key1 hello
OK
127.0.0.1:6379> get key1
"hello"
```

* -it option
  - -it를 붙여줘야 명령어를 실행 한 후 계속 이어서 명령어를 사용 가능
  - 위의 예제에서 -it가 없다면 컨테이너에 접속했다가 끊기는 결과가 나올 것
  - i : interactive 의 약자
  - t : terminal 의 약자

#### 3) 실행중인 컨테이너에서 shell 환경 사용
```
docker exec -it <컨테이너 아이디> <명령어>
```

[실습]
1. 우선 터미널을 2개 실행한다 (A, B 터미널)
2. A 터미널에 docker run alpine ping localhost 실행
3. B 터미널에 docker exec -it 5522ade2cb08 sh 명령어로 shell 접근
4. 설치된 환경에 따라서 sh, bash, zsh, powershell 등을 사용할 수 있다.
5. 화면에서 빠져나오려면 ctrl + D 로 빠져나올 수 있다.
```
# docker exec -it 5522ade2cb08 sh

/ # ls
bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var
```