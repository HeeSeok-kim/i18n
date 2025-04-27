---
id: v7-migration
title: С v6 на v7
---

Этот учебник предназначен для тех, кто все еще использует `v6` WebdriverIO и хочет перейти на `v7`. Как упоминалось в нашей [статье о выпуске](https://webdriver.io/blog/2021/02/09/webdriverio-v7-released), изменения в основном находятся под капотом, и обновление должно быть простым процессом.

:::info

Если вы используете WebdriverIO `v5` или ниже, сначала обновитесь до `v6`. Пожалуйста, ознакомьтесь с нашим [руководством по миграции на v6](v6-migration).

:::

Хотя мы хотели бы иметь полностью автоматизированный процесс, реальность выглядит иначе. У каждого своя настройка. Каждый шаг следует рассматривать как руководство, а не как пошаговую инструкцию. Если у вас возникли проблемы с миграцией, не стесняйтесь [связаться с нами](https://github.com/webdriverio/codemod/discussions/new).

## Настройка

Подобно другим миграциям, мы можем использовать [codemod](https://github.com/webdriverio/codemod) от WebdriverIO. Для этого учебника мы используем [базовый проект](https://github.com/WarleyGabriel/demo-webdriverio-cucumber), предоставленный членом сообщества, и полностью переносим его с `v6` на `v7`.

Для установки codemod выполните:

```sh
npm install jscodeshift @wdio/codemod
```

#### Коммиты:

- _install codemod deps_ [[6ec9e52]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/6ec9e52038f7e8cb1221753b67040b0f23a8f61a)

## Обновление зависимостей WebdriverIO

Учитывая, что все версии WebdriverIO связаны друг с другом, лучше всего обновляться до определенного тега, например `latest`. Для этого копируем все зависимости, связанные с WebdriverIO, из нашего `package.json` и переустанавливаем их:

```sh
npm i --save-dev @wdio/allure-reporter@7 @wdio/cli@7 @wdio/cucumber-framework@7 @wdio/local-runner@7 @wdio/spec-reporter@7 @wdio/sync@7 wdio-chromedriver-service@7 wdio-timeline-reporter@7 webdriverio@7
```

Обычно зависимости WebdriverIO являются частью dev-зависимостей, хотя в зависимости от вашего проекта это может отличаться. После этого ваши `package.json` и `package-lock.json` должны быть обновлены. __Примечание:__ это зависимости, используемые в [проекте-примере](https://github.com/WarleyGabriel/demo-webdriverio-cucumber), ваши могут отличаться.

#### Коммиты:

- _updated dependencies_ [[7097ab6]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/7097ab6297ef9f37ead0a9c2ce9fce8d0765458d)

## Преобразование файла конфигурации

Хороший первый шаг — начать с файла конфигурации. В WebdriverIO `v7` больше не требуется вручную регистрировать компиляторы. Фактически, их нужно удалить. Это можно сделать автоматически с помощью codemod:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v7 ./wdio.conf.js
```

:::caution

Codemod еще не поддерживает проекты на TypeScript. См. [`@webdriverio/codemod#10`](https://github.com/webdriverio/codemod/issues/10). Мы работаем над реализацией поддержки. Если вы используете TypeScript, пожалуйста, участвуйте!

:::

#### Коммиты:

- _transpile config file_ [[6015534]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/60155346a386380d8a77ae6d1107483043a43994)

## Обновление определений шагов

Если вы используете Jasmine или Mocha, на этом все. Последний шаг — обновить импорты Cucumber.js с `cucumber` на `@cucumber/cucumber`. Это также можно сделать автоматически с помощью codemod:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v7 ./src/e2e/*
```

Вот и все! Больше никаких изменений не требуется 🎉

#### Коммиты:

- _transpile step definitions_ [[8c97b90]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/8c97b90a8b9197c62dffe4e2954f7dad814753cc)

## Заключение

Мы надеемся, что этот учебник поможет вам в процессе миграции на WebdriverIO `v7`. Сообщество продолжает улучшать codemod, тестируя его с различными командами в различных организациях. Не стесняйтесь [создать issue](https://github.com/webdriverio/codemod/issues/new), если у вас есть отзывы, или [начать обсуждение](https://github.com/webdriverio/codemod/discussions/new), если у вас возникли трудности в процессе миграции.