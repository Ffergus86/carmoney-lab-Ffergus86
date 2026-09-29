готов
1) Учебный сервис предварительной оценки заявки на заём под ПТС: принимает заявку (VIN, год, пробег, оценочная стоимость, сумма, срок), считает LTV и возвращает решение approve / review / reject; данные синтетические (по README.md:1-10).
2) Makefile: help, up (docker compose up -d --build, сервис на http://localhost:8080), down, ps, logs, install (composer install), test (vendor/bin/phpunit, иначе внутри backend-контейнера), lint (php -l по backend/ и tests/), seed (mysql -ulab -plab carmoney_lab < db/seed.sql). docker-compose.yml поднимает два сервиса: backend (php -S 0.0.0.0:8080, порт ${APP_PORT:-8080}:8080) и db (mysql:8.0, порт ${DB_PORT:-3307}:3306, init-скрипты db/schema.sql и db/seed.sql монтируются в /docker-entrypoint-initdb.d/, том db-data, healthcheck mysqladmin ping).
3) Решение approve / review / reject считается в папке backend/src/Domain/ (файлы AssessmentService.php, DecisionEngine.php, LtvCalculator.php; пороги — в backend/config/rules.php); HTTP-обёртка — backend/src/Http/ApplicationController.php, запуск — backend/public/index.php через backend/public/router.php.

модель: training-2026-09-minimax-m3
