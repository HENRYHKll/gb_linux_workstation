# Урок 2. Настройка и знакомство с интерфейсом командной строки
## Linux. Рабочая станция

Задания выполненнено на OS windows 10 pro 64
Виртуальнаая машина VirtualBox 6.1

![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson1/linux1-0.png)


## Практическое задание
- Навигация по файловой системе. Попрактиковаться в перемещении между каталогами, используя полный и относительный путь. Перечислить, какие параметры команды cd позволят быстро вернуться в домашний каталог, позволят перейти на уровень выше.
- Управление файлами и каталогами и текстовые редакторы. Создать файл с наполнением, используя несколько способов. Использовать разобранные текстовые редакторы для наполнения файлов данными. Создать копии созданных файлов, создать несколько каталогов с подкаталогами, перенести несколько файлов в созданные каталоги. Перечислить команды и используемые параметры команд.
- *Используя дополнительный материал, настроить авторизацию по SSH с использованием ключей.

## 1. Навигация по файловой системе....

```sh
w2e@ubuntuser:~$ ls
a.out  maim.cxx
w2e@ubuntuser:~$ mkdir lesson2
w2e@ubuntuser:~$ ll
total 60
drwxr-xr-x 5 w2e  w2e   4096 Mar  2 11:27 ./
drwxr-xr-x 3 root root  4096 Feb 24 12:10 ../
-rwxrwxr-x 1 w2e  w2e  17400 Feb 28 13:49 a.out*
-rw------- 1 w2e  w2e    128 Feb 28 14:02 .bash_history
-rw-r--r-- 1 w2e  w2e    220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 w2e  w2e   3771 Feb 25  2020 .bashrc
drwx------ 2 w2e  w2e   4096 Feb 24 12:11 .cache/
drwxrwxr-x 2 w2e  w2e   4096 Mar  2 11:27 lesson2/
drwxrwxr-x 3 w2e  w2e   4096 Feb 28 13:29 .local/
-rw-rw-r-- 1 w2e  w2e    153 Feb 28 13:36 maim.cxx
-rw-r--r-- 1 w2e  w2e    807 Feb 25  2020 .profile
-rw-r--r-- 1 w2e  w2e      0 Feb 24 16:25 .sudo_as_admin_successful
w2e@ubuntuser:~$ cd lesson2/
w2e@ubuntuser:~/lesson2$ pwd
/home/w2e/lesson2
w2e@ubuntuser:~/lesson2$ cd ../../w2e/
.cache/  lesson2/ .local/
w2e@ubuntuser:~/lesson2$ cd ../../w2e/lesson2/
w2e@ubuntuser:~/lesson2$ ~
-bash: /home/w2e: Is a directory //Ну ничего страшного... пяу пяу пяу))
w2e@ubuntuser:~/lesson2$ cd
w2e@ubuntuser:~$
```

![1. Навигация по файловой системе](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson2/linux2-0.png)

##  2. Управление файлами и каталогами и текстовые редакторы...

```sh
w2e@ubuntuser:~$ cd lesson2/ && nano test1
w2e@ubuntuser:~/lesson2$ w2e@ubuntuser:~/lesson2$ cat test1
Буря мглое небо кроет
Вихри снежнные крутя
...
w2e@ubuntuser:~/lesson2$ touch test2
w2e@ubuntuser:~/lesson2$ vim test2
w2e@ubuntuser:~/lesson2$ cat test2
то как зверь она завоет
то заплачет как дитя
w2e@ubuntuser:~/lesson2$ cat test1 test2 >test3
w2e@ubuntuser:~/lesson2$ cat test3
Буря мглое небо кроет
Вихри снежнные крутя
...
то как зверь она завоет
то заплачет как дитя
w2e@ubuntuser:~/lesson2$ cp test1 test0
w2e@ubuntuser:~/lesson2$ mkdir -p dir{1..3}/{src,done} 
/********************************************************************************************
// я не нашел способо копировать несколько файлов в несколько катологов может подлскажите ? 
*********************************************************************************************/
w2e@ubuntuser:~/lesson2$ cp test{1..2}  ./dir1/src/
w2e@ubuntuser:~/lesson2$ cp test{1..2}  ./dir2/src/
w2e@ubuntuser:~/lesson2$ cp test{1..2}  ./dir3/src/
w2e@ubuntuser:~/lesson2$ cp test3  ./dir3/done/
w2e@ubuntuser:~/lesson2$ tree -a
.
├── dir1
│   ├── done
│   └── src
│       ├── test1
│       └── test2
├── dir2
│   ├── done
│   └── src
│       ├── test1
│       └── test2
├── dir3
│   ├── done
│   │   └── test3
│   └── src
│       ├── test1
│       └── test2
├── test0
├── test1
├── test2
└── test3

9 directories, 11 files

```

##  3. Настроить авторизацию по SSH с использованием ключей
в PowerShell

```sh
PS C:\Users\xxx\.ssh> ssh-keygen -t rsa -b 2048
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\xxx/.ssh/id_rsa.
Your public key has been saved in C:\Users\xxx/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:L5ahLJOUtFWLhqLv54aeQVNY+xJ9dFfUe9PyX56PkrU xxx@PC-ASUS9600k
The key's randomart image is:
+---[RSA 2048]----+
|    .   o . .oo. |
|   o + + o .    .|
|  o * = o       o|
| . + B .      .oo|
|. o = . S      oo|
| o o + . +    . o|
|  o.+ o + .  o o+|
| ..ooo . .  o Eoo|
| .++.        .. o|
+----[SHA256]-----+
PS C:\Users\xxx\.ssh> cat id_rsa.pub >> authorized_keys
PS C:\Users\xxx\.ssh> scp authorized_keys w2e@192.168.0.106:/home/w2e/.ssh/
```
На Ubuntu Server

```sh
chmod 600 /home/w2e/.ssh/authorized_keys
```
