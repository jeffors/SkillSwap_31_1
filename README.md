<div align="center">

# 🔄 SkillSwap

### «Я научу / Хочу научиться» — платформа для обмена навыками

[![React](https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white)](https://vitejs.dev/)
[![React Router](https://img.shields.io/badge/React_Router-CA4245?style=flat-square&logo=reactrouter&logoColor=white)](https://reactrouter.com/)
</div>

---

## 📖 О проекте

**SkillSwap** — одностраничное (SPA) приложение, в котором пользователи публикуют навыки двух типов:

- **«Учу»** — навыки, которыми пользователь готов делиться;
- **«Учусь»** — навыки, которым пользователь хочет научиться.

Сервис помогает находить взаимно подходящие пары, отправлять заявки на обмен и вести список текущих и завершённых сессий. Все данные берутся из JSON-моков; пользовательское состояние (избранное, заявки, авторизация) хранится в `localStorage`.

## 👤 Пользовательский сценарий

1. Гость открывает каталог навыков и может искать/фильтровать без перезагрузки страницы.
2. Открыв карточку навыка (`/skill/:id`), пользователь видит описание, автора и кнопку **«Предложить обмен»**.
3. Неавторизованный пользователь при попытке предложить обмен перенаправляется на `/login` с возвратом назад после входа.
4. Автор навыка принимает или отклоняет заявку — оба участника получают toast-уведомление.
5. Любой участник может завершить активную сессию.
6. Пользователь создаёт новый навык через форму `/create` с валидацией полей.
7. Навыки можно добавлять в избранное — список доступен на `/favorites`.

## ✨ Функциональность 

| Модуль | Описание |
|---|---|
| 🗂️ Каталог навыков | Загрузка из `skills.json`, бесконечный скролл/пагинация (≥ 20 карточек за подгрузку) |
| 🔍 Поиск и фильтры | Поиск по тексту, фильтр по категории, переключатель «Учу / Учусь / Все» (логика AND) |
| 📄 Страница навыка | Полная информация, кнопка обмена, блок похожих навыков (до 4) |
| 🔐 Аутентификация | Моковый логин/регистрация, редирект на предыдущий URL после входа |
| 🤝 Заявки на обмен | Создание / принятие / отклонение / завершение, хранение в `localStorage` |
| 👤 Профиль | Вкладки «Мои навыки» и «Мои заявки» без перезагрузки страницы |
| ➕ Создание навыка | Валидация полей (заголовок, описание, категория, теги, изображение до 2 МБ) |
| ❤️ Избранное | Toggle-кнопка на карточке, отдельная страница `/favorites` |
| 🧭 Роутинг | React Router v6+, ленивая загрузка страниц, кастомная 404 |

- 🌗 Тёмная тема (CSS variables + автосохранение выбора)
- 📅 Экспорт сессии в `.ics` для календаря
- 🏆 Бейджи/достижения с toast-уведомлениями
- 💬 Чат-заглушка на моке WebSocket
- 📶 PWA с офлайн-fallback страницей

## 🛠️ Стек технологий

- **Frontend:** React, TypeScript, Vite
- **Роутинг:** React Router (кастомный `PrivateRoute` для защищённых страниц)
- **Состояние:** Redux Toolkit (`services/slices`, `services/store.ts`)
- **Формы и валидация:** React Hook Form + Zod (`utils/schemas`)
- **UI-документация:** Storybook (`.storybook`, `*.stories.tsx` для большинства компонентов)
- **Тесты:** Vitest (+ настройка `vitest.setup.ts` для Storybook)
- **Git-хуки:** Husky (`pre-commit`)
- **Линтинг:** ESLint (flat config), Prettier, Stylelint
- **Хранение данных:** JSON-моки (`public/db`) + мок-стор (`services/mockStore`) + `localStorage`

## 📁 Структура проекта

```
src/
 ├── api/                 # методы работы с API/мок-данными (api.ts)
 ├── components/
 │    ├── ui/             # переиспользуемые UI-компоненты (Button, Input, Select,
 │    │                   #   Carousel, Modal, CreateForm, RegisterForm* и др.),
 │    │                   #   каждый со своими .module.css и .stories
 │    ├── header/         # шапка: nav, поиск, дропдауны, профиль, авторизация
 │    ├── footer/
 │    ├── aside/
 │    ├── UserCard*/      # карточки пользователей и офферов
 │    └── FilterCheckbox / FilterRadio / FilterNested
 ├── pages/               # MainPage, SkillPage, CreatePage, ProfilePage,
 │                        #   FavoritesPage, RegisterPage, NotFound404
 ├── routes/              # index.tsx, PrivateRoute, path-constants.ts
 ├── services/
 │    ├── slices/         # citiesSlice, exchangeSlice, filtersSlice,
 │    │                   #   profileSlice, skillsSlice, usersSlice
 │    ├── mockStore/      # mockSkills, mockUsers, mockCities, mockStore
 │    └── store.ts        # конфигурация Redux-стора
 ├── hooks/               # use-switch, useInput
 ├── utils/
 │    ├── schemas/        # zod-схемы валидации (profile, registration)
 │    ├── skill-category/
 │    ├── date/
 │    └── storiesHOC/
 ├── images/              # иконки, иллюстрации, изображения навыков
 ├── stories/             # служебные Storybook-примеры
 ├── App.tsx / main.tsx / index.css / App.css
 └── svg.d.ts / vite-env.d.ts
public/
 ├── db/
 │    ├── skills.json
 │    ├── users.json
 │    ├── user.json
 │    ├── cities.json
 │    └── profile-pics/       # аватары пользователей
 ├── fonts/                   # Inter, Jost, Open Sans, Roboto (variable)
 ├── icons/
 ├── og/                      # og-баннер для соцсетей
 └── favicon*, manifest, apple-touch-icon
.storybook/                   # конфигурация Storybook (main.ts, preview.ts, vitest.setup.ts)
.husky/                       # git-хуки (pre-commit)
```

## 🚀 Установка и запуск

```bash
# клонировать репозиторий
git clone https://github.com/jeffors/SkillSwap_31_1.git
cd SkillSwap_31_1

# установить зависимости
npm i

# запустить dev-сервер
npm run dev
```

Приложение будет доступно по адресу `http://localhost:5173` (порт может отличаться).

## 📜 Доступные npm-скрипты

| Команда | Описание |
|---|---|
| `npm run dev` | запуск dev-сервера |
| `npm run build` | сборка production-версии |
| `npm run lint` | проверка ESLint/Stylelint |
| `npm run test` | запуск unit-тестов (Vitest) |
| `npm run storybook` | запуск Storybook для просмотра UI-компонентов изолированно |
| `npm run build-storybook` | сборка статической версии Storybook |

Сделано с ❤️ командой SkillSwap
