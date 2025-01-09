University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)
Year: 2024/2025
Group: K4112c
Author: Kopchigashev Ilya Andreevich
Lab: Lab1
Date of create: 07.01.2025
Date of finished: _

## Лабораторная работа №2 "Развертывание веб сервиса в Minikube, доступ к веб интерфейсу сервиса. Мониторинг сервиса."

**1. Запуск кластера minikube**
```
$ minikube start
```
**2. Написание манифеста deployment-react**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-react
spec:
  replicas: 2
  selector:
    matchLabels:
      app: react
  template:
    metadata:
      labels:
        app: react
    spec:
      containers:
        - image: ifilyaninitmo/itdt-contained-frontend:master
          name: react
          ports:
          - name: react-port
            containerPort: 3000
          env:
            - name: REACT_APP_USERNAME
              value: 'test_app'
            - name: REACT_APP_COMPANY_NAME
              value: 'test_company'
```

* kind: Deployment - Определяет тип ресурса как Deployment – это контроллер, который управляет количеством работающих Pod’ов и их обновлениями.
* spec.replicas: 2 - Deployment должен поддерживать 2 работающих Pod’а.
* spec.selector.matchLabels: app: react - Выборщик Pod’ов. Deployment управляет только Pod’ами с меткой app: react.
* spec.template: - Шаблон для создания Pod’ов.
* ports.containerPort: 3000 - Порт 3000 внутри контейнера, на который будет проброс порта извне.
* env: - Переменные окружения для контейнера.

**3. Манифест файла для Service**

```
apiVersion: v1
kind: Service
metadata:
  name: react-service
spec:
  type: NodePort
  selector:
    app: react
  ports:
  - protocol: TCP
    port: 3000
    targetPort: 3000
```

* port: 3000 - внешний порт сервиса
* targetPort: 3000 - порт в контейнере
* type: NodePort - это тип сервиса, который делает сервис доступным извне кластера Kubernetes через статический порт на каждом узле (node) кластера. В отличие от ClusterIP, который доступен только внутри кластера, NodePort обеспечивает внешний доступ.

**4. Создание Deployment и Service**
Команда ```minikube kubectl -- apply -f deployment.yaml``` создает объект Deployment и два пода
Команда ```minikube kubectl -- apply -f service.yaml``` создает объект Service

**5. Проброс портов Service - pods**
Команда ```minikube kubectl -- port-forward service/react-service 3000:3000``` используется для проброса портов

**6. Переход на url для проверки**

По url ```localhost:3000``` видим

Переменные REACT_APP_USERNAME и REACT_APP_COMPANY_NAME неизменны, т.к были заданы в виде переменных среды при создании объекта deployment.
При этом переменная Container name может изменяться, что будет заметно, если адресация будет выполнена к разным контейнерам.

**7. Вывод логов**

**8. Схема**