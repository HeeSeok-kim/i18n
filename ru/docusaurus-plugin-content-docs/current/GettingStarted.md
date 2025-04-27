---
id: gettingstarted
title: Начало работы
---

Добро пожаловать в документацию WebdriverIO. Она поможет вам быстро начать работу. Если у вас возникнут проблемы, вы можете найти помощь и ответы на нашем [сервере поддержки Discord](https://discord.webdriver.io) или связаться со мной в [Twitter](https://twitter.com/webdriverio).

:::info
Это документация для последней версии (__>=9.x__) WebdriverIO. Если вы все еще используете более старую версию, пожалуйста, посетите [старые версии документации](/versions)!
:::

<LiteYouTubeEmbed
    id="rA4IFNyW54c"
    title="Getting Started with WebdriverIO"
/>

:::tip Официальный YouTube канал 🎥

Вы можете найти больше видео о WebdriverIO на [официальном YouTube канале](https://youtube.com/@webdriverio). Не забудьте подписаться!

:::

## Инициализация настройки WebdriverIO

Чтобы добавить полную настройку WebdriverIO в существующий или новый проект с помощью [WebdriverIO Starter Toolkit](https://www.npmjs.com/package/create-wdio), выполните:

Если вы находитесь в корневом каталоге существующего проекта, выполните:

<Tabs
  defaultValue="npm"
  values={[
    {label: 'NPM', value: 'npm'},
    {label: 'Yarn', value: 'yarn'},
    {label: 'pnpm', value: 'pnpm'},
    {label: 'bun', value: 'bun'},
  ]
}>
<TabItem value="npm">

```sh
npm init wdio@latest .
```

или если вы хотите создать новый проект:

```sh
npm init wdio@latest ./path/to/new/project
```

</TabItem>
<TabItem value="yarn">

```sh
yarn create wdio .
```

или если вы хотите создать новый проект:

```sh
yarn create wdio ./path/to/new/project
```

</TabItem>
<TabItem value="pnpm">

```sh
pnpm create wdio@latest .
```

или если вы хотите создать новый проект:

```sh
pnpm create wdio@latest ./path/to/new/project
```

</TabItem>
<TabItem value="bun">

```sh
bun create wdio@latest .
```

или если вы хотите создать новый проект:

```sh
bun create wdio@latest ./path/to/new/project
```

</TabItem>
</Tabs>

Эта единственная команда загружает инструмент WebdriverIO CLI и запускает мастер конфигурации, который поможет вам настроить ваш набор тестов.

<CreateProjectAnimation />

Мастер задаст ряд вопросов, которые проведут вас через настройку. Вы можете передать параметр `--yes`, чтобы выбрать настройку по умолчанию, которая будет использовать Mocha с Chrome, используя паттерн [Page Object](https://martinfowler.com/bliki/PageObject.html).

<Tabs
  defaultValue="npm"
  values={[
    {label: 'NPM', value: 'npm'},
    {label: 'Yarn', value: 'yarn'},
    {label: 'pnpm', value: 'pnpm'},
    {label: 'bun', value: 'bun'},
  ]
}>
<TabItem value="npm">

```sh
npm init wdio@latest . -- --yes
```

</TabItem>
<TabItem value="yarn">

```sh
yarn create wdio . --yes
```

</TabItem>
<TabItem value="pnpm">

```sh
pnpm create wdio@latest . --yes
```

</TabItem>
<TabItem value="bun">

```sh
bun create wdio@latest . --yes
```

</TabItem>
</Tabs>

## Установка CLI вручную

Вы также можете добавить пакет CLI в ваш проект вручную с помощью:

```sh
npm i --save-dev @wdio/cli
npx wdio --version # выводит, например, `8.13.10`

# запустить мастер конфигурации
npx wdio config
```

## Запуск теста

Вы можете запустить свой набор тестов, используя команду `run` и указывая на конфигурацию WebdriverIO, которую вы только что создали:

```sh
npx wdio run ./wdio.conf.js
```

Если вы хотите запустить определенные тестовые файлы, вы можете добавить параметр `--spec`:

```sh
npx wdio run ./wdio.conf.js --spec example.e2e.js
```

или определить наборы в вашем конфигурационном файле и запустить только тестовые файлы, определенные в наборе:

```sh
npx wdio run ./wdio.conf.js --suite exampleSuiteName
```

## Запуск в скрипте

Если вы хотите использовать WebdriverIO как движок автоматизации в [Автономном режиме](/docs/setuptypes#standalone-mode) внутри скрипта Node.JS, вы также можете напрямую установить WebdriverIO и использовать его как пакет, например, для создания скриншота веб-сайта:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/fc362f2f8dd823d294b9bb5f92bd5991339d4591/getting-started/run-in-script.js#L2-L19
```

__Примечание:__ все команды WebdriverIO асинхронные и должны быть правильно обработаны с использованием [`async/await`](https://javascript.info/async-await).

## Запись тестов

WebdriverIO предоставляет инструменты, помогающие начать работу путем записи ваших тестовых действий на экране и автоматического создания тестовых скриптов WebdriverIO. Смотрите [Запись тестов с помощью Chrome DevTools Recorder](/docs/record) для получения дополнительной информации.

## Системные требования

Вам понадобится установленный [Node.js](http://nodejs.org).

- Установите версию не ниже v18.20.0, так как это самая старая активная версия LTS
- Официально поддерживаются только выпуски, которые являются или станут выпусками LTS

Если Node в настоящее время не установлен в вашей системе, мы предлагаем использовать такой инструмент, как [NVM](https://github.com/creationix/nvm) или [Volta](https://volta.sh/), чтобы помочь в управлении несколькими активными версиями Node.js. NVM - популярный выбор, в то время как Volta также является хорошей альтернативой.