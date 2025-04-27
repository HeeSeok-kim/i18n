---
id: electron
title: Electron
description: Тестування застосунків Electron за допомогою WebdriverIO
---

Electron - це фреймворк для створення настільних додатків за допомогою JavaScript, HTML і CSS. Вбудовуючи Chromium і Node.js у свій бінарний файл, Electron дозволяє підтримувати одну кодову базу JavaScript і створювати кросплатформні додатки, які працюють на Windows, macOS і Linux — без необхідності володіння навичками нативної розробки.

WebdriverIO надає інтегрований сервіс, який спрощує взаємодію з вашим Electron додатком і робить його тестування дуже простим. Переваги використання WebdriverIO для тестування Electron додатків:

- 🚗 автоматичне налаштування необхідного Chromedriver
- 📦 автоматичне виявлення шляху до вашого Electron додатку - підтримує [Electron Forge](https://www.electronforge.io/) та [Electron Builder](https://www.electron.build/)
- 🧩 доступ до Electron API у ваших тестах
- 🕵️ мокінг Electron API через API, подібний до Vitest

Вам потрібно лише кілька простих кроків, щоб почати. Подивіться просте покрокове відео для початківців з каналу [WebdriverIO YouTube](https://www.youtube.com/@webdriverio):

<LiteYouTubeEmbed
    id="iQNxTdWedk0"
    title="Getting Started with ElectronJS Testing in WebdriverIO"
/>

Або слідуйте інструкціям у наступному розділі.

## Початок роботи

Щоб ініціювати новий проект WebdriverIO, виконайте:

```sh
npm create wdio@latest ./
```

Майстер встановлення проведе вас через цей процес. Переконайтеся, що ви вибрали _"Desktop Testing - of Electron Applications"_, коли вас запитають, який тип тестування ви хочете виконувати. Після цього вкажіть шлях до вашого скомпільованого Electron додатку, наприклад, `./dist`, потім просто залиште налаштування за замовчуванням або змініть відповідно до ваших уподобань.

Майстер налаштування встановить усі необхідні пакети та створить `wdio.conf.js` або `wdio.conf.ts` з необхідною конфігурацією для тестування вашого додатку. Якщо ви погодитеся на автоматичне створення тестових файлів, ви зможете запустити свій перший тест за допомогою `npm run wdio`.

## Ручне налаштування

Якщо ви вже використовуєте WebdriverIO у своєму проекті, ви можете пропустити майстер встановлення і просто додати наступні залежності:

```sh
npm install --save-dev wdio-electron-service
```

Потім ви можете використовувати наступну конфігурацію:

```ts
// wdio.conf.ts
export const config: WebdriverIO.Config = {
    // ...
    services: [['electron', {
        appEntryPoint: './path/to/bundled/electron/main.bundle.js',
        appArgs: [/** ... */],
    }]]
}
```

Ось і все 🎉

Дізнайтеся більше про те, [як налаштувати Electron Service](/docs/desktop-testing/electron/configuration), [як мокати Electron API](/docs/desktop-testing/electron/mocking) і [як отримати доступ до Electron API](/docs/desktop-testing/electron/api).