---
id: vscode-extensions
title: Тестирование расширений VS Code
---

WebdriverIO позволяет вам беспрепятственно тестировать ваши расширения [VS Code](https://code.visualstudio.com/) от начала до конца в настольной IDE VS Code или как веб-расширение. Вам нужно только предоставить путь к вашему расширению, а фреймворк сделает все остальное. С помощью [`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service) всё происходит автоматически и даже больше:

- 🏗️ Установка VSCode (стабильной, инсайдерской или указанной версии)
- ⬇️ Загрузка Chromedriver, специфичного для данной версии VSCode
- 🚀 Предоставление доступа к API VSCode из ваших тестов
- 🖥️ Запуск VSCode с пользовательскими настройками (включая поддержку VSCode на Ubuntu, MacOS и Windows)
- 🌐 Или размещение VSCode на сервере для доступа через любой браузер для тестирования веб-расширений
- 📔 Создание объектов страниц с локаторами, соответствующими вашей версии VSCode

## Начало работы

Чтобы инициировать новый проект WebdriverIO, запустите:

```sh
npm create wdio@latest ./
```

Мастер установки проведет вас через процесс. Убедитесь, что вы выбрали _"VS Code Extension Testing"_, когда вас спросят, какой тип тестирования вы хотите выполнить, после чего просто сохраните значения по умолчанию или измените их по своему усмотрению.

## Пример конфигурации

Чтобы использовать сервис, вам нужно добавить `vscode` в список ваших сервисов, опционально за которым следует объект конфигурации. Это заставит WebdriverIO загрузить указанные бинарные файлы VSCode и соответствующую версию Chromedriver:

```js
// wdio.conf.ts
export const config = {
    outputDir: 'trace',
    // ...
    capabilities: [{
        browserName: 'vscode',
        browserVersion: '1.71.0', // "insiders" или "stable" для последней версии VSCode
        'wdio:vscodeOptions': {
            extensionPath: __dirname,
            userSettings: {
                "editor.fontSize": 14
            }
        }
    }],
    services: ['vscode'],
    /**
     * опционально вы можете определить путь, по которому WebdriverIO хранит все
     * бинарные файлы VSCode и Chromedriver, например:
     * services: [['vscode', { cachePath: __dirname }]]
     */
    // ...
};
```

Если вы определите `wdio:vscodeOptions` с любым другим `browserName`, кроме `vscode`, например, `chrome`, сервис будет обслуживать расширение как веб-расширение. Если вы тестируете на Chrome, дополнительный драйвер-сервис не требуется, например:

```js
// wdio.conf.ts
export const config = {
    outputDir: 'trace',
    // ...
    capabilities: [{
        browserName: 'chrome',
        'wdio:vscodeOptions': {
            extensionPath: __dirname
        }
    }],
    services: ['vscode'],
    // ...
};
```

_Примечание:_ при тестировании веб-расширений вы можете выбирать только между `stable` или `insiders` в качестве `browserVersion`.

### Настройка TypeScript

В вашем `tsconfig.json` убедитесь, что добавили `wdio-vscode-service` в список типов:

```json
{
    "compilerOptions": {
        "types": [
            "node",
            "webdriverio/async",
            "@wdio/mocha-framework",
            "expect-webdriverio",
            "wdio-vscode-service"
        ],
        "target": "es2020",
        "moduleResolution": "node16"
    }
}
```

## Использование

Затем вы можете использовать метод `getWorkbench` для доступа к объектам страниц для локаторов, соответствующих вашей желаемой версии VSCode:

```ts
describe('WDIO VSCode Service', () => {
    it('should be able to load VSCode', async () => {
        const workbench = await browser.getWorkbench()
        expect(await workbench.getTitleBar().getTitle())
            .toBe('[Extension Development Host] - README.md - wdio-vscode-service - Visual Studio Code')
    })
})
```

Оттуда вы можете получить доступ ко всем объектам страниц, используя правильные методы объекта страницы. Узнайте больше обо всех доступных объектах страниц и их методах в [документации по объектам страниц](https://webdriverio-community.github.io/wdio-vscode-service/).

### Доступ к API VSCode

Если вы хотите выполнить определенную автоматизацию через [API VSCode](https://code.visualstudio.com/api/references/vscode-api), вы можете сделать это, выполняя удаленные команды через пользовательскую команду `executeWorkbench`. Эта команда позволяет удаленно выполнять код из вашего теста внутри среды VSCode и обеспечивает доступ к API VSCode. Вы можете передавать произвольные параметры в функцию, которые затем будут переданы в функцию. Объект `vscode` всегда будет передан как первый аргумент, за которым следуют параметры внешней функции. Обратите внимание, что вы не можете получить доступ к переменным вне области функции, так как обратный вызов выполняется удаленно. Вот пример:

```ts
const workbench = await browser.getWorkbench()
await browser.executeWorkbench((vscode, param1, param2) => {
    vscode.window.showInformationMessage(`I am an ${param1} ${param2}!`)
}, 'API', 'call')

const notifs = await workbench.getNotifications()
console.log(await notifs[0].getMessage()) // выводит: "I am an API call!"
```

Полную документацию по объектам страниц смотрите в [документации](https://webdriverio-community.github.io/wdio-vscode-service/modules.html). Вы можете найти различные примеры использования в [тестовом наборе этого проекта](https://github.com/webdriverio-community/wdio-vscode-service/blob/main/test/specs).

## Дополнительная информация

Вы можете узнать больше о том, как настроить [`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service) и как создавать пользовательские объекты страниц в [документации сервиса](/docs/wdio-vscode-service). Вы также можете посмотреть следующий доклад [Christian Bromann](https://twitter.com/bromann) на тему [_Testing Complex VSCode Extensions With the Power of Web Standards_](https://www.youtube.com/watch?v=PhGNTioBUiU):

<LiteYouTubeEmbed
    id="PhGNTioBUiU"
    title="Testing Complex VSCode Extensions With the Power of Web Standards"
/>