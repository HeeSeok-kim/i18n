---
id: wdio-intercept-service
title: سرویس رهگیری
custom_edit_url: https://github.com/webdriverio-community/wdio-intercept-service/edit/main/README.md
---


> wdio-intercept-service یک پکیج شخص ثالث است، برای اطلاعات بیشتر لطفا به [GitHub](https://github.com/webdriverio-community/wdio-intercept-service) | [npm](https://www.npmjs.com/package/wdio-intercept-service) مراجعه کنید

🕸 رهگیری و بررسی فراخوانی‌های HTTP ajax در [webdriver.io](http://webdriver.io/)

[![Tests](https://github.com/webdriverio-community/wdio-intercept-service/actions/workflows/test.yaml/badge.svg)](https://github.com/webdriverio-community/wdio-intercept-service/actions/workflows/test.yaml) [![Join the chat on Discord](https://img.shields.io/discord/1097401827202445382?logo=discord&logoColor=FFFFFF&color=5865F2)](https://discord.webdriver.io/)

این یک افزونه برای [webdriver.io](http://webdriver.io/) است. اگر هنوز با آن آشنا نیستید، آن را بررسی کنید، بسیار جالب است.

اگرچه از selenium و webdriver برای تست‌های e2e و به ویژه تست‌های رابط کاربری استفاده می‌شود، ممکن است بخواهید درخواست‌های HTTP انجام شده توسط کد سمت کاربر خود را ارزیابی کنید (به عنوان مثال وقتی بازخورد فوری رابط کاربری ندارید، مانند در فراخوانی‌های متریک یا ردیابی). با wdio-intercept-service می‌توانید فراخوانی‌های HTTP ajax که توسط برخی اقدامات کاربر (مثلاً فشار دکمه و غیره) آغاز می‌شوند را رهگیری کنید و بعداً درباره درخواست و پاسخ‌های مربوطه تأیید کنید.

البته یک محدودیت وجود دارد: شما نمی‌توانید فراخوانی‌های HTTP که هنگام بارگذاری صفحه آغاز می‌شوند (مانند اکثر SPA‌ها) را رهگیری کنید، زیرا نیاز به برخی کارهای راه‌اندازی دارد که فقط پس از بارگذاری صفحه می‌تواند انجام شود (به دلیل محدودیت‌های selenium). **این بدان معناست که شما فقط می‌توانید درخواست‌هایی را ثبت کنید که در داخل یک تست آغاز شده‌اند.** اگر با این محدودیت مشکلی ندارید، این افزونه ممکن است مناسب شما باشد، پس ادامه مطلب را بخوانید.

## پیش‌نیازها

* webdriver.io **v5.x** یا جدیدتر.

**توجه! اگر هنوز از webdriver.io v4 استفاده می‌کنید، لطفاً از شاخه v2.x این افزونه استفاده کنید!**

## نصب

```shell
npm install wdio-intercept-service -D
```

## استفاده

### استفاده با WebDriver CLI

باید به سادگی اضافه کردن wdio-intercept-service به `wdio.conf.js` شما باشد:

```javascript
exports.config = {
  // ...
  services: ['intercept']
  // ...
};
```

و همه چیز آماده است.

### استفاده با WebDriver Standalone

هنگام استفاده از WebdriverIO Standalone، توابع `before` و `beforeTest` / `beforeScenario` باید به صورت دستی فراخوانی شوند.

```javascript
import { remote } from 'webdriverio';
import WebdriverAjax from 'wdio-intercept-service'

const WDIO_OPTIONS = {
  port: 9515,
  path: '/',
  capabilities: {
    browserName: 'chrome'
  },
}

let browser;
const interceptServiceLauncher = WebdriverAjax();

beforeAll(async () => {
  browser = await remote(WDIO_OPTIONS)
  interceptServiceLauncher.before(null, null, browser)
})

beforeEach(async () => {
  interceptServiceLauncher.beforeTest()
})

afterAll(async () => {
  await client.deleteSession()
});

describe('', async () => {
  ... // See example usage
});
```

پس از راه‌اندازی، برخی توابع مرتبط به زنجیره دستورات مرورگر شما اضافه می‌شوند (به [API](#api) مراجعه کنید).

## شروع سریع

مثال استفاده:

```javascript
browser.url('http://foo.bar');
browser.setupInterceptor(); // capture ajax calls
browser.expectRequest('GET', '/api/foo', 200); // expect GET request to /api/foo with 200 statusCode
browser.expectRequest('POST', '/api/foo', 400); // expect POST request to /api/foo with 400 statusCode
browser.expectRequest('GET', /\/api\/foo/, 200); // can validate a URL with regex, too
browser.click('#button'); // button that initiates ajax request
browser.pause(1000); // maybe wait a bit until request is finished
browser.assertRequests(); // validate the requests
```

دریافت جزئیات درخواست‌ها:

```javascript
browser.url('http://foo.bar')
browser.setupInterceptor();
browser.click('#button')
browser.pause(1000);

var request = browser.getRequest(0);
assert.equal(request.method, 'GET');
assert.equal(request.response.headers['content-length'], '42');
```

## مرورگرهای پشتیبانی شده

باید با نسخه‌های نسبتاً جدیدتر همه مرورگرها کار کند. لطفاً اگر با مرورگر شما کار نمی‌کند، یک مسئله گزارش دهید.

## API

برای نحو کامل دستورات سفارشی اضافه شده به شیء مرورگر WebdriverIO، فایل اعلان TypeScript را مشاهده کنید. به طور کلی، هر متدی که یک شیء "options" را به عنوان پارامتر می‌پذیرد، می‌تواند بدون آن پارامتر فراخوانی شود تا رفتار پیش‌فرض را بدست آورد. این اشیاء "options اختیاری" با `?: = {}` دنبال می‌شوند و مقادیر پیش‌فرض استنباط شده برای هر متد توضیح داده شده است.

### توضیحات گزینه‌ها

این کتابخانه مقدار کمی پیکربندی هنگام صدور دستورات ارائه می‌دهد. گزینه‌های پیکربندی که توسط چندین متد استفاده می‌شوند در اینجا توضیح داده شده‌اند (برای تعیین پشتیبانی خاص به تعریف هر متد مراجعه کنید).

* `orderBy` (`'START' | 'END'`): این گزینه ترتیب درخواست‌های ثبت شده توسط رهگیر را هنگام بازگشت به تست شما کنترل می‌کند. برای سازگاری با نسخه‌های موجود این کتابخانه، ترتیب پیش‌فرض `'END'` است که مطابق با زمان تکمیل درخواست است. اگر گزینه `orderBy` را بر روی `'START'` تنظیم کنید، درخواست‌ها بر اساس زمان شروع مرتب می‌شوند.
* `includePending` (`boolean`): این گزینه کنترل می‌کند که آیا درخواست‌های هنوز تکمیل نشده برگردانده می‌شوند یا خیر. برای سازگاری با نسخه‌های موجود این کتابخانه، مقدار پیش‌فرض `false` است و فقط درخواست‌های تکمیل شده برگردانده می‌شوند.

### browser.setupInterceptor()

فراخوانی‌های ajax را در مرورگر ثبت می‌کند. همیشه باید تابع راه‌اندازی را فراخوانی کنید تا بتوانید بعداً درخواست‌ها را ارزیابی کنید.

### browser.disableInterceptor()

از ثبت بیشتر فراخوانی‌های ajax در مرورگر جلوگیری می‌کند. تمام اطلاعات درخواست ثبت شده حذف می‌شوند. اکثر کاربران نیازی به غیرفعال کردن رهگیر ندارند، اما اگر یک تست به طور خاص طولانی مدت است یا از ظرفیت ذخیره‌سازی نشست فراتر می‌رود، غیرفعال کردن رهگیر می‌تواند مفید باشد.

### `browser.excludeUrls(urlRegexes: (string | RegExp)[])`

درخواست‌های از URLهای خاص را از ثبت شدن مستثنی می‌کند. یک آرایه از رشته‌ها یا عبارات منظم می‌گیرد. قبل از نوشتن در حافظه،
URL درخواست را در برابر هر رشته یا عبارت منظم آزمایش می‌کند. اگر مطابقت داشته باشد، درخواست در حافظه نوشته نمی‌شود. مانند disableInterceptor، این می‌تواند
اگر با مشکلات مربوط به بیش از حد شدن ظرفیت ذخیره‌سازی نشست مواجه هستید، مفید باشد.

### browser.expectRequest(method: string, url: string, statusCode: number)

انتظاراتی درباره درخواست‌های ajax که قرار است در طول تست آغاز شوند، ایجاد می‌کند. می‌تواند (و باید) زنجیره‌ای شود. ترتیب انتظارات باید با ترتیب درخواست‌هایی که انجام می‌شوند مطابقت داشته باشد.

* `method` (`String`): متد http که انتظار می‌رود. می‌تواند هر چیزی باشد که `xhr.open()` به عنوان اولین آرگومان می‌پذیرد.
* `url` (`String`|`RegExp`): URL دقیقی که در درخواست فراخوانی می‌شود به صورت رشته یا RegExp برای تطبیق
* `statusCode` (`Number`): کد وضعیت مورد انتظار از پاسخ

### browser.getExpectations()

متد کمکی. تمام انتظاراتی که تا آن نقطه ایجاد کرده‌اید را برمی‌گرداند

### browser.resetExpectations()

متد کمکی. تمام انتظاراتی که تا آن نقطه ایجاد کرده‌اید را بازنشانی می‌کند

### `browser.assertRequests({ orderBy?: 'START' | 'END' }?: = {})`

این متد را زمانی فراخوانی کنید که تمام درخواست‌های ajax مورد انتظار به پایان رسیده باشند. انتظارات را با درخواست‌های واقعی انجام شده مقایسه می‌کند و موارد زیر را تأیید می‌کند:

- تعداد درخواست‌هایی که انجام شده‌اند
- ترتیب درخواست‌ها
- روش، URL و کد وضعیت باید برای هر درخواست انجام شده مطابقت داشته باشند
- شیء گزینه‌ها به طور پیش‌فرض `{ orderBy: 'END' }` است، یعنی زمانی که درخواست‌ها تکمیل شدند، تا با رفتار نسخه v4.1.10 و قبل‌تر سازگار باشد. هنگامی که گزینه `orderBy` روی `'START'` تنظیم شده باشد، درخواست‌ها بر اساس زمانی که توسط صفحه آغاز شده‌اند، مرتب می‌شوند.

### `browser.assertExpectedRequestsOnly({ inOrder?: boolean, orderBy?: 'START' | 'END' }?: = {})`

مشابه `browser.assertRequests`، اما فقط درخواست‌هایی را که در دستورات `expectRequest` خود مشخص کرده‌اید را تأیید می‌کند، بدون اینکه مجبور باشید تمام درخواست‌های شبکه که ممکن است در اطراف آن رخ دهد را ترسیم کنید. اگر گزینه `inOrder` برابر با `true` باشد (پیش‌فرض)، انتظار می‌رود درخواست‌ها به همان ترتیبی که با `expectRequest` راه‌اندازی شده‌اند، یافت شوند.

### `browser.getRequest(index: number, { includePending?: boolean, orderBy?: 'START' | 'END' }?: = {})`

برای ایجاد تأییدهای پیچیده‌تر درباره یک درخواست خاص، می‌توانید جزئیات را برای یک درخواست خاص دریافت کنید. باید شاخص مبتنی بر 0 درخواستی را که می‌خواهید به آن دسترسی پیدا کنید، به ترتیبی که درخواست‌ها تکمیل شده‌اند (پیش‌فرض)، یا آغاز شده‌اند (با گذراندن گزینه `orderBy: 'START'`) ارائه دهید.

* `index` (`number`): شماره درخواستی که می‌خواهید به آن دسترسی پیدا کنید
* `options` (`object`): گزینه‌های پیکربندی
* `options.includePending` (`boolean`): آیا درخواست‌های هنوز تکمیل نشده باید برگردانده شوند. به طور پیش‌فرض، این مقدار false است، تا با رفتار کتابخانه در نسخه v4.1.10 و قبل‌تر مطابقت داشته باشد.
* `options.orderBy` (`'START' | 'END'`): نحوه مرتب‌سازی درخواست‌ها. به طور پیش‌فرض، این مقدار `'END'` است، تا با رفتار کتابخانه در نسخه v4.1.10 و قبل‌تر مطابقت داشته باشد. اگر `'START'` باشد، درخواست‌ها بر اساس زمان شروع، به جای زمان تکمیل درخواست مرتب می‌شوند. (از آنجا که یک درخواست در حال انجام هنوز تکمیل نشده است، هنگام مرتب‌سازی بر اساس `'END'` تمام درخواست‌های در حال انجام پس از تمام درخواست‌های تکمیل شده قرار می‌گیرند.)

**برمی‌گرداند** شیء `request`:

* `request.url`: URL درخواست شده
* `request.method`: متد HTTP استفاده شده
* `request.body`: داده‌های محموله/بدنه استفاده شده در درخواست
* `request.headers`: هدرهای http درخواست به صورت شیء JS
* `request.pending`: پرچم بولی برای اینکه آیا این درخواست کامل است (یعنی دارای خاصیت `response` است) یا در حال انجام است.
* `request.response`: یک شیء JS که فقط در صورتی که درخواست تکمیل شده باشد (یعنی `request.pending === false`) حاضر است، و حاوی داده‌های مربوط به پاسخ است.
* `request.response?.headers`: هدرهای http پاسخ به صورت شیء JS
* `request.response?.body`: بدنه پاسخ (در صورت امکان به عنوان JSON تجزیه می‌شود)
* `request.response?.statusCode`: کد وضعیت پاسخ

**نکته‌ای درباره `request.body`:** wdio-intercept-service سعی می‌کند بدنه درخواست را به شرح زیر تجزیه کند:

* رشته: فقط رشته را برمی‌گرداند (`'value'`)
* JSON: شیء JSON را با استفاده از `JSON.parse()` تجزیه می‌کند (`({ key: value })`)
* FormData: FormData را در قالب `{ key: [value1, value2, ...] }` خروجی می‌دهد
* ArrayBuffer: سعی می‌کند بافر را به رشته تبدیل کند (تجربی)
* هر چیز دیگر: از `JSON.stringify()` شدید بر روی داده‌های شما استفاده می‌کند. موفق باشید!

**برای API `fetch`، ما فقط از داده‌های رشته و JSON پشتیبانی می‌کنیم!**

### `browser.getRequests({ includePending?: boolean, orderBy?: 'START' | 'END' }?: = {})`

تمام درخواست‌های ثبت شده را به صورت یک آرایه دریافت می‌کند، با پشتیبانی از همان گزینه‌های اختیاری مانند `getRequest`.

**برمی‌گرداند** آرایه‌ای از اشیاء `request`.

### browser.hasPendingRequests()

یک متد کمکی که بررسی می‌کند آیا هنوز درخواست‌های HTTP در حال انجام وجود دارند. می‌تواند توسط تست‌ها برای اطمینان از اینکه تمام درخواست‌ها در یک مدت زمان معقول تکمیل شده‌اند یا برای تأیید اینکه فراخوانی `getRequests()` یا `assertRequests()` شامل تمام درخواست‌های HTTP مورد نظر خواهد بود، استفاده شود.

**برمی‌گرداند** بولی

## پشتیبانی از TypeScript

این افزونه انواع TS خود را ارائه می‌دهد. فقط tsconfig خود را به توسعه‌های نوع مانند آنچه [اینجا](https://webdriver.io/docs/typescript.html#framework-types) ذکر شده است، اشاره دهید:

```
"compilerOptions": {
    // ..
    "types": ["node", "webdriverio", "wdio-intercept-service"]
},
```

## اجرای تست‌ها

نسخه‌های اخیر Chrome و Firefox برای اجرای تست‌ها به صورت محلی مورد نیاز است. ممکن است نیاز داشته باشید وابستگی‌های `chromedriver` و `geckodriver` را برای مطابقت با نسخه نصب شده در سیستم خود به‌روزرسانی کنید.

```shell
npm test
```

## مشارکت

از هر مشارکتی خوشحال می‌شوم. فقط یک مسئله باز کنید یا مستقیماً یک PR ارسال کنید.  
لطفاً توجه داشته باشید که این کتابخانه رهگیر برای کار با مرورگرهای قدیمی مانند Internet Explorer نوشته شده است. به عنوان چنین، هر کدی که در `lib/interceptor.js` استفاده می‌شود باید حداقل توسط زمان اجرای JavaScript مرورگر Internet Explorer قابل تجزیه باشد.

## مجوز

MIT