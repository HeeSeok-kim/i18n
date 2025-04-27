---
id: customreporter
title: Пользовательский Репортер
---

Вы можете написать свой собственный пользовательский репортер для тест-раннера WDIO, адаптированный под ваши нужды. И это легко!

Всё, что вам нужно сделать, это создать node-модуль, который наследуется от пакета `@wdio/reporter`, чтобы он мог получать сообщения от теста.

Базовая настройка должна выглядеть так:

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    constructor(options) {
        /*
         * заставляет репортер писать в выходной поток по умолчанию
         */
        options = Object.assign(options, { stdout: true })
        super(options)
    }

    onTestPass(test) {
        this.write(`Congratulations! Your test "${test.title}" passed 👏`)
    }
}
```

Чтобы использовать этот репортер, вам нужно присвоить его свойству `reporter` в вашей конфигурации.


Ваш файл `wdio.conf.js` должен выглядеть так:

```js
import CustomReporter from './reporter/my.custom.reporter'

export const config = {
    // ...
    reporters: [
        /**
         * использовать импортированный класс репортера
         */
        [CustomReporter, {
            someOption: 'foobar'
        }],
        /**
         * использовать абсолютный путь к репортеру
         */
        ['/path/to/reporter.js', {
            someOption: 'foobar'
        }]
    ],
    // ...
}
```

Вы также можете опубликовать репортер в NPM, чтобы его мог использовать каждый. Назовите пакет как другие репортеры `wdio-<reportername>-reporter`, и пометьте его ключевыми словами `wdio` или `wdio-reporter`.

## Обработчик событий

Вы можете зарегистрировать обработчик событий для нескольких событий, которые запускаются во время тестирования. Все следующие обработчики будут получать данные с полезной информацией о текущем состоянии и прогрессе.

Структура этих объектов данных зависит от события и унифицирована для всех фреймворков (Mocha, Jasmine и Cucumber). После того, как вы реализуете пользовательский репортер, он должен работать для всех фреймворков.

Следующий список содержит все возможные методы, которые вы можете добавить в класс вашего репортера:

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

Названия методов достаточно понятны.

Чтобы вывести что-то при определенном событии, используйте метод `this.write(...)`, который предоставляется родительским классом `WDIOReporter`. Он либо передает содержимое в `stdout`, либо в файл журнала (в зависимости от параметров репортера).

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    onTestPass(test) {
        this.write(`Congratulations! Your test "${test.title}" passed 👏`)
    }
}
```

Обратите внимание, что вы не можете откладывать выполнение теста каким-либо образом.

Все обработчики событий должны выполнять синхронные операции (иначе вы столкнетесь с условиями гонки).

Обязательно ознакомьтесь с [разделом примеров](https://github.com/webdriverio/webdriverio/tree/main/examples/wdio), где вы найдете пример пользовательского репортера, который выводит имя события для каждого события.

Если вы реализовали пользовательский репортер, который может быть полезен сообществу, не стесняйтесь создать Pull Request, чтобы мы могли сделать репортер доступным для общественности!

Также, если вы запускаете тестраннер WDIO через интерфейс `Launcher`, вы не сможете применить пользовательский репортер в виде функции следующим образом:

```js
import Launcher from '@wdio/cli'

import CustomReporter from './reporter/my.custom.reporter'

const launcher = new Launcher('/path/to/config.file.js', {
    // это НЕ сработает, потому что CustomReporter не сериализуем
    reporters: ['dot', CustomReporter]
})
```

## Ожидание `isSynchronised`

Если ваш репортер должен выполнять асинхронные операции для отчетности данных (например, загрузку лог-файлов или других ресурсов), вы можете переопределить метод `isSynchronised` в своем пользовательском репортере, чтобы указать WebdriverIO ожидать, пока вы не завершите все вычисления. Пример этого можно увидеть в [`@wdio/sumologic-reporter`](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-sumologic-reporter/src/index.ts):

```js
export default class SumoLogicReporter extends WDIOReporter {
    constructor (options) {
        // ...
        this.unsynced = []
        this.interval = setInterval(::this.sync, this.options.syncInterval)
        // ...
    }

    /**
     * переопределение метода isSynchronised
     */
    get isSynchronised () {
        return this.unsynced.length === 0
    }

    /**
     * синхронизация лог-файлов
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
             * удаление переданных логов из корзины логов
             */
            this.unsynced.splice(0, MAX_LINES)
            // ...
        }
    }
}
```

Таким образом, раннер будет ждать, пока вся информация журнала не будет загружена.

## Публикация репортера на NPM

Чтобы сделать репортер более доступным и обнаруживаемым сообществом WebdriverIO, следуйте этим рекомендациям:

* Сервисы должны использовать следующее соглашение об именовании: `wdio-*-reporter`
* Используйте ключевые слова NPM: `wdio-plugin`, `wdio-reporter`
* Основная запись должна `export` экземпляр репортера
* Пример репортера: [`@wdio/dot-service`](https://github.com/webdriverio/webdriverio/tree/main/packages/wdio-dot-reporter)

Следование рекомендуемому шаблону именования позволяет добавлять сервисы по имени:

```js
// Добавить wdio-custom-reporter
export const config = {
    // ...
    reporter: ['custom'],
    // ...
}
```

### Добавление опубликованного сервиса в WDIO CLI и Docs

Мы очень ценим каждый новый плагин, который может помочь другим людям запускать лучшие тесты! Если вы создали такой плагин, рассмотрите возможность добавления его в наши CLI и документацию, чтобы его было легче найти.

Пожалуйста, создайте pull request со следующими изменениями:

- добавьте ваш сервис в список [поддерживаемых репортеров](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-cli/src/constants.ts#L74-L91)) в модуле CLI
- расширьте [список репортеров](https://github.com/webdriverio/webdriverio/blob/main/scripts/docs-generation/3rd-party/reporters.json) для добавления вашей документации на официальную страницу Webdriver.io