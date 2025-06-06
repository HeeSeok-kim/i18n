---
id: assertion
title: ادعا (Assertion)
---

[تست‌رانر WDIO](https://webdriver.io/docs/clioptions) دارای یک کتابخانه ادعا داخلی است که به شما امکان می‌دهد ادعاهای قدرتمندی روی جنبه‌های مختلف مرورگر یا عناصر درون برنامه (وب) خود انجام دهید. این کتابخانه، عملکرد [Jests Matchers](https://jestjs.io/docs/en/using-matchers) را با تطبیق‌دهنده‌های اضافی که برای آزمایش e2e بهینه‌سازی شده‌اند، گسترش می‌دهد، به عنوان مثال:

```js
const $button = await $('button')
await expect($button).toBeDisplayed()
```

یا

```js
const selectOptions = await $$('form select>option')

// make sure there is at least one option in select
await expect(selectOptions).toHaveChildren({ gte: 1 })
```

برای لیست کامل، به [مستندات API expect](/docs/api/expect-webdriverio) مراجعه کنید.

## مهاجرت از Chai

[Chai](https://www.chaijs.com/) و [expect-webdriverio](https://github.com/webdriverio/expect-webdriverio#readme) می‌توانند همزیستی داشته باشند، و با برخی تنظیمات جزئی، انتقال روان به expect-webdriverio امکان‌پذیر است. اگر به WebdriverIO v6 ارتقا داده‌اید، به صورت پیش‌فرض به تمام ادعاهای `expect-webdriverio` دسترسی خواهید داشت. این بدان معناست که به صورت سراسری هر کجا که از `expect` استفاده می‌کنید، یک ادعای `expect-webdriverio` را فراخوانی می‌کنید. البته، مگر اینکه [`injectGlobals`](/docs/configuration#injectglobals) را روی `false` تنظیم کرده باشید یا به صراحت `expect` سراسری را برای استفاده از Chai بازنویسی کرده باشید. در این حالت، بدون وارد کردن صریح بسته expect-webdriverio در جایی که به آن نیاز دارید، به هیچ یک از ادعاهای expect-webdriverio دسترسی نخواهید داشت.

این راهنما نمونه‌هایی از نحوه مهاجرت از Chai را نشان می‌دهد، چه در حالتی که به صورت محلی بازنویسی شده باشد و چه در حالتی که به صورت سراسری بازنویسی شده باشد.

### محلی

فرض کنید Chai به صورت صریح در یک فایل وارد شده است، مثلاً:

```js
// myfile.js - original code
import { expect as expectChai } from 'chai'

describe('Homepage', () => {
    it('should assert', async () => {
        await browser.url('./')
        expectChai(await browser.getUrl()).to.include('/login')
    })
})
```

برای مهاجرت این کد، import مربوط به Chai را حذف کنید و به جای آن از متد جدید ادعای expect-webdriverio یعنی `toHaveUrl` استفاده کنید:

```js
// myfile.js - migrated code
describe('Homepage', () => {
    it('should assert', async () => {
        await browser.url('./')
        await expect(browser).toHaveUrl('/login') // new expect-webdriverio API method https://webdriver.io/docs/api/expect-webdriverio.html#tohaveurl
    });
});
```

اگر می‌خواهید از هر دو Chai و expect-webdriverio در یک فایل استفاده کنید، import مربوط به Chai را نگه دارید و `expect` به صورت پیش‌فرض به ادعای expect-webdriverio اشاره خواهد کرد، مثلاً:

```js
// myfile.js
import { expect as expectChai } from 'chai'
import { expect as expectWDIO } from '@wdio/globals'

describe('Element', () => {
    it('should be displayed', async () => {
        const isDisplayed = await $("#element").isDisplayed()
        expectChai(isDisplayed).to.equal(true); // Chai assertion
    })
});

describe('Other element', () => {
    it('should not be displayed', async () => {
        await expectWDIO($("#element")).not.toBeDisplayed(); // expect-webdriverio assertion
    })
})
```

### سراسری

فرض کنید `expect` به صورت سراسری برای استفاده از Chai بازنویسی شده است. برای استفاده از ادعاهای expect-webdriverio، باید یک متغیر را به صورت سراسری در هوک "before" تنظیم کنیم، مثلاً:

```js
// wdio.conf.js
before: async () => {
    await import('expect-webdriverio');
    global.wdioExpect = global.expect;
    const chai = await import('chai');
    global.expect = chai.expect;
}
```

اکنون می‌توان Chai و expect-webdriverio را در کنار یکدیگر استفاده کرد. در کد خود، ادعاهای Chai و expect-webdriverio را به صورت زیر استفاده می‌کنید:

```js
// myfile.js
describe('Element', () => {
    it('should be displayed', async () => {
        const isDisplayed = await $("#element").isDisplayed()
        expect(isDisplayed).to.equal(true); // Chai assertion
    });
});

describe('Other element', () => {
    it('should not be displayed', async () => {
        await expectWdio($("#element")).not.toBeDisplayed(); // expect-webdriverio assertion
    });
});
```

برای مهاجرت، به تدریج هر ادعای Chai را به expect-webdriverio منتقل کنید. هنگامی که تمام ادعاهای Chai در سراسر پایه کد جایگزین شدند، هوک "before" را می‌توان حذف کرد. یک جستجو و جایگزینی سراسری برای جایگزین کردن تمام نمونه‌های `wdioExpect` با `expect` مهاجرت را تکمیل خواهد کرد.