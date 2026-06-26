# Kittygram API

Django REST API для управления котами, их владельцами и достижениями.

## Локальный запуск

### Windows PowerShell

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

### Linux / macOS

```bash
python3 -m venv venv
source venv/bin/activate
python3 -m pip install --upgrade pip
pip install -r requirements.txt
python3 manage.py migrate
python3 manage.py createsuperuser
python3 manage.py runserver
```

Приложение будет доступно на `http://localhost:8000`.

## Основные адреса

- Админка: `http://localhost:8000/admin/`
- Swagger: `http://localhost:8000/swagger/`
- ReDoc: `http://localhost:8000/redoc/`
- API: `http://localhost:8000/api/`
- Коты: `http://localhost:8000/api/cats/`
- Пользователи: `http://localhost:8000/api/users/`
- Достижения: `http://localhost:8000/api/achievements/`

## Docker

```bash
docker-compose up -d --build
docker-compose logs -f web
docker-compose down
```

При запуске через Docker контейнер выполняет миграции, загружает тестовые данные и поднимает сервер на `http://localhost:8000`.

## Тестовые данные

Тестовые записи можно загрузить вручную:

```bash
python manage.py shell < create_test_data.py
```
