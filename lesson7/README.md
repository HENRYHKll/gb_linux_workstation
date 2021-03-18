# Урок 7. Управление пакетами и репозиториями. Основы сетевой безопасности
## Linux. Рабочая станция

Задания выполненнено на OS windows 10 pro 64
Виртуальнаая машина VirtualBox 6.1

![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson1/linux1-0.png)


## Практическое задание
- 1.Установить apache2, создать вебсервера на 3-х портах и настроить раздачу для каждого своего контента
- 2.Установить nginx. Настроить upstream для вебсерверов на apache2
- 3.В комментарии вставить лог, который демонстрирует, что запрос к upstream отдает разный контент.


## 1. Установить apache2, создать вебсервера на 3-х портах и настроить раздачу для каждого своего контента

```sh
w2e@ubuntuser:~$ sudo -i
root@ubuntuser:~# apt install apache2
......
root@ubuntuser:~#  vim /etc/apache2/ports.conf
root@ubuntuser:~# cat  /etc/apache2/ports.conf
# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

#Listen 80
Listen 8081
Listen 8082
Listen 8083
<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
root@ubuntuser:~# ss -tnlp
State           Recv-Q          Send-Q                   Local Address:Port                   Peer Address:Port
Process
LISTEN          0               4096                     127.0.0.53%lo:53                          0.0.0.0:*
 users:(("systemd-resolve",pid=616,fd=13))
LISTEN          0               128                            0.0.0.0:22                          0.0.0.0:*
 users:(("sshd",pid=687,fd=3))
LISTEN          0               511                                  *:80                                *:*
 users:(("apache2",pid=1591,fd=4),("apache2",pid=1590,fd=4),("apache2",pid=1577,fd=4))
LISTEN          0               128                               [::]:22                             [::]:*
 users:(("sshd",pid=687,fd=4))
root@ubuntuser:~# systemctl reload apache2
root@ubuntuser:~# ss -tnlp
State           Recv-Q          Send-Q                   Local Address:Port                   Peer Address:Port
Process
LISTEN          0               4096                     127.0.0.53%lo:53                          0.0.0.0:*
 users:(("systemd-resolve",pid=616,fd=13))
LISTEN          0               128                            0.0.0.0:22                          0.0.0.0:*
 users:(("sshd",pid=687,fd=3))
LISTEN          0               511                                  *:8081                              *:*
 users:(("apache2",pid=2180,fd=6),("apache2",pid=2179,fd=6),("apache2",pid=1577,fd=6))
LISTEN          0               511                                  *:8082                              *:*
 users:(("apache2",pid=2180,fd=10),("apache2",pid=2179,fd=10),("apache2",pid=1577,fd=10))
LISTEN          0               511                                  *:8083                              *:*
 users:(("apache2",pid=2180,fd=12),("apache2",pid=2179,fd=12),("apache2",pid=1577,fd=12))
LISTEN          0               128                               [::]:22                             [::]:*
 users:(("sshd",pid=687,fd=4))
root@ubuntuser:~# grep -v "#" /etc/apache2/sites-available/000-default.conf | grep -v "^$" > /etc/apache2/sites-available/8081.conf
root@ubuntuser:~# grep -v "#" /etc/apache2/sites-available/000-default.conf | grep -v "^$" > /etc/apache2/sites-available/8082.conf
root@ubuntuser:~# grep -v "#" /etc/apache2/sites-available/000-default.conf | grep -v "^$" > /etc/apache2/sites-available/8083.conf
root@ubuntuser:~# cd  /var/www/ && ll
total 12
drwxr-xr-x  3 root root 4096 Mar 18 11:53 ./
drwxr-xr-x 14 root root 4096 Mar 18 11:53 ../
drwxr-xr-x  2 root root 4096 Mar 18 11:53 html/
root@ubuntuser:/var/www# cp -r html/ 8081/
root@ubuntuser:/var/www# vim 8081/index.html
root@ubuntuser:/var/www# cat 8081/index.html

<head>
    <title>Welcom to port 8081</title>
    </style>
  </head>
  <body>
       <p>Welcom to port 8081</p>
  </body>
</html>
root@ubuntuser:/var/www# cp -r 8081/ 8082
root@ubuntuser:/var/www# cp -r 8081/ 8083
root@ubuntuser:/var/www# sed -i 's/8081/8082/g' 8082/index.html
root@ubuntuser:/var/www# sed -i 's/8081/8083/g' 8083/index.html
root@ubuntuser:/var/www# cat 8082/index.html

<head>
    <title>Welcom to port 8082</title>
    </style>
  </head>
  <body>
       <p>Welcom to port 8082</p>
  </body>
</html>

root@ubuntuser:/var/www# cat 8083/index.html

<head>
    <title>Welcom to port 8083</title>
    </style>
  </head>
  <body>
       <p>Welcom to port 8083</p>
  </body>
</html>
root@ubuntuser:/var/www# sed -i 's/80/8082/g' /etc/apache2/sites-available/8082.conf
root@ubuntuser:/var/www# sed -i 's/html/8082/g' /etc/apache2/sites-available/8082.conf
root@ubuntuser:/var/www# sed -i 's/80/8083/g' /etc/apache2/sites-available/8083.conf
root@ubuntuser:/var/www# sed -i 's/html/8083/g' /etc/apache2/sites-available/8083.conf
root@ubuntuser:/var/www# cat /etc/apache2/sites-available/8083.conf
<VirtualHost *:8083>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/8083
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
root@ubuntuser:/var/www# cat /etc/apache2/sites-available/8082.conf
<VirtualHost *:8082>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/8082
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
root@ubuntuser:/var/www# cd /etc/apache2/sites-enabled/
root@ubuntuser:/etc/apache2/sites-enabled# ls
000-default.conf
root@ubuntuser:/etc/apache2/sites-enabled# a2ensite 808{1..3}
Enabling site 8081.
Enabling site 8082.
Enabling site 8083.
To activate the new configuration, you need to run:
  systemctl reload apache2
root@ubuntuser:/etc/apache2/sites-enabled# systemctl reload apache2
root@ubuntuser:/etc/apache2/sites-enabled# curl localhost:808{1..3} | grep 808
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  10153      0 --:--:-- --:--:-- --:--:-- 10153
    <title>Welcom to port 8081</title>
       <p>Welcom to port 8081</p>
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  66000      0 --:--:-- --:--:-- --:--:-- 66000
    <title>Welcom to port 8082</title>
       <p>Welcom to port 8082</p>
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  66000      0 --:--:-- --:--:-- --:--:-- 66000
    <title>Welcom to port 8083</title>
       <p>Welcom to port 8083</p>
```


##  2. Установить nginx. Настроить upstream для вебсерверов на apache2

```sh
root@ubuntuser:/etc/apache2/sites-enabled# apt install nginx
..........
root@ubuntuser:/etc/nginx/conf.d# vim upsream.conf
root@ubuntuser:/etc/nginx/conf.d# cat upsream.conf
upstream apache2 {
        server 127.0.0.1:8081;
        server 127.0.0.1:8082;
        server 127.0.0.1:8083;
}
root@ubuntuser:/etc/nginx/conf.d# vim /etc/nginx/sites-enabled/default
root@ubuntuser:/etc/nginx/conf.d# cat /etc/nginx/sites-enabled/default | sed -n 48,54p
        location / {
                proxy_pass http://apache2;
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }
root@ubuntuser:/etc/nginx/conf.d# systemctl reload nginx.service
root@ubuntuser:/etc/nginx/conf.d# curl localhost:80

<head>
    <title>Welcom to port 8081</title>
    </style>
  </head>
  <body>
       <p>Welcom to port 8081</p>
  </body>
</html>

root@ubuntuser:/etc/nginx/conf.d# curl localhost:80

<head>
    <title>Welcom to port 8082</title>
    </style>
  </head>
  <body>
       <p>Welcom to port 8082</p>
  </body>
</html>

root@ubuntuser:/etc/nginx/conf.d# curl localhost:80

<head>
    <title>Welcom to port 8083</title>
    </style>
  </head>
  <body>
       <p>Welcom to port 8083</p>
  </body>
</html>

root@ubuntuser:/etc/nginx/conf.d# curl localhost:80

<head>
    <title>Welcom to port 8081</title>
    </style>
  </head>
  <body>
       <p>Welcom to port 8081</p>
  </body>
</html>
```


##  3. В комментарии вставить лог, который демонстрирует, что запрос к upstream отдает разный контент.

```sh
root@ubuntuser:/etc/nginx/conf.d# for i in {1..13}; do curl localhost:80 | grep 808 ; done > /home/w2e/hw-lesson7
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  11000      0 --:--:-- --:--:-- --:--:-- 11000
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  14666      0 --:--:-- --:--:-- --:--:-- 14666
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  13200      0 --:--:-- --:--:-- --:--:-- 13200
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  14666      0 --:--:-- --:--:-- --:--:-- 14666
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  16500      0 --:--:-- --:--:-- --:--:-- 16500
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  14666      0 --:--:-- --:--:-- --:--:-- 14666
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  11000      0 --:--:-- --:--:-- --:--:-- 11000
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  11000      0 --:--:-- --:--:-- --:--:-- 11000
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  12000      0 --:--:-- --:--:-- --:--:-- 12000
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  11000      0 --:--:-- --:--:-- --:--:-- 11000
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  13200      0 --:--:-- --:--:-- --:--:-- 13200
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  13200      0 --:--:-- --:--:-- --:--:-- 13200
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   132  100   132    0     0  12000      0 --:--:-- --:--:-- --:--:-- 12000
root@ubuntuser:/etc/nginx/conf.d# cat /home/w2e/hw-lesson7
    <title>Welcom to port 8082</title>
       <p>Welcom to port 8082</p>
    <title>Welcom to port 8083</title>
       <p>Welcom to port 8083</p>
    <title>Welcom to port 8081</title>
       <p>Welcom to port 8081</p>
    <title>Welcom to port 8082</title>
       <p>Welcom to port 8082</p>
    <title>Welcom to port 8083</title>
       <p>Welcom to port 8083</p>
    <title>Welcom to port 8081</title>
       <p>Welcom to port 8081</p>
    <title>Welcom to port 8082</title>
       <p>Welcom to port 8082</p>
    <title>Welcom to port 8083</title>
       <p>Welcom to port 8083</p>
    <title>Welcom to port 8081</title>
       <p>Welcom to port 8081</p>
    <title>Welcom to port 8082</title>
       <p>Welcom to port 8082</p>
    <title>Welcom to port 8083</title>
       <p>Welcom to port 8083</p>
    <title>Welcom to port 8081</title>
       <p>Welcom to port 8081</p>
    <title>Welcom to port 8082</title>
       <p>Welcom to port 8082</p>
```
