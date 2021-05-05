## 7. Docker Compose

#### Docker Compose 는 다중 컨테이너 도커 어플리케이션을 정의하고 실행하기 위한 도구

[실습]
* 목표 : 페이지를 refresh 할때 숫자를 +1 해주는 간단한  프로그램 
* node js + redis 사용

1. package.json 생성
```
{
  "name": "docker_compose",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
  	"start": "node server.js"
  },
  "dependencies": {
  	"express": "^4.17.1",
  	"redis": "3.0.2"
  },
  "author": "",
  "license": "ISC"
}

```

2. index.js 생성
```
const express = require('express');
const redis = require("redis");
// redis client 생성
// Docker 환경이 아닐 경우 -> redis-server host url
// Docker 환경일 경우 -> docker-compose.yml 에 명시한 컨테이너 이름
const client = redis.createClient({
    host: "redis-server",
    port: 6379
})

const PORT = 8080;
// const HOST = '0.0.0.0';

const app = express();

client.set("number", 0);

app.get('/', (req, res) => {
    client.get("number", (err, number) => {
        res.send("number: " + number)
        client.set("number", parseInt(number) + 1)
    })
})

app.listen(PORT);
console.log('Server is running');
```

3. Dockerfile 생성
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

> **주의** 멀티 컨테이너 상황에서 지금까지 각자 컨테이너를 실행했을 경우 컨테이너 간의 통신이 되지 않기 때문에 Docker Compose 를 사용한다.

4. Docker Compose 생성
[docker-compose.yml]
```
version: "3"
services:
  redis-server:
    image: "redis"
  node-app:
    build: .
    ports:
      - "8888:8080"
```
* version : docker compose 버전
* services : 실행하려는 container
  * 컨테이너이름
    * images : 컨테이너 사용 이미지
    * build : 디렉토리에 있는 Dockerfile
    * ports : 포트 매핑 <로컬포트:컨테이포트>
    
5. docker compose 실행
```
docker-compose up
```

6. docker compose down
```
docker-compose down
```

7. docker compose background 실행
* 소스코드 수정시 이미지를 재빌드 하는 방법도 추가
```
docker-compose up -d --build
```