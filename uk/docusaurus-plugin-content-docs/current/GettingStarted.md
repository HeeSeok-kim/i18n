---
id: gettingstarted
title: Початок роботи
---

Ласкаво просимо до документації WebdriverIO. Вона допоможе вам швидко розпочати роботу. Якщо у вас виникнуть проблеми, ви можете знайти допомогу та відповіді на нашому [Discord сервері підтримки](https://discord.webdriver.io) або написати мені у [Twitter](https://twitter.com/webdriverio).

:::info
Це документація для останньої версії (__>=9.x__) WebdriverIO. Якщо ви все ще використовуєте старішу версію, відвідайте [старі веб-сайти документації](/versions)!
:::

<LiteYouTubeEmbed
    id="rA4IFNyW54c"
    title="Getting Started with WebdriverIO"
/>

:::tip Офіційний YouTube канал 🎥

Ви можете знайти більше відео про WebdriverIO на [офіційному YouTube каналі](https://youtube.com/@webdriverio). Не забудьте підписатися!

:::

## Ініціалізація налаштування WebdriverIO

Щоб додати повне налаштування WebdriverIO до існуючого або нового проекту за допомогою [WebdriverIO Starter Toolkit](https://www.npmjs.com/package/create-wdio), виконайте:

Якщо ви знаходитесь у кореневому каталозі існуючого проекту, виконайте:

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

або якщо ви хочете створити новий проект:

```sh
npm init wdio@latest ./path/to/new/project
```

</TabItem>
<TabItem value="yarn">

```sh
yarn create wdio .
```

або якщо ви хочете створити новий проект:

```sh
yarn create wdio ./path/to/new/project
```

</TabItem>
<TabItem value="pnpm">

```sh
pnpm create wdio@latest .
```

або якщо ви хочете створити новий проект:

```sh
pnpm create wdio@latest ./path/to/new/project
```

</TabItem>
<TabItem value="bun">

```sh
bun create wdio@latest .
```

або якщо ви хочете створити новий проект:

```sh
bun create wdio@latest ./path/to/new/project
```

</TabItem>
</Tabs>

Ця одна команда завантажує інструмент WebdriverIO CLI та запускає майстер конфігурації, який допомагає налаштувати ваш тестовий набір.

<CreateProjectAnimation />

Майстер запропонує набір запитань, які проведуть вас через налаштування. Ви можете передати параметр `--yes`, щоб вибрати налаштування за замовчуванням, яке використовуватиме Mocha з Chrome, використовуючи патерн [Page Object](https://martinfowler.com/bliki/PageObject.html).

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

## Встановлення CLI вручну

Ви також можете додати пакет CLI до свого проекту вручну через:

```sh
npm i --save-dev @wdio/cli
npx wdio --version # prints e.g. `8.13.10`

# run configuration wizard
npx wdio config
```

## Запуск тесту

Ви можете запустити свій тестовий набір, використовуючи команду `run` та вказавши на налаштування WebdriverIO, які ви щойно створили:

```sh
npx wdio run ./wdio.conf.js
```

Якщо ви хочете запустити певні тестові файли, ви можете додати параметр `--spec`:

```sh
npx wdio run ./wdio.conf.js --spec example.e2e.js
```

або визначити набори у вашому файлі конфігурації та запустити лише тестові файли, визначені у наборі:

```sh
npx wdio run ./wdio.conf.js --suite exampleSuiteName
```

## Запуск у скрипті

Якщо ви хочете використовувати WebdriverIO як двигун автоматизації в [Автономному режимі](/docs/setuptypes#standalone-mode) у скрипті Node.JS, ви також можете напряму встановити WebdriverIO та використовувати його як пакет, наприклад, для створення скріншоту веб-сайту:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/fc362f2f8dd823d294b9bb5f92bd5991339d4591/getting-started/run-in-script.js#L2-L19
```

__Примітка:__ всі команди WebdriverIO є асинхронними і потребують правильної обробки за допомогою [`async/await`](https://javascript.info/async-await).

## Запис тестів

WebdriverIO надає інструменти, які допоможуть вам почати роботу, записуючи ваші тестові дії на екрані та автоматично генеруючи тестові скрипти WebdriverIO. Дивіться [Запис тестів з Chrome DevTools Recorder](/docs/record) для отримання додаткової інформації.

## Системні вимоги

Вам потрібно встановити [Node.js](http://nodejs.org).

- Встановіть принаймні v18.20.0 або вище, оскільки це найстаріша активна версія LTS
- Офіційно підтримуються лише релізи, які є або стануть релізами LTS

Якщо Node зараз не встановлено у вашій системі, ми пропонуємо використовувати такий інструмент, як [NVM](https://github.com/creationix/nvm) або [Volta](https://volta.sh/), щоб допомогти керувати кількома активними версіями Node.js. NVM - популярний вибір, а Volta також є хорошою альтернативою.