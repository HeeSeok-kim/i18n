---
id: vscode-extensions
title: تست افزونه‌های VS Code
---

WebdriverIO به شما امکان می‌دهد تا به راحتی افزونه‌های [VS Code](https://code.visualstudio.com/) خود را از ابتدا تا انتها در محیط VS Code Desktop IDE یا به عنوان افزونه وب تست کنید. شما فقط باید مسیری به افزونه خود ارائه دهید و فریم‌ورک بقیه کارها را انجام می‌دهد. با استفاده از [`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service) همه چیز مدیریت می‌شود و موارد بیشتری نیز ارائه می‌شود:

- 🏗️ نصب VSCode (نسخه پایدار، نسخه insider یا نسخه مشخص شده)
- ⬇️ دانلود Chromedriver مخصوص نسخه VSCode داده شده
- 🚀 به شما امکان دسترسی به API های VSCode از تست‌هایتان را می‌دهد
- 🖥️ اجرای VSCode با تنظیمات سفارشی کاربر (شامل پشتیبانی از VSCode در اوبونتو، مک‌او‌اس و ویندوز)
- 🌐 یا VSCode را از یک سرور ارائه می‌دهد تا برای تست افزونه‌های وب توسط هر مرورگری قابل دسترسی باشد
- 📔 راه‌اندازی اشیاء صفحه با locator های مطابق با نسخه VSCode شما

## شروع کار

برای ایجاد یک پروژه جدید WebdriverIO، اجرا کنید:

```sh
npm create wdio@latest ./
```

یک ویزارد نصب شما را در این فرآیند راهنمایی خواهد کرد. اطمینان حاصل کنید که _"VS Code Extension Testing"_ را هنگامی که از شما می‌پرسد چه نوع تستی می‌خواهید انجام دهید انتخاب کنید، سپس می‌توانید تنظیمات پیش‌فرض را حفظ کنید یا بر اساس ترجیحات خود تغییر دهید.

## مثال پیکربندی

برای استفاده از این سرویس نیاز دارید `vscode` را به لیست سرویس‌های خود اضافه کنید، به صورت اختیاری می‌توانید یک آبجکت پیکربندی را نیز ارائه دهید. این باعث می‌شود WebdriverIO فایل‌های باینری VSCode مورد نظر و نسخه مناسب Chromedriver را دانلود کند:

```js
// wdio.conf.ts
export const config = {
    outputDir: 'trace',
    // ...
    capabilities: [{
        browserName: 'vscode',
        browserVersion: '1.71.0', // "insiders" یا "stable" برای آخرین نسخه VSCode
        'wdio:vscodeOptions': {
            extensionPath: __dirname,
            userSettings: {
                "editor.fontSize": 14
            }
        }
    }],
    services: ['vscode'],
    /**
     * به صورت اختیاری می‌توانید مسیری که WebdriverIO همه باینری‌های
     * VSCode و Chromedriver را ذخیره می‌کند تعریف کنید، مثلاً:
     * services: [['vscode', { cachePath: __dirname }]]
     */
    // ...
};
```

اگر `wdio:vscodeOptions` را با هر `browserName` دیگری به جز `vscode`، مثلاً `chrome` تعریف کنید، سرویس افزونه را به عنوان افزونه وب ارائه می‌دهد. اگر روی Chrome تست می‌کنید، نیازی به سرویس درایور اضافی نیست، مثلاً:

```js
// wdio.conf.ts
export const config = {
    outputDir: 'trace',
    // ...
    capabilities: [{
        browserName: 'chrome',
        'wdio:vscodeOptions': {
            extensionPath: __dirname
        }
    }],
    services: ['vscode'],
    // ...
};
```

_نکته:_ هنگام تست افزونه‌های وب فقط می‌توانید بین `stable` یا `insiders` به عنوان `browserVersion` انتخاب کنید.

### تنظیمات TypeScript

در فایل `tsconfig.json` خود اطمینان حاصل کنید که `wdio-vscode-service` را به لیست types خود اضافه کرده‌اید:

```json
{
    "compilerOptions": {
        "types": [
            "node",
            "webdriverio/async",
            "@wdio/mocha-framework",
            "expect-webdriverio",
            "wdio-vscode-service"
        ],
        "target": "es2020",
        "moduleResolution": "node16"
    }
}
```

## استفاده

سپس می‌توانید از متد `getWorkbench` برای دسترسی به اشیاء صفحه برای locator های مطابق با نسخه VSCode مورد نظر خود استفاده کنید:

```ts
describe('WDIO VSCode Service', () => {
    it('should be able to load VSCode', async () => {
        const workbench = await browser.getWorkbench()
        expect(await workbench.getTitleBar().getTitle())
            .toBe('[Extension Development Host] - README.md - wdio-vscode-service - Visual Studio Code')
    })
})
```

از آنجا می‌توانید به تمام اشیاء صفحه با استفاده از متدهای مناسب اشیاء صفحه دسترسی داشته باشید. اطلاعات بیشتر در مورد تمام اشیاء صفحه موجود و متدهای آنها را در [مستندات اشیاء صفحه](https://webdriverio-community.github.io/wdio-vscode-service/) بیابید.

### دسترسی به API های VSCode

اگر می‌خواهید اتوماسیون خاصی را از طریق [API های VSCode](https://code.visualstudio.com/api/references/vscode-api) اجرا کنید، می‌توانید این کار را با اجرای دستورات از راه دور از طریق دستور سفارشی `executeWorkbench` انجام دهید. این دستور به شما امکان می‌دهد کد را از راه دور از تست خود در محیط VSCode اجرا کنید و دسترسی به API های VSCode را فراهم می‌کند. می‌توانید پارامترهای دلخواه را به تابع منتقل کنید که سپس به تابع ارسال می‌شوند. شی `vscode` همیشه به عنوان اولین آرگومان پس از پارامترهای تابع خارجی ارسال می‌شود. توجه داشته باشید که نمی‌توانید به متغیرهای خارج از محدوده تابع دسترسی داشته باشید زیرا callback به صورت از راه دور اجرا می‌شود. اینجا یک مثال است:

```ts
const workbench = await browser.getWorkbench()
await browser.executeWorkbench((vscode, param1, param2) => {
    vscode.window.showInformationMessage(`I am an ${param1} ${param2}!`)
}, 'API', 'call')

const notifs = await workbench.getNotifications()
console.log(await notifs[0].getMessage()) // خروجی: "I am an API call!"
```

برای مستندات کامل اشیاء صفحه، [مستندات](https://webdriverio-community.github.io/wdio-vscode-service/modules.html) را بررسی کنید. نمونه‌های استفاده مختلف را می‌توانید در [مجموعه تست این پروژه](https://github.com/webdriverio-community/wdio-vscode-service/blob/main/test/specs) پیدا کنید.

## اطلاعات بیشتر

می‌توانید اطلاعات بیشتری در مورد نحوه پیکربندی [`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service) و نحوه ایجاد اشیاء صفحه سفارشی در [مستندات سرویس](/docs/wdio-vscode-service) بیاموزید. همچنین می‌توانید سخنرانی زیر از [Christian Bromann](https://twitter.com/bromann) درباره [_تست افزونه‌های پیچیده VSCode با قدرت استانداردهای وب_](https://www.youtube.com/watch?v=PhGNTioBUiU) را تماشا کنید:

<LiteYouTubeEmbed
    id="PhGNTioBUiU"
    title="Testing Complex VSCode Extensions With the Power of Web Standards"
/>