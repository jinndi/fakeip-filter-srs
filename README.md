# FakeIP Filter for sing-box

Автоматическая сборка rule-set для исключения доменов из Fake-IP режима в **sing-box**.

Репозиторий собирает список доменов из локального файла `fakeip-filter.txt`, объединяет его с внешними rule-set источниками (`https://...json`) и публикует готовые файлы:

- `fakeip-filter.json` — rule-set JSON
- `fakeip-filter.srs` — бинарный формат sing-box

Сборка выполняется автоматически через GitHub Actions.

# Ссылка на файл `fakeip-filter.srs`

```
https://github.com/jinndi/fakeip-filter-srs/releases/latest/download/fakeip-filter.srs 
```

---

# Что делает этот репозиторий

Workflow:

1. Читает `fakeip-filter.txt`
2. Загружает удалённые rule-set JSON по ссылкам
3. Объединяет локальные и удалённые правила
4. Удаляет дубликаты
5. Конвертирует wildcard правила
6. Генерирует sing-box rule-set JSON
7. Компилирует `.srs`
8. Публикует Release

Результат — готовый FakeIP exclude список.

---

# Формат fakeip-filter.txt

Поддерживаются следующие форматы строк:

### sing-box rule-set JSON источники

https://example.com/rules.json

➡ автоматически скачиваются и объединяются

поддерживаются поля:

- domain
- domain_suffix
- domain_keyword
- domain_regex

---

### обычные домены

example.com

➡ автоматически становятся `domain`

---

### wildcard

*.example.com

➡ автоматически становятся `domain_suffix`

---

### Clash-стиль

+.example.com

➡ автоматически становятся `domain_suffix`

---

### сложные wildcard (конвертируются в regex)

*.*.example.com
time.*.example.com

➡ автоматически становятся `domain_regex`

---

### прямые regex (sing-box format)

^.*\.example\.com$

➡ добавляются напрямую в `domain_regex`

Важно: строка должна начинаться с `^` или содержать regex‑паттерн


# Рекомендуемые категории для FakeIP exclude

Обычно включают:

LAN / локальная сеть (обязательно)

Connectivity check ОС (обязательно)

NTP (обязательно)

STUN / TURN (обязательно)

Игровая инфраструктура (рекомендуется)

Router cloud services (рекомендуется)

OS update сервисы (часто рекомендуется)

CDN авторизации (часто рекомендуется)

Wi-Fi calling / VoLTE (редко вспоминают, но важно)


Этот репозиторий уже поддерживает их добавление через rule-set ссылку.