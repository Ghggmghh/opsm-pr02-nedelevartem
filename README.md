# opsm-pr02-nedelevartem
# Практична робота № 2
Дисципліна: Основи побудови інформаційних систем та мереж (ОК-13)

Тема: Структура повідомлень прикладного протоколу HTTP. Формування запиту вручну

| Поле | Значення |
|---|---|
| Студент | Недєлєв Артемій Євгенович | 
| Група | F5 2.02 |
| Номер варіанта | 17 |
| Індивідуальний домен | example.org |
| «Чужий» домен для завдання A.3.1 (варіант ± 15) | tnpu.edu.ua |
| Середовище виконання | Killercoda Ubuntu Playground |
| Дата виконання | 05.10.2026 |
## Частина A. Збір експериментальних даних

### Завдання A.1. Формування запиту вручну
**Команда:**

```bash
nc -C example.org 80
```
Набраний запит:
```bash
GET / HTTP/1.1
Host: example.org
Connection: close
```
Відповідь:
```bash
HTTP/1.1 200 OK
Accept-Ranges: bytes
Age: 512394
Cache-Control: max-age=604800
Content-Type: text/html; charset=UTF-8
Date: Mon, 05 Oct 2026 15:00:10 GMT
Etag: "3147526947"
Expires: Mon, 12 Oct 2026 15:00:10 GMT
Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
Server: ECS (dcb/7EEB)
Vary: Accept-Encoding
X-Cache: HIT
Content-Length: 1256
Connection: close

<!doctype html>
<html>
<head>
    <title>Example Domain</title>
    <meta charset="utf-8" />
    <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
...
</html>
```
# Завдання A.2. Запит без поля Host у версії 1.1
Команда:
```bash
printf 'GET / HTTP/1.1\r\nConnection: close\r\n\r\n' | nc example.org 80 
```
Вивід:
```bash
HTTP/1.1 400 Bad Request
Content-Type: text/html
Content-Length: 349
Connection: close
Date: Mon, 05 Oct 2026 15:05:22 GMT
Server: ECS (dcb/7EEB)

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>400 Bad Request</title>
</head><body>
<h1>Bad Request</h1>
<p>Your browser sent a request that this server could not understand.<br />
</p>
</body></html>
```
# Завдання A.3. Вплив поля Host на відповідь сервера
A.3.1. Чуже доменне ім'я в полі Host
Команда:
```bash
printf 'GET / HTTP/1.1\r\nHost: tnpu.edu.ua\r\nConnection: close\r\n\r\n' | nc example.org 80 
```
Вивід:
```bash
HTTP/1.1 404 Not Found
Content-Type: text/html
Content-Length: 315
Connection: close
Date: Mon, 05 Oct 2026 15:06:12 GMT
Server: ECS (dcb/7EEB)

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>404 Not Found</title>
</head><body>
<h1>Not Found</h1>
<p>The requested URL was not found on this server.</p>
</body></html>
```
A.3.2. Неіснуюче ім'я в полі Host
Команда:
```bash
printf 'GET / HTTP/1.1\r\nHost: opism-pr02.invalid\r\nConnection: close\r\n\r\n' | nc example.org 80 
```
Вивід:
```bash
HTTP/1.1 404 Not Found
Content-Type: text/html
Content-Length: 315
Connection: close
Date: Mon, 05 Oct 2026 15:07:05 GMT
Server: ECS (dcb/7EEB)

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>404 Not Found</title>
</head><body>
<h1>Not Found</h1>
<p>The requested URL was not found on this server.</p>
</body></html>
```
A.3.3. Запит без поля Host у версії 1.0
Команда:
```bash
printf 'GET / HTTP/1.0\r\n\r\n' | nc example.org 80 
```
Вивід:
```bash
HTTP/1.0 200 OK
Accept-Ranges: bytes
Age: 512398
Cache-Control: max-age=604800
Content-Type: text/html; charset=UTF-8
Date: Mon, 05 Oct 2026 15:08:15 GMT
Etag: "3147526947"
Expires: Mon, 12 Oct 2026 15:08:15 GMT
Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
Server: ECS (dcb/7EEB)
Vary: Accept-Encoding
X-Cache: HIT
Content-Length: 1256
Connection: close

<!doctype html>
<html>
<head>
    <title>Example Domain</title>
...
</html>
```
Зведення результатів наведено в Додатку Д.
# Завдання A.4. Два запити в одному з'єднанні
Команда:
```bash
printf 'GET /opism-pr02-12345 HTTP/1.1\r\nHost: example.org\r\n\r\nGET / HTTP/1.1\r\nHost: example.org\r\nConnection: close\r\n\r\n' | nc -C example.org 80 
```
Вивід:
```bash
HTTP/1.1 404 Not Found
Content-Type: text/html
Content-Length: 315
Date: Mon, 05 Oct 2026 15:10:00 GMT
Server: ECS (dcb/7EEB)

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>404 Not Found</title>
</head><body>
<h1>Not Found</h1>
<p>The requested URL was not found on this server.</p>
</body></html>
HTTP/1.1 200 OK
Accept-Ranges: bytes
Age: 512410
Cache-Control: max-age=604800
Content-Type: text/html; charset=UTF-8
Date: Mon, 05 Oct 2026 15:10:00 GMT
Etag: "3147526947"
Expires: Mon, 12 Oct 2026 15:10:00 GMT
Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
Server: ECS (dcb/7EEB)
Vary: Accept-Encoding
X-Cache: HIT
Content-Length: 1256
Connection: close

<!doctype html>
<html>
<head>
    <title>Example Domain</title>
...
</html>
```
Кількість отриманих відповідей: 2

Коди стану отриманих відповідей: 404, 200
# Завдання A.5. Запит за допомогою клієнтської програми
Команда:
```bash
curl -v --http1.1 [http://example.org/](http://example.org/) -o /dev/null 
```
Вивід:
```bash
*   Trying 93.184.215.14:80...
* Connected to example.org (93.184.215.14) port 80 (#0)
> GET / HTTP/1.1
> Host: example.org
> User-Agent: curl/7.81.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Accept-Ranges: bytes
< Age: 512450
< Cache-Control: max-age=604800
< Content-Type: text/html; charset=UTF-8
< Date: Mon, 05 Oct 2026 15:11:05 GMT
< Etag: "3147526947"
< Expires: Mon, 12 Oct 2026 15:11:05 GMT
< Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
< Server: ECS (dcb/7EEB)
< Vary: Accept-Encoding
< X-Cache: HIT
< Content-Length: 1256
< 
{ [1256 bytes data]
* Connection #0 to host example.org left intact
```
# Завдання A.6. Запит через захищене з'єднання
Ресурс, на якому виконано завдання: власне example.org

Підстава для використання резервного ресурсу (заповнюють за потреби): Немає (домен підтримує порт 443).
Команда:
```bash
openssl s_client -connect example.org:443 -servername example.org -crlf -quiet 
```
Набраний запит:
```bash
GET / HTTP/1.1
Host: example.org
Connection: close
```
Вивід:
```bash
HTTP/1.1 200 OK
Accept-Ranges: bytes
Age: 512500
Cache-Control: max-age=604800
Content-Type: text/html; charset=UTF-8
Date: Mon, 05 Oct 2026 15:12:10 GMT
Etag: "3147526947"
Expires: Mon, 12 Oct 2026 15:12:10 GMT
Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
Server: ECS (dcb/7EEB)
Vary: Accept-Encoding
X-Cache: HIT
Content-Length: 1256
Connection: close

<!doctype html>
<html>
...
</html>
```
# Частина B. Розбір полів заголовка
Розбирається відповідь, отримана в завданні A.1.

Загальна кількість полів заголовка у відповіді: 13
| № | Поле заголовка | Значення | Призначення (власне формулювання) | Походження: сервер / проміжний вузол / не визначено | Обґрунтування |
|---|---|---|---|---|---|
| 1 | Accept-Ranges | bytes | Вказує, що сервер підтримує запити часткового контенту в байтах. | сервер | Стандартний параметр налаштування HTTP-сервера для роздачі статичного контенту. |
| 2 | Age | 512394 | Час (у секундах), який відповідь перебувала в кеші з моменту її отримання від вихідного сервера. | проміжний вузол | Заголовок генерується CDN або проксі-сервером кешування. |
| 3 | Cache-Control | max-age=604800 | Визначає правила кешування для клієнта та проміжних вузлів. | сервер | Кінцевий сервер встановлює політики життя ресурсу. |
| 4 | Content-Type | text/html; charset=UTF-8 | Тип MIME ресурса та кодування. | сервер | Вихідний сервер визначає формат своєї відповіді. |
| 5 | Date | Mon, 05 Oct 2026 15:00:10 GMT | Точні дата та час генерації відповіді. | сервер | Генерується сервером у момент формування відповіді. |
| 6 | Etag | "3147526947" | Унікальний ідентифікатор версії документа для перевірки змін. | сервер | Вихідний сервер розраховує хеш для статичного файлу. |
| 7 | Expires | Mon, 12 Oct 2026 15:00:10 GMT | Дата і час, після яких відповідь вважається застарілою. | сервер | Формується на основі політики `Cache-Control`. |
| 8 | Last-Modified | Thu, 17 Oct 2019 07:18:26 GMT | Дата останньої зміни оригінального файлу ресурсу. | сервер | Інформація береться з файлової системи вихідного сервера. |
| 9 | Server | ECS (dcb/7EEB) | Інформація про програмне забезпечення сервера. | не визначено | Початково задається кінцевим сервером, але Edge Cache Server (ECS) вказує, що заголовок міг бути перезаписаний вузлом CDN. |
| 10 | Vary | Accept-Encoding | Вказує, які заголовки запиту впливають на те, який варіант кешу буде видано. | сервер | Конфігурується вихідним сервером для правильної роботи CDN. |
| 11 | X-Cache | HIT | Статус потрапляння запиту в кеш проміжного вузла. | проміжний вузол | Явний заголовок CDN, що підтверджує, що відповідь віддано з кешу. |
| 12 | Content-Length | 1256 | Розмір тіла повідомлення (документа) в байтах. | сервер | Сервер обчислює розмір контенту, що віддається. |
| 13 | Connection | close | Вказує, чи повинно мережеве з'єднання залишатися відкритим після передачі. | сервер | Реакція сервера на аналогічний заголовок клієнта в запиті. |
