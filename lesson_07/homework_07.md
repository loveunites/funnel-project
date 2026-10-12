# Задание 7 · Instagram → ManyChat → заявка

## Рабочие ресурсы

| Ресурс | Адрес |
| --- | --- |
| Сайт | [ga4-analytics-lab-prodan.vercel.app](https://ga4-analytics-lab-prodan.vercel.app/) |
| Страница материала | [guide.html](https://ga4-analytics-lab-prodan.vercel.app/guide.html) |
| Ссылка материала с UTM | [guide.html с UTM](https://ga4-analytics-lab-prodan.vercel.app/guide.html?utm_source=instagram&utm_medium=social&utm_campaign=backend_guide&utm_content=reel01) |
| Форма | [contacts.html](https://ga4-analytics-lab-prodan.vercel.app/contacts.html) |
| Таблица заявок | [GA4 Analytics Lab — Leads](https://docs.google.com/spreadsheets/d/1jNJM7CbJgwH_QYHW2gVndenR7-NObTthXaTl3_H09ug/edit) |
| Measurement ID | `G-BXHYB8CYC5` |
| Учебный Instagram | `@prodani29720` |
| ManyChat | аккаунт `Jonh Smith`, Free |

## Сценарий

1. Комментарий `ГАЙД` под выбранной публикацией учебного Instagram запускает automation ManyChat.
2. ManyChat публикует ответ `Проверьте Direct — там будет ссылка на материал.` и отправляет личное сообщение с кнопкой `Получить материал`.
3. Кнопка открывает страницу:
   `https://ga4-analytics-lab-prodan.vercel.app/guide.html?utm_source=instagram&utm_medium=social&utm_campaign=backend_guide&utm_content=reel01`
4. Кнопка на странице материала переводит посетителя на `contacts.html`, сохраняя четыре UTM-параметра.
5. Отправка формы фиксирует `generate_lead` в GA4 и записывает строку в лист `leads`.

## Тексты automation

- Название: `IG · Backend Guide · Reel 01`.
- Текст публикации: `Хотите чек-лист «Как начать изучать backend»? Напишите ГАЙД в комментариях — получите ссылку в Direct.`
- Ключевое слово: `ГАЙД`.
- Public Reply: `Проверьте Direct — там будет ссылка на материал.`
- Private Reply: `Привет! Вот чек-лист «Как начать изучать backend». Нажмите «Получить материал», чтобы открыть его на сайте.`
- Кнопка: `Получить материал` → Open Website.

## Тестовая заявка

| Поле | Значение |
| --- | --- |
| request_id | `REQ-E37A61FB-1EA3-4FEA-B5D1-5C84B892C441` |
| name | `Тест Instagram` |
| email | `student@example.com` |
| direction | `backend` |
| utm_source | `instagram` |
| utm_medium | `social` |
| utm_campaign | `backend_guide` |
| status | `new` |

## Проверки

| Проверка | Результат |
| --- | --- |
| Instagram подключён к ManyChat | Подтверждено: в ManyChat виден канал `@prodani29720` |
| Пост с призывом написать `ГАЙД` | Опубликован в учебном Instagram; ManyChat видит публикацию `https://www.instagram.com/p/DeaTgbxjM_B/` |
| Automation с Specific Post и `ГАЙД` | Подтверждено: выбран Specific Post, ключевое слово `ГАЙД`, automation Live |
| Public Reply и Private Reply | Подтверждено: Public Reply сохранён, Private Reply и кнопка `Получить материал` сохранены |
| Переход на `guide.html` с UTM | Подтверждено скриншотом 06 |
| `page_view` и `generate_lead` | Подтверждено скриншотом 07 |
| Строка в `leads` с источником Instagram | Подтверждено скриншотом 08 |

## Скриншоты

| № | Файл | Что подтверждает | Состояние |
| --- | --- | --- | --- |
| 1 | `screens/01-connected.png` | Instagram подключён к ManyChat; виден `@prodani29720` | Готово |
| 2 | `screens/02-flow.png` | Flow Builder: триггер, Private Reply, кнопка; статус Live | Готово |
| 3 | `screens/03-trigger.png` | Specific Post и ключевое слово `ГАЙД` | Готово |
| 4 | `screens/04-comment.png` | Тестовый комментарий и публичный ответ | Готово: `ГАЙД` и автоматический ответ видны под публикацией |
| 5 | `screens/05-direct.png` | Личное сообщение с переходом к материалу | Готово: Direct тестового профиля показывает сообщение; мобильный кадр с кнопкой можно приложить дополнительно |
| 6 | `screens/06-guide-utm.png` | `guide.html` с UTM-параметрами | Готово |
| 7 | `screens/07-ga4.png` | `page_view` и `generate_lead` в GA4 Realtime | Готово |
| 8 | `screens/08-lead.png` | Строка заявки с `instagram` / `social` / `backend_guide` | Готово |

Все тестовые данные вымышленные. В форме использованы `Тест Instagram` и `student@example.com`.
