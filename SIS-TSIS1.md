Введение в системное администрирование: 

СРО: Проведите установку и настройку операционной системы на виртуальной машине (например, Ubuntu Server). Настройте пользователи, группы и простейшие сценарии автоматизации через cron. Подготовьте отчет с командной строкой действий и выводом системных команд. 

СРОП: Практическая работа по созданию и настройке виртуальной сети в VirtualBox, установка серверных служб (например, SSH), управление пользователями и настройка прав доступа для них. 


1. Установка и настройка операционной системы (СРО)

1.1. Установка виртуальной машины: Для выполнения задания была создана виртуальная машина в VirtualBox со следующими параметрами:

    Имя машины: Linux-Administration
    Операционная система: Ubuntu (64-bit)
    Оперативная память: 2048 МБ
    Жесткий диск: 20 ГБ, динамически расширяемый

Скачан образ Ubuntu Server с официального сайта, и настроена виртуальная машина на загрузку с этого образа. Установка операционной системы прошла успешно, использованы настройки по умолчанию (автоматическая разметка диска, базовая настройка сети).

После установки системы был выполнен вход под учетной записью администратора. Далее были выполнены следующие команды:

```console
stacy@stacy-VirtualBox:~$ sudo adduser devops
[sudo] пароль для stacy: 
Добавляется пользователь «devops» ...
Добавляется новая группа «devops» (1001) ...
Добавляется новый пользователь «devops» (1001) в группу «devops» ...
Создаётся домашний каталог «/home/devops» ...
Копирование файлов из «/etc/skel» ...
Новый пароль : 
Повторите ввод нового пароля : 
passwd: пароль успешно обновлён
Изменение информации о пользователе devops
Введите новое значение или нажмите ENTER для выбора значения по умолчанию
	Полное имя []: 
	Номер комнаты []: 
	Рабочий телефон []: 
	Домашний телефон []: 
	Другое []: 
Данная информация корректна? [Y/n] Y
stacy@stacy-VirtualBox:~$ sudo usermod -aG sudo devops
stacy@stacy-VirtualBox:~$ groups devops
devops : devops sudo
```

1.3. Настройка cron для автоматизации:

Для автоматизации задач был настроен планировщик cron:


```console
stacy@stacy-VirtualBox:~$ crontab -e
crontab: installing new crontab
stacy@stacy-VirtualBox:~$ crontab -l
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command
* * * * * /home/devops/backup.sh
```


2. Настройка виртуальной сети и серверных служб (СРОП)

2.1. Создание виртуальной сети:

В настройках VirtualBox для обеих виртуальных машин была настроена внутренняя сеть (Internal Network) для обмена данными между ними:

    Открыт раздел "Сеть" для каждой машины.
    Выбрана опция "Внутренняя сеть".

После этого виртуальные машины могут обмениваться данными внутри одной сети.

2.2. Установка и настройка SSH:

Для удаленного доступа через терминал был установлен сервер SSH:
```console
stacy@stacy-VirtualBox:~$ sudo apt update
[sudo] пароль для stacy: 
12Попробуйте ещё раз.
[sudo] пароль для stacy: 
Сущ:1 http://security.ubuntu.com/ubuntu focal-security InRelease               
Сущ:2 http://kz.archive.ubuntu.com/ubuntu focal InRelease                      
Пол:3 http://kz.archive.ubuntu.com/ubuntu focal-updates InRelease [128 kB]
Сущ:4 http://kz.archive.ubuntu.com/ubuntu focal-backports InRelease            
Получено 128 kB за 7с (19,2 kB/s)                                              
Чтение списков пакетов… Готово
Построение дерева зависимостей       
Чтение информации о состоянии… Готово
Может быть обновлено 347 пакетов. Запустите «apt list --upgradable» для их показа.
stacy@stacy-VirtualBox:~$ sudo systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: e>
     Active: active (running) since Mon 2024-10-28 14:55:45 +06; 5s ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 7009 (sshd)
      Tasks: 1 (limit: 5152)
     Memory: 1.0M
     CGroup: /system.slice/ssh.service
             └─7009 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups

Қаз 28 14:55:45 stacy-VirtualBox systemd[1]: Starting OpenBSD Secure Shell serv>
Қаз 28 14:55:45 stacy-VirtualBox sshd[7009]: Server listening on 0.0.0.0 port 2>
Қаз 28 14:55:45 stacy-VirtualBox sshd[7009]: Server listening on :: port 22.
Қаз 28 14:55:45 stacy-VirtualBox systemd[1]: Started OpenBSD Secure Shell serve>
stacy@stacy-VirtualBox:~$ sudo ufw allow ssh
Правила обновлены
Правила обновлены (v6)
```

2.3. Управление пользователями и настройка прав доступа:

Были созданы пользователи и настроены права доступа:
```console
stacy@stacy-VirtualBox:~$ sudo adduser student
Добавляется пользователь «student» ...
Добавляется новая группа «student» (1002) ...
Добавляется новый пользователь «student» (1002) в группу «student» ...
Создаётся домашний каталог «/home/student» ...
Копирование файлов из «/etc/skel» ...
Новый пароль : 
Повторите ввод нового пароля : 
passwd: пароль успешно обновлён
Изменение информации о пользователе student
Введите новое значение или нажмите ENTER для выбора значения по умолчанию
	Полное имя []: 
	Номер комнаты []: 
	Рабочий телефон []: 
	Домашний телефон []: 
	Другое []: 
Данная информация корректна? [Y/n] Y
stacy@stacy-VirtualBox:~$ sudo usermod -aG sudo student
stacy@stacy-VirtualBox:~$ sudo touch /home/student/private_file.txt
stacy@stacy-VirtualBox:~$ sudo chmod 700 /home/student/private_file.txt
stacy@stacy-VirtualBox:~$ sudo chown student /home/student/private_file.txt
stacy@stacy-VirtualBox:~$ ls -l /home/student/
итого 0
-rwx------ 1 student root 0 Қаз 28 15:03 private_file.txt
```

Заключение:

В ходе работы были выполнены следующие задачи:

    Установлена операционная система Ubuntu Server на виртуальной машине.
    Настроены пользователи и группы, а также реализована автоматизация с помощью cron.
    Настроена виртуальная сеть и установлены серверные службы, такие как SSH. Также были настроены права доступа для пользователей.

Все команды, использованные в процессе выполнения работы, приведены выше вместе с их выводом.
