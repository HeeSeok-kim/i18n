---
id: sharding
title: Шардинг
---

По умолчанию WebdriverIO запускает тесты параллельно и стремится к оптимальному использованию ядер CPU на вашей машине. Чтобы достичь еще большей параллелизации, вы можете дополнительно масштабировать выполнение тестов WebdriverIO, запуская тесты на нескольких машинах одновременно. Мы называем этот режим работы "шардингом".

## Шардинг тестов между несколькими машинами

Чтобы разделить набор тестов на шарды, передайте `--shard=x/y` в командную строку. Например, чтобы разделить набор тестов на четыре шарда, каждый из которых запускает четверть тестов:

```sh
npx wdio run wdio.conf.js --shard=1/4
npx wdio run wdio.conf.js --shard=2/4
npx wdio run wdio.conf.js --shard=3/4
npx wdio run wdio.conf.js --shard=4/4
```

Теперь, если вы запустите эти шарды параллельно на разных компьютерах, ваш набор тестов завершится в четыре раза быстрее.

## Пример GitHub Actions

GitHub Actions поддерживает [шардинг тестов между несколькими задачами](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs) с помощью опции [`jobs.<job_id>.strategy.matrix`](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix). Опция matrix запустит отдельную задачу для каждой возможной комбинации предоставленных опций.

Следующий пример показывает, как настроить задачу для запуска ваших тестов на четырех машинах параллельно. Вы можете найти полную настройку пайплайна в проекте [Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate/blob/main/.github/workflows/test.yaml).

-   Сначала мы добавляем опцию matrix в нашу конфигурацию задачи с опцией shard, содержащей количество шардов, которые мы хотим создать. `shard: [1, 2, 3, 4]` создаст четыре шарда, каждый с разным номером шарда.
-   Затем мы запускаем наши тесты WebdriverIO с опцией `--shard ${{ matrix.shard }}/${{ strategy.job-total }}`. Это будет наша команда тестирования для каждого шарда.
-   Наконец, мы загружаем наш лог-отчет wdio в артефакты GitHub Actions. Это сделает логи доступными в случае, если шард завершится с ошибкой.

Пайплайн тестирования определен следующим образом:

```yaml title=.github/workflows/test.yaml
name: Test

on: [push, pull_request]

jobs:
    lint:
        # ...
    unit:
        # ...
    e2e:
        name: 🧪 Test (${{ matrix.shard }}/${{ strategy.job-total }})
        runs-on: ubuntu-latest
        needs: [lint, unit]
        strategy:
            matrix:
                shard: [1, 2, 3, 4]
        steps:
            - uses: actions/checkout@v4
            - uses: ./.github/workflows/actions/setup
            - name: E2E Test
              run: npm run test:features -- --shard ${{ matrix.shard }}/${{ strategy.job-total }}
            - uses: actions/upload-artifact@v1
              if: failure()
              with:
                  name: logs-${{ matrix.shard }}
                  path: logs
```

Это запустит все шарды параллельно, сокращая время выполнения тестов в 4 раза:

![Пример GitHub Actions](/img/sharding.png "Пример GitHub Actions")

Смотрите коммит [`96d444e`](https://github.com/webdriverio/cucumber-boilerplate/commit/96d444ea23919389682b9b1c9408ed91c452c7f8) из проекта [Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate), который ввел шардинг в пайплайн тестирования, что помогло сократить общее время выполнения с `2:23 мин` до `1:30 мин`, уменьшение на __37%__ 🎉.