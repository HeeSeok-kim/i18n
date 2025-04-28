---
id: customreporter
title: گزارشگر سفارشی
---

شما می‌توانید گزارشگر سفارشی خود را برای اجرای تست WDIO که متناسب با نیازهای شما است، بنویسید. و این کار آسان است!

تنها کاری که باید انجام دهید این است که یک ماژول Node ایجاد کنید که از بسته `@wdio/reporter` ارث بری می‌کند، تا بتواند پیام‌هایی از تست دریافت کند.

تنظیمات اولیه باید به شکل زیر باشد:

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    constructor(options) {
        /*
         * تنظیم گزارشگر برای نوشتن در جریان خروجی به صورت پیش‌فرض
         */
        options = Object.assign(options, { stdout: true })
        super(options)
    }

    onTestPass(test) {
        this.write(`Congratulations! Your test "${test.title}" passed 👏`)
    }
}
```

برای استفاده از این گزارشگر، تنها کاری که باید انجام دهید این است که آن را به ویژگی `reporter` در پیکربندی خود اختصاص دهید.


فایل `wdio.conf.js` شما باید به این شکل باشد:

```js
import CustomReporter from './reporter/my.custom.reporter'

export const config = {
    // ...
    reporters: [
        /**
         * استفاده از کلاس گزارشگر وارد شده
         */
        [CustomReporter, {
            someOption: 'foobar'
        }],
        /**
         * استفاده از مسیر مطلق برای گزارشگر
         */
        ['/path/to/reporter.js', {
            someOption: 'foobar'
        }]
    ],
    // ...
}
```

شما همچنین می‌توانید گزارشگر را در NPM منتشر کنید تا همه بتوانند از آن استفاده کنند. نام بسته را مانند سایر گزارشگرها `wdio-<reportername>-reporter` بگذارید و آن را با کلیدواژه‌هایی مانند `wdio` یا `wdio-reporter` برچسب گذاری کنید.

## پردازشگر رویداد

شما می‌توانید یک پردازشگر رویداد برای چندین رویداد که در هنگام تست فعال می‌شوند، ثبت کنید. تمام پردازشگرهای زیر محموله‌هایی با اطلاعات مفید در مورد وضعیت فعلی و پیشرفت دریافت می‌کنند.

ساختار این اشیاء محموله به رویداد بستگی دارد و در بین فریم‌ورک‌ها (Mocha، Jasmine و Cucumber) یکسان است. هنگامی که یک گزارشگر سفارشی را پیاده‌سازی می‌کنید، باید برای همه فریم‌ورک‌ها کار کند.

لیست زیر شامل تمام متدهای ممکنی است که می‌توانید به کلاس گزارشگر خود اضافه کنید:

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    onRunnerStart() {}
    onBeforeCommand() {}
    onAfterCommand() {}
    onSuiteStart() {}
    onHookStart() {}
    onHookEnd() {}
    onTestStart() {}
    onTestPass() {}
    onTestFail() {}
    onTestSkip() {}
    onTestEnd() {}
    onSuiteEnd() {}
    onRunnerEnd() {}
}
```

نام‌های متد کاملاً خودتوضیح هستند.

برای چاپ چیزی در یک رویداد خاص، از متد `this.write(...)` استفاده کنید که توسط کلاس والد `WDIOReporter` فراهم شده است. این متد محتوا را یا به `stdout` جاری می‌کند یا به یک فایل لاگ (بسته به گزینه‌های گزارشگر).

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    onTestPass(test) {
        this.write(`Congratulations! Your test "${test.title}" passed 👏`)
    }
}
```

توجه داشته باشید که نمی‌توانید اجرای تست را به هیچ وجه به تأخیر بیندازید.

تمام پردازشگرهای رویداد باید روتین‌های همگام را اجرا کنند (در غیر این صورت با شرایط مسابقه مواجه خواهید شد).

حتماً [بخش مثال](https://github.com/webdriverio/webdriverio/tree/main/examples/wdio) را بررسی کنید که در آن می‌توانید یک نمونه گزارشگر سفارشی پیدا کنید که نام رویداد را برای هر رویداد چاپ می‌کند.

اگر یک گزارشگر سفارشی را پیاده‌سازی کرده‌اید که می‌تواند برای جامعه کاربران مفید باشد، از ایجاد یک Pull Request خودداری نکنید تا بتوانیم گزارشگر را برای عموم در دسترس قرار دهیم!

همچنین، اگر اجراکننده تست WDIO را از طریق واسط `Launcher` اجرا می‌کنید، نمی‌توانید یک گزارشگر سفارشی را به صورت تابع به شکل زیر اعمال کنید:

```js
import Launcher from '@wdio/cli'

import CustomReporter from './reporter/my.custom.reporter'

const launcher = new Launcher('/path/to/config.file.js', {
    // این کار نخواهد کرد، زیرا CustomReporter قابل سریالی شدن نیست
    reporters: ['dot', CustomReporter]
})
```

## منتظر `isSynchronised` بمانید

اگر گزارشگر شما باید عملیات‌های غیرهمگام را برای گزارش داده‌ها اجرا کند (مانند آپلود فایل‌های لاگ یا سایر دارایی‌ها)، می‌توانید متد `isSynchronised` را در گزارشگر سفارشی خود بازنویسی کنید تا اجراکننده WebdriverIO منتظر بماند تا همه چیز را محاسبه کنید. نمونه‌ای از این کار را می‌توان در [`@wdio/sumologic-reporter`](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-sumologic-reporter/src/index.ts) مشاهده کرد:

```js
export default class SumoLogicReporter extends WDIOReporter {
    constructor (options) {
        // ...
        this.unsynced = []
        this.interval = setInterval(::this.sync, this.options.syncInterval)
        // ...
    }

    /**
     * بازنویسی متد isSynchronised
     */
    get isSynchronised () {
        return this.unsynced.length === 0
    }

    /**
     * همگام‌سازی فایل‌های لاگ
     */
    sync () {
        // ...
        request({
            method: 'POST',
            uri: this.options.sourceAddress,
            body: logLines
        }, (err, resp) => {
            // ...
            /**
             * حذف لاگ‌های منتقل شده از سطل لاگ
             */
            this.unsynced.splice(0, MAX_LINES)
            // ...
        }
    }
}
```

به این ترتیب اجراکننده منتظر می‌ماند تا تمام اطلاعات لاگ آپلود شوند.

## انتشار گزارشگر در NPM

برای آسان‌تر کردن مصرف و کشف گزارشگرها توسط جامعه WebdriverIO، لطفاً این توصیه‌ها را دنبال کنید:

* سرویس‌ها باید از این قرارداد نامگذاری استفاده کنند: `wdio-*-reporter`
* از کلیدواژه‌های NPM استفاده کنید: `wdio-plugin`، `wdio-reporter`
* ورودی `main` باید یک نمونه از گزارشگر را `export` کند
* گزارشگر نمونه: [`@wdio/dot-service`](https://github.com/webdriverio/webdriverio/tree/main/packages/wdio-dot-reporter)

پیروی از الگوی نامگذاری توصیه شده اجازه می‌دهد که سرویس‌ها با نام اضافه شوند:

```js
// افزودن wdio-custom-reporter
export const config = {
    // ...
    reporter: ['custom'],
    // ...
}
```

### افزودن سرویس منتشر شده به WDIO CLI و مستندات

ما واقعاً از هر افزونه جدیدی که می‌تواند به دیگران کمک کند تست‌های بهتری اجرا کنند، قدردانی می‌کنیم! اگر چنین افزونه‌ای ایجاد کرده‌اید، لطفاً آن را به CLI و مستندات ما اضافه کنید تا راحت‌تر پیدا شود.

لطفاً یک pull request با تغییرات زیر ایجاد کنید:

- سرویس خود را به لیست [گزارشگرهای پشتیبانی شده](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-cli/src/constants.ts#L74-L91)) در ماژول CLI اضافه کنید
- [لیست گزارشگر](https://github.com/webdriverio/webdriverio/blob/main/scripts/docs-generation/3rd-party/reporters.json) را برای افزودن مستندات خود به صفحه رسمی Webdriver.io تقویت کنید