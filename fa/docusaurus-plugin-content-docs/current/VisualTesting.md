---
id: visual-testing
title: تست بصری
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

## چه کاری می‌تواند انجام دهد؟

WebdriverIO امکان مقایسه تصویری برروی صفحه‌ها، عناصر یا صفحه کامل را فراهم می‌کند برای

-   🖥️ مرورگرهای دسکتاپ (Chrome / Firefox / Safari / Microsoft Edge)
-   📱 مرورگرهای موبایل / تبلت (Chrome روی شبیه‌سازهای Android / Safari روی شبیه‌سازهای iOS / دستگاه‌های واقعی) از طریق Appium
-   📱 اپلیکیشن‌های بومی (شبیه‌سازهای Android / شبیه‌سازهای iOS / دستگاه‌های واقعی) از طریق Appium (🌟 **جدید** 🌟)
-   📳 اپلیکیشن‌های هیبریدی از طریق Appium

از طریق [`@wdio/visual-service`](https://www.npmjs.com/package/@wdio/visual-service) که یک سرویس سبک WebdriverIO است.

این به شما امکان می‌دهد:

-   ذخیره یا مقایسه **صفحه‌ها/عناصر/صفحه کامل** با یک پایه مقایسه
-   به طور خودکار **یک پایه مقایسه ایجاد کنید** وقتی پایه مقایسه‌ای وجود ندارد
-   **مسدود کردن نواحی سفارشی** و حتی **به طور خودکار حذف کردن** نوار وضعیت و یا نوار ابزار (فقط موبایل) در زمان مقایسه
-   افزایش ابعاد اسکرین‌شات‌های عناصر
-   **مخفی کردن متن** در زمان مقایسه وب‌سایت برای:
    -   **بهبود ثبات** و جلوگیری از ناپایداری رندرینگ فونت
    -   تمرکز فقط روی **طرح‌بندی** یک وب‌سایت
-   استفاده از **روش‌های مقایسه مختلف** و مجموعه‌ای از **تطبیق‌دهنده‌های اضافی** برای خوانایی بهتر تست‌ها
-   بررسی اینکه وب‌سایت شما چگونه از **حرکت با کلید Tab در صفحه کلید پشتیبانی می‌کند**، همچنین ببینید [حرکت با Tab در یک وب‌سایت](#tabbing-through-a-website)
-   و موارد بیشتر، گزینه‌های [سرویس](./visual-testing/service-options) و [متد](./visual-testing/method-options) را ببینید

این سرویس یک ماژول سبک برای بازیابی داده‌ها و اسکرین‌شات‌های مورد نیاز برای تمام مرورگرها/دستگاه‌ها است. قدرت مقایسه از [ResembleJS](https://github.com/Huddle/Resemble.js) می‌آید. اگر می‌خواهید تصاویر را به صورت آنلاین مقایسه کنید، می‌توانید [ابزار آنلاین](http://rsmbl.github.io/Resemble.js/) را بررسی کنید.

:::info نکته برای اپلیکیشن‌های بومی/هیبریدی
روش‌های `saveScreen`، `saveElement`، `checkScreen`، `checkElement` و تطبیق‌دهنده‌های `toMatchScreenSnapshot` و `toMatchElementSnapshot` می‌توانند برای اپلیکیشن‌های بومی/Context استفاده شوند.

لطفاً از ویژگی `isHybridApp:true` در تنظیمات سرویس خود استفاده کنید زمانی که می‌خواهید از آن برای اپلیکیشن‌های هیبریدی استفاده کنید.
:::

## نصب

ساده‌ترین راه این است که `@wdio/visual-service` را به عنوان وابستگی توسعه در `package.json` خود نگه دارید، از طریق:

```sh
npm install --save-dev @wdio/visual-service
```

## استفاده

`@wdio/visual-service` می‌تواند به عنوان یک سرویس معمولی استفاده شود. می‌توانید آن را در فایل پیکربندی خود با موارد زیر تنظیم کنید:

```js
import path from "node:path";

// wdio.conf.ts
export const config = {
    // ...
    // =====
    // Setup
    // =====
    services: [
        [
            "visual",
            {
                // برخی گزینه‌ها، برای اطلاعات بیشتر به مستندات مراجعه کنید
                baselineFolder: path.join(process.cwd(), "tests", "baseline"),
                formatImageName: "{tag}-{logName}-{width}x{height}",
                screenshotPath: path.join(process.cwd(), "tmp"),
                savePerInstance: true,
                // ... گزینه‌های بیشتر
            },
        ],
    ],
    // ...
};
```

گزینه‌های سرویس بیشتر را می‌توانید [اینجا](/docs/visual-testing/service-options) پیدا کنید.

پس از تنظیم در پیکربندی WebdriverIO، می‌توانید تأییدیه‌های بصری را به [آزمون‌های خود](/docs/visual-testing/writing-tests) اضافه کنید.

### قابلیت‌ها
برای استفاده از ماژول تست بصری، **نیازی به افزودن گزینه‌های اضافی به قابلیت‌های خود ندارید**. با این حال، در برخی موارد، ممکن است بخواهید متادیتای اضافی به تست‌های بصری خود اضافه کنید، مانند یک `logName`.

`logName` به شما امکان می‌دهد یک نام سفارشی به هر قابلیت اختصاص دهید، که می‌تواند در نام فایل‌های تصویر گنجانده شود. این امر به‌ویژه برای تشخیص اسکرین‌شات‌هایی که در مرورگرها، دستگاه‌ها یا پیکربندی‌های مختلف گرفته شده‌اند، مفید است.

برای فعال‌سازی این ویژگی، می‌توانید `logName` را در بخش `capabilities` تعریف کنید و اطمینان حاصل کنید که گزینه `formatImageName` در سرویس تست بصری به آن اشاره می‌کند. اینجا نحوه تنظیم آن آمده است:

```js
import path from "node:path";

// wdio.conf.ts
export const config = {
    // ...
    // =====
    // Setup
    // =====
    capabilities: [
        {
            browserName: 'chrome',
            'wdio-ics:options': {
                logName: 'chrome-mac-15', // نام سفارشی برای Chrome
            },
        }
        {
            browserName: 'firefox',
            'wdio-ics:options': {
                logName: 'firefox-mac-15', // نام سفارشی برای Firefox
            },
        }
    ],
    services: [
        [
            "visual",
            {
                // برخی گزینه‌ها، برای اطلاعات بیشتر به مستندات مراجعه کنید
                baselineFolder: path.join(process.cwd(), "tests", "baseline"),
                screenshotPath: path.join(process.cwd(), "tmp"),
                // فرمت زیر از `logName` از قابلیت‌ها استفاده می‌کند
                formatImageName: "{tag}-{logName}-{width}x{height}",
                // ... گزینه‌های بیشتر
            },
        ],
    ],
    // ...
};
```

#### نحوه کار
1. تنظیم `logName`:

    - در بخش `capabilities`، یک `logName` منحصر به فرد به هر مرورگر یا دستگاه اختصاص دهید. به عنوان مثال، `chrome-mac-15` تست‌های اجرا شده روی Chrome در macOS نسخه 15 را شناسایی می‌کند.

2. نامگذاری سفارشی تصویر:

    - گزینه `formatImageName` نام `logName` را در نام فایل‌های اسکرین‌شات ادغام می‌کند. به عنوان مثال، اگر `tag` صفحه اصلی باشد و وضوح `1920x1080` باشد، نام فایل حاصل ممکن است به این شکل باشد:

        `homepage-chrome-mac-15-1920x1080.png`

3. مزایای نامگذاری سفارشی:

    - تشخیص بین اسکرین‌شات‌های مرورگرها یا دستگاه‌های مختلف بسیار آسان‌تر می‌شود، به‌ویژه هنگام مدیریت پایه‌ها و رفع مشکلات تفاوت‌ها.

4. نکته درباره مقادیر پیش‌فرض:

    - اگر `logName` در قابلیت‌ها تنظیم نشده باشد، گزینه `formatImageName` آن را به عنوان یک رشته خالی در نام فایل‌ها نشان می‌دهد (`homepage--15-1920x1080.png`)

### WebdriverIO MultiRemote

ما همچنین از [MultiRemote](https://webdriver.io/docs/multiremote/) پشتیبانی می‌کنیم. برای اینکه این کار به درستی انجام شود، اطمینان حاصل کنید که `wdio-ics:options` را به قابلیت‌های خود اضافه کرده‌اید، همانطور که در زیر می‌بینید. این اطمینان می‌دهد که هر اسکرین‌شات نام منحصر به فرد خود را خواهد داشت.

[نوشتن تست‌های شما](/docs/visual-testing/writing-tests) در مقایسه با استفاده از [testrunner](https://webdriver.io/docs/testrunner) هیچ تفاوتی نخواهد داشت.

```js
// wdio.conf.js
export const config = {
    capabilities: {
        chromeBrowserOne: {
            capabilities: {
                browserName: "chrome",
                "goog:chromeOptions": {
                    args: ["disable-infobars"],
                },
                // این!!!
                "wdio-ics:options": {
                    logName: "chrome-latest-one",
                },
            },
        },
        chromeBrowserTwo: {
            capabilities: {
                browserName: "chrome",
                "goog:chromeOptions": {
                    args: ["disable-infobars"],
                },
                // این!!!
                "wdio-ics:options": {
                    logName: "chrome-latest-two",
                },
            },
        },
    },
};
```

### اجرای برنامه‌ای

در اینجا یک مثال حداقلی از نحوه استفاده از `@wdio/visual-service` از طریق گزینه‌های `remote` آمده است:

```js
import { remote } from "webdriverio";
import VisualService from "@wdio/visual-service";

let visualService = new VisualService({
    autoSaveBaseline: true,
});

const browser = await remote({
    logLevel: "silent",
    capabilities: {
        browserName: "chrome",
    },
});

// "شروع" سرویس برای افزودن دستورات سفارشی به `browser`
visualService.remoteSetup(browser);

await browser.url("https://webdriver.io/");

// یا از این برای ONLY ذخیره اسکرین‌شات استفاده کنید
await browser.saveFullPageScreen("examplePaged", {});

// یا از این برای اعتبارسنجی استفاده کنید. هر دو روش نیازی به ترکیب ندارند، FAQ را ببینید
await browser.checkFullPageScreen("examplePaged", {});

await browser.deleteSession();
```

### حرکت با کلید Tab در یک وب‌سایت

می‌توانید بررسی کنید آیا یک وب‌سایت با استفاده از کلید <kbd>TAB</kbd> صفحه کلید قابل دسترسی است. تست این بخش از دسترسی‌پذیری همیشه یک کار (دستی) زمان‌بر و نسبتاً سخت از طریق اتوماسیون بوده است.
با روش‌های `saveTabbablePage` و `checkTabbablePage`، اکنون می‌توانید خطوط و نقاط را روی وب‌سایت خود ترسیم کنید تا ترتیب Tab را تأیید کنید.

توجه داشته باشید که این فقط برای مرورگرهای دسکتاپ مفید است و **نه** برای دستگاه‌های موبایل. تمام مرورگرهای دسکتاپ از این ویژگی پشتیبانی می‌کنند.

:::note

این کار الهام گرفته از پست وبلاگ [Viv Richards](https://github.com/vivrichards600) درباره ["AUTOMATING PAGE TABABILITY (IS THAT A WORD?) WITH VISUAL TESTING"](https://vivrichards.co.uk/accessibility/automating-page-tab-flows-using-visual-testing-and-javascript) است.

نحوه انتخاب عناصر قابل استفاده با Tab بر اساس ماژول [tabbable](https://github.com/davidtheclark/tabbable) است. اگر مشکلی در رابطه با Tab وجود دارد، لطفاً [README.md](https://github.com/davidtheclark/tabbable/blob/master/README.md) و به‌ویژه بخش [More Details](https://github.com/davidtheclark/tabbable/blob/master/README.md#more-details) را بررسی کنید.

:::

#### چگونه کار می‌کند

هر دو روش یک عنصر `canvas` روی وب‌سایت شما ایجاد می‌کنند و خطوط و نقاط را رسم می‌کنند تا به شما نشان دهند کلید TAB کجا می‌رود اگر یک کاربر نهایی از آن استفاده کند. پس از آن، یک اسکرین‌شات صفحه کامل ایجاد می‌کند تا نمایی کلی از جریان کار به شما بدهد.

:::important

**از `saveTabbablePage` فقط زمانی استفاده کنید که نیاز به ایجاد یک اسکرین‌شات دارید و نمی‌خواهید آن را با تصویر **پایه** مقایسه کنید.**

:::

زمانی که می‌خواهید جریان Tab را با یک پایه مقایسه کنید، می‌توانید از روش `checkTabbablePage` استفاده کنید. **نیازی نیست** از هر دو روش با هم استفاده کنید. اگر تصویر پایه قبلاً ایجاد شده باشد، که می‌تواند با ارائه `autoSaveBaseline: true` هنگام راه‌اندازی سرویس به طور خودکار انجام شود،
`checkTabbablePage` ابتدا تصویر _واقعی_ را ایجاد می‌کند و سپس آن را با پایه مقایسه می‌کند.

##### گزینه‌ها

هر دو روش از همان گزینه‌های [`saveFullPageScreen`](https://github.com/wswebcreation/webdriver-image-comparison/blob/master/docs/OPTIONS.md#savefullpagescreen-or-savetabbablepage) یا
[`compareFullPageScreen`](https://github.com/wswebcreation/webdriver-image-comparison/blob/master/docs/OPTIONS.md#comparefullpagescreen-or-comparetabbablepage) استفاده می‌کنند.

#### مثال

این یک مثال از نحوه کار Tab روی [وب‌سایت آزمایشی](https://guinea-pig.webdriver.io/image-compare.html) ما است:

![WDIO tabbing example](/img/visual/tabbable-chrome-latest-1366x768.png)

### به‌روزرسانی خودکار اسنپ‌شات‌های بصری ناموفق

تصاویر پایه را از طریق خط فرمان با افزودن آرگومان `--update-visual-baseline` به‌روزرسانی کنید. این کار:

-   به طور خودکار اسکرین‌شات گرفته شده واقعی را کپی کرده و در پوشه پایه قرار می‌دهد
-   اگر تفاوت‌هایی وجود داشته باشد، تست را قبول می‌کند زیرا پایه به‌روز شده است

**استفاده:**

```sh
npm run test.local.desktop  --update-visual-baseline
```

هنگام اجرا در حالت لاگ info/debug، لاگ‌های زیر اضافه می‌شوند

```logs
[0-0] ..............
[0-0] #####################################################################################
[0-0]  INFO:
[0-0]  Updated the actual image to
[0-0]  /Users/wswebcreation/Git/wdio/visual-testing/localBaseline/chromel/demo-chrome-1366x768.png
[0-0] #####################################################################################
[0-0] ..........
```

## پشتیبانی از تایپ‌اسکریپت

این ماژول پشتیبانی از TypeScript را شامل می‌شود که به شما امکان می‌دهد از تکمیل خودکار، امنیت نوع و تجربه توسعه‌دهنده بهبود یافته هنگام استفاده از سرویس Visual Testing بهره‌مند شوید.

### مرحله 1: افزودن تعاریف نوع
برای اطمینان از اینکه TypeScript انواع ماژول را تشخیص می‌دهد، ورودی زیر را به فیلد types در tsconfig.json خود اضافه کنید:

```json
{
    "compilerOptions": {
        "types": ["@wdio/visual-service"]
    }
}
```

### مرحله 2: فعال‌سازی امنیت نوع برای گزینه‌های سرویس
برای اعمال بررسی نوع روی گزینه‌های سرویس، پیکربندی WebdriverIO خود را به‌روز کنید:

```ts
// wdio.conf.ts
import { join } from 'node:path';
// وارد کردن تعریف نوع
import type { VisualServiceOptions } from '@wdio/visual-service';

export const config = {
    // ...
    // =====
    // Setup
    // =====
    services: [
        [
            "visual",
            {
                // گزینه‌های سرویس
                baselineFolder: join(process.cwd(), './__snapshots__/'),
                formatImageName: '{tag}-{logName}-{width}x{height}',
                screenshotPath: join(process.cwd(), '.tmp/'),
            } satisfies VisualServiceOptions, // اطمینان از امنیت نوع
        ],
    ],
    // ...
};
```

## نیازمندی‌های سیستم

### نسخه 5 و بالاتر

برای نسخه 5 و بالاتر، این ماژول یک ماژول کاملاً مبتنی بر JavaScript است که بدون وابستگی‌های سیستمی اضافی فراتر از [نیازمندی‌های پروژه](/docs/gettingstarted#system-requirements) عمومی است. از [Jimp](https://github.com/jimp-dev/jimp) استفاده می‌کند که یک کتابخانه پردازش تصویر برای Node است که کاملاً در JavaScript نوشته شده و هیچ وابستگی بومی ندارد.

### نسخه 4 و پایین‌تر

برای نسخه 4 و پایین‌تر، این ماژول به [Canvas](https://github.com/Automattic/node-canvas)، یک پیاده‌سازی canvas برای Node.js متکی است. Canvas به [Cairo](https://cairographics.org/) وابسته است.

#### جزئیات نصب

به طور پیش‌فرض، باینری‌ها برای macOS، Linux و Windows در طول `npm install` پروژه شما دانلود می‌شوند. اگر سیستم عامل یا معماری پردازنده پشتیبانی شده ندارید، ماژول روی سیستم شما کامپایل می‌شود. این کار به چندین وابستگی از جمله Cairo و Pango نیاز دارد.

برای اطلاعات دقیق نصب، [ویکی node-canvas](https://github.com/Automattic/node-canvas/wiki/_pages) را ببینید. در زیر دستورالعمل‌های نصب یک‌خطی برای سیستم‌عامل‌های رایج آمده است. توجه داشته باشید که `libgif/giflib`، `librsvg` و `libjpeg` اختیاری هستند و فقط برای پشتیبانی از GIF، SVG و JPEG مورد نیاز هستند. Cairo نسخه 1.10.0 یا بالاتر مورد نیاز است.

<Tabs
defaultValue="osx"
values={[
{label: 'OS', value: 'osx'},
{label: 'Ubuntu', value: 'ubuntu'},
{label: 'Fedora', value: 'fedora'},
{label: 'Solaris', value: 'solaris'},
{label: 'OpenBSD', value: 'openbsd'},
{label: 'Window', value: 'windows'},
{label: 'Others', value: 'others'},
]}

> <TabItem value="osx">

     با استفاده از [Homebrew](https://brew.sh/):

     ```sh
     brew install pkg-config cairo pango libpng jpeg giflib librsvg pixman
     ```

    **Mac OS X v10.11+:** اگر اخیراً به Mac OS X v10.11+ به‌روزرسانی کرده‌اید و هنگام کامپایل با مشکل مواجه هستید، دستور زیر را اجرا کنید: `xcode-select --install`. درباره این مشکل بیشتر [در Stack Overflow](http://stackoverflow.com/a/32929012/148072) بخوانید.
    اگر Xcode 10.0 یا بالاتر نصب کرده‌اید، برای کامپایل از منبع به NPM 6.4.1 یا بالاتر نیاز دارید.

</TabItem>
<TabItem value="ubuntu">

    ```sh
    sudo apt-get install build-essential libcairo2-dev libpango1.0-dev libjpeg-dev libgif-dev librsvg2-dev
    ```

</TabItem>
<TabItem value="fedora">

    ```sh
    sudo yum install gcc-c++ cairo-devel pango-devel libjpeg-turbo-devel giflib-devel
    ```

</TabItem>
<TabItem value="solaris">

    ```sh
    pkgin install cairo pango pkg-config xproto renderproto kbproto xextproto
    ```

</TabItem>
<TabItem value="openbsd">

    ```sh
    doas pkg_add cairo pango png jpeg giflib
    ```

</TabItem>
<TabItem value="windows">

    [ویکی](https://github.com/Automattic/node-canvas/wiki/Installation:-Windows) را ببینید

</TabItem>
<TabItem value="others">

    [ویکی](https://github.com/Automattic/node-canvas/wiki) را ببینید

</TabItem>
</Tabs>