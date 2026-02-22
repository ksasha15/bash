# bash
### Задание:
Написать скрипт для CRON, который раз в час формирует отчёт и отправляет его на заданную почту.\

Отчёт должен содержать:\

IP-адреса с наибольшим числом запросов (с момента последнего запуска);\
Запрашиваемые URL с наибольшим числом запросов (с момента последнего запуска);\
Ошибки веб-сервера/приложения (с момента последнего запуска);\
HTTP-коды ответов с указанием их количества (с момента последнего запуска).\

Скрипт должен предотвращать одновременный запуск нескольких копий, до его завершения.\

В письме должен быть прописан обрабатываемый временной диапазон.\

```
root@Ubuntu22:~# cat << EOF > /etc/cron.hourly/email_script
> sendmail teacher@otus.ru < /root/email.txt
EOF
root@Ubuntu22:~# cat /etc/cron.hourly/email_script
sendmail teacher@otus.ru < /root/email.txt
root@Ubuntu22:~#
root@Ubuntu22:~# awk '{print$1}' /var/log/nginx/access-4560-644067.log | sort | uniq -c | sort -n | tail
     16 95.165.18.146
     17 217.118.66.161
     20 185.6.8.9
     22 148.251.223.21
     24 62.75.198.172
     31 87.250.233.68
     33 188.43.241.106
     37 212.57.117.19
     39 109.236.252.130
     45 93.158.167.130
root@Ubuntu22:~# awk '{print$1}' /var/log/nginx/access-4560-644067.log | sort | uniq -c | sort -n | tail >> /root/email.txt
root@Ubuntu22:~#
root@Ubuntu22:~# grep  -o -E "http.*" /var/log/nginx/access-4560-644067.log | awk '{print$1}' | sed -r 's/https?:\/\/([^/]*).*/\1/' | sort | uniq -c | sort -n
      1 cloudsystemnetworks.com)"rt=0.000
      1 cloudsystemnetworks.com)"rt=0.185
      1 github.com
      1 http-client/1.1"rt=0.000
      1 http-client/1.1"rt=1.048
      1 yandex.ru
      2 110.249.212.46
      2 dbadmins.ru"
      2 go.backupland.com
      2 www.feedly.com
      2 www.google.ru
      3 ahrefs.com
      9 www.google.com
     11 www.bing.com
     20 www.domaincrawler.com
     21 www.semrush.com
    118 yandex.com
    156 dbadmins.ru
root@Ubuntu22:~#
root@Ubuntu22:~# grep  -o -E "http.*" /var/log/nginx/access-4560-644067.log | awk '{print$1}' | sed -r 's/https?:\/\/([^/]*).*/\1/' | sort | uniq -c | sort -n >> /root/email.txt
root@Ubuntu22:~# cat /var/log/nginx/error.log.1
2026/02/16 05:23:38 [notice] 2156#2156: using inherited sockets from "5;6;"
2026/02/16 05:36:06 [emerg] 2390#2390: bind() to 0.0.0.0:80 failed (98: Address already in use)
2026/02/16 05:36:06 [emerg] 2390#2390: bind() to [::]:80 failed (98: Address already in use)
2026/02/16 05:36:06 [emerg] 2390#2390: bind() to 0.0.0.0:80 failed (98: Address already in use)
2026/02/16 05:36:06 [emerg] 2390#2390: bind() to [::]:80 failed (98: Address already in use)
2026/02/16 05:36:06 [emerg] 2390#2390: bind() to 0.0.0.0:80 failed (98: Address already in use)
2026/02/16 05:36:06 [emerg] 2390#2390: bind() to [::]:80 failed (98: Address already in use)
2026/02/16 05:36:06 [emerg] 2390#2390: bind() to 0.0.0.0:80 failed (98: Address already in use)
2026/02/16 05:36:06 [emerg] 2390#2390: bind() to [::]:80 failed (98: Address already in use)
2026/02/16 05:36:06 [emerg] 2390#2390: bind() to 0.0.0.0:80 failed (98: Address already in use)
2026/02/16 05:36:06 [emerg] 2390#2390: bind() to [::]:80 failed (98: Address already in use)
2026/02/16 05:36:06 [emerg] 2390#2390: still could not bind()
root@Ubuntu22:~# cat /var/log/nginx/error.log.1 >> /root/email.txt
root@Ubuntu22:~#
root@Ubuntu22:~# awk '{print$0}' /var/log/nginx/access-4560-644067.log | grep -oE '\ [0-9]{3}\ ' | sort | uniq -c | sort -n
      1  304
      1  403
      1  405
      2  499
      3  500
     18  400
     51  404
     95  301
    498  200
root@Ubuntu22:~#
root@Ubuntu22:~# awk '{print$0}' /var/log/nginx/access-4560-644067.log | grep -oE '\ [0-9]{3}\ ' | sort | uniq -c | sort -n >> /root/email.txt
root@Ubuntu22:~#
```
##### добавляем обнуление email сообщения в крон (каждые 45 минут)
```
root@Ubuntu22:~# cat /dev/null > email.txt
root@Ubuntu22:~# cat email.txt
root@Ubuntu22:~#cat << EOF > /root/zeroing
> cat /dev/null > email.txt
EOF
root@Ubuntu22:~# cat << EOF >> /etc/crontab
45 * * * * root /root/zeroing
EOF
```
