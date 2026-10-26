# FOLLOW — English landing page

Статический сайт: `index.html` + `privacy.html` + `terms.html` + папка `assets/`. Сборка и сервер не нужны.

## 1. Выложить на хостинг

Загрузите содержимое папки в корень сайта так, чтобы `index.html` лежал рядом с `assets/`.

- **Netlify** — app.netlify.com → Add new site → Deploy manually → перетащить папку. Свой домен: Domain settings.
- **Vercel** — vercel.com/new → загрузить папку как статический проект.
- **GitHub Pages / Cloudflare Pages** — залить в репозиторий, включить Pages.
- **Любой обычный хостинг** — по FTP в `public_html/` (или `www/`).

## 2. Подключить приём заявок

В `index.html` найдите `<form id="signup" novalidate data-endpoint="">` и впишите адрес в `data-endpoint`.

Пока адрес пустой, страница работает в режиме превью: показывает все шаги, но заявку никуда не отправляет и пишет об этом под формой.

Варианты:
- **Formspree** (быстро, без кода): создайте форму → получите адрес вида `https://formspree.io/f/xxxxxxx` → вставьте его в `data-endpoint`. Заявки приходят на почту и в кабинет.
- **Свой бэкенд**: страница отправляет `POST` с `Content-Type: application/json` два раза:
  1. `{"type":"signup","email":"…","consent":true,"adult":true,"news":false,"recognized":["monday","quit"],"ref_code":"abc1234","attribution":{"utm_source":"…","ref":"…"},"page":"/","ts":"…"}`
  2. `{"type":"survey","lead_id":"…","email":"…","answers":{"phone":"iphone","device":["oura"],"goal":["sleep"],"quit":"logging","age":"25-34","city":"Austin"},"skipped":false,"ts":"…"}`
  
  Ответ на первый запрос может вернуть `{"id":"…","ref_code":"…"}` — страница их подхватит. Двойное подтверждение email (double opt-in), письма и подсчёт места в очереди делает бэкенд.

## 3. Заменить заглушки

- `hello@[your-domain]` в футере `index.html`.
- `og:image`: замените `assets/brand/og-image.jpg` на полный адрес `https://ваш-домен/assets/brand/og-image.jpg` — так превью ссылки появится в соцсетях и мессенджерах.
- `privacy.html` и `terms.html` — это шаблоны: заполните поля в `[квадратных скобках]` и покажите юристу.

## 4. Аналитика

Страница отправляет события в `window.dataLayer`: `cta_click`, `card_select`, `form_step1_submit`, `form_q1…form_q5`, `form_complete`, `share_click`. Вставьте сниппет Google Tag Manager или GA4 в `<head>` — и настройте цели по этим событиям. UTM-метки и `?ref=` уходят вместе с заявкой.

## 5. Перед запуском

- Логотип `assets/brand/follow-logo.png` вырезан из превью — замените векторным файлом от дизайнера (SVG).
- Фото — Unsplash и Pexels, свободная лицензия.
- Добавьте cookie-баннер, если подключаете аналитику с cookies (обязательно для посетителей из ЕС/UK).
