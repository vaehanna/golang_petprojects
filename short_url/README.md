
```markdown
# Сокращение URL с использованием Go и Redis

Этот репозиторий - реализация службы сокращения URL с использованием Golang, Fiber и Redis. Каждый пользователь может отправить максимум 10 запросов за 30 минут, чтобы сократить свои URL. Каждый сокращенный URL будет иметь срок действия по умолчанию 24 часа. Для настройки пользователи могут предложить и установить свой собственный сокращенный URL, а также срок его действия (до 720 часов).

Чтобы продолжить использовать сокращенный URL, пользователь также может продлить его старый срок действия, используя тот же URL и старый сокращенный URL с новым временем истечения срока действия.

## Особенности

- [x] Сократить URL
- [x] Настроить короткий URL
- [x] Настроить срок действия
- [x] Продлить срок действия

## Установка

Создайте файл `.env` внутри папки `api` с содержимым, аналогичным следующему:

```text
DB_ADDR="db:6379"
DB_PASS=""
APP_PORT=":3000"
DOMAIN="localhost:3000"
API_QUOTA=10
MAX_TRY=5
```

Вы можете изменить `DB_ADDR` и `DB_PASS`, чтобы использовать свой собственный сервер Redis.

Для развертывания измените `DOMAIN` на свой URL развертывания.

## Запуск службы с помощью Docker Compose

### Чтобы запустить приложение

Шаг 1: запустите Redis и приложение Go

```bash
docker-compose -f docker-compose.yaml up
```

#### Структура запроса и ответа

```Go
type request struct {
 URL         string        `json:"url"`
 CustomShort string        `json:"short"`
 Expiry      time.Duration `json:"expiry"`
}

type response struct {
 URL             string        `json:"url"`
 CustomShort     string        `json:"short"`
 Expiry          time.Duration `json:"expiry"`
 XRateRemaining  int           `json:"rate_limit"`
 XRateLimitReset time.Duration `json:"rate_limit_reset"`
}
```

#### Сокращение URL

```text
    POST http://localhost:3000/api/v1
```

Пример:

```json
{
    url: "https://www.youtube.com/watch?v=p-6pvjsH_ic"
}
```

#### Разрешение URL

```text
    GET http://localhost:3000/{id}
```

### Чтобы создать образ Docker из приложения

```bash
docker build -t mygo:1.0 ./api       
```
```

