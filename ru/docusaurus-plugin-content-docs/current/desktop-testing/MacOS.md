---
id: macos
title: MacOS
---

WebdriverIO может автоматизировать произвольные приложения MacOS с помощью [Appium](https://appium.io/docs/en/2.0/). Всё, что вам нужно - это установленный в системе [XCode](https://developer.apple.com/xcode/), Appium и [Mac2 Driver](https://github.com/appium/appium-mac2-driver) в качестве зависимости, а также правильно настроенные capabilities.

## Начало работы

Чтобы инициировать новый проект WebdriverIO, выполните:

```sh
npm create wdio@latest ./
```

Мастер установки проведет вас через весь процесс. Убедитесь, что вы выбрали _"Desktop Testing - of MacOS Applications"_, когда вас спросят, какой тип тестирования вы хотели бы выполнить. После этого просто оставьте значения по умолчанию или измените их в соответствии с вашими предпочтениями.

Мастер настройки установит все необходимые пакеты Appium и создаст `wdio.conf.js` или `wdio.conf.ts` с необходимой конфигурацией для тестирования на MacOS. Если вы согласились на автоматическую генерацию тестовых файлов, вы можете запустить свой первый тест через `npm run wdio`.

<CreateMacOSProjectAnimation />

Вот и всё 🎉

## Пример

Вот как может выглядеть простой тест, который открывает приложение Калькулятор, выполняет вычисление и проверяет его результат:

```js
describe('My Login application', () => {
    it('should set a text to a text view', async function () {
        await $('//XCUIElementTypeButton[@label="seven"]').click()
        await $('//XCUIElementTypeButton[@label="multiply"]').click()
        await $('//XCUIElementTypeButton[@label="six"]').click()
        await $('//XCUIElementTypeButton[@title="="]').click()
        await expect($('//XCUIElementTypeStaticText[@label="main display"]')).toHaveText('42')
    });
})
```

__Примечание:__ приложение калькулятора было автоматически открыто в начале сессии, потому что `'appium:bundleId': 'com.apple.calculator'` был определен как опция capability. Вы можете переключаться между приложениями во время сессии в любое время.

## Дополнительная информация

Для получения информации о специфике тестирования на MacOS мы рекомендуем ознакомиться с проектом [Appium Mac2 Driver](https://github.com/appium/appium-mac2-driver).