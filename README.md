<div align="center">

#  SENE44 • Official Bio & Music Ecosystem

[![Website](https://img.shields.io/badge/Live_Site-sene44bio.github.io-6366f1?style=for-the-badge&logo=google-chrome&logoColor=white)](https://sene44bio.github.io/)
[![GitHub stars](https://img.shields.io/github/stars/sene44bio/sene44bio.github.io?style=for-the-badge&color=a855f7)](https://github.com/sene44bio/sene44bio.github.io)
[![Status](https://img.shields.io/badge/Deployment-GitHub_Pages-10b981?style=for-the-badge&logo=github)](https://sene44bio.github.io/)

<p align="center">
  <b>Высокопроизводительный интерактивный персональный bio-link хаб рэп-исполнителя, битмейкера и независимого разработчика Sene44.</b>
</p>

[🌐 Открыть сайт](https://sene44bio.github.io/) • [🎧 Яндекс Музыка](https://music.yandex.ru/artist/25966786) • [📖 Fandom Lore](https://sene44.fandom.com/ru/wiki/Sene44_Вики) • [🎤 Genius](https://genius.com/artists/Sene44)

---

</div>

##  О проекте

Сайт представляет собой кастомную веб-визитку и медиа-хаб, спроектированный с акцентом на **Zero-Dependency Vanilla Web**, отзывчивый неоновый дизайн, глубокую поисковую оптимизацию (SEO / Schema.org) и встроенные инди-инструменты взаимодействия с аудиторией.

###  Ключевые фичи

- ** Web Audio API Спектральный анализатор:**
  - Реальная обработка звуковых частот трека через `AudioContext` и `AnalyserNode` без тяжелых библиотек.
  - Круговой частотный эквалайзер на 56 спектральных лучей вокруг аватара.
  - **Bass Bounce Effect:** математический расчет энергии суб-баса (20–150 Hz) с синхронным зумом и неоновым свечением аватара под кик и 808-й бас.
- ** Встроенный аудиоплеер:**
  - Плавный range-слайдер перемотки с кастомным бегунком и подсветкой прогресса.
  - Анимированные мини-эквалайзеры и точный тайминг.
- ** Анонимный Drop Box (Anti-Censorship Telegram Gateway):**
  - Модальное окно для сбора фидбека, панчей и вопросов от слушателей.
  - Защищённый бэкенд на **Cloudflare Workers (`tg-proxi`)**: скрывает токен бота и Chat ID, обходит блокировки API Telegram в РФ и защищает от спама.
- ** Оптимизированный Canvas FX:**
  - Легковесный снегопад с покачиванием по синусоиде на нативном HTML5 Canvas (60+ FPS на мобильных устройствах, минимум нагрузки на CPU/аккумулятор).
- ** Живой счетчик просмотров:**
  - Публичный сетевой счетчик API с автоматическим переключением на `localStorage` при сбоях сети.
- ** SEO & AI Knowledge Graph Ready:**
  - Вшитая микроразметка `Schema.org/Person` (JSON-LD) для объединения сущностей артиста (Яндекс Музыка, Genius, Fandom, Telegram, VK, YouTube) в поисковиках и AI-моделях.
  - Подтверждённый домен в Google Search Console для ускоренной индексации.
  - Полный комплект тегов Open Graph и Twitter Cards для красивых сниппетов.

---

##  Технологический стек

| Слой | Технологии |
| :--- | :--- |
| **Frontend** | Vanilla HTML5, CSS3 (Modern Glassmorphism, CSS Custom Properties, Bento Grid Layout), Vanilla JavaScript (ES6+) |
| **Audio Engine** | Web Audio API (`AudioContext`, `AnalyserNode`, `createMediaElementSource`) |
| **Graphics & FX** | HTML5 Canvas 2D Context |
| **Serverless Backend** | Cloudflare Workers (`tg-proxi`), Telegram Bot API |
| **Hosting & Deploy** | GitHub Pages (CI/CD через ветку `main`) |
| **SEO / Semantic Web** | Schema.org JSON-LD, Open Graph, Google Search Console |

---

##  Архитектура отправки сообщений

```mermaid
sequenceDiagram
    autonumber
    actor Слушатель as Фанат / Пользователь
    participant Web as Sene44 Bio (Frontend)
    participant CF as Cloudflare Worker (tg-proxi)
    participant TG as Telegram Bot API
    actor Sene as Sene44 (Telegram)

    Слушатель->>Web: Ввод сообщения в анонимный Drop Box
    Web->>CF: POST [https://tg-proxi.arsekshoy00.workers.dev](https://tg-proxi.arsekshoy00.workers.dev) (JSON)
    Note over CF: Валидация текста, фильтрация длины,<br/>скрытие токена и Chat ID
    CF->>TG: POST [https://api.telegram.org/bot](https://api.telegram.org/bot)<TOKEN>/sendMessage
    TG-->>Sene: 📬 Новое анонимное сообщение в личку
    TG-->>CF: 200 OK
    CF-->>Web: {"success": true}
    Web-->>Слушатель: Показ тоста «Сообщение доставлено!»
