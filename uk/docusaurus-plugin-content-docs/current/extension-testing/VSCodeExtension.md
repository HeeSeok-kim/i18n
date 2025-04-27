---
id: vscode-extensions
title: Тестування розширень VS Code
---

WebdriverIO дозволяє вам безперешкодно тестувати ваші розширення [VS Code](https://code.visualstudio.com/) від початку до кінця в настільному IDE VS Code або як веб-розширення. Вам лише потрібно надати шлях до вашого розширення, а фреймворк зробить все інше. З [`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service) все забезпечується та навіть більше:

- 🏗️ Встановлення VSCode (стабільної, інсайдерської або вказаної версії)
- ⬇️ Завантаження Chromedriver, відповідного для вказаної версії VSCode
- 🚀 Надає доступ до API VSCode з ваших тестів
- 🖥️ Запуск VSCode з користувацькими налаштуваннями (включаючи підтримку VSCode на Ubuntu, MacOS та Windows)
- 🌐 Або розгортає VSCode з сервера для доступу з будь-якого браузера для тестування веб-розширень
- 📔 Створення об'єктів сторінок з локаторами, що відповідають вашій версії VSCode

## Початок роботи

Щоб створити новий проект WebdriverIO, виконайте:

```sh
npm create wdio@latest ./
```

Майстер встановлення проведе вас через процес. Переконайтеся, що ви обрали _"VS Code Extension Testing"_, коли вас запитують, який тип тестування ви хотіли б зробити, після цього просто залиште значення за замовчуванням або змініть їх відповідно до ваших переваг.

## Приклад конфігурації

Щоб використовувати цей сервіс, вам потрібно додати `vscode` до списку сервісів, за бажанням з об'єктом конфігурації. Це змусить WebdriverIO завантажити вказані бінарні файли VSCode та відповідну версію Chromedriver:

```js
// wdio.conf.ts
export const config = {
    outputDir: 'trace',
    // ...
    capabilities: [{
        browserName: 'vscode',
        browserVersion: '1.71.0', // "insiders" або "stable" для останньої версії VSCode
        'wdio:vscodeOptions': {
            extensionPath: __dirname,
            userSettings: {
                "editor.fontSize": 14
            }
        }
    }],
    services: ['vscode'],
    /**
     * за бажанням ви можете визначити шлях, за яким WebdriverIO зберігає всі
     * бінарні файли VSCode та Chromedriver, наприклад:
     * services: [['vscode', { cachePath: __dirname }]]
     */
    // ...
};
```

Якщо ви визначаєте `wdio:vscodeOptions` з будь-яким іншим `browserName`, окрім `vscode`, наприклад, `chrome`, сервіс буде обслуговувати розширення як веб-розширення. Якщо ви тестуєте на Chrome, додатковий сервіс драйвера не потрібен, наприклад:

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

_Примітка:_ при тестуванні веб-розширень ви можете вибирати тільки між `stable` або `insiders` як `browserVersion`.

### Налаштування TypeScript

У вашому `tsconfig.json` переконайтеся, що додали `wdio-vscode-service` до списку типів:

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

## Використання

Ви можете використовувати метод `getWorkbench` для доступу до об'єктів сторінок для локаторів, що відповідають вашій бажаній версії VSCode:

```ts
describe('WDIO VSCode Service', () => {
    it('should be able to load VSCode', async () => {
        const workbench = await browser.getWorkbench()
        expect(await workbench.getTitleBar().getTitle())
            .toBe('[Extension Development Host] - README.md - wdio-vscode-service - Visual Studio Code')
    })
})
```

Звідти ви можете отримати доступ до всіх об'єктів сторінок, використовуючи відповідні методи об'єктів сторінок. Дізнайтеся більше про всі доступні об'єкти сторінок та їх методи в [документації об'єктів сторінок](https://webdriverio-community.github.io/wdio-vscode-service/).

### Доступ до API VSCode

Якщо ви хочете виконати певну автоматизацію за допомогою [API VSCode](https://code.visualstudio.com/api/references/vscode-api), ви можете зробити це, запустивши віддалені команди через спеціальну команду `executeWorkbench`. Ця команда дозволяє віддалено виконувати код з вашого тесту всередині середовища VSCode та надає доступ до API VSCode. Ви можете передавати довільні параметри у функцію, які потім будуть передані у функцію. Об'єкт `vscode` завжди буде переданий як перший аргумент, за яким слідують параметри зовнішньої функції. Зауважте, що ви не можете отримати доступ до змінних поза областю функції, оскільки зворотний виклик виконується віддалено. Ось приклад:

```ts
const workbench = await browser.getWorkbench()
await browser.executeWorkbench((vscode, param1, param2) => {
    vscode.window.showInformationMessage(`I am an ${param1} ${param2}!`)
}, 'API', 'call')

const notifs = await workbench.getNotifications()
console.log(await notifs[0].getMessage()) // виводить: "I am an API call!"
```

Для повної документації об'єктів сторінок перегляньте [документацію](https://webdriverio-community.github.io/wdio-vscode-service/modules.html). Ви можете знайти різні приклади використання в [наборі тестів цього проекту](https://github.com/webdriverio-community/wdio-vscode-service/blob/main/test/specs).

## Додаткова інформація

Ви можете дізнатися більше про те, як налаштувати [`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service) і як створювати власні об'єкти сторінок у [документації сервісу](/docs/wdio-vscode-service). Ви також можете переглянути наступну доповідь [Christian Bromann](https://twitter.com/bromann) про [_Тестування складних розширень VSCode з силою веб-стандартів_](https://www.youtube.com/watch?v=PhGNTioBUiU):

<LiteYouTubeEmbed
    id="PhGNTioBUiU"
    title="Testing Complex VSCode Extensions With the Power of Web Standards"
/>