# Домашнее задание к занятию "`Docker. Часть 2.`" - `Бобков Егор`

---
### Задание 1

Напишите ответ в свободной форме, не больше одного абзаца текста.

Установите Docker Compose и опишите, для чего он нужен и как может улучшить вашу жизнь.

#### Решение 1

Сам по себе Docker позволяет развернуть и управлять одним конейнером одновременно, что неплохо, но будет времязатратно когда нам нужно развернуть приложение/сервис, который требует обслуживание сразу несколькими микросервисами. Docker Compose в этом случае позволяет в один .yml файл развернуть несколько связанных друг с другом контейнеров и успешно задеплоить приложение/сервис.

---
### Задание 2

`Выполните действия и приложите текст конфига на этом этапе.

Создайте файл docker-compose.yml и внесите туда первичные настройки:

* version;
* services;
* volumes;
* networks.

При выполнении задания используйте подсеть 10.5.0.0/16. Ваша подсеть должна называться: <ваши фамилия и инициалы>-my-netology-hw. Все приложения из последующих заданий должны находиться в этой конфигурации.`

#### Решение 2

```
version: '3'

services:

volumes:

networks:
  Bobkov-E-O-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```

---
### Задание 3

**Выполните действия:**

  1. Создайте конфигурацию docker-compose для Prometheus с именем контейнера <ваши фамилия и инициалы>-netology-prometheus.
  2. Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории 6-04/prometheus).
  3. Обеспечьте внешний доступ к порту 9090 c докер-сервера.

#### Решение 3

```
version: '3'

services:
  prometheus:
    image: prom/prometheus:v2.47.2
    container_name: Bobkov-E-O-netology-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - 9090:9090
    volumes:
      - ./prometheus:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - Bobkov-E-O-my-netology-hw
    restart: always

volumes:
  prometheus-data:

```

---
### Задание 4

Выполните действия:

  1. Создайте конфигурацию docker-compose для Pushgateway с именем контейнера <ваши фамилия и инициалы>-netology-pushgateway.
  2. Обеспечьте внешний доступ к порту 9091 c докер-сервера.

#### Решение 4

```
version: '3'

services:
  prometheus:
    image: prom/prometheus:v2.47.2
    container_name: Bobkov-E-O-netology-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - 9090:9090
    volumes:
      - ./prometheus:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - Bobkov-E-O-my-netology-hw
    restart: always

  pushgateway:
    image: prom/pushgateway:v1.6.2
    container_name: Bobkov-E-O-netology-pushgateway
    ports:
      - 9091:9091
    networks:
      - Bobkov-E-O-my-netology-hw
    depends_on:
      - prometheus
    restart: unless-stopped

volumes:
  prometheus-data:

networks:
  Bobkov-E-O-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```

---
### Задание 5

Выполните действия:

  1. Создайте конфигурацию docker-compose для Grafana с именем контейнера <ваши фамилия и инициалы>-netology-grafana.
  2. Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории 6-04/grafana).
  3. Добавьте переменную окружения с путем до файла с кастомными настройками (должен быть в томе), в самом файле пропишите логин=<ваши фамилия и инициалы> пароль=netology.
  4. Обеспечьте внешний доступ к порту 3000 c порта 80 докер-сервера.

#### Решение 5
```
version: '3'

services:
  prometheus:
    image: prom/prometheus:v2.47.2
    container_name: Bobkov-E-O-netology-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - 9090:9090
    volumes:
      - ./prometheus:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - Bobkov-E-O-my-netology-hw
    restart: always

  pushgateway:
    image: prom/pushgateway:v1.6.2
    container_name: Bobkov-E-O-netology-pushgateway
    ports:
      - 9091:9091
    networks:
      - Bobkov-E-O-my-netology-hw
    depends_on:
      - prometheus
    restart: unless-stopped

  grafana:
    image: grafana/grafana
    container_name: Bobkov-E-O-netology-grafana
    environment:
      GF_PATHS_CONFIG: /etc/grafana/custom.ini
    ports:
      - 80:3000
    volumes:
      - ./grafana:/etc/grafana
      - grafana-data:/var/lib/grafana
    networks:
      - Bobkov-E-O-my-netology-hw
    depends_on:
      - prometheus
    restart: unless-stopped

volumes:
  prometheus-data:
  grafana-data:

networks:
  Bobkov-E-O-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```
custom.ini :
```
[security]

admin_user = BobkovEO
admin_password = netology
```

---
### Задание 6

**Выполните действия.**

1. Настройте поочередность запуска контейнеров.
2. Настройте режимы перезапуска для контейнеров.
3. Настройте использование контейнерами одной сети.
5. Запустите сценарий в detached режиме.

#### Решение 6

![task6](https://github.com/jinnonn/docker-compose-hw/blob/main/img/task6.png)

---
### Задание 7 

**Выполните действия.**
1. Выполните запрос в Pushgateway для помещения метрики <ваши фамилия и инициалы> со значением 5 в Prometheus: ```echo "<ваши фамилия и инициалы> 5" | curl --data-binary @- http://localhost:<внешний порт выбранный вами в пункте 2>/metrics/job/netology```.
2. Залогиньтесь в Grafana с помощью логина и пароля из предыдущего задания.
3. Cоздайте Data Source Prometheus (Home -> Connections -> Data sources -> Add data source -> Prometheus -> указать "Prometheus server URL = http://<ваши фамилия и инициалы>-netology-prometheus:9090" -> Save & Test).
4. Создайте график на основе добавленной в пункте 5 метрики (Build a dashboard -> Add visualization -> Prometheus -> Select metric -> Metric explorer -> <ваши фамилия и инициалы -> Apply.

В качестве решения приложите:

* docker-compose.yml **целиком**;
* скриншот команды docker ps после запуске docker-compose.yml;
* скриншот графика, постоенного на основе вашей метрики.

#### Решение 7

* docker-compose.yml лежит в корневой папке этого репо
* ![task6](https://github.com/jinnonn/docker-compose-hw/blob/main/img/task6.png)
* ![task7](https://github.com/jinnonn/docker-compose-hw/blob/main/img/task7.png)

---
### Задание 8

**Выполните действия:** 

1. Остановите и удалите все контейнеры одной командой.

В качестве решения приложите скриншот консоли с проделанными действиями.

#### Решение 8

* ![task8](https://github.com/jinnonn/docker-compose-hw/blob/main/img/изображение_2023-10-30_014056105.png)
* ![task8](https://github.com/jinnonn/docker-compose-hw/blob/main/img/изображение_2023-10-30_014118532.png)

## Дополнительные задания (со звездочкой*)

Эти задания дополнительные (не обязательные к выполнению) и никак не повлияют на получение вами зачета по этому домашнему заданию. Вы можете их выполнить, если хотите глубже и/или шире разобраться в материале.

### Задание 9*

* ![task8](https://github.com/jinnonn/docker-compose-hw/blob/main/img/изображение_2023-10-30_021255379.png)
* ![task8](https://github.com/jinnonn/docker-compose-hw/blob/main/img/изображение_2023-10-30_021339000.png)
