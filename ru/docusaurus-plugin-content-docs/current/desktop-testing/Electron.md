---
id: electron
title: Electron
---

Electron — это фреймворк для создания настольных приложений с использованием JavaScript, HTML и CSS. Встраивая Chromium и Node.js в свой исполняемый файл, Electron позволяет поддерживать одну кодовую базу JavaScript и создавать кроссплатформенные приложения, работающие на Windows, macOS и Linux — опыт нативной разработки не требуется.

WebdriverIO предоставляет интегрированный сервис, который упрощает взаимодействие с вашим приложением Electron и делает его тестирование очень простым. Преимущества использования WebdriverIO для тестирования приложений Electron:

- 🚗 автоматическая настройка необходимого Chromedriver
- 📦 автоматическое определение пути к вашему приложению Electron - поддерживает [Electron Forge](https://www.electronforge.io/) и [Electron Builder](https://www.electron.build/)
- 🧩 доступ к API Electron из ваших тестов
- 🕵️ моккинг API Electron через API, похожий на Vitest

Вам нужно всего несколько простых шагов, чтобы начать. Посмотрите это простое пошаговое видео-руководство по началу работы с канала [WebdriverIO YouTube](https://www.youtube.com/@webdriverio):

<LiteYouTubeEmbed
    id="iQNxTdWedk0"
    title="Getting Started with ElectronJS Testing in WebdriverIO"
/>

Или следуйте руководству в следующем разделе.

## Начало работы

Чтобы инициировать новый проект WebdriverIO, выполните:

```sh
npm create wdio@latest ./
```

Мастер установки проведет вас через процесс. Убедитесь, что вы выбрали _"Desktop Testing - of Electron Applications"_, когда вас спросят, какой тип тестирования вы хотели бы выполнить. После этого укажите путь к вашему скомпилированному приложению Electron, например, `./dist`, затем просто оставьте значения по умолчанию или измените их в соответствии с вашими предпочтениями.

Мастер конфигурации установит все необходимые пакеты и создаст `wdio.conf.js` или `wdio.conf.ts` с необходимой конфигурацией для тестирования вашего приложения. Если вы согласитесь на автоматическую генерацию некоторых тестовых файлов, вы сможете запустить свой первый тест с помощью `npm run wdio`.

## Ручная настройка

Если вы уже используете WebdriverIO в вашем проекте, вы можете пропустить мастер установки и просто добавить следующие зависимости:

```sh
npm install --save-dev wdio-electron-service
```

Затем вы можете использовать следующую конфигурацию:

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

Вот и всё 🎉

Узнайте больше о том, [как настроить сервис Electron](/docs/desktop-testing/electron/configuration), [как имитировать API Electron](/docs/desktop-testing/electron/mocking) и [как получить доступ к API Electron](/docs/desktop-testing/electron/api).