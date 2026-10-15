# Worktree

## git worktree list

```
D:/work/carmoney-lab-Ffergus86                                e19e412 [d1/1.2.1-1.2.3-Ffergus86]
D:/work/carmoney-lab-Ffergus86/.kilo/worktrees/dapper-nebula  e19e412 [dapper-nebula]
```

## Ответ агента из второй сессии

Папка: `D:/work/carmoney-lab-Ffergus86/.kilo/worktrees/dapper-nebula`. Ветка: `dapper-nebula`.

## Почему два агента в одной папке на одной ветке — плохая идея

Оба пишут в одни и те же файлы и один индекс: незакоммиченная правка одной сессии затирается другой.
Коммиты смешиваются в одной ветке, и по diff нельзя отделить, какая сессия что изменила.
