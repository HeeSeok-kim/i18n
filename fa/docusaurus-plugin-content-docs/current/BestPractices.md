---
id: bestpractices
title: بهترین روش‌ها
---

# بهترین روش‌ها

این راهنما با هدف به اشتراک گذاری بهترین روش‌های ما برای نوشتن تست‌های کارآمد و انعطاف‌پذیر تهیه شده است.

## از انتخابگرهای انعطاف‌پذیر استفاده کنید

با استفاده از انتخابگرهایی که در برابر تغییرات DOM انعطاف‌پذیر هستند، هنگامی که مثلاً یک کلاس از یک المان حذف می‌شود، تست‌های کمتری یا حتی هیچ تستی شکست نخواهد خورد.

کلاس‌ها می‌توانند به چندین المان اعمال شوند و باید در صورت امکان از آن‌ها اجتناب کرد، مگر اینکه عمداً بخواهید تمام المان‌ها با آن کلاس را بیابید.

```js
// 👎
await $('.button')
```

تمام این انتخابگرها باید یک المان منفرد را بازگردانند.

```js
// 👍
await $('aria/Submit')
await $('[test-id="submit-button"]')
await $('#submit-button')
```

__نکته:__ برای آگاهی از تمام انتخابگرهایی که WebdriverIO پشتیبانی می‌کند، صفحه [Selectors](./Selectors.md) ما را بررسی کنید.

## تعداد پرس‌وجوهای المان را محدود کنید

هر بار که از دستورات [`$`](https://webdriver.io/docs/api/browser/$) یا [`$$`](https://webdriver.io/docs/api/browser/$$) استفاده می‌کنید (این شامل زنجیره‌سازی آن‌ها نیز می‌شود)، WebdriverIO سعی می‌کند المان را در DOM پیدا کند. این پرس‌وجوها هزینه‌بر هستند، بنابراین باید تلاش کنید تا حد ممکن آن‌ها را محدود کنید.

سه المان را پرس‌وجو می‌کند.

```js
// 👎
await $('table').$('tr').$('td')
```

فقط یک المان را پرس‌وجو می‌کند.

``` js
// 👍
await $('table tr td')
```

تنها زمانی باید از زنجیره‌سازی استفاده کنید که بخواهید استراتژی‌های مختلف [انتخابگر](https://webdriver.io/docs/selectors/#custom-selector-strategies) را ترکیب کنید.
در این مثال از [Deep Selectors](https://webdriver.io/docs/selectors#deep-selectors) استفاده می‌کنیم، که استراتژی‌ای برای رفتن به داخل shadow DOM یک المان است.

``` js
// 👍
await $('custom-datepicker').$('#calendar').$('aria/Select')
```

### ترجیح دهید به جای گرفتن یک المان از لیست، یک المان منفرد را پیدا کنید

همیشه این کار امکان‌پذیر نیست، اما با استفاده از کلاس‌های شبه CSS مانند [:nth-child](https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-child) می‌توانید المان‌ها را براساس شاخص‌های آن‌ها در لیست فرزندان والدینشان تطبیق دهید.

تمام ردیف‌های جدول را پرس‌وجو می‌کند.

```js
// 👎
await $$('table tr')[15]
```

یک ردیف جدول منفرد را پرس‌وجو می‌کند.

```js
// 👍
await $('table tr:nth-child(15)')
```

## از اثبات‌های (assertions) داخلی استفاده کنید

از اثبات‌های دستی که به طور خودکار منتظر نمی‌مانند تا نتایج مطابقت داشته باشند استفاده نکنید، زیرا این کار باعث ایجاد تست‌های ناپایدار می‌شود.

```js
// 👎
expect(await button.isDisplayed()).toBe(true)
```

با استفاده از اثبات‌های داخلی، WebdriverIO به طور خودکار منتظر می‌ماند تا نتیجه واقعی با نتیجه مورد انتظار مطابقت داشته باشد، که منجر به تست‌های انعطاف‌پذیر می‌شود.
این کار با تلاش مجدد خودکار اثبات تا زمانی که موفق شود یا زمان آن به پایان برسد، انجام می‌شود.

```js
// 👍
await expect(button).toBeDisplayed()
```

## بارگذاری تنبل (Lazy loading) و زنجیره‌سازی Promise

WebdriverIO ترفندهایی برای نوشتن کد تمیز دارد، از جمله بارگذاری تنبل المان که به شما امکان می‌دهد promise‌های خود را زنجیره کنید و تعداد `await` را کاهش دهید. این همچنین به شما امکان می‌دهد المان را به عنوان یک ChainablePromiseElement به جای یک Element منتقل کنید و برای استفاده آسان‌تر با page objects.

پس چه زمانی باید از `await` استفاده کنید؟
شما همیشه باید از `await` استفاده کنید، به استثنای دستورات `$` و `$$`.

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

## از دستورات و اثبات‌ها بیش از حد استفاده نکنید

هنگام استفاده از expect.toBeDisplayed، شما به طور ضمنی منتظر می‌مانید تا المان وجود داشته باشد. نیازی به استفاده از دستورات waitForXXX نیست وقتی که از قبل یک اثبات دارید که همین کار را انجام می‌دهد.

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

هنگام تعامل یا اثبات چیزی مانند متن، نیازی به انتظار برای وجود یا نمایش المان نیست، مگر اینکه المان به طور صریح نامرئی باشد (مثلاً opacity: 0) یا به طور صریح غیرفعال باشد (مثلاً صفت disabled) که در این صورت انتظار برای نمایش المان منطقی است.

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

از متغیرهای محیطی برای ذخیره داده‌های تست پویا مانند اعتبارنامه‌های محرمانه در محیط خود استفاده کنید، به جای اینکه آنها را در تست کدنویسی کنید. برای اطلاعات بیشتر در این مورد، به صفحه [Parameterize Tests](parameterize-tests) مراجعه کنید.

## کد خود را لینت کنید

با استفاده از eslint برای لینت کردن کدتان، می‌توانید خطاها را زودتر تشخیص دهید، از [قوانین لینت](https://www.npmjs.com/package/eslint-plugin-wdio) ما استفاده کنید تا مطمئن شوید که برخی از بهترین روش‌ها همیشه اعمال می‌شوند.

## از توقف استفاده نکنید

ممکن است استفاده از دستور pause وسوسه‌انگیز باشد، اما استفاده از آن ایده بدی است زیرا انعطاف‌پذیر نیست و در طولانی مدت فقط باعث تست‌های ناپایدار می‌شود.

```js
// 👎
await nameInput.setValue('Bob')
await browser.pause(200) // منتظر فعال شدن دکمه ارسال
await submitFormButton.click()

// 👍
await nameInput.setValue('Bob')
await submitFormButton.waitForEnabled()
await submitFormButton.click()
```

## حلقه‌های ناهمزمان

وقتی کدی ناهمزمان دارید که می‌خواهید تکرار کنید، مهم است بدانید که همه حلقه‌ها نمی‌توانند این کار را انجام دهند.
به عنوان مثال، تابع forEach آرایه اجازه callback‌های ناهمزمان را نمی‌دهد، همانطور که در [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) توضیح داده شده است.

__نکته:__ شما هنوز می‌توانید از اینها استفاده کنید زمانی که نیازی نیست عملیات همزمان باشد، مانند این مثال `console.log(await $$('h1').map((h1) => h1.getText()))`.

در زیر چند مثال از معنای این مطلب آورده شده است.

مورد زیر کار نمی‌کند زیرا callback‌های ناهمزمان پشتیبانی نمی‌شوند.

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

گاهی اوقات می‌بینیم که کاربران ما داده‌هایی مانند متن یا مقادیر را نگاشت می‌کنند. این اغلب ضروری نیست و معمولاً یک بوی کد است، مثال‌های زیر را بررسی کنید تا متوجه شوید چرا اینطور است.

```js
// 👎 خیلی پیچیده، اثبات همزمان، از اثبات‌های داخلی برای جلوگیری از تست‌های ناپایدار استفاده کنید
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

// 👎 المان‌ها را با متن‌شان پیدا می‌کند اما موقعیت المان‌ها را در نظر نمی‌گیرد
await expect($('th=Products')).toExist();
await expect($('th=Prices')).toExist();
```

```js
// 👍 از شناسه‌های منحصر به فرد استفاده کنید (اغلب برای المان‌های سفارشی استفاده می‌شود)
await expect($('[data-testid="Products"]')).toHaveText('Products');
// 👍 نام‌های دسترسی (اغلب برای المان‌های html بومی استفاده می‌شود)
await expect($('aria/Product Prices')).toHaveText('Prices');
```

چیز دیگری که گاهی می‌بینیم این است که چیزهای ساده راه حل‌های بیش از حد پیچیده‌ای دارند.

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

اگر به ترتیب اجرای کد اهمیت نمی‌دهید، می‌توانید از [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) برای تسریع اجرا استفاده کنید.

__نکته:__ از آنجا که این کار باعث خوانایی کمتر کد می‌شود، می‌توانید با استفاده از page object یا یک تابع، آن را مجرد کنید، اگرچه باید از خود بپرسید که آیا مزیت عملکرد ارزش هزینه خوانایی را دارد یا خیر.

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

اگر به صورت مجرد اجرا شود، می‌تواند چیزی شبیه به زیر باشد که در آن منطق در متدی به نام submitWithDataOf قرار گرفته و داده‌ها توسط کلاس Person بازیابی می‌شوند.

```js
// 👍
await form.submitData(new Person('bob@webdriver.io'))
```