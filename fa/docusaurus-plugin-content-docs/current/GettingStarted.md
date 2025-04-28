---
id: gettingstarted
title: شروع به کار
---

به مستندات WebdriverIO خوش آمدید. این مستندات به شما کمک می‌کند تا سریع شروع کنید. اگر به مشکلی برخوردید، می‌توانید کمک و پاسخ‌ها را در [سرور پشتیبانی Discord](https://discord.webdriver.io) ما پیدا کنید یا می‌توانید با من در [توییتر](https://twitter.com/webdriverio) در تماس باشید.

:::info
این‌ها مستندات آخرین نسخه (__>=9.x__) WebdriverIO هستند. اگر هنوز از نسخه قدیمی‌تری استفاده می‌کنید، لطفاً از [وب‌سایت‌های مستندات قدیمی](/versions) بازدید کنید!
:::

<LiteYouTubeEmbed
    id="rA4IFNyW54c"
    title="Getting Started with WebdriverIO"
/>

:::tip کانال رسمی یوتیوب 🎥

شما می‌توانید ویدیوهای بیشتری درباره WebdriverIO در [کانال رسمی یوتیوب](https://youtube.com/@webdriverio) پیدا کنید. حتماً مشترک شوید!

:::

## راه‌اندازی یک پروژه WebdriverIO

برای اضافه کردن یک راه‌اندازی کامل WebdriverIO به یک پروژه موجود یا جدید با استفاده از [WebdriverIO Starter Toolkit](https://www.npmjs.com/package/create-wdio)، اجرا کنید:

اگر در دایرکتوری ریشه یک پروژه موجود هستید، اجرا کنید:

<Tabs
  defaultValue="npm"
  values={[
    {label: 'NPM', value: 'npm'},
    {label: 'Yarn', value: 'yarn'},
    {label: 'pnpm', value: 'pnpm'},
    {label: 'bun', value: 'bun'},
  ]
}>
<TabItem value="npm">

```sh
npm init wdio@latest .
```

یا اگر می‌خواهید یک پروژه جدید ایجاد کنید:

```sh
npm init wdio@latest ./path/to/new/project
```

</TabItem>
<TabItem value="yarn">

```sh
yarn create wdio .
```

یا اگر می‌خواهید یک پروژه جدید ایجاد کنید:

```sh
yarn create wdio ./path/to/new/project
```

</TabItem>
<TabItem value="pnpm">

```sh
pnpm create wdio@latest .
```

یا اگر می‌خواهید یک پروژه جدید ایجاد کنید:

```sh
pnpm create wdio@latest ./path/to/new/project
```

</TabItem>
<TabItem value="bun">

```sh
bun create wdio@latest .
```

یا اگر می‌خواهید یک پروژه جدید ایجاد کنید:

```sh
bun create wdio@latest ./path/to/new/project
```

</TabItem>
</Tabs>

این دستور ابزار خط فرمان WebdriverIO را دانلود می‌کند و یک ویزارد پیکربندی را اجرا می‌کند که به شما در پیکربندی مجموعه آزمایش خود کمک می‌کند.

<CreateProjectAnimation />

ویزارد مجموعه‌ای از سؤالات را مطرح می‌کند که شما را در راه‌اندازی راهنمایی می‌کند. می‌توانید پارامتر `--yes` را ارسال کنید تا یک راه‌اندازی پیش‌فرض را انتخاب کنید که از Mocha با Chrome با استفاده از الگوی [Page Object](https://martinfowler.com/bliki/PageObject.html) استفاده می‌کند.

<Tabs
  defaultValue="npm"
  values={[
    {label: 'NPM', value: 'npm'},
    {label: 'Yarn', value: 'yarn'},
    {label: 'pnpm', value: 'pnpm'},
    {label: 'bun', value: 'bun'},
  ]
}>
<TabItem value="npm">

```sh
npm init wdio@latest . -- --yes
```

</TabItem>
<TabItem value="yarn">

```sh
yarn create wdio . --yes
```

</TabItem>
<TabItem value="pnpm">

```sh
pnpm create wdio@latest . --yes
```

</TabItem>
<TabItem value="bun">

```sh
bun create wdio@latest . --yes
```

</TabItem>
</Tabs>

## نصب CLI به صورت دستی

همچنین می‌توانید بسته CLI را به صورت دستی به پروژه خود اضافه کنید:

```sh
npm i --save-dev @wdio/cli
npx wdio --version # prints e.g. `8.13.10`

# run configuration wizard
npx wdio config
```

## اجرای آزمایش

می‌توانید مجموعه آزمایش خود را با استفاده از دستور `run` و اشاره به پیکربندی WebdriverIO که تازه ایجاد کرده‌اید، شروع کنید:

```sh
npx wdio run ./wdio.conf.js
```

اگر می‌خواهید فایل‌های آزمایشی خاصی را اجرا کنید، می‌توانید یک پارامتر `--spec` اضافه کنید:

```sh
npx wdio run ./wdio.conf.js --spec example.e2e.js
```

یا مجموعه‌ها را در فایل پیکربندی خود تعریف کنید و فقط فایل‌های آزمایشی تعریف شده در یک مجموعه را اجرا کنید:

```sh
npx wdio run ./wdio.conf.js --suite exampleSuiteName
```

## اجرا در یک اسکریپت

اگر می‌خواهید از WebdriverIO به عنوان یک موتور اتوماسیون در [حالت مستقل](/docs/setuptypes#standalone-mode) درون یک اسکریپت Node.JS استفاده کنید، می‌توانید WebdriverIO را مستقیماً نصب کنید و از آن به عنوان یک بسته استفاده کنید، مثلاً برای تولید یک اسکرین‌شات از یک وب‌سایت:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/fc362f2f8dd823d294b9bb5f92bd5991339d4591/getting-started/run-in-script.js#L2-L19
```

__توجه:__ تمامی دستورات WebdriverIO غیرهمزمان (asynchronous) هستند و باید با استفاده از [`async/await`](https://javascript.info/async-await) به درستی مدیریت شوند.

## ضبط آزمایش‌ها

WebdriverIO ابزارهایی را برای کمک به شما در شروع با ضبط اقدامات آزمایشی شما بر روی صفحه نمایش و تولید خودکار اسکریپت‌های آزمایشی WebdriverIO ارائه می‌دهد. برای اطلاعات بیشتر به [ضبط آزمایش‌ها با Chrome DevTools Recorder](/docs/record) مراجعه کنید.

## نیازمندی‌های سیستم

شما به [Node.js](http://nodejs.org) نصب شده نیاز خواهید داشت.

- حداقل نسخه v18.20.0 یا بالاتر را نصب کنید زیرا این قدیمی‌ترین نسخه فعال LTS است
- فقط نسخه‌هایی که LTS هستند یا به LTS تبدیل خواهند شد، به طور رسمی پشتیبانی می‌شوند

اگر Node در حال حاضر روی سیستم شما نصب نیست، ما پیشنهاد می‌کنیم از ابزاری مانند [NVM](https://github.com/creationix/nvm) یا [Volta](https://volta.sh/) برای کمک به مدیریت چندین نسخه فعال Node.js استفاده کنید. NVM یک انتخاب محبوب است، در حالی که Volta نیز یک جایگزین خوب است.