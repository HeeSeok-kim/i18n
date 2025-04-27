---
id: macos
title: MacOS
---

WebdriverIO може автоматизувати будь-які застосунки MacOS за допомогою [Appium](https://appium.io/docs/en/2.0/). Все, що вам потрібно - це встановлений [XCode](https://developer.apple.com/xcode/) у вашій системі, Appium та [Mac2 Driver](https://github.com/appium/appium-mac2-driver) встановлені як залежності, і правильно налаштовані можливості.

## Початок роботи

Щоб розпочати новий проєкт WebdriverIO, виконайте:

```sh
npm create wdio@latest ./
```

Майстер установки проведе вас через цей процес. Переконайтеся, що ви вибрали _"Desktop Testing - of MacOS Applications"_, коли вас запитають, який тип тестування ви хочете виконувати. Після цього просто залиште значення за замовчуванням або змініть їх за вашими уподобаннями.

Майстер конфігурації встановить усі необхідні пакети Appium і створить `wdio.conf.js` або `wdio.conf.ts` з необхідною конфігурацією для тестування на MacOS. Якщо ви погодилися автоматично згенерувати деякі тестові файли, ви можете запустити ваш перший тест за допомогою `npm run wdio`.

<CreateMacOSProjectAnimation />

Це все 🎉

## Приклад

Ось як може виглядати простий тест, який відкриває застосунок Калькулятор, виконує обчислення та перевіряє результат:

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

__Примітка:__ застосунок калькулятора автоматично відкривається на початку сесії, оскільки `'appium:bundleId': 'com.apple.calculator'` визначено як опцію можливостей. Ви можете перемикатися між застосунками під час сесії будь-коли.

## Додаткова інформація

Для отримання інформації про особливості тестування на MacOS ми рекомендуємо переглянути проєкт [Appium Mac2 Driver](https://github.com/appium/appium-mac2-driver).