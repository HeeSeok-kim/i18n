---
id: mocksandspies
title: موک‌ها و جاسوس‌های درخواست
---

WebdriverIO با پشتیبانی داخلی برای تغییر پاسخ‌های شبکه عرضه می‌شود که به شما امکان می‌دهد بدون نیاز به راه‌اندازی بک‌اند یا سرور موک، بر روی تست اپلیکیشن فرانت‌اند خود تمرکز کنید. شما می‌توانید پاسخ‌های سفارشی برای منابع وب مانند درخواست‌های REST API در تست خود تعریف کرده و به صورت پویا آن‌ها را تغییر دهید.

:::info

توجه داشته باشید که استفاده از دستور `mock` نیاز به پشتیبانی از پروتکل Chrome DevTools دارد. این پشتیبانی زمانی فراهم می‌شود که تست‌ها را به صورت محلی در یک مرورگر مبتنی بر Chromium، از طریق Selenium Grid نسخه ۴ یا بالاتر، یا از طریق یک ارائه‌دهنده کلود با پشتیبانی از پروتکل Chrome DevTools (مانند SauceLabs، BrowserStack، LambdaTest) اجرا کنید. پشتیبانی کامل از تمام مرورگرها زمانی در دسترس خواهد بود که ویژگی‌های اولیه مورد نیاز در [Webdriver Bidi](https://wpt.fyi/results/webdriver/tests/bidi/network?label=experimental&label=master&aligned) پیاده‌سازی شوند و در مرورگرهای مربوطه اجرا شوند.

:::

## ایجاد یک موک

قبل از اینکه بتوانید پاسخ‌ها را تغییر دهید، باید ابتدا یک موک تعریف کنید. این موک با URL منبع توصیف می‌شود و می‌تواند بر اساس [روش درخواست](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) یا [هدرها](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers) فیلتر شود. منبع از عبارات glob توسط [minimatch](https://www.npmjs.com/package/minimatch) پشتیبانی می‌کند:

```js
// موک کردن تمام منابعی که با "/users/list" تمام می‌شوند
const userListMock = await browser.mock('**/users/list')

// یا می‌توانید موک را با فیلتر کردن منابع توسط هدرها یا
// کد وضعیت مشخص کنید، فقط درخواست‌های موفق به منابع json را موک کنید
const strictMock = await browser.mock('**', {
    // موک کردن تمام پاسخ‌های json
    requestHeaders: { 'Content-Type': 'application/json' },
    // که موفق بوده‌اند
    statusCode: 200
})
```

## تعیین پاسخ‌های سفارشی

پس از تعریف یک موک، می‌توانید پاسخ‌های سفارشی برای آن تعریف کنید. این پاسخ‌های سفارشی می‌توانند یک شیء برای پاسخ JSON، یک فایل محلی برای پاسخ با یک فیکسچر سفارشی یا یک منبع وب برای جایگزینی پاسخ با منبعی از اینترنت باشند.

### موک کردن درخواست‌های API

برای موک کردن درخواست‌های API که انتظار پاسخ JSON دارید، تنها کاری که باید انجام دهید فراخوانی `respond` روی شیء موک با یک شیء دلخواه است که می‌خواهید برگردانید، مثلاً:

```js
const mock = await browser.mock('https://todo-backend-express-knex.herokuapp.com/')

mock.respond([{
    title: 'Injected (non) completed Todo',
    order: null,
    completed: false
}, {
    title: 'Injected completed Todo',
    order: null,
    completed: true
}], {
    headers: {
        'Access-Control-Allow-Origin': '*'
    },
    fetchResponse: false
})

await browser.url('https://todobackend.com/client/index.html?https://todo-backend-express-knex.herokuapp.com/')

await $('#todo-list li').waitForExist()
console.log(await $$('#todo-list li').map(el => el.getText()))
// خروجی: "[ 'Injected (non) completed Todo', 'Injected completed Todo' ]"
```

همچنین می‌توانید هدرهای پاسخ و همچنین کد وضعیت را با ارسال برخی پارامترهای پاسخ موک به شرح زیر تغییر دهید:

```js
mock.respond({ ... }, {
    // پاسخ با کد وضعیت 404
    statusCode: 404,
    // ادغام هدرهای پاسخ با هدرهای زیر
    headers: { 'x-custom-header': 'foobar' }
})
```

اگر می‌خواهید موک اصلاً با بک‌اند تماس نگیرد، می‌توانید `false` را برای پرچم `fetchResponse` ارسال کنید.

```js
mock.respond({ ... }, {
    // با بک‌اند واقعی تماس نگیر
    fetchResponse: false
})
```

توصیه می‌شود پاسخ‌های سفارشی را در فایل‌های فیکسچر ذخیره کنید تا بتوانید آن‌ها را به راحتی در تست خود import کنید:

```js
// نیاز به Node.js نسخه v16.14.0 یا بالاتر برای پشتیبانی از import assertion های JSON
import responseFixture from './__fixtures__/apiResponse.json' assert { type: 'json' }
mock.respond(responseFixture)
```

### موک کردن منابع متنی

اگر می‌خواهید منابع متنی مانند فایل‌های JavaScript، CSS یا سایر منابع متنی را تغییر دهید، می‌توانید یک مسیر فایل وارد کنید و WebdriverIO منبع اصلی را با آن جایگزین می‌کند، مثلاً:

```js
const scriptMock = await browser.mock('**/script.min.js')
scriptMock.respond('./tests/fixtures/script.js')

// یا با JS سفارشی خود پاسخ دهید
scriptMock.respond('alert("I am a mocked resource")')
```

### تغییر مسیر منابع وب

همچنین می‌توانید یک منبع وب را با منبع وب دیگری جایگزین کنید اگر پاسخ مورد نظر شما قبلاً در وب میزبانی شده باشد. این کار با منابع صفحه فردی و همچنین با خود صفحه وب کار می‌کند، مثلاً:

```js
const pageMock = await browser.mock('https://google.com/')
await pageMock.respond('https://webdriver.io')
await browser.url('https://google.com')
console.log(await browser.getTitle()) // برمی‌گرداند "WebdriverIO · Next-gen browser and mobile automation test framework for Node.js"
```

### پاسخ‌های پویا

اگر پاسخ موک شما به پاسخ منبع اصلی بستگی دارد، می‌توانید منبع را به صورت پویا با ارسال یک تابع که پاسخ اصلی را به عنوان پارامتر دریافت می‌کند و موک را بر اساس مقدار برگشتی تنظیم می‌کند، تغییر دهید، مثلاً:

```js
const mock = await browser.mock('https://todo-backend-express-knex.herokuapp.com/', {
    method: 'get'
})

mock.respond((req) => {
    // جایگزینی محتوای todo با شماره لیست آن‌ها
    return req.body.map((item, i) => ({ ...item, title: i }))
})

await browser.url('https://todobackend.com/client/index.html?https://todo-backend-express-knex.herokuapp.com/')

await $('#todo-list li').waitForExist()
console.log(await $$('#todo-list li label').map((el) => el.getText()))
// برمی‌گرداند
// [
//   '0',  '1',  '2',  '19', '20',
//   '21', '3',  '4',  '5',  '6',
//   '7',  '8',  '9',  '10', '11',
//   '12', '13', '14', '15', '16',
//   '17', '18', '22'
// ]
```

## لغو موک‌ها

به جای بازگرداندن یک پاسخ سفارشی، می‌توانید درخواست را با یکی از خطاهای HTTP زیر لغو کنید:

- Failed
- Aborted
- TimedOut
- AccessDenied
- ConnectionClosed
- ConnectionReset
- ConnectionRefused
- ConnectionAborted
- ConnectionFailed
- NameNotResolved
- InternetDisconnected
- AddressUnreachable
- BlockedByClient
- BlockedByResponse

این کار زمانی بسیار مفید است که می‌خواهید اسکریپت‌های شخص ثالث را از صفحه خود مسدود کنید که تأثیر منفی بر تست کارکردی شما دارند. می‌توانید یک موک را با صدا زدن `abort` یا `abortOnce` لغو کنید، مثلاً:

```js
const mock = await browser.mock('https://www.google-analytics.com/**')
mock.abort('Failed')
```

## جاسوس‌ها

هر موک به طور خودکار یک جاسوس است که تعداد درخواست‌هایی را که مرورگر به آن منبع انجام داده است، می‌شمارد. اگر پاسخ سفارشی یا دلیل لغو را به موک اعمال نکنید، با پاسخ پیش‌فرضی که معمولاً دریافت می‌کنید ادامه می‌دهد. این به شما امکان می‌دهد بررسی کنید چند بار مرورگر درخواست را انجام داده است، مثلاً به یک نقطه پایانی API خاص.

```js
const mock = await browser.mock('**/user', { method: 'post' })
console.log(mock.calls.length) // برمی‌گرداند 0

// ثبت‌نام کاربر
await $('#username').setValue('randomUser')
await $('password').setValue('password123')
await $('password_repeat').setValue('password123')
await $('button[type="submit"]').click()

// بررسی اینکه آیا درخواست API انجام شده است
expect(mock.calls.length).toBe(1)

// بررسی پاسخ
expect(mock.calls[0].body).toEqual({ success: true })
```