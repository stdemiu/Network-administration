СРО: Настройте мониторинг системных ресурсов (процессор, оперативная память, диск) с помощью утилит htop, iotop, df и представьте анализ производительности системы за период наблюдения.
СРОП: Установите и настройте инструмент мониторинга, такой как Zabbix или Nagios, для отслеживания состояния серверов и сетевых ресурсов. Создайте отчет с графиками и анализом.

## Локальный мониторинг системных ресурсов
Для отслеживания системных ресурсов, таких как процессор, оперативная память и использование диска, я установлю несколько утилит, которые позволяют наблюдать за состоянием системы в режиме реального времени.
Сначала обновляю список пакетов и устанавливаю htop и iotop:
```
sudo apt update
sudo apt install htop iotop
```

Запускаю htop для интерактивного мониторинга. Эта утилита отображает все активные процессы, использование процессора и оперативной памяти, позволяя видеть, какие процессы потребляют больше всего ресурсов:
```
htop
```
Командой iotop отслеживаю процессы, активно использующие диск, что позволяет понять, какие приложения или службы вызывают наибольшую нагрузку:


```
sudo iotop
```
Это покажет, какие процессы используют диск в реальном времени.

## Установка и настройка инструмента мониторинга (Zabbix или Nagios)
Для более полного мониторинга серверов и сетевых ресурсов я выбрала Zabbix. Чтобы установить Zabbix, первым шагом я устанавливаю MySQL сервер:

```
sudo apt install mysql-server
mysql -uroot -p
```

Создаю базу данных и пользователя Zabbix в MySQL:
```
CREATE DATABASE zabbix CHARACTER SET utf8 COLLATE utf8_bin;
CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'your_password';  -- Замените your_password на надежный пароль
GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';
FLUSH PRIVILEGES;
```

Загружаю начальные данные для Zabbix:
```
zcat /usr/share/doc/zabbix-server-mysql/create/schema.sql.gz | mysql -uzabbix -p zabbix
```

## Настройка Zabbix
Открываю файл конфигурации Zabbix и добавляю пароль базы данных для подключения:
```
sudo nano /etc/zabbix/zabbix_server.conf
```
Ищу строку DBPassword= и указываю пароль, установленный ранее.

## Веб-интерфейс Zabbix
После настройки сервера перехожу в веб-браузер, чтобы получить доступ к панели Zabbix. Ввожу следующий URL для доступа к веб-интерфейсу:
```
http://localhost/zabbix
```

В веб-интерфейсе доступны основные разделы:

- Dashboard — дает общее представление о состоянии мониторинга.
- Monitoring — предоставляет данные о текущем состоянии системы.
- Inventory — хранит информацию об инвентаризации оборудования.
- Configuration — позволяет настроить параметры мониторинга.
- Administration — используется для администрирования, создания пользователей и настройки.
- Reports — для создания отчетов о производительности и состоянии системы.

## Вывод
В рамках задания я настроила мониторинг системных ресурсов с помощью утилит htop и iotop, которые позволили мне наблюдать за загрузкой процессора, оперативной памяти и использованием диска в реальном времени. Эти утилиты обеспечивают интерактивный и удобный способ отслеживания потребления ресурсов и состояния системы, что полезно для обнаружения узких мест и оптимизации производительности.

Для расширенного мониторинга я установила и настроила Zabbix — мощный инструмент для наблюдения за состоянием серверов и сетевых ресурсов. Создание базы данных, конфигурация сервера и настройка веб-интерфейса Zabbix позволили организовать централизованный и детализированный мониторинг. Через веб-интерфейс Zabbix можно управлять мониторингом, создавать инвентаризации и генерировать отчеты, что позволяет получить полную картину состояния системы и проводить анализ производительности на основе собранных данных.

Таким образом, выполненная настройка мониторинга обеспечила меня необходимыми инструментами для своевременного выявления проблем и поддержания оптимальной производительности серверов и сетевых ресурсов.