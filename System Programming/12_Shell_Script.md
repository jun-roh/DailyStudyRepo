## 12. Shell Script
#### 1. 쉘 스크립트
* 쉘을 사용해서 프로그래밍을 할 수 있음
* 서버 작업 자동화 및 운영(DevOps)을 위해 기본적으로는 익혀둘 필요가 있음
* 쉘 명령어를 기본으로 하되 몇가지 문법이 추가된 형태
* 시스템 프로그래밍에서 꼭 익히는 내용 중 하나

#### 2. 기본문법
* 쉘 스크립트는 파일로 작성 후 파일을 실행
* 파일의 가장 위의 첫 라인은 #!/bin/bash 로 시작
* 쉘 스크립트 파일은 실행 권한을 가지고 있어야함
* 일반적으로 '파일이름.sh' 와 같은 형태로 파일 이름을 작성

1. 변수
* 선언
    * 변수명=데이터
    * 변수명=데이터 사이에 띄어쓰기는 허용되지 않음
    
* 사용
    * $변수명으로 사용됨
```
#!/bin/bash
mysql_id='root'
mysql_directory='/etc/mysql'

echo $mysql_id
echo $mysql_directory
```

2. 리스트 변수(배열)
* 선언
    * 변수명=(데이터1 데이터2 데이터3)
    
* 사용
    * ${변수명[인덱스번호]}
```
#!/bin/bash

daemons=("httpd" "mysqld" "vsftpd")
echo ${daemons[1]}  // 배열의 두번째 인덱스에 해당하는 mysqld 출력
echo ${daemons[@]}  // 배열의 모든 데이터 출력
echo ${daemons[*]}  // 배열의 모든 데이터 출력
echo ${#daemons[@]} // 배열 크기 출력

filelist = ($(ls))  // 해당 쉘스크립트 실행 디렉토리의 파일 리스트를 배열로 $filelist 변수
echo ${filelist[*]} // 모든 데이터 출력
```

3. 사전에 정의된 지역 변수
```
$$      : 쉘 프로세스 번호
$0      : 쉘 스크립트 이름
$1~$9   : 명령줄 인수
$*      : 모든 명령줄 인수 리스트
$#      : 인수의 개수
$?      : 최근 실행한 명령어의 종료 값
        - 0(성공) 1~125(에러)
        - 126 (파일이 실행가능하지 않음)
        - 128~255(시그널 발생)
```

4. 연산자
* expr : 숫자 계산
* expr 를 사용하는 경우 역작은 따옴표 (`) 를 사용해야함(작은 따옴표가 아님)
* 연산자 * 와 괄호 () 앞에는 역슬래시 () 와 같이 사용
* 연산자와 숫자, 변수, 기호 사이에는 space 를 넣어야 함
```
num = `expr \(3 \* 5\) / 4 + 7
echo $num
```

5. 조건문
* 기본 if 구문
    * 명령문을 꼭 탭으로 띄워야 하는것은 아님 (then 과 fi 안에만 들어가 있으면 됨)
```
if [조건]
then
    명령문
fi
```
* 조건
    * 조건작성에는 다른 프로그래밍 언어와 달리 가독성이 현저히 떨어짐, 필요할 떄마다 참조
    * 문자 비교
    ```
    문자1 == 문자2  // 문자1과 문자2가 일치
    문자1 != 문자2  // 문자1과 문자2가 일치하지 않음
    -z 문자       // 문자가 null 이면 참
    -n 문자       // 문자가 null 이 아니면 참
    ```
    * 수치 비교
        * < , > 는 if 조건시 [[]] 를 넣는 경우 정상 동작하기도 하지만, 기본적으로 다음 문법을 사용 권장
    ```
    값1 -eq 값2   // 값이 같음(equal)
    값1 -ne 값2   // 값이 같지 않음 (not equal)
    값1 -lt 값2   // 값1이 값2보다 작음 (less than)
    값1 -le 값2   // 값1이 값2보다 작거나 같음 (less or equal)
    값1 -gt 값2   // 값1이 값2보다 큼 (greater than)
    값1 -ge 값2   // 값1이 값2보다 크거나 같음 (greater or equal)
    ```
    * 파일 검사
    ```
    -e 파일명  # 파일이 존재하면 참
    -d 파일명  # 파일이 디렉토리면 참
    -h 파일명  # 심볼릭 링크파일
    -f 파일명  # 파일이 일반 파일이면 참
    -r 파일명  # 파일이 읽기 가능이면 참
    -s 파일명  # 파일 크기가 0이 아니면 참
    -u 파일명  # 파일이 set-user-id가 설정되면 참
    -w 파일명  # 파일이 쓰기 가능 상태이면 참
    -x 파일명  # 파일이 실행 가능 상태이면 참
    ```
    * 논리 연산
    ```
    조건1 -a 조건2  // AND
    조건1 -o 조건2  // OR
    조건1 && 조건2  // 양쪽 다 성립
    조건1 || 조건2  // 한쪽 또는 양쪽다 성립
    !조건          // 조건이 성립하지 않음
    true          // 조건이 언제나 성립
    false         // 조건이 언제나 성립하지 않음
    ```

6. 조건문
* 기본 if/else 구문
```
if [조건]
then
    명령문
else
    명령문
fi
```

7. 쉘 명령어 해석
* ping -c 1 192.168.0.1 1 > /dev/null
* 1 > /dev/null : 표준 출력 내용은 버려라
* -c 1은 1번만 체크해라
```
#!/bin/bash
ping -c 1 192.168.0.1 1 > /dev/null

if[$? == 0]
then
    echo "게이트웨이 핑 성공"
else
    echo "게이트웨이 핑 실패"
fi
```

* 조건문 한줄 작성
    * if 구문 한라인에 작성
    ```
    if [조건]; then 명령문; fi
    if [ -x $1 ]; then echo "Insert arguments"; fi
    ```
    * if [뒤와, ] 앞에는 반드시 공백이 있어야함
    * [] 에서 &&, ||, <, > 연산자들이 에러가 나는 경우는 [[]] 를 사용하면 정상 작동하는 경우가 있음
    

8. 반복문
* 기본 for 구문
```
for 변수 in 변수값1, 변수값2 ...
do
    명령문
done
```
```
#!/bin/bash
for database in $(ls)
do
    echo $database
done

for database in $(ls); do
    echo $database
done

for database in $(ls); do echo $database; done
```

* 기본 while 구문
```
while [조건문]
do
    명령문
done
```
```
#!/bin/bash
lists=$(ls)
num=${#lists[@]}
index=0
while [ $num -ge 0 ]
do
    echo ${lists[$index]}
    index=`expr $index + 1`
    num=`expr $num -1`
done
```

9. 실습
* 백업 스크립트
```
#!/bin/bash
if [ -z $1 ] || [ -z $2 ]; then
    echo usage: $0 sourcedir targetdir
else
    SRCDIR=$1
    DSTDIR=$2
    BACKUPFILE=backup.$(date + %y%m%d%H%M%S).tar.gz
    if [-d $DSTDIR ]; then
      tar -cvzf $DSTDIR/$BACKUPFILE $SRCDIR
    else
      mkdir $DSTDIR
      tar -cvzf $DSTDIR/$BACKUPFILE $SRCDIR
    fi
fi
```
  * 압축명령 tar
  ```
  x : 묶음을 해제
  c : 파일을 묶음
  v : 묶음/해제 과정을 화면에 표시
  z : gunzip  을 사용
  f : 파일 이름을 지정
  ```
  * 압축시 주로 사용 하는 명령어
  ```
  # tar -cvzf [압축된 파일 이름] [압축할 파일이나 폴더명]
  ```
  * 압축을 풀을 때 주로 사용하는 옵션
  ```
  # tar -xvzf [압축 해제할 압축 아카이브 이름]
  ```
* 로그파일 정리하기
```
find . -type f -name '파일명검색어' -exec bash -c "명령어1; 명령어2; 명령어3;" \;
# -type f: 파일 타입 지정해서 검색(f는 일반 파일)
```
  
```
#!/bin/bash
LOGDIR=/var/log
GZIPDAY=1
DELDAY=2
cd $LOGDIR
echo "cd $LOGDIR"

sudo find . -type f -name '*log.?' -mtime +$GZIPDAY -exec bash -c "gzip {}" \; 2> /dev/null
sudo find . -type f -name '*gz.?' -mtime +$DELDAY -exec bash -c "rm -f {}" \; 2> /dev/null
```

