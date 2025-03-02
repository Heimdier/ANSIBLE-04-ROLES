# Ansible Playbook for Installing Clickhouse, Vector and Lighthouse

## Описание

playbook предназначен для установки и настройки Clickhouse Vector и Lighthouse на целевых серверах. 

## Переменные

- `clickhouse_version`: Версия Clickhouse, которую следует установить.
- `clickhouse_packages`: Список пакетов Clickhouse для установки.
- `vector_version`: версия vector
- `vector_config_dir`: путь до файла конфигурации vector
- `vector_config_file`: имя файла конфигурации vector
- `lighthouse_code_src`: путь к репозитарию lighthouse
- `lighthouse_data_dir`: путь к каталогу установки lighthouse
- `lighthouse_packages`: пакеты для установки из репозитария
- `lighthouse_nginx_port`: порт на котором lighthouse
- `lighthouse_nginx_conf`: имя файла конфигурации

## Инструкции по установке

1. **Склонируйте репозитарий**

2. **Подготовьте инвентори файл:**

Укажите целевые хосты, добавив их в файл инвентори, `inventory/prod.yml`:


3. **Запуск playbook:**

   Выполните команду для запуска playbook:

```shell
   ansible-playbook -i inventory/prod.yml site.yml
```
   
## Задачи

- **Установка Clickhouse:**
  - Скачивает необходимые пакеты.
  - Устанавливает их с помощью `apt`.
  - Создаёт базу данных.

- **Установка Vector:**
  - Скачивает необходимые пакеты.
  - Устанавливает их с помощью `apt`.
  - Через template накатывает конфиг для vector

- **Установка Lighthouse:**
  - Загружает Lighthouse из репозитория.
  - Устанавливает и настраивает Nginx для доступа к Lighthouse
  - Применяет шаблоны настройки через template


