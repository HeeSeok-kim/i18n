---
id: gettingstarted
title: شروع کار
---

به مستندات WebdriverIO خوش آمدید. این راهنما به شما کمک می‌کند تا سریع شروع کنید. اگر با مشکلی مواجه شدید، می‌توانید در [سرور پشتیبانی دیسکورد](https://discord.webdriver.io) ما راهنمایی و پاسخ دریافت کنید یا می‌توانید با من در [توییتر](https://twitter.com/webdriverio) تماس بگیرید.

:::info
این مستندات برای آخرین نسخه (__>=9.x__) WebdriverIO است. اگر هنوز از نسخه قدیمی‌تر استفاده می‌کنید، لطفاً به [وب‌سایت‌های مستندات قدیمی](/versions) مراجعه کنید!
:::

<LiteYouTubeEmbed
    id="rA4IFNyW54c"
    title="Getting Started with WebdriverIO"
/>

:::tip کانال رسمی یوتیوب 🎥

شما می‌توانید ویدیوهای بیشتری درباره WebdriverIO در [کانال رسمی یوتیوب](https://youtube.com/@webdriverio) پیدا کنید. حتماً مشترک شوید!

:::

## راه‌اندازی یک پروژه WebdriverIO

برای اضافه کردن یک تنظیمات کامل WebdriverIO به یک پروژه موجود یا جدید با استفاده از [ابزار شروع WebdriverIO](https://www.npmjs.com/package/create-wdio)، اجرا کنید:

اگر در دایرکتوری اصلی یک پروژه موجود هستید، اجرا کنید:

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

این دستور واحد، ابزار خط فرمان WebdriverIO را دانلود می‌کند و یک ویزارد پیکربندی را اجرا می‌کند که به شما در پیکربندی مجموعه آزمون خود کمک می‌کند.

<CreateProjectAnimation />

ویزارد مجموعه‌ای از سوالات را نمایش می‌دهد که شما را در تنظیمات راهنمایی می‌کند. شما می‌توانید پارامتر `--yes` را برای انتخاب یک تنظیم پیش‌فرض ارسال کنید که از Mocha با Chrome با استفاده از الگوی [Page Object](https://martinfowler.com/bliki/PageObject.html) استفاده می‌کند.

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

## اجرای آزمون

می‌توانید مجموعه آزمون خود را با استفاده از دستور `run` و اشاره به پیکربندی WebdriverIO که تازه ایجاد کرده‌اید، شروع کنید:

```sh
npx wdio run ./wdio.conf.js
```

اگر می‌خواهید فایل‌های آزمون خاصی را اجرا کنید، می‌توانید یک پارامتر `--spec` اضافه کنید:

```sh
npx wdio run ./wdio.conf.js --spec example.e2e.js
```

یا مجموعه‌ها را در فایل پیکربندی خود تعریف کنید و فقط فایل‌های آزمون تعریف شده در یک مجموعه را اجرا کنید:

```sh
npx wdio run ./wdio.conf.js --suite exampleSuiteName
```

## اجرا در یک اسکریپت

اگر می‌خواهید از WebdriverIO به عنوان یک موتور اتوماسیون در [حالت مستقل](/docs/setuptypes#standalone-mode) در یک اسکریپت Node.JS استفاده کنید، می‌توانید مستقیماً WebdriverIO را نصب کنید و از آن به عنوان یک بسته استفاده کنید، به عنوان مثال برای ایجاد یک اسکرین‌شات از یک وب‌سایت:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/fc362f2f8dd823d294b9bb5f92bd5991339d4591/getting-started/run-in-script.js#L2-L19
```

__توجه:__ تمام دستورات WebdriverIO ناهمگام هستند و باید با استفاده از [`async/await`](https://javascript.info/async-await) به درستی مدیریت شوند.

## ضبط آزمون‌ها

WebdriverIO ابزارهایی برای کمک به شروع کار از طریق ضبط اقدامات آزمون شما در صفحه و تولید خودکار اسکریپت‌های آزمون WebdriverIO ارائه می‌دهد. برای اطلاعات بیشتر به [ضبط آزمون‌ها با Chrome DevTools Recorder](/docs/record) مراجعه کنید.

## الزامات سیستم

شما به [Node.js](http://nodejs.org) نصب شده نیاز دارید.

- حداقل نسخه v18.20.0 یا بالاتر را نصب کنید زیرا این قدیمی‌ترین نسخه فعال LTS است
- فقط نسخه‌هایی که LTS هستند یا تبدیل به نسخه LTS خواهند شد، به طور رسمی پشتیبانی می‌شوند

اگر Node در حال حاضر روی سیستم شما نصب نشده است، ما پیشنهاد می‌کنیم از ابزاری مانند [NVM](https://github.com/creationix/nvm) یا [Volta](https://volta.sh/) برای کمک به مدیریت چندین نسخه فعال Node.js استفاده کنید. NVM یک انتخاب محبوب است، در حالی که Volta نیز گزینه خوبی است.