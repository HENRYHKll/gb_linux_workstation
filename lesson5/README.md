# Урок  5. Устройство файловой системы Linux. Понятие Файла и каталога
## Linux. Рабочая станция

Задания выполненнено на OS windows 10 pro 64
Виртуальнаая машина VirtualBox 6.1

![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson1/linux1-0.png)


## Практическое задание
- 1.Создать файл file1 и наполнить его произвольным содержимым. Скопировать его в file2. Создать символическую ссылку file3 на file1. Создать жёсткую ссылку file4 на file1. Посмотреть, какие inode у файлов. Удалить file1. Что стало с остальными созданными файлами? Попробовать вывести их на экран.
- 2.Дать созданным файлам другие, произвольные имена. Создать новую символическую ссылку. Переместить ссылки в другую директорию.
- 3.Создать два произвольных файла. Первому присвоить права на чтение и запись для владельца и группы, только на чтение — для всех. Второму присвоить права на чтение и запись только для владельца. Сделать это в численном и символьном виде.
- 4.*Создать группу developer и нескольких пользователей, входящих в неё. Создать директорию для совместной работы. Сделать так, чтобы созданные одними пользователями файлы могли изменять другие пользователи этой группы.
- 5.* Создать в директории для совместной работы поддиректорию для обмена файлами, но чтобы удалять файлы могли только их создатели.
- 6.* Создать директорию, в которой есть несколько файлов. Сделать так, чтобы открыть файлы можно было, только зная имя файла, а через ls список файлов посмотреть было нельзя.

## 1. Создать файл file1 и наполнить его произвольным содержимым...

```sh
w2e@ubuntuser:~/lesson5$ vim file1
w2e@ubuntuser:~/lesson5$ w2e@ubuntuser:~/lesson5$
w2e@ubuntuser:~/lesson5$ cat file1
ГАНАРЫСТЫ ПАРСЮК
1927
Бывае, праўда вочы коле...
Раз гнаў пастух свіней у поле.
Адзін вялізарны Парсюк,
Які абегаў вёску ўсю,
За раніцу абшнырыў завуголле,
Цяпер такі меў выгляд важны,
Што носа не дастаць і сажнем
Вышэй за ўсіх ён сам сябе лічыў,
А што ў самога на лычы,
Не бачыў гэтага, аднак.
I вось адзін тут Падсвінак,
Які заўважыў бруд раней,
I кажа:  Дзядзечка, твой лыч у брудзе!
Нязграбна гэта й між свіней,
А што ж, калі заўважаць людзі?
Парсюк наставіў хіб, Парсюк раз'юшан'
Цераз цябе я чырванець прымушан!
Такое мне сказаць асмеляцца нямногія,
Ды гэта ж  дэмагогія!
Парсюк наш лаецца, не дараваць клянецца:
I месца мокрага, крычыць, не застанецца!
Ты мой свінячы гонар закрануў!
I так ён Падсвінака грызянуў,
Што той за сажняў пяць адскочыў.
Парсюк не надта быў ахвочы
Глядзецца праўдзе ў вочы.
w2e@ubuntuser:~/lesson5$ cp file1 file2
w2e@ubuntuser:~/lesson5$  ln -s $(pwd)/file1 file3
w2e@ubuntuser:~/lesson5$ ln file1 file4
w2e@ubuntuser:~/lesson5$ ls -li
total 12
286424 -rw-rw-r-- 2 w2e w2e 1439 Mar 10 10:58 file1
279918 -rw-rw-r-- 1 w2e w2e 1439 Mar 10 11:26 file2
280044 lrwxrwxrwx 1 w2e w2e   23 Mar 10 11:30 file3 -> /home/w2e/lesson5/file1
286424 -rw-rw-r-- 2 w2e w2e 1439 Mar 10 10:58 file4
w2e@ubuntuser:~/lesson5$ rm file1
w2e@ubuntuser:~/lesson5$ ls -li
total 8
279918 -rw-rw-r-- 1 w2e w2e 1439 Mar 10 11:26 file2
280044 lrwxrwxrwx 1 w2e w2e   23 Mar 10 11:30 file3 -> /home/w2e/lesson5/file1
286424 -rw-rw-r-- 1 w2e w2e 1439 Mar 10 10:58 file4
```
Хардлин жив так как сслыеться inode, а софт линк это имеет 
оделную inode каторая указывветна нсуществующее inode 
поэтому "Битая сслка" ))

![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson5/linux5-0.png)


##  2. Дать созданным файлам другие, произвольные имена...

```sh
w2e@ubuntuser:~/lesson5$ ls
file2  file3  file4
w2e@ubuntuser:~/lesson5$ rm file3
w2e@ubuntuser:~/lesson5$ ls
file2  file4
w2e@ubuntuser:~/lesson5$ mv file4 hardlink
w2e@ubuntuser:~/lesson5$ mv file2 src
w2e@ubuntuser:~/lesson5$ ln -s $(pwd)/src ../softlink
w2e@ubuntuser:~/lesson5$ ls -li
total 8
286424 -rw-rw-r-- 1 w2e w2e 1439 Mar 10 10:58 hardlink
279918 -rw-rw-r-- 1 w2e w2e 1439 Mar 10 11:26 src
w2e@ubuntuser:~/lesson5$ ls -li ../
total 36
276987 -rwxrwxr-x 1 w2e w2e 17400 Feb 28 13:49 a.out
278950 drwxrwxr-x 5 w2e w2e  4096 Mar  2 11:54 lesson2
287018 drwxrwxr-x 2 w2e w2e  4096 Mar 10 09:09 lesson4
287027 drwxrwxr-x 2 w2e w2e  4096 Mar 10 15:08 lesson5
270284 -rw-rw-r-- 1 w2e w2e   153 Feb 28 13:36 maim.cxx
270162 lrwxrwxrwx 1 w2e w2e    21 Mar 10 15:09 softlink -> /home/w2e/lesson5/src
```

![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson5/linux5-1.png)

##  3.Создать два произвольных файла...

```sh
w2e@ubuntuser:~/lesson5$ ls
hardlink  src
w2e@ubuntuser:~/lesson5$ touch file{1..2}
w2e@ubuntuser:~/lesson5$ ls
file1  file2  hardlink  src
w2e@ubuntuser:~/lesson5$ chmod 664 file1
w2e@ubuntuser:~/lesson5$ chmod 600 file2
w2e@ubuntuser:~/lesson5$ ls -l
total 8
-rw-rw-r-- 1 w2e w2e    0 Mar 10 15:14 file1
-rw------- 1 w2e w2e    0 Mar 10 15:14 file2
-rw-rw-r-- 1 w2e w2e 1439 Mar 10 10:58 hardlink
-rw-rw-r-- 1 w2e w2e 1439 Mar 10 11:26 src
w2e@ubuntuser:~/lesson5$
```

## 4.* Создать группу developer...


```sh
w2e@ubuntuser:~/lesson5$ sudo groupadd developer
[sudo] password for w2e:
w2e@ubuntuser:~/lesson5$ sudo usermod -a -G developer w2e
w2e@ubuntuser:~/lesson5$ id w2e
uid=1000(w2e) gid=1000(w2e) groups=1000(w2e),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),116(lxd),1003(developer)
w2e@ubuntuser:~/lesson5$ sudo usermod -a -G developer test2
w2e@ubuntuser:~/lesson5$ id test2
uid=1001(test2) gid=1001(test2) groups=1001(test2),1002(dev11),1003(developer)
w2e@ubuntuser:~/lesson5$ cd /home/
w2e@ubuntuser:/home$ ls
test2  w2e
w2e@ubuntuser:/home$ sudo chmod g+s developer-sharad/
[sudo] password for w2e:
w2e@ubuntuser:/home$ ls
developer-sharad  test2  w2e
w2e@ubuntuser:/home$ sudo chown :developer developer-sharad
w2e@ubuntuser:/home$ stat developer-sharad/
  File: developer-sharad/
  Size: 4096            Blocks: 8          IO Block: 4096   directory
Device: fd00h/64768d    Inode: 163854      Links: 2
Access: (2755/drwxr-sr-x)  Uid: (    0/    root)   Gid: ( 1003/developer)
Access: 2021-03-10 15:58:05.121757792 +0000
Modify: 2021-03-10 15:35:40.239081537 +0000
Change: 2021-03-10 16:02:22.103037867 +0000
 Birth: -

```

##5.*Создать в директории для..

```sh
w2e@ubuntuser:/home$ sudo chmod u+s developer-sharad
w2e@ubuntuser:/home$ stat developer-sharad
  File: developer-sharad
  Size: 4096            Blocks: 8          IO Block: 4096   directory
Device: fd00h/64768d    Inode: 163854      Links: 2
Access: (6755/drwsr-sr-x)  Uid: (    0/    root)   Gid: ( 1003/developer)
Access: 2021-03-10 15:58:05.121757792 +0000
Modify: 2021-03-10 15:35:40.239081537 +0000
Change: 2021-03-10 16:13:23.214409648 +0000
 Birth: -
```

## 6.* Создать директорию...

```sh
w2e@ubuntuser:/home$ cd w2e/lesson5
w2e@ubuntuser:~/lesson5$ ls
file1  file2  hardlink  src
w2e@ubuntuser:~/lesson5$ cd ..
w2e@ubuntuser:~$ chmod -r lesson5
w2e@ubuntuser:~$ ls lesson5
ls: cannot open directory 'lesson5': Permission denied
w2e@ubuntuser:~$ cat lesson5/src
ГАНАРЫСТЫ ПАРСЮК
1927
Бывае, праўда вочы коле...
Раз гнаў пастух свіней у поле.
Адзін вялізарны Парсюк,
Які абегаў вёску ўсю,
За раніцу абшнырыў завуголле,
Цяпер такі меў выгляд важны,
Што носа не дастаць і сажнем
Вышэй за ўсіх ён сам сябе лічыў,
А што ў самога на лычы,
Не бачыў гэтага, аднак.
I вось адзін тут Падсвінак,
Які заўважыў бруд раней,
I кажа:  Дзядзечка, твой лыч у брудзе!
Нязграбна гэта й між свіней,
А што ж, калі заўважаць людзі?
Парсюк наставіў хіб, Парсюк раз'юшан:
 Цераз цябе я чырванець прымушан!
Такое мне сказаць асмеляцца нямногія,
Ды гэта ж  дэмагогія!
Парсюк наш лаецца, не дараваць клянецца:
 I месца мокрага, крычыць, не застанецца!
Ты мой свінячы гонар закрануў!
I так ён Падсвінака грызянуў,
Што той за сажняў пяць адскочыў.
Парсюк не надта быў ахвочы
Глядзецца праўдзе ў вочы.
```