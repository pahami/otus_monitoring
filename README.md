# Домашняя работа 22 
-------------------------------------------------

## Тема: Мониторинг  

### Цель домашнего задания

Научиться настраивать дашборд

### Описание домашнего задания

Настроить дашборд с 4-мя графиками

    - память;
    - процессор;
    - диск;
    - сеть.

Настроить на одной из систем:

    - zabbix (использовать screen (комплексный экран);
    - prometheus - grafana.
    
### Выполнение домашнего задания

1. Созданы 2 виртуальные машины с помощью Vagrantfile


Создаём виртуальные машины:

```vagrant up```

Результат - 2 виртуальные машины:
    - zabbix - zabbix server
    - nginx - nginx сервер

2. Настройка zabbix server

Подключаемся к ВМ
```vagrant ssh zabbix```

Переходим на сайт [Zabbix](https://www.zabbix.com/ru/download?zabbix=7.2&os_distribution=ubuntu&os_version=22.04&components=agent&db=&ws=) выбираем подходящую конфигурацию и следуем инсирукции.

<details>
<summary> Примечание </summary>

    - Инструкция подразумевает, что у вас уже установлен сервер баз данных

```apt install mysql-server -y```

    - Для установки русского языка необходимо добавить locale.

Проверить список установленных locale

```
locale -a
```
Если в списке нет нужных вам языков, откройте файл /etc/locale.gen и раскомментируйте нужные локали. Поскольку Zabbix использует кодировку UTF-8, вам нужно выбрать локали с кодировкой UTF-8.

Применяем настройки

```
locale-gen 
```

    - Имя пользователя и пароль  Admin/zabbix
</details>

Доступ на сервер http://localhost:8080

3. Настройка zabbix-agent на сервере nginx

Переходим на сайт [Zabbix](https://www.zabbix.com/ru/download?zabbix=7.2&os_distribution=ubuntu&os_version=22.04&components=agent&db=&ws=) выбираем подходящую конфигурацию и следуем инсирукции.

Для работы zabbix-agent необходимо открыть порт 10050. Проверим:

<details>
<summary> ss -tunlp </summary>

```
root@nginx:/home/vagrant# ss -tnlp
State          Recv-Q         Send-Q                 Local Address:Port                   Peer Address:Port         Process                                                                                                             
LISTEN         0              128                          0.0.0.0:22                          0.0.0.0:*             users:(("sshd",pid=863,fd=3))                                                                                      
LISTEN         0              4096                         0.0.0.0:10050                       0.0.0.0:*             users:(("zabbix_agentd",pid=2755,fd=4),("zabbix_agentd",pid=2754,fd=4),("zabbix_agentd",pid=2753,fd=4),("zabbix_agentd",pid=2752,fd=4),("zabbix_agentd",pid=2751,fd=4),("zabbix_agentd",pid=2750,fd=4),("zabbix_agentd",pid=2749,fd=4),("zabbix_agentd",pid=2748,fd=4),("zabbix_agentd",pid=2747,fd=4),("zabbix_agentd",pid=2746,fd=4),("zabbix_agentd",pid=2745,fd=4),("zabbix_agentd",pid=2744,fd=4),("zabbix_agentd",pid=2743,fd=4))
LISTEN         0              4096                   127.0.0.53%lo:53                          0.0.0.0:*             users:(("systemd-resolve",pid=532,fd=14))                                                                          
LISTEN         0              128                             [::]:22                             [::]:*             users:(("sshd",pid=863,fd=4))                                                                                      
LISTEN         0              4096                            [::]:10050                          [::]:*             users:(("zabbix_agentd",pid=2755,fd=5),("zabbix_agentd",pid=2754,fd=5),("zabbix_agentd",pid=2753,fd=5),("zabbix_agentd",pid=2752,fd=5),("zabbix_agentd",pid=2751,fd=5),("zabbix_agentd",pid=2750,fd=5),("zabbix_agentd",pid=2749,fd=5),("zabbix_agentd",pid=2748,fd=5),("zabbix_agentd",pid=2747,fd=5),("zabbix_agentd",pid=2746,fd=5),("zabbix_agentd",pid=2745,fd=5),("zabbix_agentd",pid=2744,fd=5),("zabbix_agentd",pid=2743,fd=5))
```
</details>

Переходим на стараницу zabbix server и добавляем сервер nginx в узлы сети.


4. Настраиваем дашборд

Для настройки дашборда необходимо узлам сети (zabbix server, nginx) добавить инересующие нас шаблоны. В данном случаем были добавлены шаблоны:
 - Linux by Zabbix agent
 - Zabbix server health

Редактируем главную страницу, добавив интересующие нас графики.

Результат:
[Настроенный дашборд Zabbix](assets/images/zabbix.png)