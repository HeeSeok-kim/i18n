---
id: v7-migration
title: از نسخه ۶ به نسخه ۷
---

این آموزش برای افرادی است که هنوز از نسخه `v6` WebdriverIO استفاده می‌کنند و قصد مهاجرت به نسخه `v7` را دارند. همانطور که در [پست وبلاگ انتشار](https://webdriver.io/blog/2021/02/09/webdriverio-v7-released) ما ذکر شده است، تغییرات عمدتاً در زیر پوسته هستند و ارتقا باید فرآیندی مستقیم باشد.

:::info

اگر از WebdriverIO نسخه `v5` یا پایین‌تر استفاده می‌کنید، لطفاً ابتدا به نسخه `v6` ارتقا دهید. لطفاً راهنمای مهاجرت [v6](v6-migration) ما را بررسی کنید.

:::

اگرچه دوست داریم یک فرآیند کاملاً خودکار برای این کار داشته باشیم، اما واقعیت متفاوت است. هر کسی پیکربندی متفاوتی دارد. هر مرحله باید به عنوان راهنمایی دیده شود و نه دستورالعمل گام به گام. اگر در مهاجرت مشکلی دارید، از [تماس با ما](https://github.com/webdriverio/codemod/discussions/new) دریغ نکنید.

## راه‌اندازی

مشابه سایر مهاجرت‌ها، می‌توانیم از [codemod](https://github.com/webdriverio/codemod) WebdriverIO استفاده کنیم. برای این آموزش، از یک [پروژه نمونه](https://github.com/WarleyGabriel/demo-webdriverio-cucumber) که توسط یکی از اعضای جامعه ارسال شده است استفاده می‌کنیم و آن را به طور کامل از `v6` به `v7` مهاجرت می‌دهیم.

برای نصب codemod، اجرا کنید:

```sh
npm install jscodeshift @wdio/codemod
```

#### کامیت‌ها:

- _نصب وابستگی‌های codemod_ [[6ec9e52]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/6ec9e52038f7e8cb1221753b67040b0f23a8f61a)

## ارتقاء وابستگی‌های WebdriverIO

با توجه به اینکه تمام نسخه‌های WebdriverIO به یکدیگر متصل هستند، بهترین کار همیشه ارتقا به یک برچسب خاص است، مثلاً `latest`. برای انجام این کار، تمام وابستگی‌های مربوط به WebdriverIO را از `package.json` خود کپی می‌کنیم و آنها را مجدداً نصب می‌کنیم:

```sh
npm i --save-dev @wdio/allure-reporter@7 @wdio/cli@7 @wdio/cucumber-framework@7 @wdio/local-runner@7 @wdio/spec-reporter@7 @wdio/sync@7 wdio-chromedriver-service@7 wdio-timeline-reporter@7 webdriverio@7
```

معمولاً وابستگی‌های WebdriverIO بخشی از وابستگی‌های توسعه هستند، اما بسته به پروژه شما ممکن است متفاوت باشد. پس از این، `package.json` و `package-lock.json` شما باید به‌روزرسانی شوند. __توجه:__ اینها وابستگی‌های مورد استفاده در [پروژه نمونه](https://github.com/WarleyGabriel/demo-webdriverio-cucumber) هستند، وابستگی‌های شما ممکن است متفاوت باشد.

#### کامیت‌ها:

- _به‌روزرسانی وابستگی‌ها_ [[7097ab6]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/7097ab6297ef9f37ead0a9c2ce9fce8d0765458d)

## تبدیل فایل پیکربندی

یک مرحله خوب برای شروع، کار با فایل پیکربندی است. در WebdriverIO `v7` دیگر نیازی به ثبت دستی هیچ کامپایلری نیست. در واقع آنها باید حذف شوند. این کار می‌تواند با codemod به طور کاملاً خودکار انجام شود:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v7 ./wdio.conf.js
```

:::caution

codemod هنوز از پروژه‌های TypeScript پشتیبانی نمی‌کند. به [`@webdriverio/codemod#10`](https://github.com/webdriverio/codemod/issues/10) مراجعه کنید. ما در حال کار برای پیاده‌سازی پشتیبانی از آن هستیم. اگر از TypeScript استفاده می‌کنید، لطفاً مشارکت کنید!

:::

#### کامیت‌ها:

- _ترجمه فایل پیکربندی_ [[6015534]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/60155346a386380d8a77ae6d1107483043a43994)

## به‌روزرسانی تعاریف مراحل

اگر از Jasmine یا Mocha استفاده می‌کنید، کار شما اینجا تمام شده است. آخرین مرحله به‌روزرسانی واردات‌های Cucumber.js از `cucumber` به `@cucumber/cucumber` است. این کار نیز می‌تواند از طریق codemod به صورت خودکار انجام شود:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v7 ./src/e2e/*
```

همین! دیگر تغییری لازم نیست 🎉

#### کامیت‌ها:

- _ترجمه تعاریف مراحل_ [[8c97b90]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/8c97b90a8b9197c62dffe4e2954f7dad814753cc)

## نتیجه‌گیری

امیدواریم این آموزش کمی شما را در فرآیند مهاجرت به WebdriverIO `v7` راهنمایی کند. جامعه همچنان به بهبود codemod ادامه می‌دهد و آن را با تیم‌های مختلف در سازمان‌های مختلف آزمایش می‌کند. اگر بازخوردی دارید، از [ایجاد یک مشکل](https://github.com/webdriverio/codemod/issues/new) دریغ نکنید یا اگر در طول فرآیند مهاجرت با مشکلی مواجه شدید، [یک بحث را شروع کنید](https://github.com/webdriverio/codemod/discussions/new).