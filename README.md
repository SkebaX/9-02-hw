# Домашнее задание к занятию "`Система мониторинга Zabbix`"
## `Скобелкин А.В.`

### Задание 1

`Установите Zabbix Server с веб-интерфейсом.`
#### Процесс выполнения

1. `Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.`
2. `Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.`
3. `Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache`
4. `Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.`

#### Требования к результатам

1. `Прикрепите в файл README.md скриншот авторизации в админке.`
2. `Приложите в файл README.md текст использованных команд в GitHub.`

```
sudo -i
apt update
apt install postgersql
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb
apt update
apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts
-u postgres createuser --pwprompt zabbix
-u postgres createdb -O zabbix zabbix
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
sudo nano /etc/zabbix/zabbix_server.conf
systemctl restart zabbix-server apache2
systemctl enable zabbix-server apache2
```
<img width="1414" height="944" alt="Снимок экрана 2026-06-04 001847" src="https://github.com/user-attachments/assets/1d6aee15-6ac9-4277-a808-edcc2c1eec50" />
<img width="1712" height="1029" alt="Снимок экрана 2026-06-04 002348" src="https://github.com/user-attachments/assets/9b8dd0a7-feba-4eec-9f30-613b3d79feaa" />

---

### Задание 2

`Установите Zabbix Agent на два хоста.`
#### Процесс выполнения

1. `Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.`
2. `Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.`
3. `Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.`
4. `Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.`
5. `Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.`

#### Требования к результатам

1. `Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу`
2. `Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером`
3. `Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.`
4. `Приложите в файл README.md текст использованных команд в GitHub`



```
sudo -i
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb
apt update
apt apt install zabbix-agent 
systemctl restart zabbix-agent
systemctl enable zabbix-agent
sed -i 's/Server=127.0.0.1/Server=192.168.56.102/g' /etc/zabbix/zabbix_agentd.conf
sed -i 's/ServerActive=127.0.0.1/ServerActive=192.168.56.102/g' /etc/zabbix/zabbix_agentd.conf
systemctl restart zabbix-agent
systemctl status zabbix-agent
tail -f /var/log/zabbix/zabbix_agentd.log
```
<img width="2259" height="1085" alt="Снимок экрана 2026-06-04 121422" src="https://github.com/user-attachments/assets/408a865f-06e6-4c58-ad24-ace5cf866329" />
<img width="1085" height="808" alt="Снимок экрана 2026-06-04 123546" src="https://github.com/user-attachments/assets/59c9d97b-43e8-49a8-8b25-d98b55efec26" />
<img width="2466" height="1426" alt="Снимок экрана 2026-06-04 124641" src="https://github.com/user-attachments/assets/83b19d65-a34a-4285-a13c-282e012bd6df" />

---
