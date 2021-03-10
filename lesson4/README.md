# Урок 4. Загрузка ОС и процессы
## Linux. Рабочая станция

Задания выполненнено на OS windows 10 pro 64
Виртуальнаая машина VirtualBox 6.1

![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson1/linux1-0.png)


## Практическое задание
- 1.Потоки ввода/вывода. Создать файл, используя команду echo. Используя команду cat, прочитать содержимое каталога etc, ошибки перенаправить в отдельный файл.
- 2.Конвейер (pipeline). Использовать команду cut на вывод длинного списка каталога, чтобы отобразить только права доступа к файлам. Затем отправить в конвейере этот вывод на sort и uniq, чтобы отфильтровать все повторяющиеся строки.
- 3.Управление процессами. Изменить конфигурационный файл службы SSH: /etc/ssh/sshd_config, отключив аутентификацию по паролю PasswordAuthentication no. Выполните рестарт службы systemctl restart sshd (service sshd restart), верните аутентификацию по паролю, выполните reload службы systemctl reload sshd (services sshd reload). В чём различие между действиями restart и reload? Создайте файл при помощи команды cat > file_name, напишите текст и завершите комбинацией ctrl+d. Какой сигнал передадим процессу?
- 4.Сигналы процессам. Запустите mc. Используя ps, найдите PID процесса, завершите процесс, передав ему сигнал 9.

## 1. Потоки ввода/вывода. Создать файл...

```sh
w2e@ubuntuser:~$ mkdir lesson4
w2e@ubuntuser:~$ cd lesson4
w2e@ubuntuser:~/lesson4$ echo "Это задание для дз №4" > lesson4
w2e@ubuntuser:~/lesson4$ cat lesson4
Это задание для дз №4
сw2e@ubuntuser:~/lesson4$ cat > lesson4.1
Добрыйдень, Василий Кирнос #ener + ctr+d
w2e@ubuntuser:~/lesson4$ cat lesson4.1
Добрыйдень, Василий Кирнос
w2e@ubuntuser:~/lesson4$ cat /etc 2> file-err
w2e@ubuntuser:~/lesson4$ cat file-err
cat: /etc: Is a directory
w2e@ubuntuser:~/lesson4$ ll
total 20
drwxrwxr-x 2 w2e w2e 4096 Mar  9 15:45 ./
drwxr-xr-x 8 w2e w2e 4096 Mar  9 15:35 ../
-rw-rw-r-- 1 w2e w2e   26 Mar  9 15:46 file-err
-rw-rw-r-- 1 w2e w2e   39 Mar  9 15:37 lesson4
-rw-rw-r-- 1 w2e w2e   43 Mar  9 15:41 lesson4.1

```

##  2.Конвейер (pipeline)...

```sh
w2e@ubuntuser:~/lesson4$ ls -l /etc/ | cut -b 1-10 | sort | uniq -d
drwxr-xr-x
lrwxrwxrwx
-rw-r-----
-rw-r--r--

```

##  3.Управление процессами...

```sh
w2e@ubuntuser:~/lesson4$ sudo vim /etc/ssh/sshd_config
[sudo] password for w2e:
.......
#       ForceCommand cvs server
PasswordAuthentication no
w2e@ubuntuser:~/lesson4$ sudo systemctl restart sshd
w2e@ubuntuser:~/lesson4$ sudo vim /etc/ssh/sshd_config
w2e@ubuntuser:~/lesson4$ tail -n1  /etc/ssh/sshd_config
PasswordAuthentication yes
w2e@ubuntuser:~/lesson4$ sudo systemctl reload sshd
############################################################
# reload - подтягивает просто конйиг с новыми настройками  #
# restart - перезагрузает полностью процес                 #
############################################################
w2e@ubuntuser:~/lesson4$ cat > file_name
Спасибо за задние, хотелось бы сложнее чуть чуть)
w2e@ubuntuser:~/lesson4$ ls
file-err  file_name  lesson4  lesson4.1
w2e@ubuntuser:~/lesson4$ cat file_name
Спасибо за задние, хотелось бчы сложнее чуть чуть)
############################################################
#Нажатие Ctrl + D говорит терминалу, что надо 
#зарегистрировать так называемый EOF 
#(end of file – конец файла), то есть поток 
#ввода окончен. Bash интерпретирует это как
#желание выйти из программы.
############################################################
```

## 4.* Сигналы процессам. Запустите mc...

![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson4/linux4-0.png)

```sh
w2e@ubuntuser:~$ ps -efl | grep mc
1 I root        2916       2  0  80   0 -     0 -      09:08 ?        00:00:01 [kworker/0:2-memcg_kmem_cache]
0 S w2e         3463    1038  0  80   0 -  4446 poll_s 09:37 pts/0    00:00:00 mc
0 S w2e         3473    3454  0  80   0 -  1575 pipe_w 09:38 pts/1    00:00:00 grep --color=auto mc
w2e@ubuntuser:~$ kill -9 3463
```