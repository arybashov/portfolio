# Alexander Rybashov — Portfolio Site

## Файлы

```
index.html   — сайт (GitHub Pages)
worker.js    — Cloudflare Worker (API proxy для бота)
README.md    — эта инструкция
```

---

## Шаг 1 — Разместить сайт на GitHub Pages

1. Зайди на [github.com](https://github.com) → **New repository**
2. Название: `portfolio` (или любое другое)
3. Загрузи `index.html` и `README.md` в корень
4. Зайди в **Settings → Pages → Source: Deploy from branch → main → / (root)**
5. Через 1–2 минуты сайт появится на:
   `https://ВАШ_ЛОГИН.github.io/portfolio`

---

## Шаг 2 — Получить бесплатный API ключ OpenRouter

OpenRouter бесплатен — карта не нужна, 50 запросов/день.

1. Зайди на [openrouter.ai](https://openrouter.ai) → Sign Up (Google или email)
2. Зайди в **Keys → Create Key**
3. Скопируй ключ — начинается с `sk-or-...`

---

## Шаг 3 — Создать Cloudflare Worker (прокси для бота)

Cloudflare Workers — бесплатно до 100 000 запросов/день.

### 3.1 Зарегистрируйся
Зайди на [workers.cloudflare.com](https://workers.cloudflare.com) → Create account (бесплатно)

### 3.2 Создай Worker
1. Dashboard → **Workers & Pages → Create**
2. Выбери **"Hello World" template**
3. Дай имя, например `arybashov-bot`
4. Нажми **Edit code** — вставь весь код из файла `worker.js`
5. Нажми **Deploy**

### 3.3 Добавь API ключ как секрет
1. В настройках Worker → **Settings → Variables → Environment Variables**
2. Нажми **Add variable**
   - Variable name: `OPENROUTER_API_KEY`
   - Value: твой ключ `sk-or-...`
   - Нажми **Encrypt**
3. **Save and deploy**

### 3.4 Скопируй URL Worker'а
После деплоя увидишь URL вида:
`https://arybashov-bot.ВАШ_ЛОГИН.workers.dev`

---

## Шаг 4 — Вставить URL Worker'а в index.html

Открой `index.html`, найди строку:

```js
const PROXY_URL = 'https://YOUR_WORKER.YOUR_SUBDOMAIN.workers.dev';
```

Замени на твой реальный URL:

```js
const PROXY_URL = 'https://arybashov-bot.ВАШ_ЛОГИН.workers.dev';
```

Сохрани и загрузи обновлённый `index.html` в GitHub.

---

## Шаг 5 — Ограничить CORS (рекомендуется)

В `worker.js` замени:
```js
const ALLOWED_ORIGIN = '*';
```
на твой реальный домен GitHub Pages:
```js
const ALLOWED_ORIGIN = 'https://ВАШ_ЛОГИН.github.io';
```
Это защитит Worker от использования с чужих сайтов.

---

## Шаг 6 — Добавить видео (Demo Reel)

В `index.html` найди:
```html
<!-- Replace src with actual demo reel URL -->
```

**Вариант A — YouTube (рекомендуется):**
Замени весь блок `<video>` на:
```html
<iframe
  src="https://www.youtube.com/embed/ВАШ_VIDEO_ID?autoplay=1&mute=1&loop=1&controls=0&playlist=ВАШ_VIDEO_ID"
  style="width:100%;height:100%;border:none;"
  allow="autoplay"
></iframe>
```

**Вариант B — прямой файл:**
Загрузи `.mp4` в репозиторий и пропиши путь:
```html
<video src="demo.mp4" autoplay muted loop playsinline>
```

---

## Про бота — модель `openrouter/auto`

Бот использует модель `openrouter/auto` — OpenRouter сам выбирает лучшую
доступную бесплатную модель для каждого запроса. Сейчас в ротации:
Llama 4 Maverick, Qwen 3, DeepSeek и другие.

Лимиты бесплатного плана: **50 запросов/день, 20/минуту** — для портфолио более чем достаточно.

---

## Итоговая структура GitHub репозитория

```
portfolio/
├── index.html    ← сайт
└── README.md     ← эта инструкция
```

`worker.js` хранится только в Cloudflare — в GitHub его загружать не нужно.
