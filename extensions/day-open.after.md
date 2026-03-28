## Проверка обновлений upstream (FMT-exocortex-template)

Выполни команду:
```
git -C /c/Users/pc/Github/FMT-exocortex-template fetch upstream 2>&1 && git -C /c/Users/pc/Github/FMT-exocortex-template log HEAD..upstream/main --oneline
```

Интерпретация:
- Пустой вывод → 🟢 upstream актуален
- Есть строки → 🟡 доступно N обновлений — добавить в «Требует внимания»: «Обновления FMT-exocortex-template: N коммитов. Запустить `git merge upstream/main` в репо.»
