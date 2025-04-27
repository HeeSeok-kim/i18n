---
id: sharding
title: Шардінг
---

За замовчуванням WebdriverIO запускає тести паралельно і прагне до оптимального використання ядер CPU на вашому пристрої. Щоб досягти ще більшої паралелізації, ви можете додатково масштабувати виконання тестів WebdriverIO, запускаючи тести на кількох машинах одночасно. Ми називаємо цей режим роботи "шардінгом".

## Розподіл тестів між кількома машинами

Щоб розподілити набір тестів, передайте `--shard=x/y` в командний рядок. Наприклад, щоб розділити набір на чотири шарди, кожен з яких виконує одну четверту частину тестів:

```sh
npx wdio run wdio.conf.js --shard=1/4
npx wdio run wdio.conf.js --shard=2/4
npx wdio run wdio.conf.js --shard=3/4
npx wdio run wdio.conf.js --shard=4/4
```

Тепер, якщо ви запустите ці шарди паралельно на різних комп'ютерах, ваш набір тестів буде завершено в чотири рази швидше.

## Приклад GitHub Actions

GitHub Actions підтримує [розподіл тестів між кількома завданнями](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs) за допомогою опції [`jobs.<job_id>.strategy.matrix`](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix). Опція matrix запускатиме окреме завдання для кожної можливої комбінації наданих опцій.

Наступний приклад показує, як налаштувати завдання для запуску тестів на чотирьох машинах паралельно. Повну конфігурацію пайплайну можна знайти в проекті [Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate/blob/main/.github/workflows/test.yaml).

-   Спочатку ми додаємо опцію matrix до конфігурації нашого завдання з опцією shard, що містить кількість шардів, які ми хочемо створити. `shard: [1, 2, 3, 4]` створить чотири шарди, кожен з різним номером.
-   Потім ми запускаємо наші тести WebdriverIO з опцією `--shard ${{ matrix.shard }}/${{ strategy.job-total }}`. Це буде нашою командою для запуску тестів для кожного шарду.
-   Нарешті, ми завантажуємо звіт логів wdio до артефактів GitHub Actions. Це зробить логи доступними у випадку, якщо шард завершиться з помилкою.

Пайплайн тестування визначено наступним чином:

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

Це запустить всі шарди паралельно, зменшуючи час виконання тестів у 4 рази:

![GitHub Actions example](/img/sharding.png "GitHub Actions example")

Дивіться коміт [`96d444e`](https://github.com/webdriverio/cucumber-boilerplate/commit/96d444ea23919389682b9b1c9408ed91c452c7f8) з проекту [Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate), який запровадив шардінг у тестовий пайплайн, що допомогло зменшити загальний час виконання з `2:23 хв` до `1:30 хв`, зменшення на __37%__ 🎉.