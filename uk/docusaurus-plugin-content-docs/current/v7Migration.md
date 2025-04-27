---
id: v7-migration
title: З v6 до v7
---

Цей посібник призначений для людей, які все ще використовують `v6` WebdriverIO і хочуть перейти на `v7`. Як зазначено в нашому [блозі про випуск](https://webdriver.io/blog/2021/02/09/webdriverio-v7-released), зміни здебільшого стосуються внутрішньої структури, і процес оновлення має бути досить простим.

:::info

Якщо ви використовуєте WebdriverIO `v5` або нижче, спочатку оновіть до `v6`. Будь ласка, перегляньте наш [посібник з міграції на v6](v6-migration).

:::

Хоча ми хотіли б мати повністю автоматизований процес для цього, реальність виглядає інакше. У кожного своя унікальна конфігурація. Кожен крок слід розглядати як рекомендацію, а не як покрокову інструкцію. Якщо у вас виникають проблеми з міграцією, не соромтеся [зв'язатися з нами](https://github.com/webdriverio/codemod/discussions/new).

## Налаштування

Подібно до інших міграцій, ми можемо використовувати [codemod](https://github.com/webdriverio/codemod) WebdriverIO. Для цього посібника ми використовуємо [шаблонний проект](https://github.com/WarleyGabriel/demo-webdriverio-cucumber), поданий учасником спільноти, і повністю перенесемо його з `v6` на `v7`.

Щоб встановити codemod, виконайте:

```sh
npm install jscodeshift @wdio/codemod
```

#### Коміти:

- _install codemod deps_ [[6ec9e52]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/6ec9e52038f7e8cb1221753b67040b0f23a8f61a)

## Оновіть залежності WebdriverIO

Оскільки всі версії WebdriverIO тісно пов'язані між собою, найкраще завжди оновлювати до конкретного тегу, наприклад, `latest`. Для цього ми копіюємо всі залежності, пов'язані з WebdriverIO, з нашого `package.json` і перевстановлюємо їх за допомогою:

```sh
npm i --save-dev @wdio/allure-reporter@7 @wdio/cli@7 @wdio/cucumber-framework@7 @wdio/local-runner@7 @wdio/spec-reporter@7 @wdio/sync@7 wdio-chromedriver-service@7 wdio-timeline-reporter@7 webdriverio@7
```

Зазвичай залежності WebdriverIO є частиною dev dependencies, хоча це може відрізнятися залежно від вашого проекту. Після цього ваші `package.json` та `package-lock.json` мають бути оновлені. __Примітка:__ це залежності, які використовуються [прикладом проекту](https://github.com/WarleyGabriel/demo-webdriverio-cucumber), ваші можуть відрізнятися.

#### Коміти:

- _updated dependencies_ [[7097ab6]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/7097ab6297ef9f37ead0a9c2ce9fce8d0765458d)

## Трансформуйте файл конфігурації

Хорошим першим кроком є початок з файлу конфігурації. У WebdriverIO `v7` нам більше не потрібно вручну реєструвати будь-які компілятори. Фактично, їх потрібно видалити. Це можна зробити автоматично за допомогою codemod:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v7 ./wdio.conf.js
```

:::caution

Codemod ще не підтримує проекти TypeScript. Дивіться [`@webdriverio/codemod#10`](https://github.com/webdriverio/codemod/issues/10). Ми працюємо над реалізацією підтримки найближчим часом. Якщо ви використовуєте TypeScript, будь ласка, долучайтеся!

:::

#### Коміти:

- _transpile config file_ [[6015534]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/60155346a386380d8a77ae6d1107483043a43994)

## Оновіть визначення кроків

Якщо ви використовуєте Jasmine або Mocha, на цьому все. Останнім кроком є оновлення імпорту Cucumber.js з `cucumber` на `@cucumber/cucumber`. Це також можна зробити автоматично за допомогою codemod:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v7 ./src/e2e/*
```

От і все! Більше змін не потрібно 🎉

#### Коміти:

- _transpile step definitions_ [[8c97b90]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/8c97b90a8b9197c62dffe4e2954f7dad814753cc)

## Висновок

Ми сподіваємося, що цей посібник трохи допоможе вам у процесі міграції до WebdriverIO `v7`. Спільнота продовжує вдосконалювати codemod, тестуючи його з різними командами в різних організаціях. Не соромтеся [створити проблему](https://github.com/webdriverio/codemod/issues/new), якщо у вас є відгуки, або [почати обговорення](https://github.com/webdriverio/codemod/discussions/new), якщо у вас виникають труднощі під час процесу міграції.