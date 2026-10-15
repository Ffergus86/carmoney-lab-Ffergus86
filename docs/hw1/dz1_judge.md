# Судья ДЗ.1: пробег

Сверка `docs/spec/spec_MILEAGE.md` с `tests/Unit/AssessmentServiceTest.php` и diff кода. Отдельная модель не запускалась: проверка сделана чтением спеки, тестов и кода в этой сессии.

## Покрытие

- REQ-MILEAGE-01 закрыт AC-MILEAGE-01 и AC-MILEAGE-02: `testMileage399999KeepsLtvApprove`, `testMileage400000KeepsLtvApprove`. Оба ждут approve и ненулевой лимит.
- REQ-MILEAGE-02 закрыт AC-MILEAGE-03 и AC-MILEAGE-04: `testMileage400001DowngradesApproveToReview` (review, лимит 0, LTV 50 не пересчитан) и `testMileage400001KeepsExistingLtvReview`.
- REQ-MILEAGE-03 закрыт AC-MILEAGE-05: `testEmptyMileageDoesNotBecomeReview` ждёт ошибку ввода по полю пробега, не решение review.
- Лишних REQ нет. Открытый вопрос про reject и пробег больше 400000 в спеку как требование не попал и тестом не закрыт.

## Где агент срезал угол

План меняет `AssessmentService` после `decide()` и кладёт порог в `rules.php`, но не говорит, откуда сервис возьмёт число: конструктор его не принимает, `AppFactory` в списке файлов нет. Чтобы не править тесты после красного коммита, сервис сам читает `backend/config/rules.php`. Это не смена правила, но и не явная передача порога в конструктор, как у `DecisionEngine`.
