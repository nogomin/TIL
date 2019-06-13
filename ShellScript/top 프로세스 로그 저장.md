### shell script로 로그남기기

```shell
#!/bin/sh
  
LOG_DIR="/home/monitoring/log/top/"
DATE="$(date +%Y%m%d)" //오늘날짜생성
FILE_NAME="$LOG_DIR/topLog-$DATE.log" //파일이 절대경로여야 실행할때 안
DELETE_DATE="$(date '+%Y%m%d' -d '1 month ago')" //한달전 날짜
DELETE_FILE_NAME="topLog-$DELETE_DATE.log"
if [ ! -e $FILE_NAME ]; then
    touch $FILE_NAME
    while true; do
        top -b -d 1 | head -n 5; // top process 위에서 5줄
        sleep 1;  // 1초당 반복
    FILE_LENGTH=$(wc -l < $FILE_NAME) // file의 줄 
    if [ "$FILE_LENGTH" -gt 432000 ]; then
        break
    fi
    sshpass -p '비번' scp -P 2222 $FILE_NAME root@아이피:/home/monitoring/log/top/ 
    done >> $FILE_NAME
fi
if [ -e $DELETE_FILE_NAME ]; then
    rm $DELETE_FILE_NAME
fi
```
- cron으로 매일 0시 0분에 실행
  0 0 * * * 실행파일경로
  
  실행안될 시, sh파일에 경로들을 다시 한번 살피고, 그래도 안될 시 pstree에 cron이 작동중인지 확인해본다
  
  그래도 안된다면 환경변수 문제일 가능성이 있다.
  ```
  SHELL=/bin/bash
  PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
  ```
  crontab -e에서 저것을 추가해준다. 각자의 PATH는 echo $PATH로 확인
