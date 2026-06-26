# Kittygram API

Kittygram API - учебный Django REST проект для хранения информации о котах, их владельцах и достижениях.

## Быстрый запуск проекта

### Переменные окружения

Скопируйте пример настроек и при необходимости измените значения:

```bash
cp .env.example .env
```

Файл `.env.example` содержит переменные:

- `DEBUG` - режим отладки Django.
- `SECRET_KEY` - секретный ключ проекта.
- `ALLOWED_HOSTS` - список разрешенных хостов через запятую.
- `DATABASE_ENGINE` - backend базы данных.
- `DATABASE_NAME` - имя файла или базы данных.

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

Для проверки production-запуска можно использовать Gunicorn:

```bash
gunicorn kittygram2.wsgi:application --bind 127.0.0.1:8000
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

Для списков включена постраничная выдача по 10 объектов. Доступны параметры `search` и `ordering`, например:

```text
http://localhost:8000/api/cats/?search=Barsik
http://localhost:8000/api/cats/?ordering=birth_year
```

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

Контейнер `web` применяет миграции, загружает тестовые данные и запускает приложение через Gunicorn.

Посмотреть логи веб-приложения:

```bash
docker-compose logs -f web
```

В логах должен быть виден старт Gunicorn и строка вида:

```text
Listening at: http://0.0.0.0:8000
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

## Деплой через nginx, Gunicorn и systemd

В папке `deploy/` находятся примеры конфигураций:

- `deploy/kittygram.service` - systemd service для запуска Gunicorn.
- `deploy/nginx_kittygram.conf` - nginx site config для проксирования запросов к Gunicorn.

Пример последовательности команд на сервере:

```bash
sudo mkdir -p /var/www/kittygram
sudo chown -R $USER:www-data /var/www/kittygram
git clone https://github.com/Arsenei-Gorod/Tecushii_rainting.git /var/www/kittygram
cd /var/www/kittygram
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
python manage.py migrate
python manage.py collectstatic --noinput
```

Установите systemd service:

```bash
sudo cp deploy/kittygram.service /etc/systemd/system/kittygram.service
sudo systemctl daemon-reload
sudo systemctl enable kittygram
sudo systemctl start kittygram
sudo systemctl status kittygram
```

Установите nginx config:

```bash
sudo cp deploy/nginx_kittygram.conf /etc/nginx/sites-available/kittygram
sudo ln -s /etc/nginx/sites-available/kittygram /etc/nginx/sites-enabled/kittygram
sudo nginx -t
sudo systemctl reload nginx
```

Команды для подтверждения работоспособности:

```bash
systemctl status kittygram
curl -I http://localhost/api/
curl -I http://localhost/swagger/
```

## Проверка API для отчета

Пример успешного создания кота:

```json
{
  "name": "Barsik",
  "color": "Black",
  "birth_year": 2022,
  "achievements": [
    {
      "achievement_name": "Champion"
    }
  ]
}
```

Успешный ответ:

```json
{
  "id": 1,
  "name": "Barsik",
  "color": "Black",
  "birth_year": 2022,
  "achievements": [
    {
      "id": 1,
      "achievement_name": "Champion"
    }
  ],
  "owner": 1,
  "age": 4
}
```

Примеры ошибок:

```json
{"detail": "Authentication credentials were not provided."}
```

```json
{"birth_year": ["Проверьте год рождения!"]}
```

```json
{"non_field_errors": ["Имя не может совпадать с цветом!"]}
```

## Тестовые данные

Демо-данные можно загрузить командой:

```bash
python manage.py shell < create_test_data.py
```
