* [Оглавление](../README.md)

# 1. Docker
* Docker Desktop не поддерживается Windows 7, потому приходится использовать облегченную версию - Docker Toolbox.
* [eng инструкция](https://docs.bitnami.com/containers/how-to/install-docker-in-windows/)
* [eng инструкция](https://stackoverflow.com/questions/61495838/docker-desktop-installation-on-windows-7-is-not-working) 1-ый ответ
* Docker Toolbox как одну из компонент использует VirtualBox. Но в силу прекращения его обновления (последняя версия 2021), в ходе установки будет предложено установить более старую версию. ИМХО VirtualBox лучше установить вручную до установки Docker Toolbox.
* Узнать версию Docker
> docker version

## 1.1 Проверка возможности своего CPU поддерживать виртуализацию
### 1.1.1 [скачать Speccy](https://www.ccleaner.com/ru-ru/speccy/download) и установить
### 1.1.2 Вкладка CPU, ищем параметр **Virtualization**.
Должно быть **Supported, Enabled**

## 1.2 Предварительно необходимо установить Hyper-V. 
### 1.2.1 Данная утилита находится в комплекте Remote Server Administration Tools (RSAT) for Windows 7 (Средства удалённого администрирования сервера), она же KB958830.
[скачать 64bit](https://thesystemcenterblog.files.wordpress.com/2021/02/9ec67-rsat-tools-for-windows-7-64-bit.zip)

На сайте Microsoft не удалось найти валидную ссылку (03.2023)
### 1.2.2 Распаковка архива и установка. Перезагрузка.
### 1.2.3 Включаем утилиту
Официальный хелп:
> c:\Windows\Help\mui\0419\rsat_client.CHM

Включаем:
> Панель управления -> Программы и компоненты -> Включение или отключение компонентов Windows

В открывшемся окне ищем **Средства удалённого администрирования сервера** и отмечаем галочкой.
> Средства администрирования ролей -> Средства Hyper-V

Перезагрузка
> Панель управления -> Администрирование

Будет добавлен пункт **Диспетчер Hyper-V**

## 1.3 Docker Toolbox (Minimal: Win 7 64bit) позволяет использовать Docker в системах Windows, которые не соответствуют минимальным системным требованиям для приложения Docker Desktop для Windows.
[качать](https://github.com/docker-archive/toolbox/releases)
### 1.3.1 Если у вас установлен и запущен VirtualBox, вЫключите его перед началом установки.
### 1.3.2 Установка Toolbox. В процессе будет предложено установить дополнительные программы:
* VirtualBox
* Git for Windows
### 1.3.3 После установки всех компонентов мастер уведомит об успешной установке. Снимите флажок «Просмотреть ярлыки в проводнике» (View Shortcuts in File Explorer) и нажмите «Готово».
### 1.3.4 На рабочем столе будут созданы ярлыки:
* Kitematic (Alpha)
* Docker Quickstart Terminal
### 1.3.5 Проверка установки Docker Quickstart Terminal
#### 1.3.5.1 Запустите терминал быстрого запуска Docker (Docker Quickstart Terminal). Начнется создание машины Docker и всех ее компонентов.
#### 1.3.5.2 Предпоследняя строка
> Start interactive shell

После символа **$** необходимо ввести:
> docker run hello-world

Docker загрузит и запустит контейнер «Hello world». В терминале отобразится подтверждающее сообщение.
Это означает, что ваша установка Docker Quickstart Terminal прошла успешно.
## 1.4 Донастройка среды Windows
### 1.4.1 Необходимо проконтролировать наличие **системных переменных** Windows:
> Панель управления -> Система -> Дополнительные параметры системы

* VBOX_INSTALL_PATH = C:\Program Files\Oracle\VirtualBox (Директория в которую установлен VirtualBox)
* VBOX_MSI_INSTALL_PATH = C:\Program Files\Oracle\VirtualBox (Директория в которую установлен VirtualBox)
* VBOX_USER_HOME = C:\Users\User\.VirtualBox (Расположение домашней директории .VirtualBox)
* DOCKER_TOOLBOX_INSTALL_PATH = C:\Program Files\Docker Toolbox (Директория в которую установлен Docker Toolbox)
### 1.4.2 Перезагрузка
## 1.5 Регистрация в Docker Hub
[Регистрация](https://hub.docker.com/signup)
### 1.5.1 Выбираем Plan: **Personal** (Free)
### 1.5.2 Верификация email
## 1.6 Установка Kitematic (Alpha)
### 1.6.1 Установка **Kitematic (Alpha)** с ярлыка рабочем столе.
Kitematic - это графический интерфейс Docker
#### 1.6.1.1 Если он не работает и вы получаете сообщение об уже существующем «по умолчанию» или об отсутствии файла config.json, выполните следующие действия:
* закройте терминал быстрого запуска Docker, если он открыт.
* открыть диспетчер задач -> процессы.
* Завершить процесс VBoxHeadless.exe
* добавьте пустой файл config.json вручную в %userprofile%.docker\machine\machines\default, если config.json отсутствует.
* Запустить в CMD: docker-machine rm -f по умолчанию
* Запустить в CMD: docker-machine create -d virtualbox --virtualbox-memory 2048 default
#### 1.6.1.2 Если приведенные выше команды CMD не увенчались успехом:
* удалить папку %userprofile%.docker\machine\machines\default вручную
* Перезапустить компьютер
* запустите терминал быстрого запуска Docker от имени администратора
* папка %userprofile%.docker\machine\machines\default должна была быть создана правильно на этом этапе.
* открыть Kitematic. Пользовательский интерфейс должен быть представлен правильно
#### 1.6.2 Если установка пошла:
* Первое окно: **Use VirtualBox**
* Второе окно (регистрация/авторизация Docker Hub)
#### 1.6.3 Если всё будет хорошо - Kitematic будет запущен.
В списке **Containers** будет "happy_wright" (согласно п.1.3.5.2)

## 1.7 Замечания по работе Docker Tolls:
### 1.7.1 Docker Quickstart Terminal (терминал) неудобный:
* ограничение по регулировке ширины окошка
* не работает крыса
#### 1.7.1.1 Решение - создать свой ярлык входа в Docker:
* Объект:
> "C:\Program Files\Git\git-bash.exe" -i "C:\Program Files\Docker Toolbox\start.sh"
* Рабочая папка:
> %HOMEDRIVE%%HOMEPATH%

## 1.8 Plugins for IDE
### 1.8.1 VisualStudio Code
#### 1.8.1.1 Docker (@Microsoft)
#### 1.8.1.2 Dev Containers (@Microsoft) not support Docker Toolbox
### 1.8.2 PHPStorm
#### 1.8.2.1 Docker (@JetBrains)