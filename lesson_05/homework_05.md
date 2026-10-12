# Задание 5 · Контролируемая таблица заявок

## Адреса работы

| Что | Адрес или значение |
| --- | --- |
| Сайт | <https://ga4-analytics-lab-prodan.vercel.app/> |
| Форма без меток | <https://ga4-analytics-lab-prodan.vercel.app/contacts.html> |
| Форма с метками | <https://ga4-analytics-lab-prodan.vercel.app/contacts.html?utm_source=telegram&utm_medium=social&utm_campaign=lesson07> |
| Measurement ID | `G-BXHYB8CYC5` |
| Таблица `GA4 Analytics Lab — Leads` | <https://docs.google.com/spreadsheets/d/1jNJM7CbJgwH_QYHW2gVndenR7-NObTthXaTl3_H09ug/edit?gid=0#gid=0> |
| Apps Script | <https://script.google.com/u/0/home/projects/1zwFure8rZaMLKq6utWDnYmgsu_avRu0CAj_c0-p-7vFJBirK-Gu4Mv-i/edit> |
| Web app `/exec` | <https://script.google.com/macros/s/AKfycbwaGDNQbe8Xof-0_EpyCV5IMLVZNZjfbPypt0_jX17H-78mUaHlm869hvT7eHDj9HuY/exec> |
| Сайт: исходники | <https://github.com/loveunites/analytics_lab> |
| Сайт: Pull Request | <https://github.com/loveunites/analytics_lab/pull/1> |
| Ветка сдачи | `hw-05` |

К таблице открыт доступ «Все, у кого есть ссылка → Читатель». Форма и приёмник опубликованы; deployment Vercel завершился успешно.

## Таблица и приёмник

На листе `leads` закреплена первая строка. Девять заголовков: `request_id`, `created_at`, `name`, `email`, `direction`, `utm_source`, `utm_medium`, `utm_campaign`, `status`. Столбец `created_at` имеет формат даты и времени. Для `status` задан список `new`, `in_progress`, `done`, `rejected` с запретом других значений. Неполные строки подсвечиваются красным по формуле `=AND($A2<>"";COUNTBLANK($C2:$E2)>0)`, дубли `request_id` — оранжевым по формуле `=AND($A2<>"";COUNTIF(A:A;$A2)>1)`.

В Apps Script вставлен `Code.gs` из материалов темы с `SPREADSHEET_ID = '1jNJM7CbJgwH_QYHW2gVndenR7-NObTthXaTl3_H09ug'`. Web app опубликован для всех с выполнением от имени владельца. `doPost(e)` проверяет обязательные поля, формат ID, email и направление; под блокировкой отклоняет повторный ID, проставляет серверное время и `new`, добавляет UTM или значения по умолчанию. Клиентский `status` игнорируется. В `contacts.html` стоит этот `/exec`, скрытые поля UTM и Measurement ID; `js/script.js` заполняет метки и `request_id`, отправляет форму и событие GA4 `generate_lead`.

## Тестовые заявки

Обе заявки отправлены с опубликованной страницы `contacts.html`. В таблице использованы только вымышленные данные.

| Заявка | `request_id` | `created_at` (Москва) | Имя и email | Направление | UTM | Статус после записи |
| --- | --- | --- | --- | --- | --- | --- |
| Без меток | `REQ-7D152E6B-C78A-4BB2-A3B5-C5E8D4E666E4` | 13.10.2026 00:14:19 | Тест Студент, `student@example.com` | `frontend` | `direct / none / not_set` | `new` |
| С метками | `REQ-D5D7813A-1431-4C55-8C0C-E7D416508588` | 13.10.2026 00:15:19 | Тест Студент UTM, `student.utm@example.com` | `backend` | `telegram / social / lesson07` | `new` |

## Проверки

- Форма с пустым обязательным именем не отправилась: браузер сообщил, что поле не заполнено.
- Повторный POST с первым `request_id` получил `{"ok":false,"error":"duplicate_request_id"}`; новая строка от приёмника не появилась.
- Для демонстрации правила подсветки вручную скопирована первая учебная строка: оба одинаковых ID стали оранжевыми.
- Ввод `finished` в `I5` отклонён проверкой данных. Через список статус первой строки изменён `new → in_progress → done`, затем возвращён в `new`.
- В GA4 Realtime видно событие `generate_lead`.

## Скриншоты

| № | Доказательство |
| --- | --- |
| 1 | [Таблица, заголовки, закрепление и список статусов](screens/01-sheet.png) |
| 2 | [Apps Script `Code.gs` с ID таблицы](screens/02-apps-script.png) |
| 3 | [Развёртывание Web app и адрес `/exec`](screens/03-deployment.png) |
| 4 | [`contacts.html`: action и скрытые UTM-поля](screens/04-form.png) |
| 5 | [Заявка без меток](screens/05-lead-direct.png) |
| 6 | [Заявка с метками](screens/06-lead-utm.png) |
| 7 | [`generate_lead` в GA4 Realtime](screens/07-generate-lead.png) |
| 8 | [Подсвеченный дубль `request_id`](screens/08-duplicate.png) |
| 9 | [Отказ на неверный статус](screens/09-status.png) |
