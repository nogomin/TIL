### Nginx + PHP-fpm

1. bash 사용자 목록 조회
    ```bash
    grep /bin/bash /etc/passwd | cut -f1 -d:
    ```
    없다면 useradd test 형식으로 유저 추가 -> 확인은 /etc/passwd

***
2. nginx site-available
    /etc/nginx/sites-available/default로 이동

    root /home으로 변경
    location ~ \.php$ 부분에 fastcgi_pass 부분을 주목한다. 

    보통 unix:/var/run/php7.0-fpm.sock; 인데, 해당위치에 가서 파일이 있는지 확인하자. 

    소유자, 그룹명도 가급적 현재 nginx 설정의 유저와 맞춰준다.
***
3. php fpm pool 설정
    /etc/php/7.0/fpm/pool.d/www.conf로 이동

    user와 group을 바꿔준다.

    제일 중요한 listen부분을 /var/run/php7.0-fpm.sock 처럼 바꿔준다. (정확한 위치확인 중요)

    listen.owner와 listen.group도 바꿔준다.
***
4. 서비스 재시작
    ```bash
    service nginx restart
    service php7.0-fpm restart
    ```
