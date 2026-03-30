Выполни сценарий Day-Close для роли Стратег (R1).

> **Триггер:** Ручной — по запросу пользователя (`./scripts/strategist.sh day-close`).
> Отдельный файл отчёта НЕ создаётся. Итоги дня войдут в DayPlan следующего утра.

Источник сценария: /c/Users/pc/Github/CLAUDE.md → Протокол Day-Close

## Контекст

- **WeekPlan:** /c/Users/pc/Github/DS-strategy/current/WeekPlan W*.md (последний по дате)
- **MEMORY:** ~/.claude/projects/-c-Users-pc-Github/memory/MEMORY.md
- **Exocortex backup:** /c/Users/pc/Github/DS-strategy/exocortex/

## Алгоритм

### 1. Сбор коммитов за сегодня

```bash
# Для КАЖДОГО репо в /c/Users/pc/Github/:
git -C /c/Users/pc/Github/<repo> log --since="today 00:00" --oneline --no-merges
```

- Пройди по ВСЕМ репозиториям в `/c/Users/pc/Github/`
- Сгруппируй коммиты по репозиториям
- Сопоставь с РП из недельного плана
- Определи статус каждого затронутого РП: done / partial / not started
- Выведи итоги на экран (не в файл)

### 1b. WakaTime (физическое время)

```bash
bash /c/Users/pc/Github/FMT-exocortex-template/roles/strategist/scripts/fetch-wakatime.sh today
```

- Показать итого + по проектам
- Рассчитать мультипликатор: бюджет закрытых РП / WakaTime время
- Если WakaTime недоступен — пропустить шаг молча

### 2. Обновить WeekPlan

Найди текущий `WeekPlan W*.md` в `DS-strategy/current/` и обнови:

- Пометь завершённые РП как **done**
- Обнови статусы partial с описанием прогресса
- Добавь carry-over (что переносится на завтра) — в конец файла
- **НЕ удаляй** ничего — только помечай и дописывай

### 3. Обновить MEMORY.md

Синхронизируй статусы РП в MEMORY.md с обновлённым WeekPlan:
- done → done
- partial → in_progress (с пометкой прогресса)
- Удали завершённые РП из pending, если они в done

### 4. Backup экзокортекса

Скопируй актуальные файлы в `/c/Users/pc/Github/DS-strategy/exocortex/`:

```bash
# Корневой CLAUDE.md
cp /c/Users/pc/Github/CLAUDE.md /c/Users/pc/Github/DS-strategy/exocortex/CLAUDE.md

# Memory (Слой 3)
cp ~/.claude/projects/-c-Users-pc-Github/memory/MEMORY.md /c/Users/pc/Github/DS-strategy/exocortex/MEMORY.md
cp ~/.claude/projects/-c-Users-pc-Github/memory/*.md /c/Users/pc/Github/DS-strategy/exocortex/
```

### 5. Закоммитить

- Закоммить все изменения в `DS-strategy` (WeekPlan + exocortex backup)
- Запуши

## Правила

- **Ничего не удалять** из WeekPlan — только помечать и дописывать
- **Не создавать отдельный файл отчёта** — итоги дня войдут в DayPlan следующего утра (шаг 1 day-plan)
- Если коммитов за день нет — написать «Нет активности» и всё равно сделать backup
- Выводить итоги на экран для пользователя

## Вывод на экран (шаблон)

```
📋 Day-Close: DD месяца YYYY

Коммиты: N в M репо
- repo-name: N коммитов (краткое описание)

WakaTime: Xh Ym физического времени
- Проект 1: Xh Ym
- Проект 2: Xh Ym
Мультипликатор: ~N.Nx (бюджет РП / WakaTime)

РП обновлены в WeekPlan:
- #N: статус → новый статус

MEMORY.md: синхронизирован ✅
Exocortex backup: скопирован ✅
Git: закоммичен и запушен ✅
```

Результат: обновлённый WeekPlan + MEMORY.md + backup экзокортекса.
