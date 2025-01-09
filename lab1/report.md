University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)
Year: 2024/2025
Group: K4112c
Author: Kopchigashev Ilya Andreevich
Lab: Lab1
Date of create: 07.01.2025
Date of finished: _

## Лабораторная работа №1 "Установка Docker и Minikube, мой первый манифест."

**1. Запуск кластера minikube**

```
minikube start
```
![image1](https://github.com/user-attachments/assets/6ca18bc2-c51d-434d-a5be-a8772399f35b)

**2. Запуск пода vault**

![image6](https://github.com/user-attachments/assets/961c218f-fa48-4687-828e-5f7632dde821)

Команда ```kubectl apply -f vault.yaml читает файл vault.yaml``` анализирует его содержимое (описание ресурсов), и создает или обновляет объекты Kubernetes в кластере, чтобы соответствовать описанию в файле манифеста, описанному ниже.
```
apiVersion: v1
kind: Pod
metadata:
  name: vault
  labels:
    app: vault
spec:
  containers:
  - name: vault
    image: hashicorp/vault:latest
    ports:
    - containerPort: 8200
```
Далее команда ```minikube kubectl -- expose pod vault --type=NodePort --port=8200``` создает объект service, дающий доступ к поду vault через порт 8200

Затем команда ```minikube kubectl -- port-forward service/vault 8200:8200``` устанавливает проброс порта 8200 из контейнера vault (внутри кластера Kubernetes) на порт 8200 на локальной машине

**3. Переход на url**

```
http://localhost:8200/ui/vault/dashboard
```
![image4](https://github.com/user-attachments/assets/451f515f-a0d4-416c-be37-4967e2456e42)

**4. Поиск токена**

Команда ```minikube kubectl logs vault``` осуществляет вывод логов контейнера, в логах находится строка ```Root Token: hvs.W6iLnQqWUmjLhGcyfRaxxr77``` с токеном

![image3](https://github.com/user-attachments/assets/4e0f8b8b-5333-43f2-a8d5-deef3418ff64)
