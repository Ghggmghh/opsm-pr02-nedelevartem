# opsm-pr02-nedelevartem
# Практична робота № 2
Дисципліна: Основи побудови інформаційних систем та мереж (ОК-13)

Тема: Структура повідомлень прикладного протоколу HTTP. Формування запиту вручну

| Поле | Значення |
|---|---|
| Студент Недєлєв Артемій Євгенович | 
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
