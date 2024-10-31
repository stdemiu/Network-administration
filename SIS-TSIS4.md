## Задание 4.	Управление пользователями и правами доступа:
СРО: Настройте управление пользователями на сервере Linux. Создайте группы, назначьте им права на файлы и директории, ограничьте доступ к конфиденциальным данным с помощью команд chown и chmod.
СРОП: Создайте несколько учетных записей с разными уровнями доступа (администратор, обычный пользователь) и продемонстрируйте, как правильно управлять их доступом к ресурсам в ОС. Приведите примеры на реальных сценариях использования прав доступа.



Для начала в терминале создаю пользователя stacy командой:
     ```bash
     sudo adduser stacy
     ```
Затем устанавливаю пароль и настраиваю дополнительные параметры:
```
root@stacy-VirtualBox:/home/stacy# sudo adduser stacyuser
Добавляется пользователь «stacyuser» ...
Добавляется новая группа «stacyuser» (1001) ...
Добавляется новый пользователь «stacyuser» (1001) в группу «stacyuser» ...
Создаётся домашний каталог «/home/stacyuser» ...
Копирование файлов из «/etc/skel» ...
Новый пароль : 
Повторите ввод нового пароля : 
passwd: пароль успешно обновлён
Изменение информации о пользователе stacyuser
Введите новое значение или нажмите ENTER для выбора значения по умолчанию
	Полное имя []: 
	Номер комнаты []: 
	Рабочий телефон []: 
	Домашний телефон []: 
	Другое []: 
Данная информация корректна? [Y/n] y
root@stacy-VirtualBox:/home/stacy# 
```  
Командой ниже создаю новую группу:
     ```bash
     sudo addgroup stacygroup
     ```
```
root@stacy-VirtualBox:/home/stacy# sudo addgroup stacygroup
Добавляется группа «stacygroup» (GID 1002) ...
Готово.
root@stacy-VirtualBox:/home/stacy# 

     sudo usermod -aG stacygroup stacyuser

root@stacy-VirtualBox:/home/stacy# getent group stacygroup
stacygroup:x:1002:stacyuser
```

## Назначение прав доступа

Сначала создаю файл confidential.txt и защищаю его:
   - ```
     touch confidential.txt
     ```
     ```bash
     echo "Секретная информация" > confidential.txt
     ```
     Устанавливаю права так, чтобы владелец мог читать и писать, а группа только читать:
     ```bash
     sudo chown stacy:stacygroup confidential.txt
     sudo chmod 640 confidential.txt
     ```

## Ограничение доступа к конфиденциальным данным

Создаю защищенную директорию:
     ```bash
     sudo mkdir /home/stacy/confidential
     ```

Делаю stacy владельцем и настраиваю доступ:
     ```bash
     sudo chown stacy:stacygroup /home/stacy/confidential
     sudo chmod 750 /home/stacy/confidential
     ```

## Создание учетных записей с разными уровнями доступа
Создаю учетную запись администратора:
     ```bash
     sudo adduser adminuser
     sudo usermod -aG sudo adminuser
     ```

Создаю пользователя с ограниченными правами:
   - Создайте обычного пользователя:
     ```bash
     sudo adduser normaluser
     ```

Вхожу в систему под adminuser и проверяю доступ:
     ```bash
     sudo -i
     ls /home/stacy/confidential
     ```
Вхожу как normaluser, чтобы увидеть ограничения на доступ:
```
dminuser@stacy-VirtualBox:~$ sudo - adminuser
sudo: -: команда не найдена
adminuser@stacy-VirtualBox:~$ su - adminuser
Пароль: 
adminuser@stacy-VirtualBox:~$ sudo -i 
root@stacy-VirtualBox:~# ls /home/stacy/confidential
adminfile.txt
root@stacy-VirtualBox:~# 
normaluser@stacy-VirtualBox:~$ sudo -i
[sudo] пароль для normaluser: 
normaluser отсутствует в файле sudoers. Данное действие будет занесено в журнал.
normaluser@stacy-VirtualBox:~$ 
```

## Примеры реальных сценариев использования прав доступа

Для общего доступа группы создаю директорию /shared, доступ к которой получают все члены stacygroup:
     sudo mkdir /shared
     sudo chown :stacygroup /shared
     sudo chmod 770 /shared
```
stacyuser@stacy-VirtualBox:/shared$ cd /shared
stacyuser@stacy-VirtualBox:/shared$ pwd
/shared
```
Доступ к файлу конфиденциальной информации предоставляю только администратору:
     ```bash
     sudo touch /home/stacy/confidential/secret.txt
     sudo chown adminuser:adminuser /home/stacy/confidential/secret.txt
     sudo chmod 600 /home/stacy/confidential/secret.txt
     ```

Создаю отдельную группу и файл конфигурации приложения, предоставляя доступ только участникам этой группы:
     ```bash
     sudo addgroup appgroup
     sudo touch /etc/app/config.cfg
     sudo chown :appgroup /etc/app/config.cfg
     sudo chmod 640 /etc/app/config.cfg
     ```

## Вывод
Этот процесс позволяет гибко управлять доступом пользователей и защитой данных на сервере Linux. Выполнив настройку учетных записей и групп, я создала разные уровни доступа для пользователей с правами администратора и обычных пользователей. Администраторы имеют полный контроль над системой, включая доступ к конфиденциальной информации, в то время как обычные пользователи могут видеть только те ресурсы, к которым им предоставлены права.

Примеры показали, как можно защитить файлы с конфиденциальной информацией и создать общие ресурсы для группы. Это полезно при работе с корпоративными данными, когда необходимо ограничить доступ на основе ролей и уровней привилегий.
