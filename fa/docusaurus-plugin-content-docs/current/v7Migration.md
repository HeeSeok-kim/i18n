---
id: v7-migration
title: از نسخه ۶ به نسخه ۷
---

این آموزش برای افرادی است که هنوز از نسخه `v6` وب‌درایور آی‌او استفاده می‌کنند و می‌خواهند به نسخه `v7` مهاجرت کنند. همانطور که در [پست وبلاگ انتشار](https://webdriver.io/blog/2021/02/09/webdriverio-v7-released) ما ذکر شده، تغییرات عمدتاً زیرساختی هستند و ارتقا باید فرآیندی ساده باشد.

:::info

اگر از WebdriverIO نسخه `v5` یا پایین‌تر استفاده می‌کنید، لطفاً ابتدا به نسخه `v6` ارتقا دهید. راهنمای مهاجرت به [نسخه ۶](v6-migration) را بررسی کنید.

:::

اگرچه ما دوست داریم یک فرآیند کاملاً خودکار برای این کار داشته باشیم، واقعیت متفاوت است. هر کسی تنظیمات متفاوتی دارد. هر مرحله باید به عنوان راهنمایی دیده شود و نه دستورالعملی گام به گام. اگر در مهاجرت مشکلی دارید، از [تماس با ما](https://github.com/webdriverio/codemod/discussions/new) دریغ نکنید.

## راه‌اندازی

مشابه سایر مهاجرت‌ها، می‌توانیم از [codemod](https://github.com/webdriverio/codemod) وب‌درایور آی‌او استفاده کنیم. برای این آموزش، ما از یک [پروژه پایه](https://github.com/WarleyGabriel/demo-webdriverio-cucumber) که توسط یکی از اعضای جامعه ارسال شده استفاده می‌کنیم و آن را کاملاً از `v6` به `v7` مهاجرت می‌دهیم.

برای نصب codemod، اجرا کنید:

```sh
npm install jscodeshift @wdio/codemod
```

#### کامیت‌ها:

- _نصب وابستگی‌های codemod_ [[6ec9e52]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/6ec9e52038f7e8cb1221753b67040b0f23a8f61a)

## ارتقاء وابستگی‌های WebdriverIO

با توجه به اینکه تمام نسخه‌های WebdriverIO به یکدیگر مرتبط هستند، بهترین کار این است که همیشه به یک تگ خاص، مانند `latest` ارتقا دهید. برای انجام این کار، تمام وابستگی‌های مربوط به WebdriverIO را از `package.json` خود کپی کرده و آنها را مجدداً نصب می‌کنیم:

```sh
npm i --save-dev @wdio/allure-reporter@7 @wdio/cli@7 @wdio/cucumber-framework@7 @wdio/local-runner@7 @wdio/spec-reporter@7 @wdio/sync@7 wdio-chromedriver-service@7 wdio-timeline-reporter@7 webdriverio@7
```

معمولاً وابستگی‌های WebdriverIO بخشی از وابستگی‌های توسعه هستند، اما بسته به پروژه شما، این ممکن است متفاوت باشد. پس از این، `package.json` و `package-lock.json` شما باید به‌روز شوند. __توجه:__ اینها وابستگی‌هایی هستند که توسط [پروژه نمونه](https://github.com/WarleyGabriel/demo-webdriverio-cucumber) استفاده می‌شوند، وابستگی‌های شما ممکن است متفاوت باشند.

#### کامیت‌ها:

- _به‌روزرسانی وابستگی‌ها_ [[7097ab6]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/7097ab6297ef9f37ead0a9c2ce9fce8d0765458d)

## تبدیل فایل پیکربندی

یک قدم خوب برای شروع، کار با فایل پیکربندی است. در WebdriverIO `v7` دیگر نیازی به ثبت دستی هیچ کامپایلری نیست. در واقع، آنها باید حذف شوند. این کار را می‌توان با codemod به صورت کاملاً خودکار انجام داد:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v7 ./wdio.conf.js
```

:::caution

codemod هنوز از پروژه‌های TypeScript پشتیبانی نمی‌کند. به [`@webdriverio/codemod#10`](https://github.com/webdriverio/codemod/issues/10) مراجعه کنید. ما در حال کار برای پیاده‌سازی پشتیبانی از آن هستیم. اگر از TypeScript استفاده می‌کنید، لطفاً مشارکت کنید!

:::

#### کامیت‌ها:

- _ترجمه فایل پیکربندی_ [[6015534]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/60155346a386380d8a77ae6d1107483043a43994)

## به‌روزرسانی تعاریف مراحل

اگر از Jasmine یا Mocha استفاده می‌کنید، کار شما تمام شده است. آخرین مرحله به‌روزرسانی واردات Cucumber.js از `cucumber` به `@cucumber/cucumber` است. این کار را نیز می‌توان از طریق codemod به صورت خودکار انجام داد:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v7 ./src/e2e/*
```

تمام شد! دیگر تغییری لازم نیست 🎉

#### کامیت‌ها:

- _ترجمه تعاریف مراحل_ [[8c97b90]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/8c97b90a8b9197c62dffe4e2954f7dad814753cc)

## نتیجه‌گیری

امیدواریم این آموزش کمی شما را در فرآیند مهاجرت به WebdriverIO `v7` راهنمایی کند. جامعه به بهبود codemod در حین آزمایش آن با تیم‌های مختلف در سازمان‌های مختلف ادامه می‌دهد. اگر بازخوردی دارید، از [ایجاد مسئله](https://github.com/webdriverio/codemod/issues/new) یا اگر در طول فرآیند مهاجرت با مشکلی مواجه شدید، از [شروع یک بحث](https://github.com/webdriverio/codemod/discussions/new) دریغ نکنید.