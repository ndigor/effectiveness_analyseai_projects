# effectiveness_analyseai_projects
# effectiveness_analyseai_projects
# AI Competency Assessment Agent – Манифест (на русском)

## Обзор проекта
CLI‑агент, который оценивает компетенции кандидата на позицию системного администратора (Linux, Windows, Networking, Scripting, Cloud, Security, AI/ML) с помощью рандомизированного теста, сохраняет зашифрованные результаты в MS SQL, генерирует современный строгий HTML‑дашборд и при желании отправляет его по e‑mail.

## Возможности
- Персонализированное приветствие и запрос согласия.
- Рандомизация порядка категорий, вопросов и вариантов ответов – исключает списывание.
- Мгновенная обратная связь по каждому вопросу.
- Безопасное хранение: все персональные данные и ответы шифруются алгоритмом Fernet перед записью в MS SQL.
- Дашборд:
  - Радар‑chart (Chart.js) с баллами по компетенциям.
  - Таблица с процентом баллов и уровнем компетенции (Novice → Junior → Middle → Senior → Expert) по каждой области.
  - Общий уровень и рекомендованная должность (Junior SysAdmin, SysAdmin, Senior SysAdmin, Lead/Architecture Specialist).
  - Персонализированные рекомендации по слабым и сильным сторонам.
  - Чистый, современный и строгий дизайн (минимальный CSS, адаптивный layout).
- Опциональная отправка дашборда по e‑mail через SMTP.
- Настройка через файл `.env`.

## Структура каталога
```
effectiveness_analyse_ai_projects/
│
├─ agent.py                 # Главная точка входа CLI
├─ test_engine.py           # Загрузка вопросов, рандомизация, выполнение теста
├─ database.py              # Подключение к MS SQL, шифрование, CRUD‑операции
├─ dashboard_generator.py   # Генерация HTML‑дашборда (Chart.js)
├─ email_sender.py          # Отправка e‑mail с вложением
├─ config.py                # Загрузка переменных окружения (.env)
├─ requirements.txt         # Зависимости Python
├─ .env.example             # Шаблон файла окружения
├─ manifest.md              # Этот файл (английская версия)
├─ manifest_ru.md           # Русская версия манифеста
├─ RUN.md                   # Быстрая инструкция по запуску
│
└─ knowledge_base/
   ├─ linux/
   │   └─ questions.json
   ├─ windows/
   │   └─ questions.json
   ├─ networking/
   │   └─ questions.json
   ├─ scripting/
   │   └─ questions.json
   ├─ cloud/
   │   └─ questions.json
   ├─ security/
   │   └─ questions.json
   └─ ai_ml/
       └─ questions.json
```

## Зависимости
- Python 3.8+
- `pyodbc>=4.0.30`
- `cryptography>=42.0.0`
- `python-dotenv>=1.0.0`

Установка:
```bash
pip install -r requirements.txt
```

## Настройка
1. Скопировать `.env.example` → `.env`.
2. Отредактировать `.env`:
   - **База данных** – драйвер, сервер, имя БД, имя пользователя, пароль, флаги шифрования.
   - **Ключ шифрования** – `ENCRYPTION_KEY` (32‑байтовая строка в Base64; сгенерировать можно командой  
     `python -c "from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())"`).
   - **Электронная почта** (опционально) – SMTP‑сервер, порт, логин, пароль, флаг TLS.
   - **Путь к базе знаний** – `KB_ROOT` (по умолчанию `./knowledge_base`).

## База данных
Убедитесь, что экземпляр MS SQL доступен. При первом запуске агент создаст таблицу `TestResults`, если у учётной записи есть право `CREATE TABLE`.

Схема таблицы:
```
Id INT IDENTITY PRIMARY KEY,
UserNameEnc VARBINARY(MAX) NOT NULL,
EmailEnc VARBINARY(MAX),
Results VARBINARY(MAX) NOT NULL,
TakenAt DATETIME2 DEFAULT GETDATE()
```

## Использование
См. файл `RUN.md` для пошагового руководства.

## Лицензия
Для внутреннего использования – внешнее распространение не предусмотрено.
