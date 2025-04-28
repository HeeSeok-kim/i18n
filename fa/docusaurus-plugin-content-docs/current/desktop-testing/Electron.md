---
id: electron
title: الکترون
---

الکترون چارچوبی برای ساخت برنامه‌های دسکتاپ با استفاده از جاوااسکریپت، HTML و CSS است. با جاسازی کرومیوم و Node.js در باینری خود، الکترون به شما امکان می‌دهد یک کدبیس جاوااسکریپت را نگهداری کنید و برنامه‌های چند پلتفرمی بسازید که در ویندوز، مک‌او‌اس و لینوکس کار می‌کنند — بدون نیاز به تجربه توسعه بومی.

WebdriverIO یک سرویس یکپارچه ارائه می‌دهد که تعامل با برنامه الکترون شما را ساده‌تر می‌کند و آزمایش آن را بسیار آسان می‌سازد. مزایای استفاده از WebdriverIO برای آزمایش برنامه‌های الکترون عبارتند از:

- 🚗 راه‌اندازی خودکار Chromedriver مورد نیاز
- 📦 تشخیص خودکار مسیر برنامه الکترون شما - از [Electron Forge](https://www.electronforge.io/) و [Electron Builder](https://www.electron.build/) پشتیبانی می‌کند
- 🧩 دسترسی به API‌های الکترون در آزمون‌های خود
- 🕵️ شبیه‌سازی API‌های الکترون از طریق یک API شبیه Vitest

شما فقط به چند مرحله ساده برای شروع نیاز دارید. این آموزش ویدیویی گام به گام ساده را از کانال [WebdriverIO YouTube](https://www.youtube.com/@webdriverio) تماشا کنید:

<LiteYouTubeEmbed
    id="iQNxTdWedk0"
    title="Getting Started with ElectronJS Testing in WebdriverIO"
/>

یا راهنمای بخش زیر را دنبال کنید.

## شروع کار

برای ایجاد یک پروژه جدید WebdriverIO، اجرا کنید:

```sh
npm create wdio@latest ./
```

یک راهنمای نصب شما را در این فرآیند هدایت خواهد کرد. اطمینان حاصل کنید که _"Desktop Testing - of Electron Applications"_ را انتخاب کنید وقتی از شما می‌پرسد چه نوع آزمایشی می‌خواهید انجام دهید. سپس مسیر برنامه الکترون کامپایل شده خود را وارد کنید، مثلاً `./dist`، سپس فقط تنظیمات پیش‌فرض را نگه دارید یا بر اساس ترجیح خود تغییر دهید.

راهنمای پیکربندی تمام بسته‌های مورد نیاز را نصب می‌کند و یک `wdio.conf.js` یا `wdio.conf.ts` با پیکربندی‌های لازم برای آزمایش برنامه شما ایجاد می‌کند. اگر با تولید خودکار برخی فایل‌های آزمون موافقت کنید، می‌توانید اولین آزمون خود را با `npm run wdio` اجرا کنید.

## راه‌اندازی دستی

اگر قبلاً از WebdriverIO در پروژه خود استفاده می‌کنید، می‌توانید راهنمای نصب را رد کنید و فقط وابستگی‌های زیر را اضافه کنید:

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

درباره چگونگی [پیکربندی سرویس الکترون](/docs/desktop-testing/electron/configuration)، [چگونگی شبیه‌سازی API‌های الکترون](/docs/desktop-testing/electron/mocking) و [چگونگی دسترسی به API‌های الکترون](/docs/desktop-testing/electron/api) بیشتر بیاموزید.