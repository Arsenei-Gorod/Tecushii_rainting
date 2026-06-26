# Kittygram API

Kittygram API - учебный Django REST проект для хранения информации о котах, их владельцах и достижениях.

## Быстрый запуск проекта

### Windows PowerShell

Склонируйте репозиторий и перейдите в папку проекта:

```powershell
git clone https://github.com/Arsenei-Gorod/Tecushii_rainting.git
cd Tecushii_rainting
```

Создайте виртуальное окружение и активируйте его:

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

Обновите `pip` и установите зависимости:

```powershell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Примените миграции базы данных:

```powershell
python manage.py migrate
```

Создайте администратора:

```powershell
python manage.py createsuperuser
```

Для тестового доступа можно указать такие данные:

- Username: `admin`
- Email: `admin@example.com`
- Password: `admin123`

Запустите сервер разработки:

```powershell
python manage.py runserver
```

После запуска приложение будет доступно по адресу `http://localhost:8000`.

### Linux / macOS

```bash
git clone https://github.com/Arsenei-Gorod/Tecushii_rainting.git
cd Tecushii_rainting
python3 -m venv venv
source venv/bin/activate
python3 -m pip install --upgrade pip
pip install -r requirements.txt
python3 manage.py migrate
python3 manage.py createsuperuser
python3 manage.py runserver
```

## Адреса приложения

Админ-панель:

```text
http://localhost:8000/admin/
```

Swagger-документация:

```text
http://localhost:8000/swagger/
```

ReDoc-документация:

```text
http://localhost:8000/redoc/
```

Основной API:

```text
http://localhost:8000/api/
```

Основные ресурсы API:

- `http://localhost:8000/api/cats/`
- `http://localhost:8000/api/users/`
- `http://localhost:8000/api/achievements/`

## Запуск через Docker

Перед запуском проверьте, что Docker и Docker Compose установлены:

```bash
docker --version
docker-compose --version
```

Соберите и запустите контейнеры в фоне:

```bash
docker-compose up -d --build
```

Посмотреть логи веб-приложения:

```bash
docker-compose logs -f web
```

Проверить состояние контейнеров:

```bash
docker-compose ps
```

Сервис будет доступен на `http://localhost:8000`.

## Управление контейнерами

Остановить контейнеры без удаления данных:

```bash
docker-compose stop
```

Запустить остановленные контейнеры:

```bash
docker-compose start
```

Остановить и удалить контейнеры:

```bash
docker-compose down
```

Полностью очистить контейнеры и volumes проекта:

```bash
docker-compose down -v
```

## Полезные Docker-команды

Пересобрать проект после изменений в коде:

```bash
docker-compose up -d --build
```

Выполнить миграции внутри контейнера:

```bash
docker-compose exec web python manage.py migrate
```

Создать суперпользователя в контейнере:

```bash
docker-compose exec web python manage.py createsuperuser
```

Показать список запущенных контейнеров:

```bash
docker ps
```

Посмотреть локальные Docker-образы:

```bash
docker images
```

Удалить образ проекта:

```bash
docker rmi tecushii_rainting-web
```

Удалить неиспользуемые образы:

```bash
docker image prune
```

Глубокая очистка Docker-ресурсов:

```bash
docker system prune -a --volumes
```

## Тестовые данные

Демо-данные можно загрузить командой:

```bash
python manage.py shell < create_test_data.py
```
