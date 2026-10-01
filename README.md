<div align="center">

# SENE44 • Official Bio & Music Ecosystem

[![Website](https://img.shields.io/badge/Live_Site-sene44bio.github.io-6366f1?style=for-the-badge&logo=google-chrome&logoColor=white)](https://sene44bio.github.io/)
[![Status](https://img.shields.io/badge/Deployment-GitHub_Pages-10b981?style=for-the-badge&logo=github)](https://sene44bio.github.io/)

<p align="center">
  Персональный веб-хаб, музыкальный плеер и витрина проектов исполнителя и разработчика Sene44.
</p>

[Сайт](https://sene44bio.github.io/) • [Яндекс Музыка](https://music.yandex.ru/artist/25966786) • [Fandom](https://sene44.fandom.com/ru/wiki/Sene44_Вики) • [Genius](https://genius.com/artists/Sene44)

---

</div>

## О проекте

Сайт представляет собой кастомную веб-визитку и медиа-хаб, спроектированный с акцентом на чистый Vanilla стек без сторонних зависимостей, неоновый интерфейс, адаптивность под мобильные экраны и встроенные инструменты связи с аудиторией.

### Ключевой функционал

- **Web Audio API Спектральный анализатор:**
  - Обработка звуковых частот в реальном времени через `AudioContext` и `AnalyserNode` без сторонних библиотек.
  - Круговой частотный эквалайзер на 56 спектральных полос вокруг аватара.
  - **Bass Bounce Effect:** расчет амплитуды суб-баса (20-150 Hz) с синхронным зумом и неоновым свечением аватара под кик и 808-й бас.
- **Встроенный аудиоплеер:**
  - Нативный range-слайдер перемотки с кастомным бегунком и отображением прогресса.
  - Анимированные индикаторы воспроизведения и тайминг.
- **Анонимные сообщения (Telegram Gateway):**
  - Модальное окно для отправки сообщений, панчей и вопросов.
  - Серверная часть на **Cloudflare Workers (`tg-proxi`)**: скрывает токен бота и Chat ID, решает проблему доступности API Telegram и защищает от спама.
- **Оптимизированный Canvas FX:**
  - Фоновый снегопад с покачиванием по синусоиде на чистом Canvas 2D (минимальная нагрузка на CPU и батарею устройства).
- **Счетчик посещений:**
  - Публичный API счетчик с автоматическим фоллбэком на `localStorage` при отсутствии сети.
- **SEO и семантика:**
  - Микроразметка `Schema.org/Person` (JSON-LD) для связи профилей артиста (Яндекс Музыка, Genius, Fandom, Telegram, VK, YouTube) в поисковых системах.
  - Подтвержденный домен в Google Search Console.
  - Разметка Open Graph и Twitter Cards.

---

## Технологический стек

| Слой | Технологии |
| :--- | :--- |
| **Frontend** | HTML5, CSS3 (CSS Variables, Flexbox, CSS Grid), JavaScript (ES6+) |
| **Audio** | Web Audio API (`AudioContext`, `AnalyserNode`, `createMediaElementSource`) |
| **Graphics** | HTML5 Canvas 2D |
| **Backend** | Cloudflare Workers (`tg-proxi`), Telegram Bot API |
| **Hosting** | GitHub Pages |
| **SEO** | Schema.org JSON-LD, Open Graph, Google Search Console |

---

## Архитектура отправки сообщений

```mermaid
sequenceDiagram
    autonumber
    actor User as Пользователь
    participant Web as Sene44 Bio (Frontend)
    participant CF as Cloudflare Worker (tg-proxi)
    participant TG as Telegram Bot API
    actor Sene as Sene44 (Telegram)

    User->>Web: Ввод текста в форму
    Web->>CF: POST /tg-proxi (JSON)
    Note over CF: Валидация, лимит длины,<br/>подстановка токена
    CF->>TG: POST /sendMessage
    TG-->>Sene: Новое анонимное сообщение
    TG-->>CF: 200 OK
    CF-->>Web: {"success": true}
    Web-->>User: Уведомление об успешной отправке
