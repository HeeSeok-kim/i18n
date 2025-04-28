---
id: electron
title: الکترون
---

الکترون یک فریم‌ورک برای ساخت برنامه‌های دسکتاپ با استفاده از جاوااسکریپت، HTML و CSS است. با جاسازی کرومیوم و Node.js در باینری خود، الکترون به شما امکان می‌دهد یک کدبیس جاوااسکریپت را حفظ کنید و برنامه‌های چند پلتفرمی بسازید که در ویندوز، مک‌او‌اس و لینوکس کار می‌کنند — بدون نیاز به تجربه توسعه بومی.

WebdriverIO یک سرویس یکپارچه ارائه می‌دهد که تعامل با برنامه الکترون شما را ساده‌تر کرده و تست کردن آن را بسیار آسان می‌کند. مزایای استفاده از WebdriverIO برای تست برنامه‌های الکترون عبارتند از:

- 🚗 راه‌اندازی خودکار Chromedriver مورد نیاز
- 📦 تشخیص خودکار مسیر برنامه الکترون شما - از [Electron Forge](https://www.electronforge.io/) و [Electron Builder](https://www.electron.build/) پشتیبانی می‌کند
- 🧩 دسترسی به API‌های الکترون در تست‌های خود
- 🕵️ ساخت ماک (mock) از API‌های الکترون از طریق یک API شبیه به Vitest

شما فقط به چند مرحله ساده برای شروع نیاز دارید. این آموزش ویدیویی ساده گام به گام را از کانال [WebdriverIO YouTube](https://www.youtube.com/@webdriverio) تماشا کنید:

<LiteYouTubeEmbed
    id="iQNxTdWedk0"
    title="Getting Started with ElectronJS Testing in WebdriverIO"
/>

یا راهنمای بخش زیر را دنبال کنید.

## شروع کار

برای شروع یک پروژه جدید WebdriverIO، اجرا کنید:

```sh
npm create wdio@latest ./
```

یک ویزارد نصب شما را در طول فرآیند راهنمایی خواهد کرد. اطمینان حاصل کنید که _"Desktop Testing - of Electron Applications"_ را هنگامی که از شما می‌پرسد چه نوع تستی می‌خواهید انجام دهید، انتخاب کنید. سپس مسیر برنامه کامپایل شده الکترون خود را ارائه دهید، مثلاً `./dist`، سپس فقط تنظیمات پیش‌فرض را نگه دارید یا بر اساس ترجیح خود تغییر دهید.

ویزارد پیکربندی تمام بسته‌های مورد نیاز را نصب می‌کند و یک `wdio.conf.js` یا `wdio.conf.ts` با پیکربندی لازم برای تست برنامه شما ایجاد می‌کند. اگر با تولید خودکار برخی فایل‌های تست موافقت کنید، می‌توانید اولین تست خود را از طریق `npm run wdio` اجرا کنید.

## راه‌اندازی دستی

اگر قبلاً از WebdriverIO در پروژه خود استفاده می‌کنید، می‌توانید از ویزارد نصب صرف‌نظر کرده و فقط وابستگی‌های زیر را اضافه کنید:

```sh
npm install --save-dev wdio-electron-service
```

سپس می‌توانید از پیکربندی زیر استفاده کنید:

```ts
// wdio.conf.ts
export const config: WebdriverIO.Config = {
    // ...
    services: [['electron', {
        appEntryPoint: './path/to/bundled/electron/main.bundle.js',
        appArgs: [/** ... */],
    }]]
}
```

همین! 🎉

درباره [چگونگی پیکربندی سرویس الکترون](/docs/desktop-testing/electron/configuration)، [چگونگی ساخت ماک (mock) از API‌های الکترون](/docs/desktop-testing/electron/mocking) و [چگونگی دسترسی به API‌های الکترون](/docs/desktop-testing/electron/api) بیشتر بیاموزید.