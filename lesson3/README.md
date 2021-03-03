# Урок 3. Пользователи. Управление Пользователями и группами
## Linux. Рабочая станция

Задания выполненнено на OS windows 10 pro 64
Виртуальнаая машина VirtualBox 6.1

![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson1/linux1-0.png)


## Практическое задание
- 1.Управление пользователями:
a) создать пользователя, используя утилиту useradd;
b) удалить пользователя, используя утилиту userdel;
c) создать пользователя в ручном режиме.
- 2.Управление группами:
a) создать группу с использованием утилит и в ручном режиме;
b) попрактиковаться в смене групп у пользователей;
c) добавить пользователя в группу, не меняя основной;
d) удалить пользователя из группы.
- 3.Создать пользователя с правами суперпользователя. Сделать так, чтобы sudo не требовал пароль для выполнения команд.
- 4.* Используя дополнительные материалы, выдать одному из созданных пользователей право на выполнение ряда команд, требующих прав суперпользователя (команды выбираем на своё усмотрение).

## 1. Управление пользователями

```sh
w2e@ubuntuser:~$ sudo useradd test
[sudo] password for w2e:
w2e@ubuntuser:~$ id
uid=1000(w2e) gid=1000(w2e) groups=1000(w2e),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),116(lxd)
w2e@ubuntuser:~$ id test
uid=1001(test) gid=1001(test) groups=1001(test)
w2e@ubuntuser:~$ sudo userdel test
w2e@ubuntuser:~$ id test
id: ‘test’: no such user
//вручную
w2e@ubuntuser:~$ sudo useradd test
[sudo] password for w2e:
w2e@ubuntuser:~$ id
uid=1000(w2e) gid=1000(w2e) groups=1000(w2e),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),116(lxd)
w2e@ubuntuser:~$ id test
uid=1001(test) gid=1001(test) groups=1001(test)
w2e@ubuntuser:~$ sudo userdel test
w2e@ubuntuser:~$ id test
id: ‘test’: no such user
w2e@ubuntuser:~$ sudo vim /etc/passwd
w2e@ubuntuser:~$ id test2
uid=1001(test2) gid=1001 groups=1001
w2e@ubuntuser:~$ tail -n1 /etc/passwd
test2:x:1001:1001:User Ttest2:/home/test2:/bin/bash
w2e@ubuntuser:~$ sudo passwd test2
[sudo] password for w2e:
New password:
Retype new password:
passwd: password updated successfully
w2e@ubuntuser:~$ sudo mkdir /home/test2/
w2e@ubuntuser:~$ su - test2
Password:
groups: cannot find name for group ID 1001
test2@ubuntuser:~$ ls
test2@ubuntuser:~$ pwd
/home/test2
test2@ubuntuser:~$
test2@ubuntuser:~$ su - w2e
Password:
w2e@ubuntuser:~$
```

##  2. Управление группами

```sh
w2e@ubuntuser:~$ sudo head -n 1 vim /etc/group
w2e@ubuntuser:~$ tail -n1 /etc/group
test2:x:1001:
w2e@ubuntuser:~$ id test2
uid=1001(test2) gid=1001(test2) groups=1001(test2)
w2e@ubuntuser:~$ id
uid=1000(w2e) gid=1000(w2e) groups=1000(w2e),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),116(lxd)
w2e@ubuntuser:~$ sudo usermod -a -G cdrom test2
[sudo] password for w2e:
w2e@ubuntuser:~$ id test2
uid=1001(test2) gid=1001(test2) groups=1001(test2),24(cdrom)
w2e@ubuntuser:~$ sudo vim /etc/group // отредактировал на cdrom:x:24:w2e
w2e@ubuntuser:~$ head -n 18 vim /etc/group 
head: cannot open 'vim' for reading: No such file or directory
==> /etc/group <==
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:syslog,w2e
tty:x:5:syslog
disk:x:6:
lp:x:7:
mail:x:8:
news:x:9:
uucp:x:10:
man:x:12:
proxy:x:13:
kmem:x:15:
dialout:x:20:
fax:x:21:
voice:x:22:
cdrom:x:24:w2e
w2e@ubuntuser:~$ sudo usermod -a -G cdrom test2
w2e@ubuntuser:~$ head -n 18 vim /etc/group
head: cannot open 'vim' for reading: No such file or directory
==> /etc/group <==
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:syslog,w2e
tty:x:5:syslog
disk:x:6:
lp:x:7:
mail:x:8:
news:x:9:
uucp:x:10:
man:x:12:
proxy:x:13:
kmem:x:15:
dialout:x:20:
fax:x:21:
voice:x:22:
cdrom:x:24:w2e,test2
w2e@ubuntuser:~$ sudo gpasswd -d test2 cdrom
Removing user test2 from group cdrom
w2e@ubuntuser:~$ head -n 18 vim /etc/group
head: cannot open 'vim' for reading: No such file or directory
==> /etc/group <==
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:syslog,w2e
tty:x:5:syslog
disk:x:6:
lp:x:7:
mail:x:8:
news:x:9:
uucp:x:10:
man:x:12:
proxy:x:13:
kmem:x:15:
dialout:x:20:
fax:x:21:
voice:x:22:
cdrom:x:24:w2e
w2e@ubuntuser:~$
w2e@ubuntuser:~$ sudo addgroup dev11
Adding group `dev11' (GID 1002) ...
Done.
w2e@ubuntuser:~$ sudo usermod -a -G dev11 test2
w2e@ubuntuser:~$ id test2
uid=1001(test2) gid=1001(test2) groups=1001(test2),1002(dev11)

```

##  3. Создать пользователя с правами суперпользователя

```sh
w2e@ubuntuser:~$ sudo visudo
```
![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson3/linux3-0.png)

## 4.* Используя дополнительные материалы..

![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson3/linux3-1.png)