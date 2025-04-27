---
id: testrunner
title: Тестраннер
---

WebdriverIO поставляется с собственным тестраннером, чтобы помочь вам начать тестирование как можно быстрее. Он предназначен для выполнения всей работы за вас, позволяет интегрироваться со сторонними сервисами и помогает запускать тесты максимально эффективно.

Тестраннер WebdriverIO поставляется отдельно в NPM-пакете `@wdio/cli`.

Установите его так:

```sh npm2yarn
npm install @wdio/cli
```

Чтобы увидеть справку по интерфейсу командной строки, введите следующую команду в вашем терминале:

```sh
$ npx wdio --help

wdio <command>

Commands:
  wdio config                           Initialize WebdriverIO and setup configuration in
                                        your current project.
  wdio install <type> <name>            Add a `reporter`, `service`, or `framework` to
                                        your WebdriverIO project
  wdio repl <option> [capabilities]     Run WebDriver session in command line
  wdio run <configPath>                 Run your WDIO configuration file to initialize
                                        your tests.

Options:
  --version  Show version number                                       [boolean]
  --help     Show help                                                 [boolean]
```

Отлично! Теперь вам нужно определить конфигурационный файл, в котором будет содержаться вся информация о ваших тестах, возможностях и настройках. Перейдите к разделу [Конфигурационный файл](/docs/configuration), чтобы узнать, как должен выглядеть этот файл.

С помощью утилиты настройки `wdio` создать файл конфигурации очень просто. Просто запустите:

```sh
$ npx wdio config
```

...и запустится вспомогательная утилита.

Она задаст вам вопросы и создаст файл конфигурации менее чем за минуту.

![Утилита конфигурации WDIO](/img/config-utility.gif)

После настройки файла конфигурации вы можете запустить тесты, выполнив:

```sh
npx wdio run wdio.conf.js
```

Вы также можете инициализировать запуск тестов без команды `run`:

```sh
npx wdio wdio.conf.js
```

Вот и всё! Теперь вы можете получить доступ к экземпляру selenium через глобальную переменную `browser`.

## Команды

### `wdio config`

Команда `config` запускает помощник по настройке WebdriverIO. Этот помощник задаст вам несколько вопросов о вашем проекте WebdriverIO и создаст файл `wdio.conf.js` на основе ваших ответов.

Пример:

```sh
wdio config
```

Опции:

```
--help            выводит меню справки WebdriverIO                      [boolean]
--npm             Устанавливать ли пакеты с помощью NPM вместо yarn     [boolean]
```

### `wdio run`

> Это команда по умолчанию для запуска вашей конфигурации.

Команда `run` инициализирует ваш конфигурационный файл WebdriverIO и запускает ваши тесты.

Пример:

```sh
wdio run ./wdio.conf.js --watch
```

Опции:

```
--help                выводит меню справки WebdriverIO            [boolean]
--version             выводит версию WebdriverIO                  [boolean]
--hostname, -h        адрес хоста драйвера автоматизации           [string]
--port, -p            порт драйвера автоматизации                  [number]
--user, -u            имя пользователя при использовании облачного сервиса
                        в качестве бэкенда автоматизации             [string]
--key, -k             соответствующий ключ доступа для пользователя [string]
--watch               отслеживать изменения в спецификациях        [boolean]
--logLevel, -l        уровень подробности логирования
                            [выбор: "trace", "debug", "info", "warn", "error", "silent"]
--bail                остановить тестраннер после определенного количества
                        проваленных тестов                           [number]
--baseUrl             сократить вызовы команд url, установив базовый url [string]
--waitforTimeout, -w  таймаут для всех команд waitForXXX             [number]
--framework, -f       определяет фреймворк (Mocha, Jasmine или Cucumber)
                        для запуска спецификаций                     [string]
--reporters, -r       репортеры для вывода результатов в stdout       [array]
--suite               переопределяет атрибут specs и запускает определенный
                        набор тестов                                   [array]
--spec                запуск определенного файла спецификации или шаблона -
                        переопределяет specs, поданные через stdin     [array]
--exclude             исключить файл(ы) спецификации из запуска -
                        переопределяет specs, поданные через stdin     [array]
--repeat              Повторить определенные спецификации и/или наборы N раз [number]
--mochaOpts           Опции Mocha
--jasmineOpts         Опции Jasmine
--cucumberOpts        Опции Cucumber
```

> Примечание: Автокомпиляцией можно легко управлять с помощью переменных среды `tsx`. Также смотрите [документацию по TypeScript](/docs/typescript).

### `wdio install`
Команда `install` позволяет добавлять репортеры и сервисы в ваши проекты WebdriverIO через CLI.

Пример:

```sh
wdio install service sauce # устанавливает @wdio/sauce-service
wdio install reporter dot # устанавливает @wdio/dot-reporter
wdio install framework mocha # устанавливает @wdio/mocha-framework
```

Если вы хотите установить пакеты с помощью `yarn` вместо npm, вы можете передать флаг `--yarn` в команду:

```sh
wdio install service sauce --yarn
```

Вы также можете указать пользовательский путь к конфигурации, если ваш файл конфигурации WDIO находится не в той папке, в которой вы работаете:

```sh
wdio install service sauce --config="./path/to/wdio.conf.js"
```

#### Список поддерживаемых сервисов

```
sauce
testingbot
firefox-profile
devtools
browserstack
appium
intercept
zafira-listener
reportportal
docker
wiremock
lambdatest
vite
nuxt
```

#### Список поддерживаемых репортеров

```
dot
spec
junit
allure
sumologic
concise
reportportal
video
html
json
mochawesome
timeline
```

#### Список поддерживаемых фреймворков

```
mocha
jasmine
cucumber
```

### `wdio repl`

Команда repl позволяет запустить интерактивный интерфейс командной строки для выполнения команд WebdriverIO. Он может использоваться для тестирования или просто для быстрого запуска сессии WebdriverIO.

Запуск тестов в локальном Chrome:

```sh
wdio repl chrome
```

или запуск тестов на Sauce Labs:

```sh
wdio repl chrome -u $SAUCE_USERNAME -k $SAUCE_ACCESS_KEY
```

Вы можете применять те же аргументы, что и в [команде run](#wdio-run).