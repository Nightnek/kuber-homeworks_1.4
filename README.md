# Домашнее задание к занятию «Сетевое взаимодействие в Kubernetes» Стасенко Григорий Михайлович

### Примерное время выполнения задания

120 минут

### Цель задания

Научиться настраивать доступ к приложениям в Kubernetes:
- Внутри кластера через **Service** (ClusterIP, NodePort).
- Снаружи кластера через **Ingress**.

Это задание поможет вам освоить базовые принципы сетевого взаимодействия в Kubernetes — ключевого навыка для работы с кластерами.
На практике Service и Ingress используются для доступа к приложениям, балансировки нагрузки и маршрутизации трафика. Понимание этих механизмов поможет вам упростить управление сервисами в рабочих окружениях и снизит риски ошибок при развёртывании.


## **Задание 1: Настройка Service (ClusterIP и NodePort)**
### **Задача**
Развернуть приложение из двух контейнеров (`nginx` и `multitool`) и обеспечить доступ к ним:
- Внутри кластера через **ClusterIP**.
- Снаружи через **NodePort**.

### **Шаги выполнения**
1. **Создать Deployment** с двумя контейнерами:
   - `nginx` (порт `80`).
   - `multitool` (порт `8080`).
   - Количество реплик: `3`.
  
<img width="725" height="146" alt="image" src="https://github.com/user-attachments/assets/b0c1bf38-6224-4c70-935a-bfb4dfeb3da2" />


2. **Создать Service типа ClusterIP**, который:
   - Открывает `nginx` на порту `9001`.
   - Открывает `multitool` на порту `9002`.
  
<img width="715" height="130" alt="image" src="https://github.com/user-attachments/assets/c28f5946-ffca-4d96-a78b-14f60bd4f96f" />


3. **Проверить доступность** изнутри кластера:
```bash
 kubectl run test-pod --image=wbitt/network-multitool --rm -it -- sh
 curl <service-name>:9001 # Проверить nginx
 curl <service-name>:9002 # Проверить multitool
```

<img width="730" height="537" alt="image" src="https://github.com/user-attachments/assets/0540ec56-87c5-43dc-968d-7ed7de0466d8" />


4. **Создать Service типа NodePort** для доступа к `nginx` снаружи.
5. **Проверить доступ** с локального компьютера:
```bash
 curl <node-ip>:<node-port>
   ```
 или через браузер.

<img width="683" height="436" alt="image" src="https://github.com/user-attachments/assets/3b1c3116-bf5c-433b-a5e5-f992d9b45510" />



### **Что сдать на проверку**
- Манифесты:
  - `deployment-multi-container.yaml`
  - `service-clusterip.yaml`
  - `service-nodeport.yaml`
- Скриншоты проверки доступа (`curl` или браузер).

---
## **Задание 2: Настройка Ingress**
### **Задача**
Развернуть два приложения (`frontend` и `backend`) и обеспечить доступ к ним через **Ingress** по разным путям.

### **Шаги выполнения**
1. **Развернуть два Deployment**:
   - `frontend` (образ `nginx`).
   - `backend` (образ `wbitt/network-multitool`).
2. **Создать Service** для каждого приложения.
3. **Включить Ingress-контроллер**:
```bash
 microk8s enable ingress
   ```
4. **Создать Ingress**, который:
   - Открывает `frontend` по пути `/`.
   - Открывает `backend` по пути `/api`.
5. **Проверить доступность**:
```bash
 curl <host>/
 curl <host>/api
   ```
 или через браузер.

<img width="554" height="81" alt="image" src="https://github.com/user-attachments/assets/ec49ed45-45df-4af4-8110-2057b6c7805e" />
<img width="569" height="94" alt="image" src="https://github.com/user-attachments/assets/83e62edb-5c5a-4a3c-a68d-22d4043e41e8" />
<img width="548" height="68" alt="image" src="https://github.com/user-attachments/assets/24b07d0b-f93f-4de7-8a33-997f313316d2" />
<img width="790" height="100" alt="image" src="https://github.com/user-attachments/assets/b06eea93-5462-43d6-81cf-42cb07ceea24" />
<img width="786" height="483" alt="image" src="https://github.com/user-attachments/assets/c05b0082-e69a-4eb4-8d09-a40b0f1db6a1" />


### **Что сдать на проверку**
- Манифесты:
  - `deployment-frontend.yaml`
  - `deployment-backend.yaml`
  - `service-frontend.yaml`
  - `service-backend.yaml`
  - `ingress.yaml`
- Скриншоты проверки доступа (`curl` или браузер).
