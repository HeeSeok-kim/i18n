---
id: v6-migration
title: З v5 до v6
---

Цей урок призначений для людей, які все ще використовують `v5` WebdriverIO і хочуть перейти на `v6` або на останню версію WebdriverIO. Як зазначено в нашому [блозі про випуск](https://webdriver.io/blog/2020/03/26/webdriverio-v6-released), зміни для цього оновлення версії можна підсумувати наступним чином:

- ми консолідували параметри для деяких команд (наприклад, `newWindow`, `react$`, `react$$`, `waitUntil`, `dragAndDrop`, `moveTo`, `waitForDisplayed`, `waitForEnabled`, `waitForExist`) і перемістили всі необов'язкові параметри в єдиний об'єкт, наприклад:

    ```js
    // v5
    browser.newWindow(
        'https://webdriver.io',
        'WebdriverIO window',
        'width=420,height=230,resizable,scrollbars=yes,status=1'
    )
    // v6
    browser.newWindow('https://webdriver.io', {
        windowName: 'WebdriverIO window',
        windowFeature: 'width=420,height=230,resizable,scrollbars=yes,status=1'
    })
    ```

- конфігурації для сервісів перемістилися в список сервісів, наприклад:

    ```js
    // v5
    exports.config = {
        services: ['sauce'],
        sauceConnect: true,
        sauceConnectOpts: { foo: 'bar' },
    }
    // v6
    exports.config = {
        services: [['sauce', {
            sauceConnect: true,
            sauceConnectOpts: { foo: 'bar' }
        }]],
    }
    ```

- деякі опції сервісів були перейменовані з метою спрощення
- ми перейменували команду `launchApp` на `launchChromeApp` для сесій Chrome WebDriver

:::info

Якщо ви використовуєте WebdriverIO `v4` або нижче, спочатку оновіться до `v5`.

:::

Хоча ми хотіли б мати повністю автоматизований процес для цього, реальність виглядає інакше. У кожного свій стиль налаштування. Кожен крок слід розглядати як рекомендацію, а не як покрокову інструкцію. Якщо у вас виникли проблеми з міграцією, не соромтеся [звертатися до нас](https://github.com/webdriverio/codemod/discussions/new).

## Налаштування

Подібно до інших міграцій, ми можемо використовувати WebdriverIO [codemod](https://github.com/webdriverio/codemod). Щоб встановити codemod, виконайте:

```sh
npm install jscodeshift @wdio/codemod
```

## Оновіть залежності WebdriverIO

Оскільки всі версії WebdriverIO пов'язані між собою, найкраще завжди оновлюватися до певного тегу, наприклад, `6.12.0`. Якщо ви вирішите оновитися з `v5` безпосередньо до `v7`, ви можете опустити тег і встановити останні версії всіх пакетів. Для цього ми копіюємо всі залежності, пов'язані з WebdriverIO, з нашого `package.json` і перевстановлюємо їх за допомогою:

```sh
npm i --save-dev @wdio/allure-reporter@6 @wdio/cli@6 @wdio/cucumber-framework@6 @wdio/local-runner@6 @wdio/spec-reporter@6 @wdio/sync@6 wdio-chromedriver-service@6 webdriverio@6
```

Зазвичай залежності WebdriverIO є частиною dev залежностей, хоча залежно від вашого проекту це може відрізнятися. Після цього ваші `package.json` і `package-lock.json` повинні бути оновлені. __Примітка:__ це приклади залежностей, ваші можуть відрізнятися. Переконайтеся, що ви знайшли останню версію v6, виконавши, наприклад:

```sh
npm show webdriverio versions
```

Спробуйте встановити останню доступну версію 6 для всіх основних пакетів WebdriverIO. Для пакетів спільноти це може відрізнятися від пакета до пакета. Тут ми рекомендуємо перевірити журнал змін для отримання інформації про те, яка версія ще сумісна з v6.

## Трансформація конфігураційного файлу

Гарним першим кроком є починати з конфігураційного файлу. Всі критичні зміни можна вирішити за допомогою codemod повністю автоматично:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v6 ./wdio.conf.js
```

:::caution

Codemod ще не підтримує проекти TypeScript. Дивіться [`@webdriverio/codemod#10`](https://github.com/webdriverio/codemod/issues/10). Ми працюємо над впровадженням підтримки для нього найближчим часом. Якщо ви використовуєте TypeScript, будь ласка, долучайтеся!

:::

## Оновіть spec-файли та Page Objects

Щоб оновити всі зміни команд, запустіть codemod на всіх ваших e2e файлах, які містять команди WebdriverIO, наприклад:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v6 ./e2e/*
```

Ось і все! Більше ніяких змін не потрібно 🎉

## Висновок

Ми сподіваємося, що цей урок трохи допоможе вам у процесі міграції до WebdriverIO `v6`. Ми наполегливо рекомендуємо продовжувати оновлення до останньої версії, враховуючи, що оновлення до `v7` є тривіальним через майже відсутність критичних змін. Будь ласка, ознайомтеся з посібником з міграції [для оновлення до v7](v7-migration).

Спільнота продовжує вдосконалювати codemod, тестуючи його з різними командами в різних організаціях. Не соромтеся [повідомляти про проблему](https://github.com/webdriverio/codemod/issues/new), якщо у вас є відгуки, або [почати обговорення](https://github.com/webdriverio/codemod/discussions/new), якщо у вас виникли труднощі під час процесу міграції.