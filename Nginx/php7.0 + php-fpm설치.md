### Nginx + PHP7.0설치 + PHP-fpm

1. bash 사용자 목록 조회
    ```bash
    grep /bin/bash /etc/passwd | cut -f1 -d:
    ```
    없다면 useradd test 형식으로 유저 추가 -> 확인은 /etc/passwd

***
2. nginx 설치

    * sudo apt-get install nginx로 설치
    
    * /etc/nginx/nginx.conf로 가서 기본 설정을 잡아준다

***
    
3. php 7.0설치

   현재 우분투 공식 apt-get 배포지에서는 php7를 공식 지원하지 않는다. 따라서 개인 패키지 어카이브 (Personal Package Archive)를 이용해야한다.
   
   * sudo add-apt-repository ppa:ondrej/php
   
     (2016/02/25 기준) PHP 7.0을 배포하는 PPA가 기존의 ppa:ondrej/php-7.0 에서 ppa:ondrej/php 로 통합됨.
     기존에 php-7.0 ppa를 이용하시던 분들은 반드시 sudo add-apt-repository –remove ppa:ondrej/php-7.0 명령어를 실행한 이후 sudo add-apt-repository ppa:ondrej/php로 ppa를 변경할것.
     
    * sudo apt-get update
    
    * sudo apt-get install php7.0

***

 4. php 7.0 fpm 설치
 
    nginx는 php를 기본적으로 지원하지않아서, php를 구동하기위해서 php-fpm 패키지가 필요하다.
    
    * sudo apt-get install php7.0-fpm
    
    * sudo apt-get install php7.0-mysql (MySQL/MariaDB 이용시)
    
    * sudo apt-get install php7.0-gd php7.0-curl php7.0-mbstring php7.0-xml php7.0-pgsql php7.0-json php7.0-cgi (많이 사용되는 모듈들)
    
    * /etc/php/7.0/fpm/php.ini로 이동
    
    ```shell
        short_open_tag = On
        upload_man_filesize = 50M
        date.timezone = Asia/Seoul
    ```
    
    * sudo service php7.0-fpm restart
    
    ---
    
    * /etc/nginx/sites-available/default로 이동
    
    * root 경로 지정
    
    * Add index.php to the list if you are using PHP 여기에 index index.html index.php처럼 원하는 확장명 기입
    
    * pass PHP scripts to FastCGI server 부분  
    ```php
    location ~ \.php$ {
                fastcgi_pass    unix:/var/run/php/php7.0-fpm.sock; //이 위치에 정말 존재하는지 확인 필수
                fastcgi_index   index.php;
                fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
                fastcgi_buffer_size 128k;
                fastcgi_buffers 4 256k;
                fastcgi_busy_buffers_size 256k;
                include         /etc/nginx/fastcgi_params;
        }
    ```
    ---    
    
    * /etc/php/7.0/fpm/pool.d/www.conf로 이동
    
    * user, group을 바꿔주고 가장 중요한 listen에 php-fpm.sock 경로를 적어준다.
    
    * service nginx restart
***




