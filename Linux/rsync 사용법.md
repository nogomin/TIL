### lsync && rsync

1. 메인(원본파일이 있는 곳) 서버에 lsync 설치
```shell
lsyncd -version
sudo apt install lsyncd
```
---
2. /etc/lsyncd 이동 후 lsyncd.conf 파일 생성
```shell
settings {
     logfile = "/var/log/lsyncd/lsyncd.log",
     statusFile = "/var/log/lsyncd/lsyncd-status.log",
     delay = 1,
     statusInterval = 1
}
sync {
    default.rsync,
    source="/home/test/data/",
    target="192.168.0.3::test-data",
    rsync = {
        binary = "/usr/bin/rsync",
        compress = false,
        archive = true,
    }
}
```
---
3. lysncd 재시작 
```shell
service lsyncd restart
```
---
4. 서브(복제할 대상) 서버에 rsync 설치
```shell
apt-get install -y rsync
```
---
5. /etc/rsyncd.conf 이동 없을시 파일 생성하기
```shell
log file = /var/log/rsync.log            #로그파일 설정
[backup]                                 #미러링 될 이름 (Destination)
path = /var/backup                       #rsync 할 디렉토리 설정
comment = back rsync                     #설명 부분
uid = root                               #rsync 접근 가능 유저
gid = root                               #rsync 접근 가능 그룹
use chroot = yes                         #chroot 여부
read only = no                           #읽기 전용으로 설정
host allow = xxx.xxx.xxx.xxx             #해당 호스트만 접근 가능
max connection = 100                     #최대 연결 개수
timeout 300                              #시간 초과 설정   
```
---
6. rsync 재시작
```shell
/etc/init.d/rsync restart
```
---
7. 서브(복제할 대상) 서버에서 rsync 확인
```shell
rsync -avz 원본.파일.있는.서버.아이피::미러링될이름 파일경로
// 예시 : rsync -avz 192.168.10.01::test-ko /home/test
```
