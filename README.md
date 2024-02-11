# bitrix-lemp

## Стек
* Nginx (порт: 8081)
* Php 8.1
* MySQL (порт: 20002)


* phpMyAdmin (порт: 8082)
* Memcached
* Cronjobber
* Сomposer
* Xdebug (порт: 9999)

## Доступ к базе
- DB-HOST: mysql
- DB-USER: bitrix
- DB-NAME: sitemanager
- DB-PASS: example

### Примечание
Не забываем положить файлик ```cron_events.php``` из корня в ```/www/bitrix/php_interface/```  - чтобы заработал ```cron```

Установка Bitrix через скрипт [bitrixsetup.php](https://www.1c-bitrix.ru/download/scripts/bitrixsetup.php) производится довольно долго, но ее можно ускорить, если дождаться процесса распаковки скачанных архивов и остановить процесс распаковки в браузере.
Затем зайти в домашний каталог и запустить в терминале команду распаковки:

```
cat *$(ls -v *tar.gz*) | tar xzf -
```

Команда склеит все части архивов и распакует их (для сжатых архивов).  
Подходит и для установки из ```backup``` при помощи ```restore```