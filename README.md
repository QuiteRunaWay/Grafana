# Домашнее задание к занятию "10.03. Grafana"

## Обязательные задания

### Задание 1
Используя директорию [help](./help) внутри данного домашнего задания - запустите связку prometheus-grafana.

Зайдите в веб-интерфейс графана, используя авторизационные данные, указанные в манифесте docker-compose.

Подключите поднятый вами prometheus как источник данных.

Решение домашнего задания - скриншот веб-интерфейса grafana со списком подключенных Datasource.

### Ответ:

![image](https://user-images.githubusercontent.com/92969676/177263410-f9d28b3f-b814-43c2-b5d9-57d16861aadf.png)


## Задание 2
Изучите самостоятельно ресурсы:
- [promql-for-humans](https://timber.io/blog/promql-for-humans/#cpu-usage-by-instance)
- [understanding prometheus cpu metrics](https://www.robustperception.io/understanding-machine-cpu-usage)

Создайте Dashboard и в ней создайте следующие Panels:
- Утилизация CPU для nodeexporter (в процентах, 100-idle)
- CPULA 1/5/15
- Количество свободной оперативной памяти
- Количество места на файловой системе

Для решения данного ДЗ приведите promql запросы для выдачи этих метрик, а также скриншот получившейся Dashboard.

- Утилизация CPU для nodeexporter (в процентах, 100-idle)

```100 * (1-avg by(instance)(rate(node_cpu_seconds_total{mode="idle"}[5m])))```

![image](https://user-images.githubusercontent.com/92969676/177320967-0f94542b-96f5-4b9d-95df-9065d09acf1d.png)

- CPULA 1/5/15: три запроса: 

```node_load1, node_load5, node_load15```

![image](https://user-images.githubusercontent.com/92969676/177318225-353f8034-b2e4-4727-ab0d-e37daec859d3.png)

- Количество свободной оперативной памяти: 

```100*(node_memory_Inactive_bytes  / on (instance) node_memory_MemTotal_bytes)``` (чтобы в процентах посмотреть, еще на 100 умножил).

![image](https://user-images.githubusercontent.com/92969676/177319997-6e768241-8460-4aa2-902d-429644834867.png)

- Количество места на файловой системе: 

```node_filesystem_avail_bytes{fstype!~"tmpfs|fuse.lxcfs|squashfs"} / node_filesystem_size_bytes{fstype!~"tmpfs|fuse.lxcfs|squashfs"}```

![image](https://user-images.githubusercontent.com/92969676/177318786-61feb948-ae76-4a93-99b5-cfab1b71d327.png)

В Prometheus так же порпобовал повыбирать метрики произвольные...

![image](https://user-images.githubusercontent.com/92969676/177316874-d6db5f1f-60ef-4705-aab8-82d5d8ce70ec.png)


Чуть поднастроил ось Y, итоговый Дашборд:

![image](https://user-images.githubusercontent.com/92969676/177335380-ddac37f1-5653-4b6f-975d-c2b47c1c7822.png)


## Задание 3
Создайте для каждой Dashboard подходящее правило alert (можно обратиться к первой лекции в блоке "Мониторинг").

Для решения ДЗ - приведите скриншот вашей итоговой Dashboard.

## Задание 4
Сохраните ваш Dashboard.

Для этого перейдите в настройки Dashboard, выберите в боковом меню "JSON MODEL".

Далее скопируйте отображаемое json-содержимое в отдельный файл и сохраните его.

В решении задания - приведите листинг этого файла.

---

## Задание повышенной сложности

**В части задания 1** не используйте директорию [help](./help) для сборки проекта, самостоятельно разверните grafana, где в 
роли источника данных будет выступать prometheus, а сборщиком данных node-exporter:
- grafana
- prometheus-server
- prometheus node-exporter

За дополнительными материалами, вы можете обратиться в официальную документацию grafana и prometheus.

В решении к домашнему заданию приведите также все конфигурации/скрипты/манифесты, которые вы 
использовали в процессе решения задания.

**В части задания 3** вы должны самостоятельно завести удобный для вас канал нотификации, например Telegram или Email
и отправить туда тестовые события.

В решении приведите скриншоты тестовых событий из каналов нотификаций.
