## 9. 간단한 React 앱 개발 - 운영 환경

#### 1. 운영환경에서 필요한 Dockerfile
* 빌드 파일 생성
* Nginx  가동

[Dockerfile]
```
FROM node:alpine as builder
WORKDIR 'usr/src/app'
COPY package.json .
RUN npm install
COPY ./ ./
RUN npm run build

FROM nginx
COPY --from=builder /usr/src/app/build /usr/share/nginx/html
```
[이미지 생성]
```
docker build .
```
[실행]
```
docker run -p 8080:80 e2604e972fc1
```