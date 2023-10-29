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

`Выполните действия:

Создайте конфигурацию docker-compose для Prometheus с именем контейнера <ваши фамилия и инициалы>-netology-prometheus.
Добавьте необходимые тома с данными и конфигурацией (конфигурация лежит в репозитории в директории 6-04/prometheus).
Обеспечьте внешний доступ к порту 9090 c докер-сервера.

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

Создайте конфигурацию docker-compose для Pushgateway с именем контейнера <ваши фамилия и инициалы>-netology-pushgateway.
Обеспечьте внешний доступ к порту 9091 c докер-сервера.

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


---
## Дополнительные задания (со звездочкой*)

Эти задания дополнительные (не обязательные к выполнению) и никак не повлияют на получение вами зачета по этому домашнему заданию. Вы можете их выполнить, если хотите глубже и/или шире разобраться в материале.

### Задание 5

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`
