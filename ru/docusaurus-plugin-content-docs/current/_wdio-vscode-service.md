---
id: wdio-vscode-service
title: Сервис тестирования расширений VSCode
custom_edit_url: https://github.com/webdriverio-community/wdio-vscode-service/edit/main/README.md
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

> wdio-vscode-service является пакетом от третьих лиц, для получения дополнительной информации смотрите [GitHub](https://github.com/webdriverio-community/wdio-vscode-service) | [npm](https://www.npmjs.com/package/wdio-vscode-service)

Протестировано на:

[![VSCode Version](https://img.shields.io/badge/VSCode%20Version-insiders%20/%20stable%20/%20v1.86.0%20/%20web-brightgreen)](https://github.com/webdriverio-community/wdio-vscode-service/actions/workflows/ci.yml) [![CI Status](https://img.shields.io/badge/Platform-windows%20%2F%20macos%20%2F%20ubuntu-brightgreen)](https://github.com/webdriverio-community/wdio-vscode-service/actions/workflows/ci.yml)

> WebdriverIO сервис для тестирования расширений VSCode.

Этот сервис WebdriverIO позволяет легко тестировать ваши расширения VSCode от начала до конца в настольной IDE VSCode или как веб-расширение. Вам нужно только указать путь к вашему расширению, а сервис сделает все остальное:

- 🏗️ Установит VSCode (либо `stable`, `insiders` или указанную версию)
- ⬇️ Загрузит Chromedriver, соответствующий данной версии VSCode
- 🚀 Позволит вам получить доступ к API VSCode из ваших тестов
- 🖥️ Запустит VSCode с пользовательскими настройками (включая поддержку VSCode на Ubuntu, MacOS и Windows)
- 🌐 Или запустит VSCode с сервера для доступа из любого браузера для тестирования [веб-расширений](https://code.visualstudio.com/api/extension-guides/web-extensions)
- 📔 Предоставит готовые объекты страниц с локаторами, соответствующими вашей версии VSCode

Этот проект был вдохновлен проектом [vscode-extension-tester](https://www.npmjs.com/package/vscode-extension-tester), который основан на Selenium. Этот пакет берет идею и адаптирует ее для WebdriverIO.

Начиная с VSCode v1.86 требуется использовать `webdriverio` v8.14 или новее для установки Chromedriver без необходимости дополнительной настройки. Если вам нужно тестировать более ранние версии VSCode, смотрите раздел [Конфигурация Chromedriver](#chromedriver) ниже.

## Установка

Чтобы инициировать новый проект WebdriverIO, выполните:

```bash
npm create wdio ./
```

Мастер установки проведет вас через процесс. Убедитесь, что вы выбрали TypeScript в качестве компилятора и не генерируйте объекты страниц, поскольку этот проект поставляется со всеми необходимыми объектами страниц. Затем убедитесь, что вы выбрали `vscode` в списке сервисов:

![Install Demo](https://raw.githubusercontent.com/webdriverio-community/wdio-vscode-service/main/.github/assets/demo.gif "Install Demo")

Для получения дополнительной информации о том, как установить `WebdriverIO`, пожалуйста, ознакомьтесь с [документацией проекта](https://webdriver.io/docs/gettingstarted).

## Пример конфигурации

Чтобы использовать сервис, вам нужно добавить `vscode` в список сервисов, опционально за которым следует объект конфигурации. Это заставит WebdriverIO загрузить указанные бинарные файлы VSCode и соответствующую версию Chromedriver:

```js
// wdio.conf.ts
export const config = {
    outputDir: 'trace',
    // ...
    capabilities: [{
        browserName: 'vscode',
        browserVersion: '1.86.0', // "insiders" или "stable" для последней версии VSCode
        'wdio:vscodeOptions': {
            extensionPath: __dirname,
            userSettings: {
                "editor.fontSize": 14
            }
        }
    }],
    services: ['vscode'],
    /**
     * Опционально определите путь, где WebdriverIO будет хранить все бинарные файлы VSCode, например:
     * services: [['vscode', { cachePath: __dirname }]]
     */
    // ...
};
```

Если вы определяете `wdio:vscodeOptions` с любым другим значением `browserName` кроме `vscode`, например, `chrome`, сервис будет обслуживать расширение как веб-расширение. Если вы тестируете в Chrome, дополнительный драйвер-сервис не требуется, например:

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

В вашем `tsconfig.json` убедитесь, что вы добавили `wdio-vscode-service` в список типов:

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
        "target": "es2019",
        "moduleResolution": "node",
        "esModuleInterop": true
    }
}
```

## Использование

Затем вы можете использовать метод `getWorkbench` для доступа к объектам страниц для локаторов, соответствующих вашей версии VSCode:

```ts
describe('WDIO VSCode Service', () => {
    it('should be able to load VSCode', async () => {
        const workbench = await browser.getWorkbench()
        expect(await workbench.getTitleBar().getTitle())
            .toBe('[Extension Development Host] - README.md - wdio-vscode-service - Visual Studio Code')
    })
})
```

### Доступ к API VSCode

Если вы хотите выполнить определенную автоматизацию через [API VSCode](https://code.visualstudio.com/api/references/vscode-api), вы можете сделать это, запустив удаленные команды через пользовательскую команду `executeWorkbench`. Эта команда позволяет удаленно выполнять код из вашего теста внутри среды VSCode и дает доступ к API VSCode. Вы можете передавать произвольные параметры в функцию, которые будут переданы в функцию. Объект `vscode` всегда будет передан в качестве первого аргумента, за которым следуют параметры внешней функции. Обратите внимание, что вы не можете получить доступ к переменным вне области видимости функции, так как обратный вызов выполняется удаленно. Пример:

```ts
const workbench = await browser.getWorkbench()
await browser.executeWorkbench((vscode, param1, param2) => {
    vscode.window.showInformationMessage(`I am an ${param1} ${param2}!`)
}, 'API', 'call')

const notifs = await workbench.getNotifications()
console.log(await notifs[0].getMessage()) // выводит: "I am an API call!"
```

Для полной документации по объектам страниц, проверьте [документацию](https://webdriverio-community.github.io/wdio-vscode-service/modules.html). Вы можете найти различные примеры использования в [наборе тестов этого проекта](https://github.com/webdriverio-community/wdio-vscode-service/blob/main/test/specs).

## Конфигурация

Через конфигурацию сервиса вы можете управлять версией VSCode, а также пользовательскими настройками для VSCode:

### Опции сервиса

Опции сервиса - это параметры, необходимые для настройки тестовой среды.

#### `cachePath`

Определите путь кэша, чтобы избежать повторной загрузки пакетов VS Code. Это полезно для CI/CD, чтобы избежать повторной загрузки VSCode для каждого запуска теста.

Тип: `string`<br />
По умолчанию: `process.cwd()`

### Возможности VSCode (`wdio:vscodeOptions`)

Для запуска тестов через VSCode вы должны определить `vscode` как `browserName`. Вы можете указать версию VSCode, предоставив возможность `browserVersion`. Пользовательские опции VSCode затем определяются в пользовательской возможности `wdio:vscodeOptions`. Доступны следующие опции:

#### `binary`

Путь к локально установленному VSCode. Если опция не указана, сервис загрузит VSCode на основе заданного `browserVersion` (или `stable`, если не указан).

Тип: `string`

#### `extensionPath`

Определите директорию расширения, которое вы хотите тестировать.

Тип: `string`

#### `storagePath`

Определите пользовательское расположение для VS Code для хранения всех данных. Это корень для внутренних директорий VS Code, таких как (частичный список)
* **user-data-dir**: Директория, где хранятся все пользовательские настройки (глобальные настройки), логи расширений и т.д.
* **extension-install-dir**: Директория, где установлены расширения VS Code.

Если не указано, используется временная директория.

Тип: `string`

#### `userSettings`

Определите пользовательские настройки, которые будут применены к VSCode.

Тип: `Record<string, number | string | object | boolean>`<br />
По умолчанию: `{}`

#### `workspacePath`

Открывает VSCode для конкретного рабочего пространства. Если не указано, VSCode запускается без открытого рабочего пространства.

Тип: `string`

#### `filePath`

Открывает VSCode с конкретным открытым файлом.

Тип: `string`

#### `vscodeArgs`

Дополнительные аргументы запуска в виде объекта, например

```ts
vscodeArgs: { fooBar: true, 'bar-foo': '/foobar' }
```

будут переданы как:

```ts
--foo-bar --fooBar --bar-foo=/foobar
```

Тип: `Record<string, string | boolean>`<br />
По умолчанию: см. [`constants.ts#L5-L14`](https://github.com/webdriverio-community/wdio-vscode-service/blob/196a69be3ac2fb82d9c7e4f19a2a1c8ccbaec1e2/src/constants.ts#L5-L14)

#### `verboseLogging`

Если установлено в true, сервис регистрирует вывод VSCode из хоста расширения и консольного API.

Тип: `boolean`<br />
По умолчанию: `false`

#### `vscodeProxyOptions`

Конфигурации прокси API VSCode определяют, как WebdriverIO подключается к рабочей области VSCode, чтобы предоставить вам доступ к API VSCode.

Тип: `VSCodeProxyOptions`<br />
По умолчанию:

```ts
{
    /**
     * Если установлено в true, сервис пытается установить соединение с
     * рабочей областью VSCode, чтобы обеспечить доступ к API VSCode
     */
    enable: true,
    /**
     * Порт WebSocket-соединения, используемого для подключения к рабочей области.
     * По умолчанию устанавливается на доступный порт в вашей операционной системе.
     */
    // port?: number
    /**
     * Тайм-аут для подключения к WebSocket внутри VSCode
     */
    connectionTimeout: 5000,
    /**
     * Тайм-аут для выполнения команды внутри VSCode
     */
    commandTimeout: 5000
}
```

### Chromedriver

Начиная с VSCode v1.86 требуется использовать `webdriverio` v8.14 или новее для установки Chromedriver без необходимости дополнительной настройки. [Упрощенная настройка автоматизации браузера](https://webdriver.io/blog/2023/07/31/driver-management) делает все за вас.

Для тестирования более ранних версий VS Code найдите ожидаемую версию Chromedriver из логов, загрузите [Chromedriver](https://chromedriver.chromium.org/downloads) и настройте путь. Например:

```
[0-0] ERROR webdriver: Failed downloading chromedriver v108: Download failed: ...
```

```ts
    capabilities: [{
        browserName: 'vscode',
        browserVersion: '1.80.0',
        'wdio:chromedriverOptions': {
            binary: path.join(cacheDir, 'chromedriver-108.0.5359.71')
```

## Создание собственных PageObjects

Вы можете повторно использовать компоненты из этого сервиса для создания своих объектов страниц. Для этого сначала создайте файл, определяющий все ваши селекторы, например:

```ts
// например, в /test/pageobjects/locators.ts
export const componentA = {
    elem: 'form', // элемент-контейнер компонента
    submit: 'button[type="submit"]', // кнопка отправки
    username: 'input.username', // поле ввода имени пользователя
    password: 'input.password' // поле ввода пароля
}
```

Теперь вы можете создать объект страницы следующим образом:

```ts
// например, в /test/pageobjects/loginForm.ts
import { PageDecorator, IPageDecorator, BasePage } from 'wdio-vscode-service'
import * as locatorMap, { componentA as componentALocators } from './locators'
export interface LoginForm extends IPageDecorator<typeof componentALocators> {}
@PageDecorator(componentALocators)
export class LoginForm extends BasePage<typeof componentALocators, typeof locatorMap> {
    /**
     * @private ключ локатора для идентификации карты локаторов (см. locators.ts)
     */
    public locatorKey = 'componentA' as const

    public login (username: string, password: string) {
        await this.username$.setValue(username)
        await this.password$.setValue(password)
        await this.submit$.click()
    }
}
```

Теперь в вашем тесте вы можете использовать ваш объект страницы следующим образом:

```ts
import { LoginForm } from '../pageobjects/loginForm'
import * as locatorMap from '../locators'

// например, в /test/specs/example.e2e.ts
describe('my extension', () => {
    it('should login', async () => {
        const loginForm = new LoginForm(locatorMap)
        await loginForm.login('admin', 'test123')

        // вы также можете использовать элементы объекта страницы напрямую через `[selector]$`
        // или `[selector]$$`, например:
        await loginForm.submit$.click()

        // или получить доступ к локаторам напрямую
        console.log(loginForm.locators.username)
        // выводит: "input.username"
    })
})
```

## Поддержка TypeScript

Если вы используете WebdriverIO с TypeScript, убедитесь, что вы добавили `wdio-vscode-service` в ваши `types` в `tsconfig.json`, например:

```json
{
    "compilerOptions": {
        "moduleResolution": "node",
        "types": [
            "webdriverio/async",
            "@wdio/mocha-framework",
            "expect-webdriverio",
            // добавьте этот сервис в ваши типы
            "wdio-devtools-service"
        ],
        "target": "es2019"
    }
}
```

## Поддержка прокси

При инициализации этого сервиса загружаются ChromeDriver и дистрибутив VSCode. Вы можете перенаправить эти запросы через прокси, установив переменную окружения `HTTPS_PROXY` или `https_proxy`. Например:

```bash
HTTPS_PROXY=http://127.0.0.1:1080 npm run wdio
```

## Ссылки

Следующие расширения VS Code используют `wdio-vscode-service`:

- [Marquee](https://marketplace.visualstudio.com/items?itemName=stateful.marquee) (27 тыс. загрузок)
- [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) (27,8 млн загрузок)
- [DVC Extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=Iterative.dvc) (11,2 тыс. загрузок)
- [Nx Console](https://marketplace.visualstudio.com/items?itemName=nrwl.angular-console) (1,2 млн загрузок)
- [inlang – i18n supercharged](https://marketplace.visualstudio.com/items?itemName=inlang.vs-code-extension) (3 тыс. загрузок)

## Содействие

Перед отправкой pull request выполните следующие действия:

1. `git clone git@github.com:webdriverio-community/wdio-vscode-service.git`
1. `cd wdio-vscode-service`
1. `npm install`
1. `npm run build`
1. `npm run test` (или `npm run ci`)

## Узнать больше

Если вы хотите узнать больше о тестировании расширений VSCode, ознакомьтесь с выступлением [Christian Bromann](https://twitter.com/bromann) на [OpenJS World 2022](https://www.youtube.com/watch?v=PhGNTioBUiU):

[![Testing VSCode Extensions at OpenJS World 2022](https://img.youtube.com/vi/PhGNTioBUiU/sddefault.jpg)](https://www.youtube.com/watch?v=PhGNTioBUiU)

---

Для получения дополнительной информации о WebdriverIO, посетите [домашнюю страницу](https://webdriver.io) проекта.