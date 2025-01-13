# OM-DZ03
На виртуальной машине установлена CMS wordpress, которая включает в себя следующие компоненты:

nginx, php-fpm, database (MySQL)

На CMS развернут тестовый домен http://www.example.com/

---

В соответствии с целью домашего задания, мы настроим отправку уведомлений на телеграм бот и на почту. 

Оповещения будет выполнять AlertManager, который интегрируется с Prometheus.

С помощью @BotFather создаем бота @alertOtusDZ3bot с именем учетной записи prometheus_alert_dz.
Получаем bot_token бота.

После этого создадим канал alert_otusdz3_chat в Telegram, куда будут приходить алерты. Добавляем бота в этот канал и выдаем ему права администратора. 

Выясним chat_id по запросу https://api.telegram.org/bot<ТОКЕН_БОТА>/getUpdates. Значение result[0]message.chat.id показывает chat_id

---

В файле docker-compose.yml настраиваем конфигурацию для запуска alertmanager в контейнере.

В конфигурацию prometheus.yml добавляем файл alert.rules с описанием правил оповещения.

Также в этот файл добавляем , что сервер мониторинга должен использовать в качестве системы оповещения alertmanager, который доступен по адресу alertmanager:9093


Создаем файл с правилами оповещения alert.rules, в котором создаем правило, которое будет срабатывать при недоступности exporter-ов c уровнем Critical
и правило, которое будет срабатывать при недоступности страниц CMS с уровнем Warning

В файле конфигурации config.yml AlertManager настраиваем конфигурацию для отправки оповещений в телеграмм telegram-test и на почту mail-test

Также в этом файле настраиваем маршрутизацию по уровню критичности, Critical в телеграмм, Warning на почту с помощью конструкции match_re

Запускаем Prometheus и AlertManager:

docker-compose up -d

---

Подключаемся к Prometheus и видим созданные правила

![dz3-s6](https://github.com/user-attachments/assets/6d66ef54-7f10-4cc3-974c-8ee9f8beadf8)




