Лучшие практики конфигурирования cron:

- Задача никогда не должна запускаться с привилегиями root. Не только в целях безопасности, но и для предотвращения возможных ошибок связанных с запуском скрипта.
```
0 * * * *  root  cron-task
```

- Задача запускается пользователем который имеет привилегии, необходимые для запуска данного приложения и только!
```
0 * * * *  myappcron  cron-task
```

- Тестирование запланированного скрипта под пользователем, который cron будет использовать для запуска скрипта


```
sudo -i -u cronuser
```

- Cобытия ошибок cron кладет в:
```
tail -f /var/log/syslog
```

- Не отбрасывать ошибки и полезную информацию, не зная куда. Но без фанатизма. Rule of Silence.

```
*/5 * * *  root  curl https://example.com/cron.php >/dev/null 2>&1
```

4. Отправлять логи скриптов/приложений в правильные места

```
0 * * * *   you  cron-task | logger -it cron-task
```

- crontab нужно держать чистым, не допускать многострочных правил, все класть в скрипты, у скриптов разрешение только +xc 
Локальные скрипты можно класть в ~/.local/bin
При этом добавить адрес в переменную окружения PATH. Если задач много, делим их.


- Использовать git для контроля директории /etc/cron.d

- Контроль над временем запуска скрипта
```
0 * * * *  you  timeout 10s cron-task
```

- Блокирование скрипта
```
0 * * * *  you  flock -nx /var/lock/cron-task cron-task
```