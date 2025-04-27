---
id: bestpractices
title: Найкращі практики
---

# Найкращі практики

Цей посібник має на меті поділитися нашими найкращими практиками, які допоможуть вам писати ефективні та стійкі тести.

## Використовуйте стійкі селектори

Використовуючи селектори, які стійкі до змін у DOM, ви отримаєте менше або навіть взагалі не матимете тестів, що не працюють, коли, наприклад, клас видаляється з елемента.

Класи можуть застосовуватися до кількох елементів, і їх слід уникати, якщо це можливо, якщо ви навмисно не хочете отримати всі елементи з цим класом.

```js
// 👎
await $('.button')
```

Усі ці селектори повинні повертати один елемент.

```js
// 👍
await $('aria/Submit')
await $('[test-id="submit-button"]')
await $('#submit-button')
```

__Примітка:__ Щоб дізнатися про всі можливі селектори, які підтримує WebdriverIO, перегляньте нашу сторінку [Selectors](./Selectors.md).

## Обмежте кількість запитів до елементів

Кожного разу, коли ви використовуєте команду [`$`](https://webdriver.io/docs/api/browser/$) або [`$$`](https://webdriver.io/docs/api/browser/$$) (це включає їх ланцюжок), WebdriverIO намагається знайти елемент у DOM. Ці запити є дорогими, тому вам слід намагатися обмежити їх кількість.

Запитує три елементи.

```js
// 👎
await $('table').$('tr').$('td')
```

Запитує лише один елемент.

``` js
// 👍
await $('table tr td')
```

Єдиний випадок, коли слід використовувати ланцюжок, — це коли ви хочете поєднати різні [стратегії селекторів](https://webdriver.io/docs/selectors/#custom-selector-strategies).
У прикладі ми використовуємо [Deep Selectors](https://webdriver.io/docs/selectors#deep-selectors), це стратегія для входу в shadow DOM елемента.

``` js
// 👍
await $('custom-datepicker').$('#calendar').$('aria/Select')
```

### Віддавайте перевагу знаходженню одного елемента замість вибору одного зі списку

Не завжди можливо це зробити, але за допомогою CSS-псевдокласів, таких як [:nth-child](https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-child), ви можете зіставляти елементи на основі індексів елементів у дочірньому списку їхніх батьків.

Запитує всі рядки таблиці.

```js
// 👎
await $$('table tr')[15]
```

Запитує один рядок таблиці.

```js
// 👍
await $('table tr:nth-child(15)')
```

## Використовуйте вбудовані твердження

Не використовуйте ручні твердження, які не чекають автоматично, поки результати відповідатимуть, оскільки це призведе до нестабільних тестів.

```js
// 👎
expect(await button.isDisplayed()).toBe(true)
```

Використовуючи вбудовані твердження, WebdriverIO автоматично чекатиме, поки фактичний результат відповідатиме очікуваному, що призведе до стійких тестів.
Він досягає цього, автоматично повторюючи твердження, доки воно не пройде або не вичерпається час.

```js
// 👍
await expect(button).toBeDisplayed()
```

## Ліниве завантаження та ланцюжок обіцянок

WebdriverIO має деякі трюки для написання чистого коду, оскільки він може ліниво завантажувати елемент, що дозволяє створювати ланцюжки обіцянок і зменшує кількість `await`. Це також дозволяє передавати елемент як ChainablePromiseElement замість Element для легшого використання з об'єктами сторінок.

Отже, коли потрібно використовувати `await`?
Ви завжди повинні використовувати `await`, за винятком команд `$` та `$$`.

```js
// 👎
const div = await $('div')
const button = await div.$('button')
await button.click()
// або
await (await (await $('div')).$('button')).click()
```

```js
// 👍
const button = $('div').$('button')
await button.click()
// або
await $('div').$('button').click()
```

## Не зловживайте командами та твердженнями

Коли ви використовуєте expect.toBeDisplayed, ви неявно чекаєте, поки елемент існуватиме. Немає потреби використовувати команди waitForXXX, коли у вас вже є твердження, що робить те саме.

```js
// 👎
await button.waitForExist()
await expect(button).toBeDisplayed()

// 👎
await button.waitForDisplayed()
await expect(button).toBeDisplayed()

// 👍
await expect(button).toBeDisplayed()
```

Немає потреби чекати, поки елемент існує або відображається при взаємодії або при твердженні чогось, наприклад, його тексту, якщо елемент не може бути явно невидимим (opacity: 0, наприклад) або бути явно вимкненим (атрибут disabled, наприклад), у такому випадку чекання, поки елемент відображатиметься, має сенс.

```js
// 👎
await expect(button).toBeExisting()
await expect(button).toHaveText('Submit')

// 👎
await expect(button).toBeDisplayed()
await expect(button).toHaveText('Submit')

// 👎
await expect(button).toBeDisplayed()
await button.click()
```

```js
// 👍
await button.click()

// 👍
await expect(button).toHaveText('Submit')
```

## Динамічні тести

Використовуйте змінні середовища для зберігання динамічних тестових даних, наприклад, секретних облікових даних, у вашому середовищі замість жорсткого кодування їх у тесті. Перейдіть на сторінку [Parameterize Tests](parameterize-tests) для отримання додаткової інформації з цієї теми.

## Проводьте статичний аналіз коду

Використовуючи eslint для аналізу вашого коду, ви потенційно можете виявити помилки на ранніх етапах. Використовуйте наші [правила лінтингу](https://www.npmjs.com/package/eslint-plugin-wdio), щоб переконатися, що деякі з найкращих практик завжди застосовуються.

## Не використовуйте паузи

Може виникнути спокуса використати команду pause, але її використання — погана ідея, оскільки вона не є стійкою і в довгостроковій перспективі призведе лише до нестабільних тестів.

```js
// 👎
await nameInput.setValue('Bob')
await browser.pause(200) // wait for submit button to enable
await submitFormButton.click()

// 👍
await nameInput.setValue('Bob')
await submitFormButton.waitForEnabled()
await submitFormButton.click()
```

## Асинхронні цикли

Коли у вас є асинхронний код, який ви хочете повторити, важливо знати, що не всі цикли можуть це зробити.
Наприклад, функція forEach масивів не дозволяє асинхронні зворотні виклики, як можна прочитати на [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach).

__Примітка:__ Ви все ще можете використовувати їх, коли вам не потрібно, щоб операція була синхронною, як показано в цьому прикладі `console.log(await $$('h1').map((h1) => h1.getText()))`.

Нижче наведено кілька прикладів того, що це означає.

Наступне не працюватиме, оскільки асинхронні зворотні виклики не підтримуються.

```js
// 👎
const characters = 'this is some example text that should be put in order'
characters.forEach(async (character) => {
    await browser.keys(character)
})
```

Наступне працюватиме.

```js
// 👍
const characters = 'this is some example text that should be put in order'
for (const character of characters) {
    await browser.keys(character)
}
```

## Зберігайте простоту

Іноді ми бачимо, як наші користувачі відображають дані, такі як текст або значення. Це часто не потрібно і часто є ознакою поганого коду. Перевірте приклади нижче, чому це так.

```js
// 👎 занадто складно, синхронне твердження, використовуйте вбудовані твердження, щоб запобігти нестабільним тестам
const headerText = ['Products', 'Prices']
const texts = await $$('th').map(e => e.getText());
expect(texts).toBe(headerText)

// 👎 занадто складно
const headerText = ['Products', 'Prices']
const columns = await $$('th');
await expect(columns).toBeElementsArrayOfSize(2);
for (let i = 0; i < columns.length; i++) {
    await expect(columns[i]).toHaveText(headerText[i]);
}

// 👎 знаходить елементи за їхнім текстом, але не враховує положення елементів
await expect($('th=Products')).toExist();
await expect($('th=Prices')).toExist();
```

```js
// 👍 використовуйте унікальні ідентифікатори (часто використовуються для користувацьких елементів)
await expect($('[data-testid="Products"]')).toHaveText('Products');
// 👍 імена доступності (часто використовуються для власних HTML-елементів)
await expect($('aria/Product Prices')).toHaveText('Prices');
```

Інша річ, яку ми іноді бачимо, — це коли прості речі мають занадто складне рішення.

```js
// 👎
class BadExample {
    public async selectOptionByValue(value: string) {
        await $('select').click();
        await $$('option')
            .map(async function (element) {
                const hasValue = (await element.getValue()) === value;
                if (hasValue) {
                    await $(element).click();
                }
                return hasValue;
            });
    }

    public async selectOptionByText(text: string) {
        await $('select').click();
        await $$('option')
            .map(async function (element) {
                const hasText = (await element.getText()) === text;
                if (hasText) {
                    await $(element).click();
                }
                return hasText;
            });
    }
}
```

```js
// 👍
class BetterExample {
    public async selectOptionByValue(value: string) {
        await $('select').click();
        await $(`option[value=${value}]`).click();
    }

    public async selectOptionByText(text: string) {
        await $('select').click();
        await $(`option=${text}]`).click();
    }
}
```

## Виконання коду паралельно

Якщо вам не важливий порядок виконання деякого коду, ви можете використовувати [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all), щоб прискорити виконання.

__Примітка:__ Оскільки це ускладнює читання коду, ви можете абстрагуватися, використовуючи об'єкт сторінки або функцію, хоча вам також слід запитати, чи варта вигода в продуктивності витрат на читабельність.

```js
// 👎
await name.setValue('Bob')
await email.setValue('bob@webdriver.io')
await age.setValue('50')
await submitFormButton.waitForEnabled()
await submitFormButton.click()

// 👍
await Promise.all([
    name.setValue('Bob'),
    email.setValue('bob@webdriver.io'),
    age.setValue('50'),
])
await submitFormButton.waitForEnabled()
await submitFormButton.click()
```

Якщо це абстраговано, це може виглядати приблизно так, де логіка розміщена в методі під назвою submitWithDataOf, а дані отримуються класом Person.

```js
// 👍
await form.submitData(new Person('bob@webdriver.io'))
```