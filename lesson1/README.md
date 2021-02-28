# Урок 1. Введение. Установка ОС
## Linux. Рабочая станция

Задания выполненнено на OS windows 10 pro 64
Виртуальнаая машина VirtualBox 6.1

![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson1/linux1-0.png)


## Практическое задание в методичке
- Повторить процесс установки ОС, но в качестве дистрибутива использовать десктоп-версию Ubuntu. 
- *Провести установку Centos 7.x и сравнить отличия в процессе установки.
- *Провести сравнение дистрибутивов.


## Практическое задание от преподователя

- Установить операционную систему
- Получить доступ по ssh
- Чего вы ждете от курса?
- *Установить операционную систему centos
- *Установить ubuntu server

Практическое задание в методичке состоит из того чтобы, просто утановить   
два дистрбьютива и произвыести отличе.
Информция взята [hostinger.ru] пряммая ссылка [https://www.hostinger.ru/rukovodstva/centos-vs-ubuntu-web-server/][df1]

> 1.Самое большое отличие между двумя дистрибутивами Linux является то,
что Ubuntu базируется на архитектуре Debian, 
в то время как CentOS имеет свои корни в Red Hat Enterprise Linux.
> 2.В Ubuntu вы можете скачать пакеты DEB используя менеджер пакетов apt-get.
В то время как в CentOS, вам нужно использовать команду yum для скачивания 
и установки RPM пакетов из центрального репозитория.
> 3.CentOS считает более стабильным дистрибутивом, нежели Ubuntu. 
Большей частью по причине не столь частого обновления пакетов. 
Это также может оказаться недостатком CentOS. Если вы захотите 
последнюю версию определённого приложения или программы, вам придётся 
устанавливать её вручную.

По своему опыту скажу что это карденльно две разве ОС,
Ели Ubuntu про узабалити, понятность и низкий порог вхождения. 
Тогда как CentOS про более продвинутуй уровень вледение, 
так же поддержка больше и самое гланое более стабильная работа 
чем Ubuntu.

## 1. Установка centos и ubuntu server

Алгоритм

- Скачать образвы с официального сайта
- Настроить виртувльнве машины
- Уставновить (пользуешь пошаговой инструкцией)
- ✨Magic ✨

##  2. Получить доступ по ssh

У ubuntu server при установки спрашивает установить ssh server
просто соглашаемся.


Для CentOS

```sh
sudo yum check-update
sudo yum update
sudo yum upgrade
sudo yum install openssh-server openssh-clients
// Для секьюрности
//sudo vim /etc/ssh/sshd_config
// port 22  ->  port 2002
//sudo systemctl restart sshd
```

##  Получить доступ по ssh
Подключаюсь из PowerShell

```sh
//centOS
PS C:\Users\adam> ssh w2e@192.168.0.108  uname -a
w2e@192.168.0.108's password:
Linux localhost.localdomain 4.18.0-277.el8.x86_64 #1 SMP Wed Feb 3 20:35:19 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
//ubuntu server
PS C:\Users\adam> ssh w2e@192.168.0.107
w2e@192.168.0.107's password:
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-66-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun 28 Feb 2021 02:00:48 PM UTC

  System load:  0.0               Processes:               110
  Usage of /:   46.1% of 8.79GB   Users logged in:         1
  Memory usage: 9%                IPv4 address for enp0s3: 192.168.0.107
  Swap usage:   0%


16 updates can be installed immediately.
0 of these updates are security updates.
To see these additional updates run: apt list --upgradable


Last login: Sun Feb 28 13:58:02 2021 from 192.168.0.108
w2e@ubuntuser:~$ cat maim.cxx
#include <iostream>

int main ( int argc, char** args){
        std::cout <<"Hello Basic Linux cuorse in GB"
        <<std::endl;

        return std::cout.good() ? 0 : 1;
}
```

![Иллюстрация к проекту](https://github.com/HENRYHKll/gb_linux_workstation/raw/main/lesson1/linux1-1.png)


не успел паблик настроить ключи для подключение 
```sh
ssh w2e@192.168.0.108  ssh w2e@192.168.0.107 /home/w2e/a.out
>>Hello Basic Linux cuorse in GB
```


## Чего вы ждете от курса?

Вот порабы расказать и показать как работаь 
в emacs space (или spacevim https://spacevim.org/) или подбных 
ведь экономит кучу времени 