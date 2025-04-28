---
id: cloudservices
title: استفاده از سرویس‌های ابری
---

استفاده از سرویس‌های آنلاین مانند Sauce Labs، Browserstack، TestingBot، LambdaTest یا Perfecto با WebdriverIO بسیار ساده است. تنها کاری که باید انجام دهید این است که `user` و `key` سرویس خود را در گزینه‌های خود تنظیم کنید.

به صورت اختیاری، می‌توانید تست خود را با تنظیم قابلیت‌های خاص ابری مانند `build` پارامتری کنید. اگر می‌خواهید فقط سرویس‌های ابری را در Travis اجرا کنید، می‌توانید از متغیر محیطی `CI` برای بررسی اینکه آیا در Travis هستید استفاده کنید و پیکربندی را به طور متناسب تغییر دهید.

```js
// wdio.conf.js
export let config = {...}
if (process.env.CI) {
    config.user = process.env.SAUCE_USERNAME
    config.key = process.env.SAUCE_ACCESS_KEY
}
```

## Sauce Labs

می‌توانید تست‌های خود را به صورت از راه دور در [Sauce Labs](https://saucelabs.com) تنظیم کنید.

تنها نیاز، تنظیم `user` و `key` در پیکربندی شما (چه صادر شده توسط `wdio.conf.js` یا ارسال شده به `webdriverio.remote(...)`) به نام کاربری و کلید دسترسی Sauce Labs شما است.

همچنین می‌توانید هر گزینه اختیاری [پیکربندی تست](https://docs.saucelabs.com/dev/test-configuration-options/) را به صورت کلید/مقدار در قابلیت‌ها برای هر مرورگر ارسال کنید.

### Sauce Connect

اگر می‌خواهید تست‌ها را در برابر سروری که از طریق اینترنت قابل دسترسی نیست (مانند `localhost`) اجرا کنید، باید از [Sauce Connect](https://docs.saucelabs.com/secure-connections/#sauce-connect-proxy) استفاده کنید.

این خارج از محدوده WebdriverIO است که از این پشتیبانی کند، بنابراین باید خودتان آن را راه‌اندازی کنید.

اگر از WDIO testrunner استفاده می‌کنید، [`@wdio/sauce-service`](https://github.com/webdriverio/webdriverio/tree/main/packages/wdio-sauce-service) را دانلود و در `wdio.conf.js` خود پیکربندی کنید. این به اجرای Sauce Connect کمک می‌کند و دارای ویژگی‌های اضافی است که تست‌های شما را بهتر در سرویس Sauce ادغام می‌کند.

### با Travis CI

با این حال، Travis CI [پشتیبانی](http://docs.travis-ci.com/user/sauce-connect/#Setting-up-Sauce-Connect) برای شروع Sauce Connect قبل از هر تست دارد، بنابراین دنبال کردن دستورالعمل‌های آنها برای این کار یک گزینه است.

اگر چنین کاری می‌کنید، باید گزینه پیکربندی تست `tunnel-identifier` را در `capabilities` هر مرورگر تنظیم کنید. Travis به طور پیش‌فرض این را به متغیر محیطی `TRAVIS_JOB_NUMBER` تنظیم می‌کند.

همچنین، اگر می‌خواهید Sauce Labs تست‌های شما را بر اساس شماره ساخت گروه‌بندی کند، می‌توانید `build` را به `TRAVIS_BUILD_NUMBER` تنظیم کنید.

در آخر، اگر `name` را تنظیم کنید، این نام تست را در Sauce Labs برای این ساخت تغییر می‌دهد. اگر از WDIO testrunner همراه با [`@wdio/sauce-service`](https://github.com/webdriverio/webdriverio/tree/main/packages/wdio-sauce-service) استفاده می‌کنید، WebdriverIO به طور خودکار یک نام مناسب برای تست تنظیم می‌کند.

مثال `capabilities`:

```javascript
browserName: 'chrome',
version: '27.0',
platform: 'XP',
'tunnel-identifier': process.env.TRAVIS_JOB_NUMBER,
name: 'integration',
build: process.env.TRAVIS_BUILD_NUMBER
```

### مهلت‌های زمانی

از آنجا که تست‌های خود را از راه دور اجرا می‌کنید، ممکن است لازم باشد برخی از مهلت‌های زمانی را افزایش دهید.

می‌توانید [مهلت زمانی بیکار](https://docs.saucelabs.com/dev/test-configuration-options/#idletimeout) را با ارسال `idle-timeout` به عنوان یک گزینه پیکربندی تست تغییر دهید. این کنترل می‌کند که Sauce چه مدت بین دستورات قبل از بستن اتصال منتظر بماند.

## BrowserStack

WebdriverIO همچنین دارای ادغام داخلی با [Browserstack](https://www.browserstack.com) است.

تنها نیاز، تنظیم `user` و `key` در پیکربندی شما (چه صادر شده توسط `wdio.conf.js` یا ارسال شده به `webdriverio.remote(...)`) به نام کاربری اتوماسیون و کلید دسترسی Browserstack شما است.

همچنین می‌توانید هر [قابلیت پشتیبانی شده](https://www.browserstack.com/automate/capabilities) اختیاری را به صورت کلید/مقدار در قابلیت‌ها برای هر مرورگر ارسال کنید. اگر `browserstack.debug` را به `true` تنظیم کنید، یک فیلم از جلسه ضبط می‌کند که ممکن است مفید باشد.

### تست محلی

اگر می‌خواهید تست‌ها را در برابر سروری که از طریق اینترنت قابل دسترسی نیست (مانند `localhost`) اجرا کنید، باید از [تست محلی](https://www.browserstack.com/local-testing#command-line) استفاده کنید.

این خارج از محدوده WebdriverIO است که از این پشتیبانی کند، بنابراین باید خودتان آن را شروع کنید.

اگر از local استفاده می‌کنید، باید `browserstack.local` را به `true` در قابلیت‌های خود تنظیم کنید.

اگر از WDIO testrunner استفاده می‌کنید، [`@wdio/browserstack-service`](https://github.com/webdriverio/webdriverio/tree/master/packages/wdio-browserstack-service) را دانلود و در `wdio.conf.js` خود پیکربندی کنید. این به اجرای BrowserStack کمک می‌کند و دارای ویژگی‌های اضافی است که تست‌های شما را بهتر در سرویس BrowserStack ادغام می‌کند.

### با Travis CI

اگر می‌خواهید تست محلی را در Travis اضافه کنید، باید خودتان آن را شروع کنید.

اسکریپت زیر آن را دانلود و در پس‌زمینه شروع می‌کند. باید این را قبل از شروع تست‌ها در Travis اجرا کنید.

```sh
wget https://www.browserstack.com/browserstack-local/BrowserStackLocal-linux-x64.zip
unzip BrowserStackLocal-linux-x64.zip
./BrowserStackLocal -v -onlyAutomate -forcelocal $BROWSERSTACK_ACCESS_KEY &
sleep 3
```

همچنین، ممکن است بخواهید `build` را به شماره ساخت Travis تنظیم کنید.

مثال `capabilities`:

```javascript
browserName: 'chrome',
project: 'myApp',
version: '44.0',
build: `myApp #${process.env.TRAVIS_BUILD_NUMBER}.${process.env.TRAVIS_JOB_NUMBER}`,
'browserstack.local': 'true',
'browserstack.debug': 'true'
```

## TestingBot

تنها نیاز، تنظیم `user` و `key` در پیکربندی شما (چه صادر شده توسط `wdio.conf.js` یا ارسال شده به `webdriverio.remote(...)`) به نام کاربری و کلید مخفی [TestingBot](https://testingbot.com) شما است.

همچنین می‌توانید هر [قابلیت پشتیبانی شده](https://testingbot.com/support/other/test-options) اختیاری را به صورت کلید/مقدار در قابلیت‌ها برای هر مرورگر ارسال کنید.

### تست محلی

اگر می‌خواهید تست‌ها را در برابر سروری که از طریق اینترنت قابل دسترسی نیست (مانند `localhost`) اجرا کنید، باید از [تست محلی](https://testingbot.com/support/other/tunnel) استفاده کنید. TestingBot یک تونل مبتنی بر جاوا را فراهم می‌کند تا به شما اجازه دهد وب‌سایت‌هایی را که از اینترنت قابل دسترسی نیستند تست کنید.

صفحه پشتیبانی تونل آنها حاوی اطلاعات لازم برای راه‌اندازی این است.

اگر از WDIO testrunner استفاده می‌کنید، [`@wdio/testingbot-service`](https://github.com/webdriverio/webdriverio/tree/main/packages/wdio-testingbot-service) را دانلود و در `wdio.conf.js` خود پیکربندی کنید. این به اجرای TestingBot کمک می‌کند و دارای ویژگی‌های اضافی است که تست‌های شما را بهتر در سرویس TestingBot ادغام می‌کند.

## LambdaTest

ادغام [LambdaTest](https://www.lambdatest.com) نیز داخلی است.

تنها نیاز، تنظیم `user` و `key` در پیکربندی شما (چه صادر شده توسط `wdio.conf.js` یا ارسال شده به `webdriverio.remote(...)`) به نام کاربری حساب و کلید دسترسی LambdaTest شما است.

همچنین می‌توانید هر [قابلیت پشتیبانی شده](https://www.lambdatest.com/capabilities-generator/) اختیاری را به صورت کلید/مقدار در قابلیت‌ها برای هر مرورگر ارسال کنید. اگر `visual` را به `true` تنظیم کنید، یک فیلم از جلسه ضبط می‌کند که ممکن است مفید باشد.

### تونل برای تست محلی

اگر می‌خواهید تست‌ها را در برابر سروری که از طریق اینترنت قابل دسترسی نیست (مانند `localhost`) اجرا کنید، باید از [تست محلی](https://www.lambdatest.com/support/docs/testing-locally-hosted-pages/) استفاده کنید.

این خارج از محدوده WebdriverIO است که از این پشتیبانی کند، بنابراین باید خودتان آن را شروع کنید.

اگر از local استفاده می‌کنید، باید `tunnel` را به `true` در قابلیت‌های خود تنظیم کنید.

اگر از WDIO testrunner استفاده می‌کنید، [`wdio-lambdatest-service`](https://github.com/LambdaTest/wdio-lambdatest-service) را دانلود و در `wdio.conf.js` خود پیکربندی کنید. این به اجرای LambdaTest کمک می‌کند و دارای ویژگی‌های اضافی است که تست‌های شما را بهتر در سرویس LambdaTest ادغام می‌کند.

### با Travis CI

اگر می‌خواهید تست محلی را در Travis اضافه کنید، باید خودتان آن را شروع کنید.

اسکریپت زیر آن را دانلود و در پس‌زمینه شروع می‌کند. باید این را قبل از شروع تست‌ها در Travis اجرا کنید.

```sh
wget http://downloads.lambdatest.com/tunnel/linux/64bit/LT_Linux.zip
unzip LT_Linux.zip
./LT -user $LT_USERNAME -key $LT_ACCESS_KEY -cui &
sleep 3
```

همچنین، ممکن است بخواهید `build` را به شماره ساخت Travis تنظیم کنید.

مثال `capabilities`:

```javascript
platform: 'Windows 10',
browserName: 'chrome',
version: '79.0',
build: `myApp #${process.env.TRAVIS_BUILD_NUMBER}.${process.env.TRAVIS_JOB_NUMBER}`,
'tunnel': 'true',
'visual': 'true'
```

## Perfecto

هنگام استفاده از wdio با [`Perfecto`](https://www.perfecto.io)، باید یک توکن امنیتی برای هر کاربر ایجاد کنید و این را در ساختار قابلیت‌ها (علاوه بر سایر قابلیت‌ها) به شرح زیر اضافه کنید:

```js
export const config = {
  capabilities: [{
    // ...
    securityToken: "your security token"
  }],
```

علاوه بر این، باید پیکربندی ابر را به شرح زیر اضافه کنید:

```js
  hostname: "your_cloud_name.perfectomobile.com",
  path: "/nexperience/perfectomobile/wd/hub",
  port: 443,
  protocol: "https",
```