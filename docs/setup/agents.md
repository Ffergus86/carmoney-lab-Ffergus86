# Агенты planner и scout

## planner

Режим `primary`. Пишет только в `docs/plan/`: `edit` для `*` — `deny`, для `docs/plan/**` — `allow`. `bash` — `deny`. Читает код, `AGENTS.md` и `docs/setup/code_map.md` и составляет план с разделами Файлы, Шаги, Тесты, Риски. Код не меняет. Граничные значения в разделе Тесты — отдельными строками.

Проверка прав: просьба «добавь комментарий в `backend/public/index.php`» не выполняется. Правка вне `docs/plan/` закрыта `edit: "*": deny`, команд нет из-за `bash: deny`.

Без явного `@scout` planner субагента не вызывает: в его инструкции нет шага «поручи scout». Тот же поиск по пробегу он делает сам по коду и по `code_map.md`.

## scout

Режим `subagent`. `edit: deny`, `bash: deny`. Только читает код и возвращает список «файл, строка, что там». Исправлений не предлагает.

Вызов: `@scout найди все места, где читается пробег (mileage)`.

Ответ scout:

- `backend/src/Domain/ApplicationValidator.php:43` — читает `payload['mileage']`, пустое поле становится `-1`.
- `backend/src/Domain/ApplicationValidator.php:44` — сравнивает пробег с `max_mileage_km`.
- `backend/src/Domain/ApplicationValidator.php:78` — кладёт прочитанный пробег в нормализованный input.
- `backend/config/rules.php:23` — порог валидации `max_mileage_km` = 500000, не порог решения.
- `backend/src/Domain/AssessmentService.php:30` — пробег приходит внутри `$input` после валидации; в `decide()` не передаётся.
- `backend/src/Repository/ApplicationRepository.php:45` — пишет `$input['mileage']` в `vehicles.mileage_km`.
- `backend/src/Repository/ApplicationRepository.php:68` — читает `v.mileage_km` при выборке заявки.
- `frontend/app.js:8` — поле `mileage` входит в числовые поля формы.
- `frontend/app.js:14` — значение пробега из формы превращается в число.
- `frontend/index.html:31` — поле ввода пробега.
- `db/schema.sql:22` — колонка `mileage_km`.
- `tests/Unit/ApplicationValidatorTest.php:34` — в фикстуре пробег 84000.
- `tests/Unit/AssessmentServiceTest.php:38` — в фикстуре пробег 96000.
