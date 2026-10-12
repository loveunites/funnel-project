# Задание 5 · Контролируемая таблица заявок

## Данные работы

| Поле | Значение |
| --- | --- |
| Сайт | <https://ga4-analytics-lab-prodan.vercel.app/> |
| Форма | <https://ga4-analytics-lab-prodan.vercel.app/contacts.html> |
| Исходники сайта | <https://github.com/loveunites/analytics_lab> |
| Подготовленная ветка сайта | <https://github.com/loveunites/analytics_lab/tree/hw-05> |
| Ссылка для второй заявки | <https://ga4-analytics-lab-prodan.vercel.app/contacts.html?utm_source=telegram&utm_medium=social&utm_campaign=lesson07> |
| Measurement ID | `G-BXHYB8CYC5` |
| Таблица | _Вставить ссылку после создания таблицы_ |
| Web app `/exec` | _Вставить URL своей публикации после создания Apps Script; текущий опубликованный сайт пока содержит старый URL_ |
| Ветка | `hw-05` |

> В ветке сайта `hw-05` обработчик формы заменён фрагментом из материалов, а загрузчик GA4 исправлен на `G-BXHYB8CYC5`. В опубликованной версии сайта пока остаётся старый обработчик с `preventDefault`: ветку ещё нужно объединить с `main` после подстановки URL **своего** Apps Script Web app и проверить новый Deployment в Vercel.

## 1. Таблица

Название файла таблицы: **GA4 Analytics Lab — Leads**. Название листа: **leads**.

Заголовки в строке 1, слева направо:

```text
request_id | created_at | name | email | direction | utm_source | utm_medium | utm_campaign | status
```

Настройки листа:

- закрепить строку 1;
- для столбца `created_at` выбрать формат «Дата и время»;
- для `status` (`I2:I`) создать список: `new`, `in_progress`, `done`, `rejected`;
- для списка включить режим «Запрещать ввод данных»;
- добавить условное форматирование неполной заявки на диапазон `A2:I` с формулой `=AND($A2<>"";COUNTBLANK($C2:$E2)>0)` и красной заливкой;
- добавить условное форматирование дубля на диапазон `A2:A` с формулой `=AND($A2<>"";COUNTIF(A:A;$A2)>1)` и оранжевой заливкой.

## 2. Приёмник Apps Script

1. Открыть Apps Script, вставить `Code.gs` из [материалов урока](https://algorthimization-course-vvodnoe.vercel.app/op03-informacionnye-tehnologii/lessons/07-google-sheets-leads/files/Code.gs).
2. В константе `SPREADSHEET_ID` заменить `PASTE_YOUR_SPREADSHEET_ID` на ID созданной таблицы — часть URL между `/d/` и `/edit`.
3. Проверить, что лист называется `leads`, а заголовки совпадают с девятью заголовками выше.
4. Опубликовать как Web app: запуск от имени владельца, доступ — `Anyone`, скопировать URL, оканчивающийся на `/exec`.
5. После изменения кода создать новую версию развёртывания и убедиться, что Deployment имеет статус **Ready**.

`doPost(e)` проверяет обязательные поля и формат `request_id`, блокирует одновременную запись, сверяет заголовки, отклоняет дубль, ставит серверные `created_at` и `new`, подставляет `direct`/`none`/`not_set` для отсутствующих UTM и добавляет одну строку из девяти значений.

## 3. Форма и обработчик

В `contacts.html` форма должна иметь такую структуру:

```html
<form id="lead-form" action="ВАШ_URL_WEB_APP/exec" method="POST" target="submission-frame">
  <input name="name" required maxlength="200">
  <input name="email" type="email" required maxlength="200">
  <select name="direction" required>...</select>
  <input type="hidden" name="request_id" id="request_id">
  <input type="hidden" name="utm_source" id="utm_source">
  <input type="hidden" name="utm_medium" id="utm_medium">
  <input type="hidden" name="utm_campaign" id="utm_campaign">
</form>
<iframe name="submission-frame" id="submission-frame" hidden></iframe>
```

В `js/script.js` заменить только старый обработчик формы фрагментом `lead-form.js` из материалов. Он читает UTM из URL страницы `contacts.html`, создаёт один UUID `REQ-...` на отправку, передаёт `generate_lead` без имени и email, не вызывает `preventDefault`, очищает ID после reset и оставляет остальные события сайта.

## 4. Тестовые заявки

Использовать только вымышленные данные:

| Тест | URL | Имя | Email | Направление | Ожидаемые UTM | Ожидаемый статус |
| --- | --- | --- | --- | --- | --- | --- |
| Без меток | `/contacts.html` | `Тест Студент` | `student@example.com` | `frontend` | `direct` / `none` / `not_set` | `new` |
| С метками | ссылка из таблицы выше | `Тест Студент UTM` | `student.utm@example.com` | `backend` | `telegram` / `social` / `lesson07` | `new` |

Зафиксировать фактические `request_id` и `created_at` из строк таблицы и вписать их сюда:

```text
direct request_id: ______________________________
UTM request_id:    ______________________________
```

## 5. Проверки

- очистить обязательное имя и убедиться, что браузер не отправляет форму;
- повторить POST с уже использованным `request_id` и получить `duplicate_request_id` без новой строки;
- в отдельной тестовой строке ввести статус `finished`: список запрещает значение и показывает отказ;
- сменить статус разрешённой строки `new` → `in_progress` → `done` через список;
- скопировать только тестовую строку с тем же `request_id`, чтобы увидеть оранжевую подсветку дубля;
- открыть GA4 Realtime или DebugView и показать событие `generate_lead`.

## 6. Кадры для сдачи

Файлы должны лежать в `lesson_05/screens/` и называться по списку в [screens/README.md](screens/README.md). На кадрах должны читаться URL сайта, URL таблицы и Measurement ID. В репозитории сейчас нет авторизованной сессии Google и исходников Vercel, поэтому фактические PNG нужно снять после создания таблицы, публикации Web app и новой публикации сайта.

