## 6. Docker 활용 (Node.js Application Example)

Docker Example 연습용으로 Node js 공식 홈페이지에서 Docker 를 이용하는 예시 부분 사용

참고 URL : [nodejs-docker-webapp](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)

[node 설치 for mac]
1. Homebrew 설치
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2. node 설치
```
brew install node
```

3. 설치 버전 확인
```
npm --version

node --version
```

[Node js App 생성]

1. 폴더 생성

2. 명령어를 통해서 초기화
```
npm init
```

3. package.json 수정
```
{
  "name": "nodejs",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.17.1"
  },
  "author": "",
  "license": "ISC"
}
```

4. server.js 생성
```
const express = require('express');

const PORT = 8080;
// const HOST = '0.0.0.0';

const app = express();
app.get('/', (req, res) => {
	res.send('Hello world');
});

app.listen(PORT);
// app.listen(PORT, HOST);
//console.log('Running on http://${HOST}:${PORT}');
```

5. Dockerfile 작성
```
FROM node:10

#Create app directory
WORKDIR /usr/src/app

#Install App Dependencies
COPY package*.json ./

RUN npm install

# Bundle app source
COPY . .

Expose 8080
CMD ["node", "server.js"]
```
* WORKDIR
    * 이미지 안에서 어플리케이션 소스코드의 디렉토리를 생성하는 것
    * 따로 디렉토리를 생성하는 이유
        * COPY 한 어플리케이션 소스코드들이 ROOT 디렉토리로 저장되지 않게 하기 위해서
    * WORKDIR 을 설정하지 않을 경우 생기는 문제
        * COPY 할 파일 이름이 ROOT 파일 시스템에 있는 파일과 같을 경우 덮어쓰기함
        * 소스코드와 파일시스템이 정돈되지 않은채로 될수도 있음
    * WORKDIR 설정 후 터미널로 접근을 하면 기본적으로 WORK 디렉토리에서 시작

* COPY 를 해주는 이유
    * npm install 할때 종속성을 찾아야하는데 Container 안에 있지 않고 밖에 있음
    * 그래서 위에 설정했듯이 WORKDIR 을 지정해주고 package 파일을 COPY
    * npm install 후 app source 도 필요하기 때문에 한번 더 디렉토리 안에 내용을 copy
    
6. Docker 실행 
```
docker run -p 5000:8080 <이미지이름>
```

* 브라우저의 연결 포트와 Container 안에 포트와 맞도록 매핑
    ```
    -p <브라우저포트>:<컨테이너포트>
    ```
* http://localhsot:<브라우저포트> 로 실행

7. [주의] 소스 변경으로 다시 빌드 할때 문제점
* 소스코드 변경하면 이미지 생성 부터 실행까지 다시 진행
* 서비스에서는 변경된 소스 코드를 바로바로 반영 해야함

```
# docker build -t jun/nodetest:latest ./
# docker run -d -p 5000:8080 jun/nodetest
```

> 비효율적인 빌드를 개선한 방법
* package.json 먼저 복사 후 전체 코드를 복사
* 전체 코드를 복사하고 npm install VS package.json 먼저 복사 npm install 후 전체 코드를 복사
    * 전체 코드를 복사하고 npm install 빌드 할때 마다 종속성을 다운받아 설치
    * package.json 만 먼저 복사 후에 npm install 했을때 package.json 이 변경된 것이 아니기 때문에 소스코드 변경만 했을 경우 종속성을 다시 다운받지 않음
    * 불필요한 종속성을 다운받는 일은 줄이는게 좋음
    
> 소스코드 변경으로 계속 빌드 해야 하나? 개선 방법
* 위에 예제에서는 COPY 를 통해서 소스코드를 전달
* Docker Volume 을 이용하여 개선
    * 로컬 디렉토리와 도커 컨테이너의 디렉토리 간 Mapping
* Docker Volume 반영 방법
```
# docker run -d -p 5000:8080 -v /usr/src/app/node_modules -v $(pwd):/usr/src/app jun/nodetest
```
* -v /usr/src/app/node_modules
    * 호스트 디렉토리에 node_module 은 없기 때문에 컨테이너에 매핑 하지 말것
* -v $(pwd):/usr/src/app
    * pwd 경로에 있는 디렉토리를 /usr/src/app 에 mapping
* 소스 코드 변경 후 컨테이너 재시작
```
# docker stop <컨테이너 아이디>
# docker run -d -p 5000:8080 -v /usr/src/app/node_modules -v $(pwd):/usr/src/app jun/nodetest
```
* 이미지 빌드는 하지 않아도 된다!!!