---
id: customreporter
title: Власний Репортер
---

Ви можете написати свій власний репортер для тестового запускача WDIO, який буде адаптований до ваших потреб. І це легко!

Все, що вам потрібно зробити, це створити модуль Node, який успадковується від пакету `@wdio/reporter`, щоб він міг отримувати повідомлення від тесту.

Базова структура повинна виглядати так:

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    constructor(options) {
        /*
         * make reporter to write to the output stream by default
         */
        options = Object.assign(options, { stdout: true })
        super(options)
    }

    onTestPass(test) {
        this.write(`Congratulations! Your test "${test.title}" passed 👏`)
    }
}
```

Щоб використовувати цей репортер, вам лише потрібно призначити його до властивості `reporter` у вашій конфігурації.


Ваш файл `wdio.conf.js` повинен виглядати так:

```js
import CustomReporter from './reporter/my.custom.reporter'

export const config = {
    // ...
    reporters: [
        /**
         * use imported reporter class
         */
        [CustomReporter, {
            someOption: 'foobar'
        }],
        /**
         * use absolute path to reporter
         */
        ['/path/to/reporter.js', {
            someOption: 'foobar'
        }]
    ],
    // ...
}
```

Ви також можете опублікувати репортер у NPM, щоб кожен міг ним користуватися. Назвіть пакет як інші репортери `wdio-<reportername>-reporter` і позначте його ключовими словами як `wdio` або `wdio-reporter`.

## Обробник подій

Ви можете зареєструвати обробник подій для кількох подій, які запускаються під час тестування. Всі наступні обробники отримують корисну інформацію про поточний стан і прогрес.

Структура цих об'єктів залежить від події і є уніфікованою для всіх фреймворків (Mocha, Jasmine та Cucumber). Після реалізації власного репортера він повинен працювати для всіх фреймворків.

Наступний список містить усі можливі методи, які ви можете додати до свого класу репортера:

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    onRunnerStart() {}
    onBeforeCommand() {}
    onAfterCommand() {}
    onSuiteStart() {}
    onHookStart() {}
    onHookEnd() {}
    onTestStart() {}
    onTestPass() {}
    onTestFail() {}
    onTestSkip() {}
    onTestEnd() {}
    onSuiteEnd() {}
    onRunnerEnd() {}
}
```

Назви методів досить зрозумілі.

Щоб вивести щось при певній події, використовуйте метод `this.write(...)`, який надається батьківським класом `WDIOReporter`. Він передає вміст до `stdout` або до лог-файлу (залежно від налаштувань репортера).

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    onTestPass(test) {
        this.write(`Congratulations! Your test "${test.title}" passed 👏`)
    }
}
```

Зверніть увагу, що ви не можете відкласти виконання тесту жодним чином.

Усі обробники подій повинні виконувати синхронні процедури (інакше ви матимете проблеми з умовами гонки).

Обов'язково ознайомтеся з [секцією прикладів](https://github.com/webdriverio/webdriverio/tree/main/examples/wdio), де ви можете знайти приклад власного репортера, який виводить назву події для кожної події.

Якщо ви реалізували власний репортер, який може бути корисним для спільноти, не соромтеся зробити Pull Request, щоб ми могли зробити репортер доступним для публіки!

Також, якщо ви запускаєте тестувальник WDIO через інтерфейс `Launcher`, ви не можете застосувати власний репортер як функцію наступним чином:

```js
import Launcher from '@wdio/cli'

import CustomReporter from './reporter/my.custom.reporter'

const launcher = new Launcher('/path/to/config.file.js', {
    // this will NOT work, because CustomReporter is not serializable
    reporters: ['dot', CustomReporter]
})
```

## Почекати поки `isSynchronised`

Якщо вашому репортеру потрібно виконати асинхронні операції для звітування даних (наприклад, завантаження файлів журналу або інших ресурсів), ви можете перевизначити метод `isSynchronised` у своєму власному репортері, щоб запускач WebdriverIO чекав, поки ви не завершите всі обчислення. Приклад цього можна побачити в [`@wdio/sumologic-reporter`](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-sumologic-reporter/src/index.ts):

```js
export default class SumoLogicReporter extends WDIOReporter {
    constructor (options) {
        // ...
        this.unsynced = []
        this.interval = setInterval(::this.sync, this.options.syncInterval)
        // ...
    }

    /**
     * overwrite isSynchronised method
     */
    get isSynchronised () {
        return this.unsynced.length === 0
    }

    /**
     * sync log files
     */
    sync () {
        // ...
        request({
            method: 'POST',
            uri: this.options.sourceAddress,
            body: logLines
        }, (err, resp) => {
            // ...
            /**
             * remove transferred logs from log bucket
             */
            this.unsynced.splice(0, MAX_LINES)
            // ...
        }
    }
}
```

Таким чином, запускач чекатиме, поки вся інформація журналу не буде завантажена.

## Публікація репортера в NPM

Щоб зробити репортер більш зручним і доступним для спільноти WebdriverIO, дотримуйтесь цих рекомендацій:

* Сервіси повинні використовувати таку конвенцію найменування: `wdio-*-reporter`
* Використовуйте ключові слова NPM: `wdio-plugin`, `wdio-reporter`
* Основний запис повинен `export` екземпляр репортера
* Приклад репортера: [`@wdio/dot-service`](https://github.com/webdriverio/webdriverio/tree/main/packages/wdio-dot-reporter)

Дотримуючись рекомендованого шаблону найменування, сервіси можна додавати за назвою:

```js
// Add wdio-custom-reporter
export const config = {
    // ...
    reporter: ['custom'],
    // ...
}
```

### Додати опублікований сервіс до WDIO CLI та документації

Ми дуже цінуємо кожний новий плагін, який може допомогти іншим людям проводити кращі тести! Якщо ви створили такий плагін, будь ласка, розгляньте можливість додати його до нашого CLI та документації, щоб його було легше знайти.

Будь ласка, створіть pull request з наступними змінами:

- додайте свій сервіс до списку [підтримуваних репортерів](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-cli/src/constants.ts#L74-L91)) у модулі CLI
- розширте [список репортерів](https://github.com/webdriverio/webdriverio/blob/main/scripts/docs-generation/3rd-party/reporters.json) для додавання вашої документації до офіційної сторінки Webdriver.io