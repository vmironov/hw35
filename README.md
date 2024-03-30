# Домашняя работа №35

Теоретическая часть описана на вики https://github.com/vmironov/hw35/wiki/%D0%A2%D0%B5%D0%BE%D1%80%D0%B5%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B0%D1%8F-%D1%87%D0%B0%D1%81%D1%82%D1%8C

В практической части реализован второй вариант:
1. Взаимодействие между микросервисами биллинговой подсистемы осуществляется через RestAPI
2. Взаимодействие с сервисом нотификаций осуществляется посредством очереди в Kafka.


## Описание системы:

1. Биллинговая подсистема состоит из: 
  - accountService - сервис управления лицевыми счетами
  - chargeService - сервис для создания начислений
  - paymentService - сервис для приёма платежей
  - orderingService - сервис обработки заказа
2. Сервис доставки нотификаций состоит из:
  - notifyService - сервис доставки сообщений (сохранения их в БД)
  - notifyConsumerService - сервис получения сообщений из топика Kafka

### Архитектура системы
![Архитектура системы](https://github.com/vmironov/hw35/blob/main/img/4.%20otus%20-%20%D0%9F%D1%80%D0%B0%D0%BA%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B0%D1%8F%20%D1%87%D0%B0%D1%81%D1%82%D1%8C%20-%20%D0%BA%D0%BE%D0%BC%D0%BF%D0%BE%D0%BD%D0%B5%D0%BD%D1%82%D0%BD%D0%B0%D1%8F%20%D1%81%D1%85%D0%B5%D0%BC%D0%B0.png?raw=true)

### Пример взаимодействия при обработке заказа
![Пример взаимодействия](https://github.com/vmironov/hw35/blob/main/img/4.%20otus%20-%20%D0%9F%D1%80%D0%B0%D0%BA%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B0%D1%8F%20%D1%87%D0%B0%D1%81%D1%82%D1%8C%20-%20%D1%81%D0%B8%D0%BA%D0%B2%D0%B5%D0%BD%D1%81.png?raw=true)


## Инструкция по установке:

1. Установить основную систему
- $ cd helm
- $ helm install hw35 ./

2. Установить в этот же неймспейс кафку
- $ helm repo add bitnami https://charts.bitnami.com/bitnami
- $ helm install kfk bitnami/kafka --version 28.0.0 --namespace hw35-in-helm -f ./values-kafka.yaml 

>[!IMPORTANT]
>Необходимо дождаться пока не запустится кластер kafka. Он запускается не сразу.
>
>После того как будет запущен kafka подождать пока автоматически восстановится notify-consumer-deployment

3. Запустить тесты на newman
- $ cd ../
- $ newman run HW35.postman_collection.json

