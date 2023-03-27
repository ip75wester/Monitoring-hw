# Домашнее задание к занятию "`9.2. Система мониторинга Zabbix`" - `Илья Попов`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

Что нужно сделать:

Установите Zabbix Server с веб-интерфейсом.

Процесс выполнения
Выполняя ДЗ сверяйтесь с процессом отражённым в записи лекции.
Установите PostgreSQL. Для установки достаточна та версия что есть в системном репозитороии Debian 11
Пользуясь конфигуратором комманд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache
Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server
Требования к результаты
Прикрепите в файл README.md скриншот авторизации в админке
Приложите в файл README.md текст использованных команд в GitHub

![Скриншот](https://github.com/ip75wester/Monitoring-hw/blob/main/1-1.PNG)
---

Текст использованных команд:
# Установите PostgreSQL
sudo apt install postgresql
# Установите репозиторий Zabbix
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4+debian11_all.deb
dpkg -i zabbix-release_6.0-4+debian11_all.deb
apt update
# Установите Zabbix сервер, веб-интерфейс и агент
apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
# Создание пользователя с помощью psql из под root
su - postgres -c 'psql --command "CREATE USER zabbix WITH PASSWORD '\'123456789\'';"'
su - postgres -c 'psql --command "CREATE DATABASE zabbix OWNER zabbix;"'
# Импортируем схему
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix 
# Настраиваем пароль DBPassword в файле /etc/zabbix/zabbix_server.conf
sudo nano /etc/zabbix/zabbix_server.conf
# Запускаем Zabbix Server, Zabbix Agent и web-сервер
sudo systemctl restart zabbix-server apache2 # zabbix-agent
sudo systemctl enable zabbix-server apache2 # zabbix-agent


### Задание 2

Что нужно сделать:

Установите Zabbix Agent на два хоста.

Процесс выполнения
Выполняя ДЗ сверяйтесь с процессом отражённым в записи лекции.
Установите Zabbix Agent на 2 виртмашины, одной из них может быть ваш Zabbix Server
Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов
Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera
Проверьте что в разделе Latest Data начали появляться данные с добавленных агентов

Требования к результаты

Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
![Скриншот](https://github.com/ip75wester/Monitoring-hw/blob/main/2.PNG)


Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером

![Скриншот](https://github.com/ip75wester/Monitoring-hw/blob/main/3.PNG)


![Скриншот](https://github.com/ip75wester/Monitoring-hw/blob/main/4.PNG)

Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
![Скриншот](https://github.com/ip75wester/Monitoring-hw/blob/main/5.PNG)


![Скриншот](https://github.com/ip75wester/Monitoring-hw/blob/main/6.PNG)

Приложите в файл README.md текст использованных команд в GitHub

# Добавьте репозиторий Zabbix
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4%2Bdebian11_all.deb
dpkg -i zabbix-release_6.0-4+debian11_all.deb
apt update 
# Установите Zabbix Server и компоненты
sudo apt install zabbix-agent -y
# Запустите Zabbix Agent
sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent
#Посмотрите лог агента
cat /var/log/zabbix/zabbix_agentd.log
#Настройка агента
sudo nano /etc/zabbix/zabbix_agentd.conf
#Перезапуск агента
sudo systemctl restart zabbix-agent
