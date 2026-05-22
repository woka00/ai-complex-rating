# AI CharacherHub
### Платформа комплексной оценки моделей искусственного интеллекта

---

## Быстрый старт

```bash
# Клонировать / распаковать проект
cd ai_eval

# Установить зависимости
pip install -r requirements.txt

# Запустить с демо-данными
bash start.sh --demo

# Или просто запустить
bash start.sh
```

Открыть: **http://localhost:8000**

---

## Структура проекта

```
ai_eval/
├── backend/
│   ├── main.py          # FastAPI backend — все API роуты
│   ├── evaluator.py     # Автоматический прогон YOLO / классификаторов / NLP
│   └── seed_demo.py     # Заполнение демо-данными
├── frontend/
│   └── index.html       # SPA — 6 обязательных окон
├── requirements.txt
├── start.sh
└── README.md
```

---

## API эндпоинты

| Метод | URL | Описание |
|-------|-----|----------|
| GET  | /api/projects | Список проектов |
| POST | /api/projects | Создать проект |
| GET  | /api/projects/{id} | Детали проекта |
| DELETE | /api/projects/{id} | Удалить |
| POST | /api/projects/{id}/models | Добавить модель |
| DELETE | /api/projects/{id}/models/{mid} | Удалить модель |
| GET  | /api/projects/{id}/criteria | Список критериев |
| POST | /api/projects/{id}/criteria | Добавить критерий |
| PUT  | /api/projects/{id}/criteria/{cid} | Обновить критерий |
| DELETE | /api/projects/{id}/criteria/{cid} | Удалить |
| POST | /api/projects/{id}/criteria/normalize | Нормировать веса |
| GET  | /api/projects/{id}/scores | Получить оценки |
| POST | /api/projects/{id}/scores | Выставить оценку |
| POST | /api/projects/{id}/scores/import | Импорт CSV |
| POST | /api/projects/{id}/calculate | Запустить расчёт K_k |
| GET  | /api/projects/{id}/results | Последние результаты |
| POST | /api/projects/{id}/sensitivity | Анализ чувствительности |
| GET  | /api/projects/{id}/report | Финальный отчёт |

---

## Математическая модель

```
S_k   = Σ(w_i × a_ik)          # взвешенная сумма по модели k
S_max = 5 × Σ(w_i)             # максимум при всех оценках = 5
K_k   = S_k / S_max             # итоговый коэффициент [0..1]
```

**Интерпретация K:**
- 0.90–1.00: Отличная модель — рекомендуется
- 0.75–0.89: Хорошая модель
- 0.60–0.74: Приемлемая модель
- 0.40–0.59: Слабая модель
- 0.00–0.39: Не рекомендуется

---

## Автоматический прогон моделей

```bash
# Установить ML зависимости
pip install ultralytics torch torchvision transformers

# Создать проект и добавить модели через UI, затем:
python backend/evaluator.py --project_id 1 --model_type detection
python backend/evaluator.py --project_id 1 --model_type classification
python backend/evaluator.py --project_id 1 --model_type text

# Dry run (посмотреть оценки без отправки)
python backend/evaluator.py --project_id 1 --dry_run
```

---

## CSV формат для импорта оценок

```csv
model_name,criterion_name,score
YOLOv8n,Точность ответа,3.5
YOLOv8n,Устойчивость к шуму,4.0
YOLOv8s,Точность ответа,4.2
```
