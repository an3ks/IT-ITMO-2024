# Лабораторная работа №4: Настройка и работа с Docker

## **Цель работы**
1. Научиться работать с Docker: создавать образы, запускать контейнеры и настраивать их взаимодействие.
2. Установить и настроить сеть между двумя контейнерами.

---

## **Ход работы**

### 1. **Установка Docker**
Docker был установлен на Linux-системе. В качестве окружения использовалась виртуальная машина с установленной ОС Ubuntu.

---

### 2. **Создание Dockerfile и установка aafire**
- Создан Dockerfile с базовым образом Ubuntu.
- В Dockerfile добавлены команды для установки необходимых пакетов:
  - `aafire`: приложение для визуализации.
  - `iputils-ping`: утилита для проверки сетевого соединения.

**Содержимое Dockerfile**:
```Dockerfile
# Использование официального образа Ubuntu
FROM ubuntu:latest

# Установка необходимых утилит
RUN apt-get update && apt-get install -y aafire iputils-ping

# Запуск aafire
CMD ["aafire"]
```

- Собран образ командой:
```bash
docker build -t aafire_image .
```
![photo_2024-12-10 17 21 02](https://github.com/user-attachments/assets/97ce3821-cdc3-4178-93b7-c6ecb6caa568)

---

### 3. **Запуск контейнеров**
- Запущены два контейнера на основе созданного образа:
```bash
docker run -it --name container1 aafire_image
docker run -it --name container2 aafire_image
```
![photo_2024-12-10 17 21 03 (1)](https://github.com/user-attachments/assets/1d2245dd-e069-4cf3-8873-01b9cca4500f)

---

### 4. **Создание сети и подключение контейнеров**
- Создана сеть с помощью команды:
```bash
docker network create myNetwork
```
- Контейнеры подключены к этой сети:
```bash
docker network connect myNetwork container1
docker network connect myNetwork container2
```
- Настройки сети проверены с помощью команды:
```bash
docker network inspect myNetwork
```
Скриншоты с выводом информации о сети приложены.

---

### 5. **Тестирование соединения**
- В каждом контейнере установлена утилита `ping`.
- Проведено тестирование сетевого соединения между контейнерами с помощью команды `ping`.

**Результаты пинга**:
- Пинг от `container1` к `container2`: успешный, без потерь пакетов, минимальная задержка — 0.064 мс.
  ![photo_2024-12-10 17 21 04](https://github.com/user-attachments/assets/b72ebc5c-6fd1-43c9-b00e-aa18a4cbe7a7)

- Пинг от `container2` к `container1`: успешный, без потерь пакетов, минимальная задержка — 0.050 мс.
  ![photo_2024-12-10 17 21 00](https://github.com/user-attachments/assets/5399bbe1-4657-4a0c-ac71-c2dbbfafb6ba)


Скриншоты результатов пинга приложены.

---

## **Выводы**
- Научились создавать образы и контейнеры в Docker.
- Успешно настроена сеть между двумя контейнерами.
- Проверена возможность их взаимодействия с использованием утилиты `ping`.
