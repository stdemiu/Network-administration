СРО: Настройте автоматизированную систему резервного копирования с использованием утилит rsync и cron. Выполните полный цикл: создание резервной копии, внесение изменений в оригинальные данные и восстановление резервной копии.
СРОП: Создайте план резервного копирования и восстановления для серверов, опишите процессы использования инкрементальных и полных бэкапов. Проведите тестирование восстановления данных после повреждения файловой системы.

## Установка утилиты rsync
Для настройки резервного копирования использую утилиту rsync. Если она не установлена, обновляю пакеты и устанавливаю rsync:
```
sudo apt update
sudo apt install rsync
```

Для начала создаю тестовую директорию с файлами:
```
mkdir -p /home/stacy/data
echo "Test file 1" > /home/stacy/data/file1.txt
echo "Test file 2" > /home/stacy/data/file2.txt
```

Чтобы создать резервную копию этой директории в /mnt/backup, использую команду:
```
rsync -av --delete /home/stacy/data/ /mnt/backup/
```

## Настройка автоматического резервного копирования с помощью cron

Для автоматизации резервного копирования открываю crontab:
```
crontab -e
```

Добавляю строку для выполнения резервного копирования ежедневно в 2:00:
0 2 * * * rsync -av --delete /home/stacy/data/ /mnt/backup/

## Внесение изменений в оригинальные данные и восстановление резервной копии
Добавляю новый файл в директорию /home/stacy/data, чтобы внести изменения:
```
echo "New data" > /home/user/data/newfile.txt
```
```
root@stacy-VirtualBox:/home/stacy/data# ls
file1.txt  file2.txt  newfile.txt
```

Чтобы восстановить данные из резервной копии, выполняю:
```
rsync -av --delete /mnt/backup/ /home/stacy/data/
```

```
root@stacy-VirtualBox:/home/stacy/data# rsync -av --delete /mnt/backup/ /home/stacy/data/
sending incremental file list
deleting newfile.txt
./

sent 91 bytes  received 30 bytes  242.00 bytes/sec
total size is 24  speedup is 0.20
root@stacy-VirtualBox:/home/stacy/data# ls
file1.txt  file2.txt
```

## Тестирование восстановления данных
Создаю отдельную тестовую директорию и добавляю тестовые данные:
```
mkdir /home/stacy/test_data
echo "Test data" > /home/stacy/test_data/testfile.txt
```

Делаю полную копию этой директории:
```
rsync -av --delete /home/stacy/test_data/ /mnt/backup/test_data/
```

Удаляю оригинальные файлы для проверки восстановления:
```
rm -r /home/stacy/test_data/*
```

Для восстановления из резервной копии выполняю:
```
rsync -av --delete /mnt/backup/test_data/ /home/stacy/test_data/
```

После восстановления проверяю, что файлы успешно восстановлены:
```
ls /home/stacy/test_data/
root@stacy-VirtualBox:/home/stacy/test_data# rm -r /home/stacy/test_data/*
root@stacy-VirtualBox:/home/stacy/test_data# ls
root@stacy-VirtualBox:/home/stacy/test_data# rsync -av --delete /mnt/backup/test_data/ /home/stacy/test_data/
sending incremental file list
./
testfile.txt

sent 127 bytes  received 38 bytes  330.00 bytes/sec
total size is 10  speedup is 0.06
root@stacy-VirtualBox:/home/stacy/test_data# ls
testfile.txt
```
Этот процесс создает надежную систему для регулярного резервного копирования и восстановления, защищая данные от потерь и повреждений.
## Вывод
В результате проделанной работы я настроила автоматизированное резервное копирование данных с помощью rsync и cron, обеспечив возможность регулярного бэкапа важных данных. Выполнив резервное копирование, внесла изменения в оригинальные данные и успешно восстановила их из резервной копии, подтвердив, что система работает корректно.

Настроенная автоматизация позволяет ежедневно сохранять актуальные копии данных, минимизируя риск потери информации. Процесс восстановления был протестирован и показал, что даже при удалении или повреждении файлов данные могут быть быстро возвращены из резервной копии, что обеспечивает надежную защиту данных.
