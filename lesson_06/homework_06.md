# Задание 6 · Подготовка данных в Google Таблицах

## Работа

| Поле | Значение |
| --- | --- |
| Таблица | [leads_cleaning](https://docs.google.com/spreadsheets/d/1XoxVlIMW_t2uPImTfysJfEbEYTSOhiwGdLclYq53BMk/edit) |
| Листы | `source`, `leads_clean`, `check` |
| Результат | [`leads_clean.csv`](./leads_clean.csv) |
| Ветка | `hw-06` |
| Доступ к таблице | «Все, у кого есть ссылка» — читатель |

## Структура и правила

- CSV v1 импортирован на `source` с целью «Заменить текущий лист», разделителем «Запятая» и выключенным преобразованием текста в числа, даты и формулы.
- CSV v2 импортирован на тот же лист `source`; исходные значения остаются отдельным источником и не исправляются вручную.
- `leads_clean` содержит девять заголовков: `request_id`, `created_at`, `name`, `email`, `direction`, `utm_source`, `utm_medium`, `utm_campaign`, `status`.
- В `A2:F2`, `H2` и `I2` записаны формулы `ARRAYFORMULA`. Ячейка `G2` оставлена пустой: формула `F2` с `SPLIT` разливается в `utm_source` и `utm_medium`.
- Для столбца `created_at` установлен формат «Дата и время».
- На листе `check` формула проверяет заголовки `source` и возвращает `структура source: OK`.

## Проверенный результат

| Проверка | Результат |
| --- | --- |
| Строк после обработки v1 | 5 |
| Строк после обработки v2 | 8 |
| Столбцов в `leads_clean` | 9 |
| Значения `status` | только `done` и `new` |
| Ошибочные даты | `REQ-003` и `REQ-008` показывают `#VALUE!` |
| Строка `REQ-006` | Елена / `telegram` / `social` / `lesson08` / `new` |
| Строка `REQ-007` | Максим / `vk` / `social` / `lesson08` / `done` |
| Проверка структуры | `структура source: OK` |

## Доказательства

| № | Файл | Что видно |
| --- | --- | --- |
| 1 | [`screens/01-import.png`](./screens/01-import.png) | окно импорта v1 с выключенным преобразованием типов |
| 2 | [`screens/02-source.png`](./screens/02-source.png) | отдельный исходник `source` после импорта v1 |
| 3 | [`screens/03-formulas.png`](./screens/03-formulas.png) | `leads_clean` в режиме показа формул |
| 4 | [`screens/04-value-error.png`](./screens/04-value-error.png) | `#VALUE!` и подсказка для `wrong_date` |
| 5 | [`screens/05-status-filter.png`](./screens/05-status-filter.png) | фильтр `status` со значениями `done` и `new` |
| 6 | [`screens/06-v2.png`](./screens/06-v2.png) | результат после v2, включая `REQ-006` и `REQ-007` |
| 7 | [`screens/07-check.png`](./screens/07-check.png) | лист `check` с результатом `структура source: OK` |

## Статус выполнения

- [x] Импортирована исходная выгрузка v1.
- [x] Создан `leads_clean` с формулами.
- [x] Проверены ошибка `#VALUE!` и значения `status`.
- [x] Импортирована выгрузка v2 на лист `source`.
- [x] Проверен лист `check`.
- [x] Скачан [`leads_clean.csv`](./leads_clean.csv) после v2.
- [x] Добавлены семь обязательных PNG-скриншотов.
