---
id: wdio-visual-service
title: خدمة مقارنة الصور (اختبار التراجع البصري)
custom_edit_url: https://github.com/webdriverio/visual-testing/edit/main/README.md
---


> @wdio/visual-service هي حزمة طرف ثالث، لمزيد من المعلومات يرجى مراجعة [GitHub](https://github.com/webdriverio/visual-testing) | [npm](https://www.npmjs.com/package/@wdio/visual-service)

للحصول على وثائق حول الاختبار البصري مع WebdriverIO، يرجى الرجوع إلى [الوثائق](https://webdriver.io/docs/visual-testing). يحتوي هذا المشروع على جميع الوحدات ذات الصلة لإجراء اختبارات بصرية باستخدام WebdriverIO. في دليل `./packages` ستجد:

-   `@wdio/visual-testing`: خدمة WebdriverIO لدمج الاختبار البصري
-   `webdriver-image-comparison`: وحدة مقارنة الصور التي يمكن استخدامها لأطر اختبار أتمتة NodeJS المختلفة التي تدعم بروتوكول WebDriver

## مشغل Storybook (تجريبي)

<details>
  <summary>انقر لمعرفة المزيد من الوثائق حول مشغل Storybook التجريبي</summary>

> مشغل Storybook لا يزال في المرحلة التجريبية، وسيتم نقل الوثائق لاحقًا إلى صفحات وثائق [WebdriverIO](https://webdriver.io/docs/visual-testing).

تدعم هذه الوحدة الآن Storybook مع مشغل Visual جديد. يقوم هذا المشغل تلقائيًا بمسح مثيل Storybook محلي/بعيد وسينشئ لقطات شاشة للعناصر لكل مكون. يمكن القيام بذلك عن طريق إضافة

```ts
export const config: WebdriverIO.Config = {
    // ...
    services: ["visual"],
    // ....
};
```

إلى `services` الخاصة بك وتشغيل `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook` من خلال سطر الأوامر.
سيستخدم Chrome في وضع headless كمتصفح افتراضي.

> [!NOTE]
>
> -   ستعمل معظم خيارات الاختبار البصري أيضًا مع مشغل Storybook، راجع وثائق [WebdriverIO](https://webdriver.io/docs/visual-testing).
> -   سيقوم مشغل Storybook بتجاوز جميع إمكانياتك ويمكنه التشغيل فقط على المتصفحات التي يدعمها، انظر [`--browsers`](#browsers).
> -   لا يدعم مشغل Storybook التكوين الحالي الذي يستخدم إمكانيات Multiremote وسيظهر خطأ.
> -   يدعم مشغل Storybook فقط ويب سطح المكتب، وليس ويب الجوال.

### خيارات خدمة مشغل Storybook

يمكن توفير خيارات الخدمة بهذه الطريقة

```ts
export const config: WebdriverIO.Config  = {
    // ...
    services: [
      [
        'visual',
        {
            // Some default options
            baselineFolder: join(process.cwd(), './__snapshots__/'),
            debug: true,
            // The storybook options, see cli options for the description
            storybook: {
                additionalSearchParams: new URLSearchParams({foo: 'bar', abc: 'def'}),
                clip: false,
                clipSelector: ''#some-id,
                numShards: 4,
                // `skipStories` can be a string ('example-button--secondary'),
                // an array (['example-button--secondary', 'example-button--small'])
                // or a regex which needs to be provided as as string ("/.*button.*/gm")
                skipStories: ['example-button--secondary', 'example-button--small'],
                url: 'https://www.bbc.co.uk/iplayer/storybook/',
                version: 6,
                // Optional - Allows overriding the baselines path. By default it will group the baselines by category and component (e.g. forms/input/baseline.png)
                getStoriesBaselinePath: (category, component) => `path__${category}__${component}`,
            },
        },
      ],
    ],
    // ....
}
```

### خيارات سطر أوامر مشغل Storybook

#### `--additionalSearchParams`

-   **النوع:** `string`
-   **إلزامي:** لا
-   **الافتراضي:** ''
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --additionalSearchParams="foo=bar&abc=def"`

سيضيف معلمات بحث إضافية إلى عنوان URL الخاص بـ Storybook.
راجع وثائق [URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) لمزيد من المعلومات. يجب أن تكون السلسلة عبارة عن سلسلة URLSearchParams صالحة.

> [!NOTE]
> هناك حاجة إلى علامات الاقتباس المزدوجة لمنع تفسير `&` كفاصل أمر.
> على سبيل المثال، مع `--additionalSearchParams="foo=bar&abc=def"` سيقوم بإنشاء عنوان URL التالي لـ Storybook لاختبار القصص: `http://storybook.url/iframe.html?id=story-id&foo=bar&abc=def`.

#### `--browsers`

-   **النوع:** `string`
-   **إلزامي:** لا
-   **الافتراضي:** `chrome`، يمكنك الاختيار من `chrome|firefox|edge|safari`
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --browsers=chrome,firefox,edge,safari`
-   **ملاحظة:** متاح فقط من خلال سطر الأوامر

سيستخدم المتصفحات المقدمة لالتقاط لقطات شاشة للمكونات

> [!NOTE]
> تأكد من تثبيت المتصفحات التي تريد التشغيل عليها على جهازك المحلي

#### `--clip`

-   **النوع:** `boolean`
-   **إلزامي:** لا
-   **الافتراضي:** `true`
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --clip=false`

عند تعطيله، سيقوم بإنشاء لقطة شاشة لمنفذ العرض. عند تمكينه، سيقوم بإنشاء لقطات شاشة للعناصر بناءً على [`--clipSelector`](#clipselector) مما سيقلل من كمية المساحة البيضاء حول لقطة شاشة المكون ويقلل من حجم لقطة الشاشة.

#### `--clipSelector`

-   **النوع:** `string`
-   **إلزامي:** لا
-   **الافتراضي:** `#storybook-root > :first-child` لـ Storybook V7 و `#root > :first-child:not(script):not(style)` لـ Storybook V6، انظر أيضًا [`--version`](#version)
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --clipSelector="#some-id"`

هذا هو المحدد الذي سيتم استخدامه:

-   لتحديد العنصر الذي سيتم التقاط لقطة الشاشة له
-   للعنصر للانتظار ليكون مرئيًا قبل التقاط لقطة شاشة

#### `--devices`

-   **النوع:** `string`
-   **إلزامي:** لا
-   **الافتراضي:** يمكنك الاختيار من [`deviceDescriptors.ts`](https://github.com/webdriverio/visual-testing/blob/main/./packages/service/src/storybook/deviceDescriptors.ts)
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --devices="iPhone 14 Pro Max","Pixel 3 XL"`
-   **ملاحظة:** متاح فقط من خلال سطر الأوامر

سيستخدم الأجهزة المقدمة التي تتطابق مع [`deviceDescriptors.ts`](https://github.com/webdriverio/visual-testing/blob/main/./packages/service/src/storybook/deviceDescriptors.ts) لالتقاط لقطات شاشة المكونات

> [!NOTE]
>
> -   إذا كنت تفتقد تكوين جهاز، فلا تتردد في تقديم [طلب ميزة](https://github.com/webdriverio/visual-testing/issues/new?assignees=&labels=&projects=&template=--feature-request.md)
> -   سيعمل هذا فقط مع Chrome:
>     -   إذا قدمت `--devices` فستعمل جميع نسخ Chrome في وضع **محاكاة الجوال**
>     -   إذا قدمت أيضًا متصفحات أخرى غير Chrome، مثل `--devices --browsers=firefox,safari,edge` فسيضيف تلقائيًا Chrome في وضع محاكاة الجوال
> -   سينشئ مشغل Storybook افتراضيًا لقطات عنصر، إذا كنت تريد رؤية لقطة شاشة محاكاة الجوال الكاملة، فقدم `--clip=false` من خلال سطر الأوامر
> -   سيبدو اسم الملف على سبيل المثال مثل `__snapshots__/example/button/desktop_chrome/example-button--large-local-chrome-iPhone-14-Pro-Max-430x932-dpr-3.png`
> -   **[المصدر:](https://chromedriver.chromium.org/mobile-emulation#h.p_ID_167)** اختبار موقع ويب للجوال على سطح المكتب باستخدام محاكاة الجوال يمكن أن يكون مفيدًا، ولكن يجب أن يكون المختبرون على دراية بوجود العديد من الاختلافات الدقيقة مثل:
>     -   وحدة معالجة رسومات مختلفة تمامًا، مما قد يؤدي إلى تغييرات كبيرة في الأداء؛
>     -   واجهة مستخدم الجوال غير محاكاة (على وجه الخصوص، يؤثر إخفاء شريط عنوان URL على ارتفاع الصفحة)؛
>     -   نافذة منبثقة للتمييز (حيث تحدد أحد أهداف اللمس القليلة) غير مدعومة؛
>     -   العديد من واجهات برمجة التطبيقات للأجهزة (على سبيل المثال، حدث تغيير الاتجاه) غير متوفرة.

#### `--headless`

-   **النوع:** `boolean`
-   **إلزامي:** لا
-   **الافتراضي:** `true`
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --headless=false`
-   **ملاحظة:** متاح فقط من خلال سطر الأوامر

هذا سيقوم بتشغيل الاختبارات افتراضيًا في وضع غير مرئي (عندما يدعم المتصفح ذلك) أو يمكن تعطيله

#### `--numShards`

-   **النوع:** `number`
-   **إلزامي:** لا
-   **الافتراضي:** `true`
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --numShards=10`

سيكون هذا عدد المثيلات المتوازية التي سيتم استخدامها لتشغيل القصص. سيتم تحديد هذا بواسطة `maxInstances` في ملف `wdio.conf` الخاص بك.

> [!IMPORTANT]
> عند التشغيل في وضع `headless`، لا تزيد العدد إلى أكثر من 20 لمنع عدم الاستقرار بسبب قيود الموارد

#### `--skipStories`

-   **النوع:** `string|regex`
-   **إلزامي:** لا
-   **الافتراضي:** null
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --skipStories="/.*button.*/gm"`

يمكن أن يكون هذا:

-   سلسلة (`example-button--secondary,example-button--small`)
-   أو تعبير منتظم (`"/.*button.*/gm"`)

لتخطي قصص معينة. استخدم `id` الخاص بالقصة التي يمكن العثور عليها في عنوان URL للقصة. على سبيل المثال، `id` في عنوان URL هذا `http://localhost:6006/?path=/story/example-page--logged-out` هو `example-page--logged-out`

#### `--url`

-   **النوع:** `string`
-   **إلزامي:** لا
-   **الافتراضي:** `http://127.0.0.1:6006`
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --url="https://example.com"`

عنوان URL حيث يتم استضافة مثيل Storybook الخاص بك.

#### `--version`

-   **النوع:** `number`
-   **إلزامي:** لا
-   **الافتراضي:** 7
-   **مثال:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --version=6`

هذا هو إصدار Storybook، الافتراضي هو `7`. هذا مطلوب لمعرفة ما إذا كان يجب استخدام [`clipSelector`](#clipselector) الخاص بـ V6.

### اختبار التفاعل في Storybook

يسمح لك اختبار التفاعل في Storybook بالتفاعل مع المكون الخاص بك من خلال إنشاء نصوص مخصصة باستخدام أوامر WDIO لوضع المكون في حالة معينة. على سبيل المثال، انظر إلى مقتطف الكود أدناه:

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

يتم تنفيذ اختبارين على مكونين مختلفين. يقوم كل اختبار أولاً بتعيين حالة ثم يلتقط لقطة شاشة. ستلاحظ أيضًا أنه تم تقديم أمر مخصص جديد، والذي يمكن العثور عليه [هنا](#new-custom-command).

يمكن حفظ ملف المواصفات أعلاه في مجلد وإضافته إلى سطر الأوامر باستخدام الأمر التالي:

```sh
pnpm run test.local.desktop.storybook.localhost -- --spec='tests/specs/storybook-interaction/*.ts'
```

سيقوم مشغل Storybook أولاً بمسح مثيل Storybook الخاص بك تلقائيًا ثم إضافة اختباراتك إلى القصص التي تحتاج إلى مقارنة. إذا كنت لا تريد مقارنة المكونات التي تستخدمها لاختبار التفاعل مرتين، يمكنك إضافة عامل تصفية لإزالة القصص "الافتراضية" من المسح عن طريق توفير عامل تصفية [`--skipStories`](#--skipstories). سيبدو هذا كما يلي:

```sh
pnpm run test.local.desktop.storybook.localhost -- --skipStories="/example-page.*/gm" --spec='tests/specs/storybook-interaction/*.ts'
```

### أمر مخصص جديد

سيتم إضافة أمر مخصص جديد يسمى `browser.waitForStorybookComponentToBeLoaded({ id: 'componentId' })` إلى كائن `browser/driver` الذي سيقوم تلقائيًا بتحميل المكون والانتظار حتى ينتهي، حتى لا تضطر إلى استخدام طريقة `browser.url('url.com')`. يمكن استخدامه بهذه الطريقة

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

الخيارات هي:

#### `additionalSearchParams`

-   **النوع:** [`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)
-   **إلزامي:** لا
-   **الافتراضي:** `new URLSearchParams()`
-   **مثال:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    additionalSearchParams: new URLSearchParams({ foo: "bar", abc: "def" }),
    id: "componentId",
});
```

هذا سيضيف معلمات بحث إضافية إلى عنوان URL الخاص بـ Storybook، في المثال أعلاه سيكون عنوان URL `http://storybook.url/iframe.html?id=story-id&foo=bar&abc=def`.
راجع وثائق [URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) لمزيد من المعلومات.

#### `clipSelector`

-   **النوع:** `string`
-   **إلزامي:** لا
-   **الافتراضي:** `#storybook-root > :first-child` لـ Storybook V7 و `#root > :first-child:not(script):not(style)` لـ Storybook V6
-   **مثال:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    clipSelector: "#your-selector",
    id: "componentId",
});
```

هذا هو المحدد الذي سيتم استخدامه:

-   لتحديد العنصر الذي سيتم التقاط لقطة الشاشة له
-   للعنصر للانتظار ليكون مرئيًا قبل التقاط لقطة شاشة

#### `id`

-   **النوع:** `string`
-   **إلزامي:** نعم
-   **مثال:**

```ts
await browser.waitForStorybookComponentToBeLoaded({ '#your-selector', id: 'componentId' })
```

استخدم `id` الخاص بالقصة التي يمكن العثور عليها في عنوان URL للقصة. على سبيل المثال، `id` في عنوان URL هذا `http://localhost:6006/?path=/story/example-page--logged-out` هو `example-page--logged-out`

#### `timeout`

-   **النوع:** `number`
-   **إلزامي:** لا
-   **الافتراضي:** 1100 مللي ثانية
-   **مثال:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    id: "componentId",
    timeout: 20000,
});
```

الحد الأقصى للمهلة التي نريد الانتظار حتى يكون المكون مرئيًا بعد التحميل على الصفحة

#### `url`

-   **النوع:** `string`
-   **إلزامي:** لا
-   **الافتراضي:** `http://127.0.0.1:6006`
-   **مثال:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    id: "componentId",
    url: "https://your.url",
});
```

عنوان URL حيث يتم استضافة مثيل Storybook الخاص بك.

</details>

## المساهمة

### تحديث الحزم

يمكنك تحديث الحزم باستخدام أداة CLI بسيطة. تأكد من تثبيت جميع التبعيات، يمكنك بعد ذلك تشغيل

```sh
pnpm update.packages
```

سيؤدي هذا إلى تشغيل CLI الذي سيطرح عليك الأسئلة التالية

```logs
==========================
🤖 Package update Wizard 🧙
==========================

? Which version target would you like to update to? (Minor|Latest)
? Do you want to update the package.json files? (Y/n)
? Do you want to remove all "node_modules" and reinstall dependencies? (Y/n)
? Would you like reinstall the dependencies? (Y/n)
```

سيؤدي هذا إلى السجلات التالية

<details>
    <summary>افتح لرؤية مثال على السجلات</summary>
    
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

### الأسئلة

يرجى الانضمام إلى خادم [Discord](https://discord.webdriver.io) الخاص بنا إذا كانت لديك أي أسئلة أو مشكلات تتعلق بالمساهمة في هذا المشروع. ابحث عن المساهمين في قناة `🙏-contributing`.

### المشكلات

إذا كانت لديك أسئلة أو أخطاء أو طلبات ميزات، يرجى تقديم مشكلة. قبل إرسال مشكلة، يرجى البحث في أرشيف المشكلات للمساعدة في تقليل التكرارات، وقراءة [الأسئلة الشائعة](https://webdriver.io/docs/visual-testing/faq/).

إذا لم تتمكن من العثور عليها هناك، يمكنك تقديم مشكلة حيث يمكنك تقديم:

-   🐛**تقرير الخطأ**: إنشاء تقرير لمساعدتنا في التحسين
-   📖**الوثائق**: اقتراح تحسينات أو الإبلاغ عن وثائق مفقودة/غير واضحة.
-   💡**طلب ميزة**: اقتراح فكرة لهذه الوحدة.
-   💬**سؤال**: طرح أسئلة.

### سير عمل التطوير

لإنشاء PR لهذا المشروع والبدء في المساهمة، اتبع هذا الدليل خطوة بخطوة:

-   قم بعمل fork للمشروع.
-   استنسخ المشروع في مكان ما على جهاز الكمبيوتر الخاص بك

    ```sh
    $ git clone https://github.com/webdriverio/visual-testing.git
    ```

-   انتقل إلى الدليل وقم بإعداد المشروع

    ```sh
    $ cd visual-testing
    $ corepack enable
    $ pnpm pnpm.install.workaround
    ```

-   قم بتشغيل وضع المراقبة الذي سيقوم تلقائيًا بترجمة الكود

    ```sh
    $ pnpm watch
    ```

    لبناء المشروع، قم بتشغيل:

    ```sh
    $ pnpm build
    ```

-   تأكد من أن تغييراتك لا تكسر أي اختبارات، قم بتشغيل:

    ```sh
    $ pnpm test
    ```

يستخدم هذا المشروع [changesets](https://github.com/changesets/changesets) لإنشاء سجلات التغييرات والإصدارات تلقائيًا.

### الاختبار

يجب تنفيذ العديد من الاختبارات ليكون قادرًا على اختبار الوحدة. عند إضافة PR، يجب أن تجتاز جميع الاختبارات على الأقل الاختبارات المحلية. يتم اختبار كل PR تلقائيًا مقابل Sauce Labs، انظر [خط أنابيب GitHub Actions](https://github.com/webdriverio/visual-testing/actions/workflows/tests.yml). قبل الموافقة على PR، سيقوم المساهمون الأساسيون باختبار PR مقابل المحاكيات/محاكيات الأجهزة / الأجهزة الحقيقية.

#### الاختبار المحلي

أولاً، يجب إنشاء خط أساس محلي. يمكن القيام بذلك باستخدام:

```sh
// With the webdriver protocol
$ pnpm run test.local.init
````

سيقوم هذا الأمر بإنشاء مجلد يسمى `localBaseline` الذي سيحتفظ بجميع صور الخط الأساسي.

ثم قم بتشغيل:

```sh
// With the webdriver protocol
pnpm run test.local.desktop
```

سيقوم هذا بتشغيل جميع الاختبارات على جهاز محلي على Chrome.

#### اختبار مشغل Storybook المحلي (تجريبي)

أولاً، يجب إنشاء خط أساس محلي. يمكن القيام بذلك باستخدام:

```sh
pnpm run test.local.desktop.storybook
```

سيقوم هذا بإجراء اختبارات Storybook باستخدام Chrome في وضع headless مقابل مستودع Storybook التجريبي الموجود في https://govuk-react.github.io/govuk-react/.

لتشغيل الاختبارات باستخدام المزيد من المتصفحات، يمكنك تشغيل

```sh
pnpm run test.local.desktop.storybook -- --browsers=chrome,firefox,edge,safari
```

> [!NOTE]
> تأكد من تثبيت المتصفحات التي تريد التشغيل عليها على جهازك المحلي

#### اختبار CI مع Sauce Labs (غير مطلوب لـ PR)

يتم استخدام الأمر أدناه لاختبار البناء على GitHub Actions، يمكن استخدامه هناك فقط وليس للتطوير المحلي.

```
$ pnpm run test.saucelabs
```

سيختبر مقابل الكثير من التكوينات التي يمكن العثور عليها [هنا](https://github.com/webdriverio/visual-testing/blob/main/./tests/configs/wdio.saucelabs.web.conf.ts).
يتم فحص جميع PRs تلقائيًا مقابل Sauce Labs.

## الإصدار

لإصدار إصدار من أي من الحزم المذكورة أعلاه، قم بما يلي:

-   تشغيل [خط أنابيب الإصدار](https://github.com/webdriverio/visual-testing/actions/workflows/release.yml)
-   يتم إنشاء PR للإصدار، اطلب مراجعته والموافقة عليه من قبل عضو آخر في WebdriverIO
-   قم بدمج PR
-   تشغيل [خط أنابيب الإصدار](https://github.com/webdriverio/visual-testing/actions/workflows/release.yml) مرة أخرى
-   يجب إصدار إصدار جديد 🎉

## الائتمانات

يستخدم `@wdio/visual-testing` ترخيصًا مفتوح المصدر من [LambdaTest](https://www.lambdatest.com/) و[Sauce Labs](https://saucelabs.com/).