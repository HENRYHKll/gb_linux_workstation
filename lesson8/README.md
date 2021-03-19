# Урок 8. Введение в docker
## Linux. Рабочая станция

Задания выполненнено на OS windows 10 pro 64
Виртуальнаая машина VirtualBox 6.1

![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson1/linux1-0.png)


## Практическое задание
- 1.Скачать контейнер с Ubuntu.
- 2.Собрать любой Dockerfile
- 3.Собранный Dockerfile залить на hub.docker.com. По результату предоставить ссылку на образ.

## 1. Установить apache2, создать вебсервера на 3-х портах и настроить раздачу для каждого своего контента

```sh
w2e@ubuntuser:~$ mkdir lesson8
w2e@ubuntuser:~$ cd lesson8
w2e@ubuntuser:~/lesson8$ sudo apt install docker.io
......
w2e@ubuntuser:~/lesson8$ docker search ubuntu
NAME                                                      DESCRIPTION                                     STARS
 OFFICIAL            AUTOMATED
ubuntu                                                    Ubuntu is a Debian-based Linux operating sys…   11953
 [OK]
dorowu/ubuntu-desktop-lxde-vnc                            Docker image to provide HTML5 VNC interface …   504
                     [OK]
websphere-liberty                                         WebSphere Liberty multi-architecture images …   267
 [OK]
rastasheep/ubuntu-sshd                                    Dockerized SSH service, built on top of offi…   250
                     [OK]
consol/ubuntu-xfce-vnc                                    Ubuntu container with "headless" VNC session…   234
                     [OK]
ubuntu-upstart                                            Upstart is an event-based replacement for th…   110
 [OK]
neurodebian                                               NeuroDebian provides neuroscience research s…   81
 [OK]
1and1internet/ubuntu-16-nginx-php-phpmyadmin-mysql-5      ubuntu-16-nginx-php-phpmyadmin-mysql-5          50
                     [OK]
ubuntu-debootstrap                                        debootstrap --variant=minbase --components=m…   44
 [OK]
open-liberty                                              Open Liberty multi-architecture images based…   42
 [OK]
nuagebec/ubuntu                                           Simple always updated Ubuntu docker images w…   24
                     [OK]
i386/ubuntu                                               Ubuntu is a Debian-based Linux operating sys…   24

1and1internet/ubuntu-16-apache-php-5.6                    ubuntu-16-apache-php-5.6                        14
                     [OK]
1and1internet/ubuntu-16-apache-php-7.0                    ubuntu-16-apache-php-7.0                        13
                     [OK]
1and1internet/ubuntu-16-nginx-php-phpmyadmin-mariadb-10   ubuntu-16-nginx-php-phpmyadmin-mariadb-10       11
                     [OK]
1and1internet/ubuntu-16-nginx-php-5.6-wordpress-4         ubuntu-16-nginx-php-5.6-wordpress-4             8
                     [OK]
1and1internet/ubuntu-16-nginx-php-5.6                     ubuntu-16-nginx-php-5.6                         8
                     [OK]
1and1internet/ubuntu-16-apache-php-7.1                    ubuntu-16-apache-php-7.1                        6
                     [OK]
pivotaldata/ubuntu                                        A quick freshening-up of the base Ubuntu doc…   4

1and1internet/ubuntu-16-nginx-php-7.0                     ubuntu-16-nginx-php-7.0                         4
                     [OK]
pivotaldata/ubuntu16.04-build                             Ubuntu 16.04 image for GPDB compilation         2

pivotaldata/ubuntu-gpdb-dev                               Ubuntu images for GPDB development              1

1and1internet/ubuntu-16-php-7.1                           ubuntu-16-php-7.1                               1
                     [OK]
smartentry/ubuntu                                         ubuntu with smartentry                          1
                     [OK]
pivotaldata/ubuntu16.04-test                              Ubuntu 16.04 image for GPDB testing             0

w2e@ubuntuser:~/lesson8$ docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
5d3b2c2d21bb: Pull complete
3fc2062ea667: Pull complete
75adf526d75b: Pull complete
Digest: sha256:b4f9e18267eb98998f6130342baacaeb9553f136142d40959a1b46d6401f0f2b
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
w2e@ubuntuser:~/lesson8$ docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
qx13/gb_linux_workstation   lesson8             f8214b3cea1f        2 hours ago         895MB
gb_linux_workstation        latest              f487d6691ed9        2 hours ago         895MB
<none>                      <none>              4c9062e9e93a        3 hours ago         895MB
python                      3.9                 2c31ca135cf9        6 days ago          885MB
ubuntu                      latest              4dd97cefde62        2 weeks ago         72.9MB
```


##  2. Установить nginx. Настроить upstream для вебсерверов на apache2

```sh
w2e@ubuntuser:~/lesson8$ sudo usermod -aG docker $USER
w2e@ubuntuser:~/lesson8$ mkdir server
w2e@ubuntuser:~/lesson8$ cd server/
w2e@ubuntuser:~/lesson8/server$ vim server.py
w2e@ubuntuser:~/lesson8/server$ cat server.py
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello User\n"


@app.route('/health')
def health():
    return "OK\n"


if __name__ == '__main__':
    app.run(host='0.0.0.0')

w2e@ubuntuser:~/lesson8/server$ python3 -m pip install flask
w2e@ubuntuser:~/lesson8/server$ python3 server.py
 * Serving Flask app "server" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
^Z
[1]+  Stopped                 python3 server.py
w2e@ubuntuser:~/lesson8/server$ bg 1
[1]+ python3 server.py &
w2e@ubuntuser:~/lesson8/server$ curl localhost:5000
127.0.0.1 - - [19/Mar/2021 16:31:58] "GET / HTTP/1.1" 200 -
Hello User
w2e@ubuntuser:~/lesson8/server$ fg 1
python3 server.py
^Cw2e@ubuntuser:~/lesson8/server$ vim requirements.txt
w2e@ubuntuser:~/lesson8/server$ cat requirements.txt
flask
w2e@ubuntuser:~/lesson8/server$ vim Dockerfile
w2e@ubuntuser:~/lesson8/server$ w2e@ubuntuser:~/lesson8/server$ cat Dockerfile
FROM python:3.9
WORKDIR /app
COPY requirements.txt /app
RUN pip3 install -r requirements.txt
COPY . /app

CMD ["python3", "server.py"]
w2e@ubuntuser:~/lesson8/server$ docker build -t gb_linux_workstation .
Sending build context to Docker daemon  4.096kB
Step 1/6 : FROM python:3.9
3.9: Pulling from library/python
e22122b926a1: Pull complete
f29e09ae8373: Pull complete
e319e3daef68: Pull complete
e499244fe254: Pull complete
5a6ebed20e89: Pull complete
56b703a5a371: Pull complete
4dd0a64393b6: Pull complete
1d2280ee5e8b: Pull complete
8b6ce844793f: Pull complete
Digest: sha256:168fd55b03929f88cd3e1e05b9ebe8f9cc1c095af8b53a8c0cd50da04a8c3a40
Status: Downloaded newer image for python:3.9
 ---> 2c31ca135cf9
Step 2/6 : WORKDIR /app
 ---> Running in af933dd9ec6d
Removing intermediate container af933dd9ec6d
 ---> 5a0df55b0a3b
Step 3/6 : COPY requirements.txt /app
 ---> aef6cb103db2

Step 4/6 : RUN pip3 install -r requirements.txt

from flask import Flask
 ---> Running in 24202165f179
Collecting flask
  Downloading Flask-1.1.2-py2.py3-none-any.whl (94 kB)
Collecting Werkzeug>=0.15
  Downloading Werkzeug-1.0.1-py2.py3-none-any.whl (298 kB)
Collecting itsdangerous>=0.24
  Downloading itsdangerous-1.1.0-py2.py3-none-any.whl (16 kB)
Collecting Jinja2>=2.10.1
  Downloading Jinja2-2.11.3-py2.py3-none-any.whl (125 kB)
Collecting click>=5.1
  Downloading click-7.1.2-py2.py3-none-any.whl (82 kB)
Collecting MarkupSafe>=0.23
  Downloading MarkupSafe-1.1.1-cp39-cp39-manylinux2010_x86_64.whl (32 kB)
Installing collected packages: MarkupSafe, Werkzeug, Jinja2, itsdangerous, click, flask
Successfully installed Jinja2-2.11.3 MarkupSafe-1.1.1 Werkzeug-1.0.1 click-7.1.2 flask-1.1.2 itsdangerous-1.1.0
Removing intermediate container 24202165f179
 ---> 0d3e14a15244
Step 5/6 : COPY . /app
 ---> c68ef3ffbb35
Step 6/6 : CMD ["python3", "server.py"]
 ---> Running in 28c1a5911de5
Removing intermediate container 28c1a5911de5
 ---> 4c9062e9e93a
Successfully built 4c9062e9e93a
Successfully tagged gb_linux_workstation:latest
w2e@ubuntuser:~/lesson8/server$ docker run -d --rm -p 5000:5000 --name=gb_docker gb_linux_workstation
34967b302ee2499e808d89fd81007f1c037f37f374dd13ba2f7a84b10eb7e88d
w2e@ubuntuser:~/lesson8/server$ docker ps
CONTAINER ID        IMAGE                  COMMAND               CREATED             STATUS              PORTS
     NAMES
34967b302ee2        gb_linux_workstation   "python3 server.py"   22 seconds ago      Up 21 seconds       0.0.0.0:5000->5000/tcp   gb_docker
w2e@ubuntuser:~/lesson8/server$ curl localhost:5000
Hello User
w2e@ubuntuser:~/lesson8/server$ curl localhost:5000/health
OK

```


##  3. Собранный Dockerfile залить на hub.docker.com. По результату предоставить ссылку на образ.

```sh
w2e@ubuntuser:~/lesson8/server$ docker commit gb_docker qx13/gb_linux_workstation:lesson8
sha256:f8214b3cea1f1e557c76650cb6b6affe30a47b7ea52889ef2a50dbfc43153470
w2e@ubuntuser:~/lesson8/server$ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: qx13
Password:
WARNING! Your password will be stored unencrypted in /home/w2e/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
w2e@ubuntuser:~/lesson8/server$ docker push qx13/gb_linux_workstation:lesson8
The push refers to repository [docker.io/qx13/gb_linux_workstation]
d1445bafd980: Pushed
9855edf94c46: Pushed
ede7459e5474: Pushed
faeccba14de2: Pushed
a38baa6362d1: Pushed
78db0f6b59c6: Mounted from library/python
b7bcac1b5fe8: Mounted from library/python
e8846e341673: Mounted from library/python
04d1717d0e01: Mounted from library/python
dacb447ffe30: Mounted from library/python
bde301416dd2: Mounted from library/python
81496d8c72c2: Mounted from library/python
644448d6e877: Mounted from library/python
0e41e5bdb921: Mounted from library/python
lesson8: digest: sha256:fd1d4d69e768e1f5aafb39ceee71f7385bd1c339ed9f47ec6d1e6d673ccf0ba0 size: 3258
```


https://hub.docker.com/layers/142221924/qx13/gb_linux_workstation/lesson8/images/sha256-fd1d4d69e768e1f5aafb39ceee71f7385bd1c339ed9f47ec6d1e6d673ccf0ba0?context=explore