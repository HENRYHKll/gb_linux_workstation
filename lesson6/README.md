# Урок 6. Введение в скрипты bash. Планировщики задач crontab и at
## Linux. Рабочая станция

Задания выполненнено на OS windows 10 pro 64
Виртуальнаая машина VirtualBox 6.1

![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson1/linux1-0.png)


## Практическое задание
- 1.Написать скрипт, который удаляет из текстового файла пустые строки и заменяет маленькие символы на большие. Воспользуйтесь tr или SED.
- 2.Создать однострочный скрипт, который создаст директории для нескольких годов (2010–2017), в них — поддиректории для месяцев (от 01 до 12), и в каждый из них запишет несколько файлов с произвольными записями. Например, 001.txt, содержащий текст «Файл 001», 002.txt с текстом «Файл 002» и т. д.
- 3.*Использовать команду AWK на вывод длинного списка каталога, чтобы отобразить только права доступа к файлам. Затем отправить в конвейере этот вывод на sort и uniq, чтобы отфильтровать все повторяющиеся строки.
- 4.Используя grep, проанализировать файл /var/log/syslog, отобрав события на своё усмотрение.
- 5.Создать разовое задание на перезагрузку операционной системы, используя at.
- 6.* Написать скрипт, делающий архивную копию каталога etc, и прописать задание в crontab.


## 1. Написать скрипт, который удаляет из текстового файла...

```sh
w2e@ubuntuser:~/lesson6$ mkdir lesson6
w2e@ubuntuser:~/lesson6$ cd lesson6
w2e@ubuntuser:~/lesson6$ vi file
w2e@ubuntuser:~/lesson6$ cat file
Написать скрипт, который удаляет из текстового файла пустые строки и заменяет маленькие символы на большие. Воспользуйтесь tr или SED.
w2e@ubuntuser:~/lesson6$ vi task1.sh
w2e@ubuntuser:~/lesson6$ cat task1
#!/usr/bin/bash

sed -i '/^$/d' file1 > temp1
tr [:lower:] [:upper:] temp1 > out1
w2e@ubuntuser:~/lesson6$ chmod +x task1.sh
w2e@ubuntuser:~/lesson6$ ./task1.sh
w2e@ubuntuser:~/lesson6$ cat out1
НАПИСАТЬ СКРИПТ, КОТОРЫЙ УДАЛЯЕТ ИЗ ТЕКСТОВОГО ФАЙЛА ПУСТЫЕ СТРОКИ И ЗАМЕНЯЕТ МАЛЕНЬКИЕ СИМВОЛЫ НА БОЛЬШИЕ. ВОСПОЛЬЗУЙТЕСЬ TR ИЛИ SED.

```


##  2. Создать однострочный скрипт...

```sh
w2e@ubuntuser:~/lesson6$ vi task2.sh
w2e@ubuntuser:~/lesson6$ cat task2
for  y in 20{10..17}; do for m in {1..12}; do for f in {1..3}; do mkdir -p $y/$m; echo file 00$f > $y/$m/00$f.txt; done; done; done
w2e@ubuntuser:~/lesson6$ chmod +x task2.sh
w2e@ubuntuser:~/lesson6$ ./task2.sh
w2e@ubuntuser:~/lesson6$ tree
.
├── 2010
│   ├── 1
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 10
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 11
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 12
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 2
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 3
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 4
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 5
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 6
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 7
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 8
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   └── 9
│       ├── 001.txt
│       ├── 002.txt
│       └── 003.txt
├── 2011
│   ├── 1
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 10
.....
├── 2017
│   ├── 1
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 10
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 11
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 12
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 2
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 3
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 4
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 5
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 6
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 7
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   ├── 8
│   │   ├── 001.txt
│   │   ├── 002.txt
│   │   └── 003.txt
│   └── 9
│       ├── 001.txt
│       ├── 002.txt
│       └── 003.txt
├── file
├── file1
├── out
├── out1
├── output
├── task1.sh
├── task2.sh
└── temp1

```


## 3. Использовать команду AWK ...

```sh
w2e@ubuntuser:~/lesson6$ ls -l|awk '{print $1}'|sort -u|grep -vi 'done'
drwxrwxr-x
-rw-rw-r--
-rwxrwxr-x
total
```

## 4. Используя grep, проанализировать файл...


```sh
w2e@ubuntuser:~/lesson6$ cat /var/log/syslog | grep ssh
Mar 17 13:35:25 ubuntuser systemd[941]: Listening on GnuPG cryptographic agent (ssh-agent emulation).
Mar 17 13:44:55 ubuntuser systemd[1]: ssh.service: Succeeded.
```
Все события что произошли по ssh за промежуток что помощаеться в syslog

## 5.Создать разовое задание на перезагрузкуя..

```sh
w2e@ubuntuser:~/lesson6$ sudo -i
[sudo] password for w2e:
root@ubuntuser:~# echo "reboot" | at -m now +10 minute
warning: commands will be executed using /bin/sh
job 1 at Wed Mar 17 15:23:00 2021
root@ubuntuser:~# at -l
1       Wed Mar 17 15:23:00 2021 a root
root@ubuntuser:~# at -r 1
root@ubuntuser:~# at -l
```

## 6.* Написать скрипт, делающий архивную копию каталога...

![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson6/linux6-0.png)
