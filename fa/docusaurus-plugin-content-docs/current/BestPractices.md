---
id: bestpractices
title: بهترین شیوه‌ها
---

# بهترین شیوه‌ها

این راهنما با هدف اشتراک‌گذاری بهترین شیوه‌های ما برای نوشتن تست‌های کارآمد و مقاوم تهیه شده است.

## از انتخابگرهای مقاوم استفاده کنید

با استفاده از انتخابگرهایی که در برابر تغییرات DOM مقاوم هستند، تست‌های شما کمتر یا حتی هیچ وقت شکست نمی‌خورند، مثلاً زمانی که یک کلاس از یک عنصر حذف می‌شود.

کلاس‌ها می‌توانند به چندین عنصر اعمال شوند و در صورت امکان باید از آن‌ها اجتناب کرد، مگر اینکه عمداً بخواهید تمام عناصر با آن کلاس را بازیابی کنید.

```js
// 👎
await $('.button')
```

همه این انتخابگرها باید یک عنصر واحد را برگردانند.

```js
// 👍
await $('aria/Submit')
await $('[test-id="submit-button"]')
await $('#submit-button')
```

__نکته:__ برای پیدا کردن تمام انتخابگرهای پشتیبانی شده توسط WebdriverIO، صفحه [انتخابگرها](./Selectors.md) ما را بررسی کنید.

## تعداد پرس‌وجوهای عنصر را محدود کنید

هر بار که از دستور [`$`](https://webdriver.io/docs/api/browser/$) یا [`$$`](https://webdriver.io/docs/api/browser/$$) استفاده می‌کنید (این شامل زنجیره‌سازی آن‌ها نیز می‌شود)، WebdriverIO سعی می‌کند عنصر را در DOM پیدا کند. این پرس‌وجوها پرهزینه هستند، بنابراین باید تا حد امکان آن‌ها را محدود کنید.

سه عنصر را پرس‌وجو می‌کند.

```js
// 👎
await $('table').$('tr').$('td')
```

فقط یک عنصر را پرس‌وجو می‌کند.

``` js
// 👍
await $('table tr td')
```

تنها زمانی که باید از زنجیره‌سازی استفاده کنید، وقتی است که می‌خواهید [استراتژی‌های انتخابگر](https://webdriver.io/docs/selectors/#custom-selector-strategies) مختلف را با هم ترکیب کنید.
در این مثال، ما از [انتخابگرهای عمیق](https://webdriver.io/docs/selectors#deep-selectors) استفاده می‌کنیم، که یک استراتژی برای ورود به shadow DOM یک عنصر است.

``` js
// 👍
await $('custom-datepicker').$('#calendar').$('aria/Select')
```

### ترجیح دهید به جای گرفتن یک عنصر از لیست، یک عنصر منفرد را پیدا کنید

همیشه انجام این کار ممکن نیست اما با استفاده از شبه کلاس‌های CSS مانند [:nth-child](https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-child) می‌توانید عناصر را بر اساس شاخص‌های آن‌ها در لیست فرزندان والدین‌شان تطبیق دهید.

تمام سطرهای جدول را پرس‌وجو می‌کند.

```js
// 👎
await $$('table tr')[15]
```

یک سطر جدول واحد را پرس‌وجو می‌کند.

```js
// 👍
await $('table tr:nth-child(15)')
```

## از تأییدکننده‌های داخلی استفاده کنید

از تأییدکننده‌های دستی که به طور خودکار منتظر تطابق نتایج نمی‌شوند استفاده نکنید، زیرا این کار باعث تست‌های ناپایدار می‌شود.

```js
// 👎
expect(await button.isDisplayed()).toBe(true)
```

با استفاده از تأییدکننده‌های داخلی، WebdriverIO به طور خودکار منتظر می‌ماند تا نتیجه واقعی با نتیجه مورد انتظار مطابقت پیدا کند، که منجر به تست‌های مقاوم می‌شود.
این کار را با تلاش مجدد خودکار تأییدکننده تا زمانی که موفق شود یا مهلت زمانی به پایان برسد، انجام می‌دهد.

```js
// 👍
await expect(button).toBeDisplayed()
```

## بارگذاری تنبل و زنجیره‌سازی پرامیس

WebdriverIO ترفندهایی برای نوشتن کد تمیز دارد، زیرا می‌تواند عنصر را به صورت تنبل بارگذاری کند که امکان زنجیره‌سازی پرامیس‌ها و کاهش استفاده از `await` را فراهم می‌کند. این همچنین به شما امکان می‌دهد عنصر را به عنوان ChainablePromiseElement به جای Element منتقل کنید و استفاده از Page Objects را ساده‌تر کنید.

پس چه زمانی باید از `await` استفاده کنید؟
شما همیشه باید از `await` استفاده کنید به استثنای دستورات `$` و `$$`.

```js
// 👎
const div = await $('div')
const button = await div.$('button')
await button.click()
// یا
await (await (await $('div')).$('button')).click()
```

```js
// 👍
const button = $('div').$('button')
await button.click()
// یا
await $('div').$('button').click()
```

## از دستورات و تأییدکننده‌ها بیش از حد استفاده نکنید

هنگام استفاده از expect.toBeDisplayed، شما به طور ضمنی منتظر وجود عنصر نیز می‌مانید. نیازی به استفاده از دستورات waitForXXX نیست وقتی که یک تأییدکننده را دارید که همان کار را انجام می‌دهد.

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

نیازی نیست منتظر وجود عنصر یا نمایش آن بمانید هنگام تعامل یا تأیید چیزی مانند متن آن، مگر اینکه عنصر به طور صریح می‌تواند نامرئی باشد (برای مثال opacity: 0) یا به طور صریح می‌تواند غیرفعال باشد (برای مثال ویژگی disabled) که در این صورت انتظار برای نمایش عنصر منطقی است.

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

## تست‌های پویا

از متغیرهای محیطی برای ذخیره داده‌های تست پویا مانند اطلاعات محرمانه استفاده کنید، به جای اینکه آن‌ها را در تست کدگذاری کنید. برای اطلاعات بیشتر درباره این موضوع به صفحه [پارامتری کردن تست‌ها](parameterize-tests) مراجعه کنید.

## کد خود را lint کنید

با استفاده از eslint برای lint کردن کد خود، می‌توانید خطاها را زودتر تشخیص دهید. از [قوانین lint ما](https://www.npmjs.com/package/eslint-plugin-wdio) استفاده کنید تا مطمئن شوید برخی از بهترین شیوه‌ها همیشه اعمال می‌شوند.

## از مکث استفاده نکنید

ممکن است استفاده از دستور مکث وسوسه‌انگیز باشد، اما استفاده از آن ایده بدی است زیرا مقاوم نیست و در دراز مدت فقط باعث تست‌های ناپایدار می‌شود.

```js
// 👎
await nameInput.setValue('Bob')
await browser.pause(200) // منتظر فعال شدن دکمه ثبت می‌ماند
await submitFormButton.click()

// 👍
await nameInput.setValue('Bob')
await submitFormButton.waitForEnabled()
await submitFormButton.click()
```

## حلقه‌های ناهمگام

وقتی کدی ناهمگام دارید که می‌خواهید تکرار کنید، مهم است بدانید که همه حلقه‌ها نمی‌توانند این کار را انجام دهند.
به عنوان مثال، تابع forEach آرایه اجازه عملیات ناهمگام را نمی‌دهد، همانطور که در [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) قابل مطالعه است.

__نکته:__ هنوز هم می‌توانید از اینها استفاده کنید وقتی نیازی به همگام بودن عملیات ندارید، مانند مثال نشان داده شده `console.log(await $$('h1').map((h1) => h1.getText()))`.

در زیر نمونه‌هایی از معنی این موضوع آمده است.

مورد زیر کار نخواهد کرد زیرا callback‌های ناهمگام پشتیبانی نمی‌شوند.

```js
// 👎
const characters = 'this is some example text that should be put in order'
characters.forEach(async (character) => {
    await browser.keys(character)
})
```

مورد زیر کار خواهد کرد.

```js
// 👍
const characters = 'this is some example text that should be put in order'
for (const character of characters) {
    await browser.keys(character)
}
```

## ساده نگه دارید

گاهی اوقات می‌بینیم که کاربران ما داده‌هایی مانند متن یا مقادیر را نگاشت می‌کنند. این کار اغلب ضروری نیست و معمولاً نشانه‌ای از کد ضعیف است. مثال‌های زیر را ببینید که چرا این مورد صدق می‌کند.

```js
// 👎 خیلی پیچیده، تأیید همگام، از تأییدکننده‌های داخلی برای جلوگیری از تست‌های ناپایدار استفاده کنید
const headerText = ['Products', 'Prices']
const texts = await $$('th').map(e => e.getText());
expect(texts).toBe(headerText)

// 👎 خیلی پیچیده
const headerText = ['Products', 'Prices']
const columns = await $$('th');
await expect(columns).toBeElementsArrayOfSize(2);
for (let i = 0; i < columns.length; i++) {
    await expect(columns[i]).toHaveText(headerText[i]);
}

// 👎 عناصر را با متن آنها پیدا می‌کند اما موقعیت عناصر را در نظر نمی‌گیرد
await expect($('th=Products')).toExist();
await expect($('th=Prices')).toExist();
```

```js
// 👍 از شناسه‌های منحصر به فرد استفاده کنید (اغلب برای عناصر سفارشی استفاده می‌شود)
await expect($('[data-testid="Products"]')).toHaveText('Products');
// 👍 نام‌های دسترسی‌پذیری (اغلب برای عناصر HTML بومی استفاده می‌شود)
await expect($('aria/Product Prices')).toHaveText('Prices');
```

موضوع دیگری که گاهی می‌بینیم این است که چیزهای ساده راه حل‌های بیش از حد پیچیده‌ای دارند.

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

## اجرای کد به صورت موازی

اگر به ترتیب اجرای برخی کدها اهمیت نمی‌دهید، می‌توانید از [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) برای افزایش سرعت اجرا استفاده کنید.

__نکته:__ از آنجا که این کار باعث سخت‌تر شدن خواندن کد می‌شود، می‌توانید آن را با استفاده از Page Object یا یک تابع انتزاعی کنید، اگرچه باید این سؤال را نیز مطرح کنید که آیا مزیت عملکرد ارزش هزینه خوانایی را دارد یا خیر.

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

اگر به صورت انتزاعی باشد، می‌تواند چیزی شبیه به زیر باشد که در آن منطق در متدی به نام submitWithDataOf قرار گرفته و داده‌ها توسط کلاس Person بازیابی می‌شوند.

```js
// 👍
await form.submitData(new Person('bob@webdriver.io'))
```