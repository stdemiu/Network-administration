СРО: Установите и настройте виртуальные машины с помощью VirtualBox или VMware. Подготовьте отчет по созданию и настройке гипервизора, сетевого окружения и настройки доступа к виртуальным машинам.
СРОП: Настройте Docker-контейнеры для развертывания веб-сервиса. Включите в отчет подробное описание процесса создания Dockerfile, сборки контейнера и проверки его работоспособности на реальном сервере.


## Установка Docker на Ubuntu

Для начала обновляю пакеты системы:
```
sudo apt update
```

Затем устанавливаю необходимые пакеты для работы с репозиториями Docker:
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

Добавляю GPG-ключ для подтверждения подлинности пакетов Docker:
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Подключаю репозиторий Docker:
```
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
Обновляю список пакетов снова, чтобы включить репозиторий Docker:
```
sudo apt update
```

Теперь устанавливаю Docker:
```
sudo apt install docker-ce
```

Чтобы проверить, запущен ли Docker, использую следующую команду:
```
sudo systemctl status docker
root@stacy-VirtualBox:/home/stacy# sudo systemctl status docker
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2024-10-30 15:19:47 +05; 1min 8s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 28924 (dockerd)
      Tasks: 10
     Memory: 21.3M
     CGroup: /system.slice/docker.service
             └─28924 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Қаз 30 15:19:47 stacy-VirtualBox dockerd[28924]: time="2024-10-30T15:19:47.411022845+05:00" level=info msg="Starting up"
Қаз 30 15:19:47 stacy-VirtualBox dockerd[28924]: time="2024-10-30T15:19:47.411938138+05:00" level=info msg="detected 127.0.0.53 names>
Қаз 30 15:19:47 stacy-VirtualBox dockerd[28924]: time="2024-10-30T15:19:47.481194698+05:00" level=info msg="Loading containers: start>
Қаз 30 15:19:47 stacy-VirtualBox dockerd[28924]: time="2024-10-30T15:19:47.630001851+05:00" level=info msg="Loading containers: done."
Қаз 30 15:19:47 stacy-VirtualBox dockerd[28924]: time="2024-10-30T15:19:47.651322416+05:00" level=warning msg="WARNING: bridge-nf-cal>
Қаз 30 15:19:47 stacy-VirtualBox dockerd[28924]: time="2024-10-30T15:19:47.651345370+05:00" level=warning msg="WARNING: bridge-nf-cal>
Қаз 30 15:19:47 stacy-VirtualBox dockerd[28924]: time="2024-10-30T15:19:47.651363574+05:00" level=info msg="Docker daemon" commit=41c>
Қаз 30 15:19:47 stacy-VirtualBox dockerd[28924]: time="2024-10-30T15:19:47.651440519+05:00" level=info msg="Daemon has completed init>
Қаз 30 15:19:47 stacy-VirtualBox dockerd[28924]: time="2024-10-30T15:19:47.681506375+05:00" level=info msg="API listen on /run/docker>
Қаз 30 15:19:47 stacy-VirtualBox systemd[1]: Started Docker Application Container Engine.
```

Чтобы не вводить sudo перед каждой командой Docker, добавляю своего пользователя в группу Docker:
```
sudo usermod -aG docker stacy
su - stacy
```

## Запустите контейнер Ubuntu
Чтобы запустить контейнер на базе Ubuntu в интерактивном режиме, использую:
```
docker run -it ubuntu /bin/bash
stacy@stacy-VirtualBox:~$ docker run -it ubuntu /bin/bash
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
ff65ddf9395b: Pull complete 
Digest: sha256:99c35190e22d294cdace2783ac55effc69d32896daaa265f0bbedbcde4fbe3e5
Status: Downloaded newer image for ubuntu:latest
```
Если локального образа Ubuntu нет, Docker автоматически скачает его с Docker Hub. Опции -it позволяют работать с контейнером в интерактивном режиме. Как только команда выполнена, появляется новый терминал:

```
root@cf22eff543fb:/#
```

Внутри контейнера можно устанавливать приложения. Например, чтобы установить curl:
```
apt update && apt install curl
```

Выхожу из контейнера командой:
```
exit
```

Для просмотра всех контейнеров использую:
```
docker ps -a
stacy@stacy-VirtualBox:~$ docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS                       PORTS     NAMES
cf22eff543fb   ubuntu    "/bin/bash"   7 minutes ago   Exited (127) 5 seconds ago             sharp_antonelli
```

Проверка работы контейнера в интерактивном режиме: 
```
root@aa8a241db37b:/# curl --version
wget --version
curl 8.5.0 (x86_64-pc-linux-gnu) libcurl/8.5.0 OpenSSL/3.0.13 zlib/1.3 brotli/1.1.0 zstd/1.5.5 libidn2/2.3.7 libpsl/0.21.2 (+libidn2/2.3.7) libssh/0.10.6/openssl/zlib nghttp2/1.59.0 librtmp/2.3 OpenLDAP/2.6.7
Release-Date: 2023-12-06, security patched: 8.5.0-2ubuntu10.4
Protocols: dict file ftp ftps gopher gophers http https imap imaps ldap ldaps mqtt pop3 pop3s rtmp rtsp scp sftp smb smbs smtp smtps telnet tftp
Features: alt-svc AsynchDNS brotli GSS-API HSTS HTTP2 HTTPS-proxy IDN IPv6 Kerberos Largefile libz NTLM PSL SPNEGO SSL threadsafe TLS-SRP UnixSockets zstd
GNU Wget 1.21.4 built on linux-gnu.

-cares +digest -gpgme +https +ipv6 +iri +large-file -metalink +nls 
+ntlm +opie +psl +ssl/openssl 

Wgetrc: 
    /etc/wgetrc (system)
Locale: 
    /usr/share/locale 
Compile: 
    gcc -DHAVE_CONFIG_H -DSYSTEM_WGETRC="/etc/wgetrc" 
    -DLOCALEDIR="/usr/share/locale" -I. -I../../src -I../lib 
    -I../../lib -Wdate-time -D_FORTIFY_SOURCE=3 -DHAVE_LIBSSL -DNDEBUG 
    -g -O2 -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer 
    -ffile-prefix-map=/build/wget-LWnKWI/wget-1.21.4=. -flto=auto 
    -ffat-lto-objects -fstack-protector-strong -fstack-clash-protection 
    -Wformat -Werror=format-security -fcf-protection 
    -fdebug-prefix-map=/build/wget-LWnKWI/wget-1.21.4=/usr/src/wget-1.21.4-1ubuntu4.1 
    -DNO_SSLv2 -D_FILE_OFFSET_BITS=64 -g -Wall 
Link: 
    gcc -DHAVE_LIBSSL -DNDEBUG -g -O2 -fno-omit-frame-pointer 
    -mno-omit-leaf-frame-pointer 
    -ffile-prefix-map=/build/wget-LWnKWI/wget-1.21.4=. -flto=auto 
    -ffat-lto-objects -fstack-protector-strong -fstack-clash-protection 
    -Wformat -Werror=format-security -fcf-protection 
    -fdebug-prefix-map=/build/wget-LWnKWI/wget-1.21.4=/usr/src/wget-1.21.4-1ubuntu4.1 
    -DNO_SSLv2 -D_FILE_OFFSET_BITS=64 -g -Wall -Wl,-Bsymbolic-functions 
    -flto=auto -ffat-lto-objects -Wl,-z,relro -Wl,-z,now -lpcre2-8 
    -luuid -lidn2 -lssl -lcrypto -lz -lpsl ../lib/libgnu.a 

Copyright (C) 2015 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later
<http://www.gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Originally written by Hrvoje Niksic <hniksic@xemacs.org>.
Please send bug reports and questions to <bug-wget@gnu.org>.
root@aa8a241db37b:/# 
```

## Вывод
В ходе работы я успешно настроила Docker на Ubuntu, что позволило мне развертывать контейнеры для запуска приложений в изолированной среде. Установив необходимые зависимости и подключив репозиторий Docker, я запустила Docker-демон, а затем развернула контейнер на базе Ubuntu, проверив его работоспособность и функциональность.

Docker предоставляет гибкие возможности для управления контейнерами и создания повторяемой среды для приложений, что позволяет легко проводить эксперименты и тестировать приложения в изоляции от основной системы. Этот подход к контейнеризации упрощает процесс развертывания веб-сервисов и управления ими.
