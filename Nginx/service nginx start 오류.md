### service nginx start가 안될 때

- 에러 메시지 내용 : Failed to start A high performance web server and a reverse proxy server.

가장 먼저 /etc/nginx/sites-available/default에 파일 문법오류를 점검할것, 특히 ; 조심

1. lsof -i:80 명령어로 80번 포트가 사용중인지 확인

2. /etc/nginx/sites-available/default로 이동 후
```bash
//이런식으로 고쳐주자 (다른포트를 쓰도록)
server {
  listen 8080 default_server;
  listen [::]:8080 default_server; 
}

```
