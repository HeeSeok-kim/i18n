---
id: v6-migration
title: С v5 на v6
---

Этот учебник предназначен для тех, кто все еще использует `v5` WebdriverIO и хочет перейти на `v6` или на последнюю версию WebdriverIO. Как упоминалось в нашем [блоге о выпуске](https://webdriver.io/blog/2020/03/26/webdriverio-v6-released), изменения для этого обновления версии можно обобщить следующим образом:

- мы объединили параметры для некоторых команд (например, `newWindow`, `react$`, `react$$`, `waitUntil`, `dragAndDrop`, `moveTo`, `waitForDisplayed`, `waitForEnabled`, `waitForExist`) и переместили все необязательные параметры в единый объект, например:

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

- конфигурации для сервисов перемещены в список сервисов, например:

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

- некоторые параметры сервисов были переименованы для упрощения
- мы переименовали команду `launchApp` в `launchChromeApp` для сессий Chrome WebDriver

:::info

Если вы используете WebdriverIO `v4` или ниже, сначала обновитесь до `v5`.

:::

Хотя мы хотели бы иметь полностью автоматизированный процесс для этого, реальность выглядит иначе. У каждого своя конфигурация. Каждый шаг следует рассматривать как руководство, а не как пошаговую инструкцию. Если у вас возникают проблемы с миграцией, не стесняйтесь [связаться с нами](https://github.com/webdriverio/codemod/discussions/new).

## Настройка

Подобно другим миграциям, мы можем использовать WebdriverIO [codemod](https://github.com/webdriverio/codemod). Для установки codemod выполните:

```sh
npm install jscodeshift @wdio/codemod
```

## Обновление зависимостей WebdriverIO

Учитывая, что все версии WebdriverIO связаны друг с другом, лучше всего всегда обновлять до определенного тега, например, `6.12.0`. Если вы решите обновиться с `v5` прямо до `v7`, вы можете опустить тег и установить последние версии всех пакетов. Для этого копируем все зависимости WebdriverIO из нашего `package.json` и переустанавливаем их:

```sh
npm i --save-dev @wdio/allure-reporter@6 @wdio/cli@6 @wdio/cucumber-framework@6 @wdio/local-runner@6 @wdio/spec-reporter@6 @wdio/sync@6 wdio-chromedriver-service@6 webdriverio@6
```

Обычно зависимости WebdriverIO являются частью dev-зависимостей, хотя в зависимости от вашего проекта это может отличаться. После этого ваши `package.json` и `package-lock.json` должны быть обновлены. __Примечание:__ это пример зависимостей, ваши могут отличаться. Убедитесь, что вы нашли последнюю версию v6, вызвав, например:

```sh
npm show webdriverio versions
```

Постарайтесь установить последнюю доступную версию 6 для всех основных пакетов WebdriverIO. Для сторонних пакетов это может отличаться от пакета к пакету. Здесь мы рекомендуем проверить список изменений для получения информации о том, какая версия все еще совместима с v6.

## Преобразование файла конфигурации

Хорошим первым шагом является начало с файла конфигурации. Все критические изменения можно разрешить с помощью codemod полностью автоматически:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v6 ./wdio.conf.js
```

:::caution

Codemod пока не поддерживает проекты TypeScript. См. [`@webdriverio/codemod#10`](https://github.com/webdriverio/codemod/issues/10). Мы работаем над внедрением поддержки в ближайшее время. Если вы используете TypeScript, пожалуйста, примите участие!

:::

## Обновление файлов спецификаций и объектов страниц

Чтобы обновить все изменения команд, запустите codemod для всех ваших e2e-файлов, содержащих команды WebdriverIO, например:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v6 ./e2e/*
```

Вот и всё! Больше никаких изменений не требуется 🎉

## Заключение

Мы надеемся, что этот учебник немного помогает вам в процессе миграции на WebdriverIO `v6`. Мы настоятельно рекомендуем продолжить обновление до последней версии, учитывая, что обновление до `v7` тривиально из-за почти отсутствующих критических изменений. Пожалуйста, ознакомьтесь с руководством по миграции [для обновления до v7](v7-migration).

Сообщество продолжает улучшать codemod, тестируя его с различными командами в различных организациях. Не стесняйтесь [создать issue](https://github.com/webdriverio/codemod/issues/new), если у вас есть отзывы, или [начать обсуждение](https://github.com/webdriverio/codemod/discussions/new), если у вас возникли проблемы в процессе миграции.