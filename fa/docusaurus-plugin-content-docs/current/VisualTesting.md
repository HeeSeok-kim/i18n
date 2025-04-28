---
id: visual-testing
title: تست بصری
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

## چه کاری می‌تواند انجام دهد؟

WebdriverIO امکان مقایسه تصویر روی صفحه‌نمایش‌ها، عناصر یا صفحه کامل را فراهم می‌کند برای

-   🖥️ مرورگرهای دسکتاپ (کروم / فایرفاکس / سافاری / مایکروسافت اج)
-   📱 مرورگرهای موبایل / تبلت (کروم در شبیه‌سازهای اندروید / سافاری در شبیه‌سازهای iOS / شبیه‌سازها / دستگاه‌های واقعی) از طریق Appium
-   📱 اپلیکیشن‌های بومی (شبیه‌سازهای اندروید / شبیه‌سازهای iOS / دستگاه‌های واقعی) از طریق Appium (🌟 **جدید** 🌟)
-   📳 اپلیکیشن‌های هیبریدی از طریق Appium

از طریق [`@wdio/visual-service`](https://www.npmjs.com/package/@wdio/visual-service) که یک سرویس سبک WebdriverIO است.

این به شما امکان می‌دهد:

-   ذخیره یا مقایسه **صفحه‌نمایش‌ها/عناصر/صفحه کامل** با یک خط پایه
-   به طور خودکار **یک خط پایه ایجاد کنید** وقتی خط پایه‌ای وجود ندارد
-   **مسدود کردن نواحی سفارشی** و حتی **به طور خودکار حذف** نوار وضعیت و یا نوار ابزار (فقط موبایل) در هنگام مقایسه
-   افزایش ابعاد اسکرین‌شات‌های عناصر
-   **پنهان کردن متن** در هنگام مقایسه وب‌سایت برای:
    -   **بهبود ثبات** و جلوگیری از ناپایداری رندر فونت
    -   تمرکز فقط بر روی **لایه** یک وب‌سایت
-   استفاده از **روش‌های مقایسه مختلف** و مجموعه‌ای از **تطبیق‌دهنده‌های اضافی** برای آزمون‌های خواناتر
-   بررسی چگونگی **پشتیبانی وب‌سایت شما از جابجایی با کلید Tab**، همچنین ببینید [جابجایی با Tab در یک وب‌سایت](#tabbing-through-a-website)
-   و بسیاری موارد دیگر، به گزینه‌های [سرویس](./visual-testing/service-options) و [متد](./visual-testing/method-options) مراجعه کنید

این سرویس یک ماژول سبک برای بازیابی داده‌ها و اسکرین‌شات‌های مورد نیاز برای تمام مرورگرها/دستگاه‌ها است. قدرت مقایسه از [ResembleJS](https://github.com/Huddle/Resemble.js) می‌آید. اگر می‌خواهید تصاویر را به صورت آنلاین مقایسه کنید، می‌توانید [ابزار آنلاین](http://rsmbl.github.io/Resemble.js/) را بررسی کنید.

:::info نکته برای اپلیکیشن‌های بومی/هیبریدی
متدهای `saveScreen`، `saveElement`، `checkScreen`، `checkElement` و تطبیق‌دهنده‌های `toMatchScreenSnapshot` و `toMatchElementSnapshot` می‌توانند برای اپلیکیشن‌های بومی/محتوا استفاده شوند.

لطفاً از ویژگی `isHybridApp:true` در تنظیمات سرویس خود استفاده کنید، هنگامی که می‌خواهید از آن برای اپلیکیشن‌های هیبریدی استفاده کنید.
:::

## نصب

ساده‌ترین راه این است که `@wdio/visual-service` را به عنوان یک وابستگی توسعه در `package.json` خود نگه دارید، از طریق:

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
                // Some options, see the docs for more
                baselineFolder: path.join(process.cwd(), "tests", "baseline"),
                formatImageName: "{tag}-{logName}-{width}x{height}",
                screenshotPath: path.join(process.cwd(), "tmp"),
                savePerInstance: true,
                // ... more options
            },
        ],
    ],
    // ...
};
```

گزینه‌های سرویس بیشتر را می‌توانید [اینجا](/docs/visual-testing/service-options) پیدا کنید.

پس از تنظیم در پیکربندی WebdriverIO خود، می‌توانید ادامه دهید و تأییدیه‌های بصری را به [آزمون‌های خود](/docs/visual-testing/writing-tests) اضافه کنید.

### قابلیت‌ها
برای استفاده از ماژول تست بصری، **نیازی به افزودن گزینه‌های اضافی به قابلیت‌های خود ندارید**. با این حال، در برخی موارد، ممکن است بخواهید متادیتای اضافی به تست‌های بصری خود اضافه کنید، مانند `logName`.

`logName` به شما امکان می‌دهد یک نام سفارشی به هر قابلیت اختصاص دهید، که سپس می‌تواند در نام‌های فایل تصویر گنجانده شود. این به ویژه برای تمایز بین اسکرین‌شات‌های گرفته شده در مرورگرها، دستگاه‌ها یا پیکربندی‌های مختلف مفید است.

برای فعال کردن این امکان، می‌توانید `logName` را در بخش `capabilities` تعریف کنید و اطمینان حاصل کنید که گزینه `formatImageName` در سرویس تست بصری به آن اشاره دارد. اینجا نحوه تنظیم آن آمده است:

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
                logName: 'chrome-mac-15', // Custom log name for Chrome
            },
        }
        {
            browserName: 'firefox',
            'wdio-ics:options': {
                logName: 'firefox-mac-15', // Custom log name for Firefox
            },
        }
    ],
    services: [
        [
            "visual",
            {
                // Some options, see the docs for more
                baselineFolder: path.join(process.cwd(), "tests", "baseline"),
                screenshotPath: path.join(process.cwd(), "tmp"),
                // The format below will use the `logName` from capabilities
                formatImageName: "{tag}-{logName}-{width}x{height}",
                // ... more options
            },
        ],
    ],
    // ...
};
```

#### نحوه کارکرد
1. تنظیم `logName`:

    - در بخش `capabilities`، یک `logName` منحصر به فرد به هر مرورگر یا دستگاه اختصاص دهید. به عنوان مثال، `chrome-mac-15` تست‌های در حال اجرا روی کروم در نسخه 15 macOS را شناسایی می‌کند.

2. نامگذاری سفارشی تصویر:

    - گزینه `formatImageName` نام `logName` را در نام‌های فایل اسکرین‌شات ادغام می‌کند. به عنوان مثال، اگر `tag` صفحه اصلی و وضوح `1920x1080` باشد، نام فایل حاصل ممکن است به این شکل باشد:

        `homepage-chrome-mac-15-1920x1080.png`

3. مزایای نامگذاری سفارشی:

    - تمایز بین اسکرین‌شات‌های گرفته شده از مرورگرها یا دستگاه‌های مختلف بسیار آسان‌تر می‌شود، به ویژه هنگام مدیریت خط پایه و اشکال‌زدایی تناقض‌ها.

4. نکته در مورد پیش‌فرض‌ها:

    - اگر `logName` در قابلیت‌ها تنظیم نشده باشد، گزینه `formatImageName` آن را به عنوان یک رشته خالی در نام‌های فایل نشان خواهد داد (`homepage--15-1920x1080.png`)

### WebdriverIO MultiRemote

ما همچنین از [MultiRemote](https://webdriver.io/docs/multiremote/) پشتیبانی می‌کنیم. برای اینکه این کار به درستی انجام شود، مطمئن شوید که `wdio-ics:options` را به قابلیت‌های خود اضافه کرده‌اید، همانطور که در زیر می‌بینید. این اطمینان حاصل می‌کند که هر اسکرین‌شات نام منحصر به فرد خود را خواهد داشت.

[نوشتن آزمون‌های شما](/docs/visual-testing/writing-tests) در مقایسه با استفاده از [testrunner](https://webdriver.io/docs/testrunner) متفاوت نخواهد بود

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
                // THIS!!!
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
                // THIS!!!
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

// "Start" the service to add the custom commands to the `browser`
visualService.remoteSetup(browser);

await browser.url("https://webdriver.io/");

// or use this for ONLY saving a screenshot
await browser.saveFullPageScreen("examplePaged", {});

// or use this for validating. Both methods don't need to be combined, see the FAQ
await browser.checkFullPageScreen("examplePaged", {});

await browser.deleteSession();
```

### جابجایی با Tab در یک وب‌سایت

می‌توانید بررسی کنید که آیا یک وب‌سایت با استفاده از کلید <kbd>TAB</kbd> صفحه‌کلید قابل دسترسی است. آزمایش این بخش از قابلیت دسترسی همیشه کاری زمان‌بر (دستی) و انجام آن از طریق اتوماسیون بسیار دشوار بوده است.
با متدهای `saveTabbablePage` و `checkTabbablePage`، اکنون می‌توانید خطوط و نقاط را روی وب‌سایت خود ترسیم کنید تا ترتیب جابجایی با Tab را تأیید کنید.

توجه داشته باشید که این فقط برای مرورگرهای دسکتاپ مفید است و **نه** برای دستگاه‌های تلفن همراه. تمام مرورگرهای دسکتاپ از این ویژگی پشتیبانی می‌کنند.

:::note

این کار از پست وبلاگ [Viv Richards](https://github.com/vivrichards600) با عنوان ["AUTOMATING PAGE TABABILITY (IS THAT A WORD?) WITH VISUAL TESTING"](https://vivrichards.co.uk/accessibility/automating-page-tab-flows-using-visual-testing-and-javascript) الهام گرفته شده است.

نحوه انتخاب عناصر قابل جابجایی با Tab براساس ماژول [tabbable](https://github.com/davidtheclark/tabbable) است. اگر مشکلی در مورد Tab وجود دارد، لطفاً [README.md](https://github.com/davidtheclark/tabbable/blob/master/README.md) و به ویژه بخش [More Details](https://github.com/davidtheclark/tabbable/blob/master/README.md#more-details) را بررسی کنید.

:::

#### چگونه کار می‌کند

هر دو متد یک عنصر `canvas` روی وب‌سایت شما ایجاد می‌کنند و خطوط و نقاط را ترسیم می‌کنند تا به شما نشان دهند که Tab شما به کجا می‌رود اگر یک کاربر نهایی از آن استفاده کند. پس از آن، یک اسکرین‌شات صفحه کامل ایجاد می‌کند تا دید خوبی از جریان به شما بدهد.

:::important

**از `saveTabbablePage` فقط زمانی استفاده کنید که نیاز به ایجاد اسکرین‌شات دارید و نمی‌خواهید آن را با تصویر **خط پایه** مقایسه کنید.**

:::

وقتی می‌خواهید جریان Tab را با یک خط پایه مقایسه کنید، می‌توانید از متد `checkTabbablePage` استفاده کنید. **نیازی نیست** دو متد را با هم استفاده کنید. اگر قبلاً یک تصویر خط پایه ایجاد شده باشد، که می‌تواند با ارائه `autoSaveBaseline: true` هنگام راه‌اندازی سرویس به طور خودکار انجام شود،
`checkTabbablePage` ابتدا تصویر _واقعی_ را ایجاد می‌کند و سپس آن را با خط پایه مقایسه می‌کند.

##### گزینه‌ها

هر دو متد از همان گزینه‌های [`saveFullPageScreen`](https://github.com/wswebcreation/webdriver-image-comparison/blob/master/docs/OPTIONS.md#savefullpagescreen-or-savetabbablepage) یا
[`compareFullPageScreen`](https://github.com/wswebcreation/webdriver-image-comparison/blob/master/docs/OPTIONS.md#comparefullpagescreen-or-comparetabbablepage) استفاده می‌کنند.

#### مثال

این یک مثال از نحوه کار Tab روی [وب‌سایت خوکچه هندی](https://guinea-pig.webdriver.io/image-compare.html) ما است:

![WDIO tabbing example](/img/visual/tabbable-chrome-latest-1366x768.png)

### به‌روزرسانی خودکار اسنپ‌شات‌های بصری ناموفق

تصاویر خط پایه را از طریق خط فرمان با افزودن آرگومان `--update-visual-baseline` به‌روزرسانی کنید. این کار:

-   به طور خودکار اسکرین‌شات گرفته شده را کپی و در پوشه خط پایه قرار می‌دهد
-   اگر تفاوت‌هایی وجود دارد، آزمون را موفق می‌داند زیرا خط پایه به‌روز شده است

**استفاده:**

```sh
npm run test.local.desktop  --update-visual-baseline
```

هنگام اجرای گزارش‌های اطلاعات/اشکال‌زدایی، گزارش‌های زیر را مشاهده خواهید کرد

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

این ماژول دارای پشتیبانی از تایپ‌اسکریپت است که به شما امکان می‌دهد از تکمیل خودکار، ایمنی نوع و تجربه توسعه‌دهنده بهتری هنگام استفاده از سرویس تست بصری بهره‌مند شوید.

### مرحله 1: افزودن تعاریف نوع
برای اطمینان از اینکه تایپ‌اسکریپت انواع ماژول را تشخیص می‌دهد، ورودی زیر را به فیلد types در tsconfig.json خود اضافه کنید:

```json
{
    "compilerOptions": {
        "types": ["@wdio/visual-service"]
    }
}
```

### مرحله 2: فعال‌سازی ایمنی نوع برای گزینه‌های سرویس
برای اعمال بررسی نوع روی گزینه‌های سرویس، پیکربندی WebdriverIO خود را به‌روزرسانی کنید:

```ts
// wdio.conf.ts
import { join } from 'node:path';
// Import the type definition
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
                // Service options
                baselineFolder: join(process.cwd(), './__snapshots__/'),
                formatImageName: '{tag}-{logName}-{width}x{height}',
                screenshotPath: join(process.cwd(), '.tmp/'),
            } satisfies VisualServiceOptions, // Ensures type safety
        ],
    ],
    // ...
};
```

## نیازمندی‌های سیستم

### نسخه 5 و بالاتر

برای نسخه 5 و بالاتر، این ماژول یک ماژول کاملاً مبتنی بر جاوااسکریپت است که هیچ وابستگی سیستمی اضافی فراتر از [نیازمندی‌های پروژه](/docs/gettingstarted#system-requirements) ندارد. از [Jimp](https://github.com/jimp-dev/jimp) استفاده می‌کند که یک کتابخانه پردازش تصویر برای Node است که کاملاً در جاوااسکریپت نوشته شده، بدون هیچ وابستگی بومی.

### نسخه 4 و پایین‌تر

برای نسخه 4 و پایین‌تر، این ماژول به [Canvas](https://github.com/Automattic/node-canvas) متکی است، یک پیاده‌سازی canvas برای Node.js. Canvas به [Cairo](https://cairographics.org/) وابسته است.

#### جزئیات نصب

به طور پیش‌فرض، باینری‌های برای macOS، لینوکس و ویندوز در طول نصب `npm install` پروژه شما دانلود خواهند شد. اگر سیستم عامل یا معماری پردازنده پشتیبانی شده ندارید، ماژول روی سیستم شما کامپایل خواهد شد. این نیاز به چندین وابستگی دارد، از جمله Cairo و Pango.

برای اطلاعات نصب دقیق، به [ویکی node-canvas](https://github.com/Automattic/node-canvas/wiki/_pages) مراجعه کنید. در زیر دستورالعمل‌های نصب تک خطی برای سیستم‌های عامل رایج آمده است. توجه داشته باشید که `libgif/giflib`، `librsvg` و `libjpeg` اختیاری هستند و فقط برای پشتیبانی از GIF، SVG و JPEG مورد نیاز هستند. Cairo نسخه 1.10.0 یا بالاتر مورد نیاز است.

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

    **Mac OS X v10.11+:** اگر اخیراً به Mac OS X v10.11+ به‌روزرسانی کرده‌اید و هنگام کامپایل با مشکل مواجه هستید، دستور زیر را اجرا کنید: `xcode-select --install`. درباره این مشکل در [Stack Overflow](http://stackoverflow.com/a/32929012/148072) بیشتر بخوانید.
    اگر Xcode 10.0 یا بالاتر نصب کرده‌اید، برای ساخت از منبع به NPM 6.4.1 یا بالاتر نیاز دارید.

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

    به [ویکی](https://github.com/Automattic/node-canvas/wiki/Installation:-Windows) مراجعه کنید

</TabItem>
<TabItem value="others">

    به [ویکی](https://github.com/Automattic/node-canvas/wiki) مراجعه کنید

</TabItem>
</Tabs>