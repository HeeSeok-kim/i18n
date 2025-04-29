---
id: wdio-ocr-service
title: سرویس تست OCR
custom_edit_url: https://github.com/webdriverio/visual-testing/edit/main/README.md
---


> @wdio/ocr-service یک پکیج شخص ثالث است، برای اطلاعات بیشتر لطفاً به [GitHub](https://github.com/webdriverio/visual-testing) | [npm](https://www.npmjs.com/package/@wdio/ocr-service) مراجعه کنید

برای مستندات تست بصری با WebdriverIO، لطفاً به [مستندات](https://webdriver.io/docs/visual-testing) مراجعه کنید. این پروژه شامل تمام ماژول‌های مرتبط برای اجرای تست‌های بصری با WebdriverIO است. در دایرکتوری `./packages` موارد زیر را خواهید یافت:

-   `@wdio/visual-testing`: سرویس WebdriverIO برای یکپارچه‌سازی تست بصری
-   `webdriver-image-comparison`: یک ماژول مقایسه تصویر که می‌تواند برای چارچوب‌های مختلف تست خودکار NodeJS که از پروتکل WebDriver پشتیبانی می‌کنند، استفاده شود

## اجراکننده Storybook (بتا)

<details>
  <summary>برای مطالعه مستندات بیشتر درباره اجراکننده Storybook بتا کلیک کنید</summary>

> اجراکننده Storybook هنوز در مرحله بتا است، مستندات بعداً به صفحات مستندات [WebdriverIO](https://webdriver.io/docs/visual-testing) منتقل خواهد شد.

این ماژول اکنون از Storybook با یک اجراکننده بصری جدید پشتیبانی می‌کند. این اجراکننده به طور خودکار یک نمونه محلی/راه دور Storybook را اسکن می‌کند و اسکرین‌شات‌های عنصر را برای هر کامپوننت ایجاد می‌کند. این کار با اضافه کردن

```ts
export const config: WebdriverIO.Config = {
    // ...
    services: ["visual"],
    // ....
};
```

به `services` و اجرای `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook` از طریق خط فرمان انجام می‌شود.
به طور پیش‌فرض از Chrome در حالت headless به عنوان مرورگر استفاده می‌کند.

> [!NOTE]
>
> -   بیشتر گزینه‌های تست بصری برای اجراکننده Storybook نیز کار می‌کنند، به مستندات [WebdriverIO](https://webdriver.io/docs/visual-testing) مراجعه کنید.
> -   اجراکننده Storybook تمام قابلیت‌های شما را بازنویسی می‌کند و فقط می‌تواند روی مرورگرهایی که پشتیبانی می‌کند اجرا شود، به [`--browsers`](#browsers) مراجعه کنید.
> -   اجراکننده Storybook از پیکربندی موجود که از قابلیت‌های Multiremote استفاده می‌کند پشتیبانی نمی‌کند و خطا ایجاد می‌کند.
> -   اجراکننده Storybook فقط از وب دسکتاپ پشتیبانی می‌کند، نه وب موبایل.

### گزینه‌های سرویس اجراکننده Storybook

گزینه‌های سرویس می‌توانند به این صورت ارائه شوند

```ts
export const config: WebdriverIO.Config  = {
    // ...
    services: [
      [
        'visual',
        {
            // برخی گزینه‌های پیش‌فرض
            baselineFolder: join(process.cwd(), './__snapshots__/'),
            debug: true,
            // گزینه‌های storybook، به گزینه‌های cli برای توضیحات مراجعه کنید
            storybook: {
                additionalSearchParams: new URLSearchParams({foo: 'bar', abc: 'def'}),
                clip: false,
                clipSelector: ''#some-id,
                numShards: 4,
                // `skipStories` می‌تواند یک رشته ('example-button--secondary')،
                // یک آرایه (['example-button--secondary', 'example-button--small'])
                // یا یک regex باشد که باید به عنوان یک رشته ارائه شود ("/.*button.*/gm")
                skipStories: ['example-button--secondary', 'example-button--small'],
                url: 'https://www.bbc.co.uk/iplayer/storybook/',
                version: 6,
                // اختیاری - اجازه می‌دهد مسیر baselines را تغییر دهید. به طور پیش‌فرض baselines را بر اساس دسته و کامپوننت گروه‌بندی می‌کند (مثلاً forms/input/baseline.png)
                getStoriesBaselinePath: (category, component) => `path__${category}__${component}`,
            },
        },
      ],
    ],
    // ....
}
```

### گزینه‌های CLI اجراکننده Storybook

#### `--additionalSearchParams`

-   **نوع:** `string`
-   **اجباری:** خیر
-   **پیش‌فرض:** ''
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --additionalSearchParams="foo=bar&abc=def"`

پارامترهای جستجوی اضافی را به URL Storybook اضافه می‌کند.
برای اطلاعات بیشتر به مستندات [URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) مراجعه کنید. رشته باید یک رشته URLSearchParams معتبر باشد.

> [!NOTE]
> نیاز به علامت نقل قول دوگانه است تا از تفسیر `&` به عنوان جداکننده دستور جلوگیری شود.
> برای مثال با `--additionalSearchParams="foo=bar&abc=def"` URL Storybook زیر برای تست‌های داستان ایجاد می‌شود: `http://storybook.url/iframe.html?id=story-id&foo=bar&abc=def`.

#### `--browsers`

-   **نوع:** `string`
-   **اجباری:** خیر
-   **پیش‌فرض:** `chrome`، می‌توانید از `chrome|firefox|edge|safari` انتخاب کنید
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --browsers=chrome,firefox,edge,safari`
-   **توجه:** فقط از طریق CLI در دسترس است

از مرورگرهای ارائه شده برای گرفتن اسکرین‌شات کامپوننت استفاده می‌کند

> [!NOTE]
> اطمینان حاصل کنید که مرورگرهایی که می‌خواهید روی آن‌ها اجرا کنید، روی دستگاه محلی شما نصب شده‌اند

#### `--clip`

-   **نوع:** `boolean`
-   **اجباری:** خیر
-   **پیش‌فرض:** `true`
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --clip=false`

وقتی غیرفعال است، یک اسکرین‌شات از viewport ایجاد می‌کند. وقتی فعال است، اسکرین‌شات‌های عنصر بر اساس [`--clipSelector`](#clipselector) ایجاد می‌کند که فضای خالی اطراف اسکرین‌شات کامپوننت را کاهش می‌دهد و اندازه اسکرین‌شات را کاهش می‌دهد.

#### `--clipSelector`

-   **نوع:** `string`
-   **اجباری:** خیر
-   **پیش‌فرض:** `#storybook-root > :first-child` برای Storybook V7 و `#root > :first-child:not(script):not(style)` برای Storybook V6، همچنین به [`--version`](#version) مراجعه کنید
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --clipSelector="#some-id"`

این انتخابگر برای موارد زیر استفاده می‌شود:

-   انتخاب عنصر برای گرفتن اسکرین‌شات
-   انتظار برای قابل مشاهده شدن عنصر قبل از گرفتن اسکرین‌شات

#### `--devices`

-   **نوع:** `string`
-   **اجباری:** خیر
-   **پیش‌فرض:** می‌توانید از [`deviceDescriptors.ts`](https://github.com/webdriverio/visual-testing/blob/main/./packages/service/src/storybook/deviceDescriptors.ts) انتخاب کنید
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --devices="iPhone 14 Pro Max","Pixel 3 XL"`
-   **توجه:** فقط از طریق CLI در دسترس است

از دستگاه‌های ارائه شده که با [`deviceDescriptors.ts`](https://github.com/webdriverio/visual-testing/blob/main/./packages/service/src/storybook/deviceDescriptors.ts) مطابقت دارند برای گرفتن اسکرین‌شات‌های کامپوننت استفاده می‌کند

> [!NOTE]
>
> -   اگر تنظیمات دستگاهی را نیاز دارید، لطفاً یک [درخواست ویژگی](https://github.com/webdriverio/visual-testing/issues/new?assignees=&labels=&projects=&template=--feature-request.md) ارسال کنید
> -   این فقط با Chrome کار می‌کند:
>     -   اگر `--devices` را ارائه دهید، تمام نمونه‌های Chrome در حالت **شبیه‌سازی موبایل** اجرا می‌شوند
>     -   اگر مرورگرهای دیگری غیر از Chrome نیز ارائه دهید، مانند `--devices --browsers=firefox,safari,edge`، به طور خودکار Chrome را در حالت شبیه‌سازی موبایل اضافه می‌کند
> -   اجراکننده Storybook به طور پیش‌فرض اسکرین‌شات‌های عنصر ایجاد می‌کند، اگر می‌خواهید کل اسکرین‌شات شبیه‌سازی شده موبایل را ببینید، `--clip=false` را از طریق خط فرمان ارائه دهید
> -   نام فایل به عنوان مثال به صورت `__snapshots__/example/button/desktop_chrome/example-button--large-local-chrome-iPhone-14-Pro-Max-430x932-dpr-3.png` خواهد بود
> -   **[منبع:](https://chromedriver.chromium.org/mobile-emulation#h.p_ID_167)** تست یک وب‌سایت موبایل روی دسکتاپ با استفاده از شبیه‌سازی موبایل می‌تواند مفید باشد، اما تست‌کنندگان باید از تفاوت‌های ظریف زیادی آگاه باشند، مانند:
>     -   GPU کاملاً متفاوت، که ممکن است منجر به تغییرات بزرگ در عملکرد شود؛
>     -   رابط کاربری موبایل شبیه‌سازی نشده است (به ویژه، پنهان کردن نوار URL بر ارتفاع صفحه تأثیر می‌گذارد)؛
>     -   پاپ‌آپ ابهام‌زدایی (جایی که یکی از چند هدف لمسی را انتخاب می‌کنید) پشتیبانی نمی‌شود؛
>     -   بسیاری از APIهای سخت‌افزاری (برای مثال، رویداد orientationchange) در دسترس نیستند.

#### `--headless`

-   **نوع:** `boolean`
-   **اجباری:** خیر
-   **پیش‌فرض:** `true`
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --headless=false`
-   **توجه:** فقط از طریق CLI در دسترس است

این تست‌ها را به طور پیش‌فرض در حالت headless اجرا می‌کند (وقتی مرورگر از آن پشتیبانی می‌کند) یا می‌تواند غیرفعال شود

#### `--numShards`

-   **نوع:** `number`
-   **اجباری:** خیر
-   **پیش‌فرض:** `true`
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --numShards=10`

این تعداد نمونه‌های موازی خواهد بود که برای اجرای داستان‌ها استفاده می‌شود. این با `maxInstances` در فایل `wdio.conf` شما محدود می‌شود.

> [!IMPORTANT]
> هنگام اجرا در حالت `headless`، تعداد را به بیش از 20 افزایش ندهید تا از ناپایداری به دلیل محدودیت منابع جلوگیری شود

#### `--skipStories`

-   **نوع:** `string|regex`
-   **اجباری:** خیر
-   **پیش‌فرض:** null
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --skipStories="/.*button.*/gm"`

این می‌تواند:

-   یک رشته (`example-button--secondary,example-button--small`)
-   یا یک regex (`"/.*button.*/gm"`)

باشد تا داستان‌های خاصی را رد کند. از `id` داستان که در URL داستان قابل مشاهده است استفاده کنید. به عنوان مثال، `id` در این URL `http://localhost:6006/?path=/story/example-page--logged-out` برابر با `example-page--logged-out` است

#### `--url`

-   **نوع:** `string`
-   **اجباری:** خیر
-   **پیش‌فرض:** `http://127.0.0.1:6006`
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --url="https://example.com"`

URL جایی که نمونه Storybook شما میزبانی می‌شود.

#### `--version`

-   **نوع:** `number`
-   **اجباری:** خیر
-   **پیش‌فرض:** 7
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --version=6`

این نسخه Storybook است، به طور پیش‌فرض `7` است. این برای دانستن اینکه آیا [`clipSelector`](#clipselector) نسخه V6 باید استفاده شود، مورد نیاز است.

### تست تعاملی Storybook

تست تعاملی Storybook به شما امکان می‌دهد با کامپوننت خود با ایجاد اسکریپت‌های سفارشی با دستورات WDIO برای قرار دادن یک کامپوننت در یک وضعیت خاص تعامل داشته باشید. به عنوان مثال، به قطعه کد زیر نگاه کنید:

```ts
import { browser, expect } from "@wdio/globals";

describe("Storybook Interaction", () => {
    it("should create screenshots for the logged in state when it logs out", async () => {
        const componentId = "example-page--logged-in";
        await browser.waitForStorybookComponentToBeLoaded({ id: componentId });

        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-in-state`
        );
        await $("button=Log out").click();
        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-out-state`
        );
    });

    it("should create screenshots for the logged out state when it logs in", async () => {
        const componentId = "example-page--logged-out";
        await browser.waitForStorybookComponentToBeLoaded({ id: componentId });

        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-out-state`
        );
        await $("button=Log in").click();
        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-in-state`
        );
    });
});
```

دو تست روی دو کامپوننت مختلف اجرا می‌شوند. هر تست ابتدا یک وضعیت را تنظیم می‌کند و سپس یک اسکرین‌شات می‌گیرد. همچنین متوجه خواهید شد که یک دستور سفارشی جدید معرفی شده است که می‌توانید آن را [اینجا](#new-custom-command) بیابید.

فایل مشخصات بالا را می‌توان در یک پوشه ذخیره کرد و با دستور زیر به خط فرمان اضافه کرد:

```sh
pnpm run test.local.desktop.storybook.localhost -- --spec='tests/specs/storybook-interaction/*.ts'
```

اجراکننده Storybook ابتدا به طور خودکار نمونه Storybook شما را اسکن می‌کند و سپس تست‌های شما را به داستان‌هایی که باید مقایسه شوند، اضافه می‌کند. اگر نمی‌خواهید کامپوننت‌هایی که برای تست تعاملی استفاده می‌کنید دوبار مقایسه شوند، می‌توانید یک فیلتر اضافه کنید تا داستان‌های "پیش‌فرض" را از اسکن حذف کنید با ارائه فیلتر [`--skipStories`](#--skipstories). این به شکل زیر خواهد بود:

```sh
pnpm run test.local.desktop.storybook.localhost -- --skipStories="/example-page.*/gm" --spec='tests/specs/storybook-interaction/*.ts'
```

### دستور سفارشی جدید

یک دستور سفارشی جدید به نام `browser.waitForStorybookComponentToBeLoaded({ id: 'componentId' })` به شیء `browser/driver` اضافه می‌شود که به طور خودکار کامپوننت را بارگذاری می‌کند و منتظر می‌ماند تا کار تمام شود، بنابراین نیازی به استفاده از روش `browser.url('url.com')` ندارید. می‌توان از آن به این صورت استفاده کرد

```ts
import { browser, expect } from "@wdio/globals";

describe("Storybook Interaction", () => {
    it("should create screenshots for the logged in state when it logs out", async () => {
        const componentId = "example-page--logged-in";
        await browser.waitForStorybookComponentToBeLoaded({ id: componentId });

        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-in-state`
        );
        await $("button=Log out").click();
        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-out-state`
        );
    });

    it("should create screenshots for the logged out state when it logs in", async () => {
        const componentId = "example-page--logged-out";
        await browser.waitForStorybookComponentToBeLoaded({ id: componentId });

        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-out-state`
        );
        await $("button=Log in").click();
        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-in-state`
        );
    });
});
```

گزینه‌ها عبارتند از:

#### `additionalSearchParams`

-   **نوع:** [`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)
-   **اجباری:** خیر
-   **پیش‌فرض:** `new URLSearchParams()`
-   **مثال:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    additionalSearchParams: new URLSearchParams({ foo: "bar", abc: "def" }),
    id: "componentId",
});
```

این پارامترهای جستجوی اضافی را به URL Storybook اضافه می‌کند، در مثال بالا URL به صورت `http://storybook.url/iframe.html?id=story-id&foo=bar&abc=def` خواهد بود.
برای اطلاعات بیشتر به مستندات [URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) مراجعه کنید.

#### `clipSelector`

-   **نوع:** `string`
-   **اجباری:** خیر
-   **پیش‌فرض:** `#storybook-root > :first-child` برای Storybook V7 و `#root > :first-child:not(script):not(style)` برای Storybook V6
-   **مثال:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    clipSelector: "#your-selector",
    id: "componentId",
});
```

این انتخابگر برای موارد زیر استفاده می‌شود:

-   انتخاب عنصر برای گرفتن اسکرین‌شات
-   انتظار برای قابل مشاهده شدن عنصر قبل از گرفتن اسکرین‌شات

#### `id`

-   **نوع:** `string`
-   **اجباری:** بله
-   **مثال:**

```ts
await browser.waitForStorybookComponentToBeLoaded({ '#your-selector', id: 'componentId' })
```

از `id` داستان که در URL داستان قابل مشاهده است استفاده کنید. به عنوان مثال، `id` در این URL `http://localhost:6006/?path=/story/example-page--logged-out` برابر با `example-page--logged-out` است

#### `timeout`

-   **نوع:** `number`
-   **اجباری:** خیر
-   **پیش‌فرض:** 1100 میلی‌ثانیه
-   **مثال:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    id: "componentId",
    timeout: 20000,
});
```

حداکثر زمان انتظار برای قابل مشاهده شدن یک کامپوننت پس از بارگذاری در صفحه

#### `url`

-   **نوع:** `string`
-   **اجباری:** خیر
-   **پیش‌فرض:** `http://127.0.0.1:6006`
-   **مثال:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    id: "componentId",
    url: "https://your.url",
});
```

URL جایی که نمونه Storybook شما میزبانی می‌شود.

</details>

## مشارکت

### به‌روزرسانی پکیج‌ها

می‌توانید پکیج‌ها را با یک ابزار CLI ساده به‌روزرسانی کنید. مطمئن شوید که تمام وابستگی‌ها را نصب کرده‌اید، سپس می‌توانید اجرا کنید

```sh
pnpm update.packages
```

این یک CLI را فعال می‌کند که از شما سؤالات زیر را می‌پرسد

```logs
==========================
🤖 Package update Wizard 🧙
==========================

? Which version target would you like to update to? (Minor|Latest)
? Do you want to update the package.json files? (Y/n)
? Do you want to remove all "node_modules" and reinstall dependencies? (Y/n)
? Would you like reinstall the dependencies? (Y/n)
```

این منجر به لاگ‌های زیر می‌شود

<details>
    <summary>برای دیدن نمونه‌ای از لاگ‌ها باز کنید</summary>
    
```logs
==========================
🤖 Package update Wizard 🧙
==========================

? Which version target would you like to update to? Minor
? Do you want to update the package.json files? yes
Updating root 'package.json' for minor updates...
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/package.json
[====================] 38/38 100%

@typescript-eslint/eslint-plugin ^8.7.0 → ^8.8.0
@typescript-eslint/parser ^8.7.0 → ^8.8.0
@typescript-eslint/utils ^8.7.0 → ^8.8.0
@vitest/coverage-v8 ^2.1.1 → ^2.1.2
vitest ^2.1.1 → ^2.1.2

Run pnpm install to install new versions.
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing/packages/ocr-service...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/packages/ocr-service/package.json
[====================] 11/11 100%

All dependencies match the minor package versions :)
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing/packages/visual-reporter...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/packages/visual-reporter/package.json
[====================] 11/11 100%

eslint-config-next 14.2.13 → 14.2.14
next 14.2.13 → 14.2.14

Run pnpm install to install new versions.
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing/packages/visual-service...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/packages/visual-service/package.json
[====================] 5/5 100%

All dependencies match the minor package versions :)
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing/packages/webdriver-image-comparison...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/packages/webdriver-image-comparison/package.json
[====================] 8/8 100%

All dependencies match the minor package versions :)
? Do you want to remove all "node_modules" and reinstall dependencies? yes
Removing root dependencies in /Users/wswebcreation/Git/wdio/visual-testing...
Removing dependencies in ocr-service...
Removing dependencies in visual-reporter...
Removing dependencies in visual-service...
Removing dependencies in webdriver-image-comparison...
? Would you like reinstall the dependencies? yes
Installing dependencies in /Users/wswebcreation/Git/wdio/visual-testing...

> @wdio/visual-testing-monorepo@ pnpm.install.workaround /Users/wswebcreation/Git/wdio/visual-testing
> pnpm install --shamefully-hoist

Scope: all 5 workspace projects
Lockfile is up to date, resolution step is skipped
Packages: +1274
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Progress: resolved 1274, reused 1265, downloaded 0, added 1274, done

dependencies:

-   @wdio/ocr-service 2.0.0 <- packages/ocr-service
-   @wdio/visual-service 6.0.0 <- packages/visual-service

devDependencies:

-   @changesets/cli 2.27.8
-   @inquirer/prompts 5.5.0
-   @tsconfig/node20 20.1.4
-   @types/eslint 9.6.1
-   @types/jsdom 21.1.7
-   @types/node 20.16.4
-   @types/react 18.3.5
-   @types/react-dom 18.3.0
-   @types/xml2js 0.4.14
-   @typescript-eslint/eslint-plugin 8.8.0
-   @typescript-eslint/parser 8.8.0
-   @typescript-eslint/utils 8.8.0
-   @vitest/coverage-v8 2.1.2
-   @wdio/appium-service 9.1.2
-   @wdio/cli 9.1.2
-   @wdio/globals 9.1.2
-   @wdio/local-runner 9.1.2
-   @wdio/mocha-framework 9.1.2
-   @wdio/sauce-service 9.1.2
-   @wdio/shared-store-service 9.1.2
-   @wdio/spec-reporter 9.1.2
-   @wdio/types 9.1.2
-   eslint 9.11.1
-   eslint-plugin-import 2.30.0
-   eslint-plugin-unicorn 55.0.0
-   eslint-plugin-wdio 9.0.8
-   husky 9.1.6
-   jsdom 25.0.1
-   pnpm-run-all2 6.2.3
-   release-it 17.6.0
-   rimraf 6.0.1
-   saucelabs 8.0.0
-   ts-node 10.9.2
-   typescript 5.6.2
-   vitest 2.1.2
-   webdriverio 9.1.2

. prepare$ husky
└─ Done in 204ms
Done in 9.5s
All packages updated!

````

</details>

### سؤالات

لطفاً به سرور [Discord](https://discord.webdriver.io) ما بپیوندید اگر سؤال یا مشکلی در مشارکت در این پروژه دارید. مشارکت‌کنندگان ما را در کانال `🙏-contributing` پیدا کنید.

### مشکلات

اگر سؤال، باگ یا درخواست ویژگی دارید، لطفاً یک مشکل ثبت کنید. قبل از ثبت یک مشکل، لطفاً آرشیو مشکلات را جستجو کنید تا از موارد تکراری جلوگیری شود و [سؤالات متداول](https://webdriver.io/docs/visual-testing/faq/) را بخوانید.

اگر نمی‌توانید آن را در آنجا پیدا کنید می‌توانید یک مشکل ثبت کنید که در آن می‌توانید موارد زیر را ارسال کنید:

-   🐛**گزارش باگ**: یک گزارش ایجاد کنید تا به ما در بهبود کمک کنید
-   📖**مستندات**: پیشنهاد بهبود یا گزارش مستندات ناقص/مبهم.
-   💡**درخواست ویژگی**: یک ایده برای این ماژول پیشنهاد دهید.
-   💬**سؤال**: سؤالات خود را بپرسید.

### روند توسعه

برای ایجاد PR برای این پروژه و شروع مشارکت، این راهنمای گام به گام را دنبال کنید:

-   پروژه را fork کنید.
-   پروژه را در جایی از کامپیوتر خود کلون کنید

    ```sh
    $ git clone https://github.com/webdriverio/visual-testing.git
    ```

-   به دایرکتوری بروید و پروژه را راه‌اندازی کنید

    ```sh
    $ cd visual-testing
    $ corepack enable
    $ pnpm pnpm.install.workaround
    ```

-   حالت نظارت را اجرا کنید که به طور خودکار کد را ترجمه می‌کند

    ```sh
    $ pnpm watch
    ```

    برای ساخت پروژه، اجرا کنید:

    ```sh
    $ pnpm build
    ```

-   اطمینان حاصل کنید که تغییرات شما هیچ تستی را خراب نمی‌کند، اجرا کنید:

    ```sh
    $ pnpm test
    ```

این پروژه از [changesets](https://github.com/changesets/changesets) برای ایجاد خودکار گزارش‌های تغییر و انتشار نسخه‌ها استفاده می‌کند.

### تست

چندین تست باید اجرا شوند تا بتوان ماژول را آزمایش کرد. هنگام افزودن PR، تمام تست‌ها باید حداقل تست‌های محلی را پاس کنند. هر PR به طور خودکار در برابر Sauce Labs آزمایش می‌شود، به [خط لوله GitHub Actions ما](https://github.com/webdriverio/visual-testing/actions/workflows/tests.yml) مراجعه کنید. قبل از تأیید PR، مشارکت‌کنندگان اصلی PR را در برابر شبیه‌سازها/امولاتورها / دستگاه‌های واقعی آزمایش خواهند کرد.

#### تست محلی

ابتدا، یک خط پایه محلی باید ایجاد شود. این کار را می‌توان با دستور زیر انجام داد:

```sh
// با پروتکل webdriver
$ pnpm run test.local.init
````

این دستور یک پوشه به نام `localBaseline` ایجاد می‌کند که تمام تصاویر خط پایه را نگه می‌دارد.

سپس اجرا کنید:

```sh
// با پروتکل webdriver
pnpm run test.local.desktop
```

این تمام تست‌ها را روی یک ماشین محلی با Chrome اجرا می‌کند.

#### تست محلی اجراکننده Storybook (بتا)

ابتدا، یک خط پایه محلی باید ایجاد شود. این کار را می‌توان با دستور زیر انجام داد:

```sh
pnpm run test.local.desktop.storybook
```

این تست‌های Storybook را با Chrome در حالت headless در برابر یک مخزن نمایشی Storybook واقع در https://govuk-react.github.io/govuk-react/ اجرا می‌کند.

برای اجرای تست‌ها با مرورگرهای بیشتر می‌توانید اجرا کنید

```sh
pnpm run test.local.desktop.storybook -- --browsers=chrome,firefox,edge,safari
```

> [!NOTE]
> اطمینان حاصل کنید که مرورگرهایی که می‌خواهید روی آن‌ها اجرا کنید، روی دستگاه محلی شما نصب شده‌اند

#### تست CI با Sauce Labs (برای PR مورد نیاز نیست)

دستور زیر برای آزمایش ساخت در GitHub Actions استفاده می‌شود، فقط می‌تواند در آنجا استفاده شود و نه برای توسعه محلی.

```
$ pnpm run test.saucelabs
```

این در برابر بسیاری از پیکربندی‌ها آزمایش می‌کند که می‌توان آنها را [اینجا](https://github.com/webdriverio/visual-testing/blob/main/./tests/configs/wdio.saucelabs.web.conf.ts) یافت.
تمام PRها به طور خودکار در برابر Sauce Labs بررسی می‌شوند.

## انتشار

برای انتشار یک نسخه از هر یک از پکیج‌های ذکر شده در بالا، کارهای زیر را انجام دهید:

-   خط لوله [انتشار](https://github.com/webdriverio/visual-testing/actions/workflows/release.yml) را فعال کنید
-   یک PR انتشار تولید می‌شود، این را برای بررسی و تأیید توسط یک عضو دیگر WebdriverIO ارائه دهید
-   PR را ادغام کنید
-   خط لوله [انتشار](https://github.com/webdriverio/visual-testing/actions/workflows/release.yml) را دوباره فعال کنید
-   یک نسخه جدید باید منتشر شود 🎉

## اعتبارات

`@wdio/visual-testing` از یک مجوز منبع باز از [LambdaTest](https://www.lambdatest.com/) و [Sauce Labs](https://saucelabs.com/) استفاده می‌کند.