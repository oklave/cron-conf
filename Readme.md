cron
вообщем лучшие практики, с которыми сталкивалсяы

1. Задача никогда не должна запускаться с привилегиями root. Не только в целях безопасности, но и для предотвращения возможных ошибок связанных с запуском скрипта.
0 * * * *  root  cron-task

Запускаем задачу пользователем который имеет привилегии, необходимые для запуска данного приложения и только!
0 * * * *  myappcron  cron-task

2. Тестирование запланированного скрипта под пользователем, который cron будет использовать для запуска скрипта

sudo -i -u cronuser

События ошибок cron кладет в:
tail -f /var/log/syslog
#для systemd



3. Не отбрасывать ошибки и полезную информацию, не зная куда. Но без фанатизма. Rule of Silence.

*/5 * * *  root  curl https://example.com/cron.php >/dev/null 2>&1

4. Отправлять логи скриптов/приложений в правильные места

0 * * * *   you  cron-task | logger -it cron-task

5. crontab нужно держать чистым, не допускать многострочных правил, все класть в скрипты, у скриптов разрешение только +x

Локальные скрипты можно класть в ~/.local/bin
При этом добавить адрес в переменную окружения PATH 
Если задач много, делим их


6. Использовать git для контроля директории /etc/cron.d

7. Контроль над временем запуска скрипта

0 * * * *  you  timeout 10s cron-task

8. Блокирование скрипта

0 * * * *  you  flock -nx /var/lock/cron-task cron-task