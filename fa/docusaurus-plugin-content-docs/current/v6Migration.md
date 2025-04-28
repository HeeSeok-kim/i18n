---
id: v6-migration
title: از نسخه ۵ به نسخه ۶
---

این آموزش برای افرادی است که هنوز از نسخه `v5` WebdriverIO استفاده می‌کنند و می‌خواهند به نسخه `v6` یا آخرین نسخه WebdriverIO مهاجرت کنند. همانطور که در [پست وبلاگ انتشار](https://webdriver.io/blog/2020/03/26/webdriverio-v6-released) ما ذکر شده، تغییرات برای این ارتقاء نسخه را می‌توان به شرح زیر خلاصه کرد:

- ما پارامترهای برخی دستورات را یکپارچه کردیم (مانند `newWindow`، `react$`، `react$$`، `waitUntil`، `dragAndDrop`، `moveTo`، `waitForDisplayed`، `waitForEnabled`، `waitForExist`) و تمام پارامترهای اختیاری را در یک شیء واحد قرار دادیم، به عنوان مثال:

    ```js
    // v5
    browser.newWindow(
        'https://webdriver.io',
        'WebdriverIO window',
        'width=420,height=230,resizable,scrollbars=yes,status=1'
    )
    // v6
    browser.newWindow('https://webdriver.io', {
        windowName: 'WebdriverIO window',
        windowFeature: 'width=420,height=230,resizable,scrollbars=yes,status=1'
    })
    ```

- پیکربندی‌های سرویس‌ها به لیست سرویس منتقل شده‌اند، به عنوان مثال:

    ```js
    // v5
    exports.config = {
        services: ['sauce'],
        sauceConnect: true,
        sauceConnectOpts: { foo: 'bar' },
    }
    // v6
    exports.config = {
        services: [['sauce', {
            sauceConnect: true,
            sauceConnectOpts: { foo: 'bar' }
        }]],
    }
    ```

- برخی از گزینه‌های سرویس برای ساده‌سازی تغییر نام داده‌اند
- ما دستور `launchApp` را برای جلسات Chrome WebDriver به `launchChromeApp` تغییر نام دادیم

:::info

اگر از WebdriverIO `v4` یا پایین‌تر استفاده می‌کنید، لطفاً ابتدا به `v5` ارتقا دهید.

:::

در حالی که ما دوست داریم یک فرآیند کاملاً خودکار برای این کار داشته باشیم، واقعیت متفاوت است. هر کسی تنظیمات متفاوتی دارد. هر مرحله باید به عنوان راهنمایی دیده شود و نه دستورالعمل قدم به قدم. اگر در مهاجرت با مشکلی مواجه شدید، تردید نکنید و با [ما تماس بگیرید](https://github.com/webdriverio/codemod/discussions/new).

## راه‌اندازی

مشابه سایر مهاجرت‌ها، می‌توانیم از [codemod](https://github.com/webdriverio/codemod) WebdriverIO استفاده کنیم. برای نصب codemod، اجرا کنید:

```sh
npm install jscodeshift @wdio/codemod
```

## ارتقاء وابستگی‌های WebdriverIO

با توجه به اینکه تمام نسخه‌های WebdriverIO به یکدیگر وابسته هستند، بهترین کار همیشه ارتقاء به یک تگ خاص است، مثلاً `6.12.0`. اگر تصمیم دارید مستقیماً از `v5` به `v7` ارتقاء دهید، می‌توانید تگ را حذف کنید و آخرین نسخه‌های تمام بسته‌ها را نصب کنید. برای انجام این کار، تمام وابستگی‌های مربوط به WebdriverIO را از `package.json` خود کپی و آنها را دوباره از طریق زیر نصب می‌کنیم:

```sh
npm i --save-dev @wdio/allure-reporter@6 @wdio/cli@6 @wdio/cucumber-framework@6 @wdio/local-runner@6 @wdio/spec-reporter@6 @wdio/sync@6 wdio-chromedriver-service@6 webdriverio@6
```

معمولاً وابستگی‌های WebdriverIO بخشی از وابستگی‌های توسعه هستند، اما بسته به پروژه شما ممکن است متفاوت باشد. پس از این، `package.json` و `package-lock.json` شما باید به‌روز شوند. __توجه:__ اینها وابستگی‌های نمونه هستند، وابستگی‌های شما ممکن است متفاوت باشد. مطمئن شوید که آخرین نسخه v6 را با فراخوانی، به عنوان مثال، پیدا می‌کنید:

```sh
npm show webdriverio versions
```

سعی کنید آخرین نسخه 6 موجود را برای تمام بسته‌های اصلی WebdriverIO نصب کنید. برای بسته‌های انجمن این ممکن است از بسته به بسته متفاوت باشد. در اینجا توصیه می‌کنیم تغییرات را برای اطلاعات در مورد اینکه کدام نسخه هنوز با v6 سازگار است، بررسی کنید.

## تبدیل فایل پیکربندی

یک قدم خوب اول شروع با فایل پیکربندی است. تمام تغییرات شکننده را می‌توان با استفاده از codemod به طور کاملاً خودکار حل کرد:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v6 ./wdio.conf.js
```

:::caution

codemod هنوز از پروژه‌های TypeScript پشتیبانی نمی‌کند. به [`@webdriverio/codemod#10`](https://github.com/webdriverio/codemod/issues/10) مراجعه کنید. ما در حال کار برای پیاده‌سازی پشتیبانی از آن هستیم. اگر از TypeScript استفاده می‌کنید، لطفا مشارکت کنید!

:::

## به‌روزرسانی فایل‌های Spec و Page Objects

برای به‌روزرسانی تمام تغییرات دستور، codemod را روی تمام فایل‌های e2e خود که حاوی دستورات WebdriverIO هستند اجرا کنید، به عنوان مثال:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v6 ./e2e/*
```

همین! دیگر تغییرات بیشتری لازم نیست 🎉

## نتیجه‌گیری

امیدواریم این آموزش شما را کمی در فرآیند مهاجرت به WebdriverIO `v6` راهنمایی کند. ما قویاً توصیه می‌کنیم که به ارتقاء به آخرین نسخه ادامه دهید، با توجه به اینکه ارتقاء به `v7` به دلیل تقریباً عدم وجود تغییرات شکننده، بسیار ساده است. لطفاً راهنمای مهاجرت [برای ارتقاء به v7](v7-migration) را بررسی کنید.

جامعه به بهبود codemod با آزمایش آن با تیم‌های مختلف در سازمان‌های مختلف ادامه می‌دهد. اگر بازخوردی دارید، تردید نکنید و [یک مسئله را مطرح کنید](https://github.com/webdriverio/codemod/issues/new) یا اگر در طول فرآیند مهاجرت با مشکل مواجه شدید، [یک بحث را شروع کنید](https://github.com/webdriverio/codemod/discussions/new).