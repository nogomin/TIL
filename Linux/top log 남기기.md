### 간단한 서버 모니터링

- B서버에서 우선 top log를 남기는 로직 구현
```php
<?php
$DOCUMENT_ROOT = $_SERVER['DOCUMENT_ROOT'];
$date = date("Ymd");
$fileName = "topLog-{$date}.log";
$output;
$error;

exec("/usr/bin/top -b -n 1", $output, $error); // top command 한번 친것과 같음

if(!file_exists($fileName)) {
    $f_made = fopen($fileName, "w");
    for ($i=0; $i<5; $i++) { //해당 날짜에 대한 파일생성후 top로그 상단 5줄 가져오기
        fwrite($f_made, $output[$i]);
        fwrite($f_made, "\n");
    }
    fclose($f_made);
} else {
    $f_made = fopen($fileName, "a");
    for ($i=0; $i<5; $i++) {
        fwrite($f_made, $output[$i]);
        fwrite($f_made, "\n");
    }
    fclose($f_made);
}

$file = file($fileName);
$logData = array();
for($i = max(0, count($file)-5); $i < count($file); $i++) { //파일의 하단 5줄 가져오기
    $logData[$i] = $file[$i];
}

$url = '1.000.00.18/monitoring/log/top/No.26/getTopLog.php'; // 미리 생성 해 둔 getTopLog.php에 post데이터 전송
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($logData));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, 0);
$post = curl_exec($ch);
curl_close($ch);

?>
```
---
- A서버에서 B에서 보내준 post 데이터 처리

```php
<?php
$date = date("Ymd");
$fileName = "topLog-{$date}.log";
ob_start();
foreach($_POST as $value) {
        echo $value;
}
$output = ob_get_clean();
$fp = fopen($fileName, 'a');
fwrite($fp, $output);
fclose($fp);
?>

```
---
- 마지막으로 cron설정
```shell
// createTopLog.php 파일을 1초마다 실행하는 .sh 파일
#!/bin/sh
(sleep 1 && php /home/monitoring/log/top/createTopLog.php) &
(sleep 2 && php /home/monitoring/log/top/createTopLog.php) &
(sleep 3 && php /home/monitoring/log/top/createTopLog.php) &
(sleep 4 && php /home/monitoring/log/top/createTopLog.php) &
(sleep 5 && php /home/monitoring/log/top/createTopLog.php) &
...
(sleep 60 && php /home/monitoring/log/top/createTopLog.php) &
```
---
마지막으로 저 .sh파일을 cron으로 돌리자
```shell
* * * * * cd /home/monitoring/log/top && /home/monitoring/log/top/createTopLog.sh
```
주의할점 : cd로 먼저 해당 경로에 이동해주어야 cron이 위치를 찾는다


   
