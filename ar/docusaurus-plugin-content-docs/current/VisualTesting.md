---
id: visual-testing
title: الاختبار البصري
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

## ما الذي يمكنه فعله؟

توفر WebdriverIO مقارنات للصور على الشاشات أو العناصر أو صفحة كاملة لـ

-   🖥️ متصفحات سطح المكتب (Chrome / Firefox / Safari / Microsoft Edge)
-   📱 متصفحات الجوال / الأجهزة اللوحية (Chrome على محاكيات Android / Safari على محاكيات iOS / أجهزة حقيقية) عبر Appium
-   📱 تطبيقات أصلية (محاكيات Android / محاكيات iOS / أجهزة حقيقية) عبر Appium (🌟 **جديد** 🌟)
-   📳 تطبيقات هجينة عبر Appium

من خلال [`@wdio/visual-service`](https://www.npmjs.com/package/@wdio/visual-service) وهي خدمة خفيفة من WebdriverIO.

هذا يتيح لك:

-   حفظ أو مقارنة **شاشات/عناصر/شاشات كاملة** مقابل خط الأساس
-   **إنشاء خط أساس تلقائيا** عندما لا يكون هناك خط أساس
-   **حظر مناطق مخصصة** وحتى **استبعاد شريط الحالة و/أو شريط الأدوات تلقائيا** (للجوال فقط) أثناء المقارنة
-   زيادة أبعاد لقطات شاشة العناصر
-   **إخفاء النص** أثناء مقارنة الموقع لـ:
    -   **تحسين الاستقرار** ومنع تقلب عرض الخطوط
    -   التركيز فقط على **تخطيط** الموقع
-   استخدام **طرق مقارنة مختلفة** ومجموعة من **المطابقات الإضافية** للحصول على اختبارات أكثر قابلية للقراءة
-   التحقق من كيفية **دعم موقعك للتنقل بلوحة المفاتيح**، انظر أيضا [التنقل عبر موقع بالمفاتيح](#tabbing-through-a-website)
-   والكثير غير ذلك، راجع خيارات [الخدمة](./visual-testing/service-options) و[الطريقة](./visual-testing/method-options)

الخدمة هي وحدة خفيفة لاسترجاع البيانات ولقطات الشاشة اللازمة لجميع المتصفحات/الأجهزة. قوة المقارنة تأتي من [ResembleJS](https://github.com/Huddle/Resemble.js). إذا كنت ترغب في مقارنة الصور عبر الإنترنت، يمكنك التحقق من [الأداة عبر الإنترنت](http://rsmbl.github.io/Resemble.js/).

:::info ملاحظة للتطبيقات الأصلية/الهجينة
يمكن استخدام الطرق `saveScreen` و`saveElement` و`checkScreen` و`checkElement` والمطابقات `toMatchScreenSnapshot` و`toMatchElementSnapshot` للتطبيقات/السياقات الأصلية.

الرجاء استخدام الخاصية `isHybridApp:true` في إعدادات الخدمة الخاصة بك عندما تريد استخدامها للتطبيقات الهجينة.
:::

## التثبيت

الطريقة الأسهل هي الاحتفاظ بـ `@wdio/visual-service` كتبعية تطوير في ملف `package.json` الخاص بك، عبر:

```sh
npm install --save-dev @wdio/visual-service
```

## الاستخدام

يمكن استخدام `@wdio/visual-service` كخدمة عادية. يمكنك إعدادها في ملف التكوين الخاص بك على النحو التالي:

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
                // بعض الخيارات، راجع المستندات للمزيد
                baselineFolder: path.join(process.cwd(), "tests", "baseline"),
                formatImageName: "{tag}-{logName}-{width}x{height}",
                screenshotPath: path.join(process.cwd(), "tmp"),
                savePerInstance: true,
                // ... المزيد من الخيارات
            },
        ],
    ],
    // ...
};
```

يمكن العثور على المزيد من خيارات الخدمة [هنا](/docs/visual-testing/service-options).

بمجرد الإعداد في تكوين WebdriverIO الخاص بك، يمكنك المضي قدمًا وإضافة التأكيدات البصرية إلى [اختباراتك](/docs/visual-testing/writing-tests).

### القدرات
لاستخدام وحدة الاختبار البصري، **لا تحتاج إلى إضافة أي خيارات إضافية إلى قدراتك**. ومع ذلك، في بعض الحالات، قد ترغب في إضافة بيانات وصفية إضافية إلى اختباراتك البصرية، مثل `logName`.

يتيح `logName` تعيين اسم مخصص لكل قدرة، والتي يمكن تضمينها بعد ذلك في أسماء ملفات الصور. هذا مفيد بشكل خاص لتمييز لقطات الشاشة التي تم التقاطها عبر متصفحات أو أجهزة أو تكوينات مختلفة.

لتمكين هذا، يمكنك تحديد `logName` في قسم `capabilities` والتأكد من أن خيار `formatImageName` في خدمة الاختبار البصري يشير إليه. إليك كيفية إعداده:

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
                logName: 'chrome-mac-15', // اسم سجل مخصص لـ Chrome
            },
        }
        {
            browserName: 'firefox',
            'wdio-ics:options': {
                logName: 'firefox-mac-15', // اسم سجل مخصص لـ Firefox
            },
        }
    ],
    services: [
        [
            "visual",
            {
                // بعض الخيارات، راجع المستندات للمزيد
                baselineFolder: path.join(process.cwd(), "tests", "baseline"),
                screenshotPath: path.join(process.cwd(), "tmp"),
                // التنسيق أدناه سيستخدم `logName` من القدرات
                formatImageName: "{tag}-{logName}-{width}x{height}",
                // ... المزيد من الخيارات
            },
        ],
    ],
    // ...
};
```

#### كيف يعمل
1. إعداد `logName`:

    - في قسم `capabilities`، قم بتعيين `logName` فريد لكل متصفح أو جهاز. على سبيل المثال، `chrome-mac-15` يحدد الاختبارات التي تعمل على Chrome على نظام macOS الإصدار 15.

2. تسمية الصور المخصصة:

    - يدمج خيار `formatImageName` اسم `logName` في أسماء ملفات لقطات الشاشة. على سبيل المثال، إذا كان `tag` هو homepage والدقة هي `1920x1080`، فقد يبدو اسم الملف الناتج كما يلي:

        `homepage-chrome-mac-15-1920x1080.png`

3. فوائد التسمية المخصصة:

    - يصبح التمييز بين لقطات الشاشة من متصفحات أو أجهزة مختلفة أسهل بكثير، خاصة عند إدارة خطوط الأساس وتصحيح التناقضات.

4. ملاحظة حول الإعدادات الافتراضية:

    - إذا لم يتم تعيين `logName` في القدرات، فسيعرض خيار `formatImageName` كسلسلة فارغة في أسماء الملفات (`homepage--15-1920x1080.png`)

### WebdriverIO MultiRemote

نحن ندعم أيضًا [MultiRemote](https://webdriver.io/docs/multiremote/). لجعل هذا يعمل بشكل صحيح، تأكد من إضافة `wdio-ics:options` إلى
قدراتك كما ترى أدناه. هذا سيضمن أن كل لقطة شاشة سيكون لها اسمها الفريد.

[كتابة اختباراتك](/docs/visual-testing/writing-tests) لن تكون مختلفة مقارنة باستخدام [testrunner](https://webdriver.io/docs/testrunner)

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
                // هذا!!!
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
                // هذا!!!
                "wdio-ics:options": {
                    logName: "chrome-latest-two",
                },
            },
        },
    },
};
```

### التشغيل برمجيًا

فيما يلي مثال بسيط لكيفية استخدام `@wdio/visual-service` عبر خيارات `remote`:

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

// "بدء" الخدمة لإضافة الأوامر المخصصة إلى `browser`
visualService.remoteSetup(browser);

await browser.url("https://webdriver.io/");

// أو استخدم هذا لحفظ لقطة شاشة فقط
await browser.saveFullPageScreen("examplePaged", {});

// أو استخدم هذا للتحقق. لا يلزم الجمع بين الطريقتين، راجع الأسئلة الشائعة
await browser.checkFullPageScreen("examplePaged", {});

await browser.deleteSession();
```

### التنقل عبر موقع بالمفاتيح

يمكنك التحقق مما إذا كان الموقع يمكن الوصول إليه باستخدام مفتاح <kbd>TAB</kbd> في لوحة المفاتيح. اختبار هذا الجزء من إمكانية الوصول كان دائمًا مهمة تستغرق وقتًا طويلاً (يدويًا) ومن الصعب القيام به من خلال الأتمتة.
مع الطرق `saveTabbablePage` و`checkTabbablePage`، يمكنك الآن رسم خطوط ونقاط على موقعك للتحقق من ترتيب التبويب.

كن على علم بحقيقة أن هذا مفيد فقط لمتصفحات سطح المكتب و**ليس** للأجهزة المحمولة. جميع متصفحات سطح المكتب تدعم هذه الميزة.

:::note

العمل مستوحى من منشور مدونة [Viv Richards](https://github.com/vivrichards600) حول ["AUTOMATING PAGE TABABILITY (IS THAT A WORD?) WITH VISUAL TESTING"](https://vivrichards.co.uk/accessibility/automating-page-tab-flows-using-visual-testing-and-javascript).

تعتمد طريقة تحديد العناصر القابلة للتبويب على وحدة [tabbable](https://github.com/davidtheclark/tabbable). إذا كانت هناك أي مشاكل تتعلق بالتبويب، يرجى التحقق من [README.md](https://github.com/davidtheclark/tabbable/blob/master/README.md) وخاصة قسم [المزيد من التفاصيل](https://github.com/davidtheclark/tabbable/blob/master/README.md#more-details).

:::

#### كيف يعمل

ستقوم كلتا الطريقتين بإنشاء عنصر `canvas` على موقعك ورسم خطوط ونقاط لإظهار إلى أين سيذهب التبويب الخاص بك إذا استخدمه المستخدم النهائي. بعد ذلك، سيتم إنشاء لقطة شاشة للصفحة بأكملها لمنحك نظرة عامة جيدة على التدفق.

:::important

**استخدم `saveTabbablePage` فقط عندما تحتاج إلى إنشاء لقطة شاشة ولا ترغب في مقارنتها مع صورة خط الأساس.**

:::

عندما ترغب في مقارنة تدفق التبويب مع خط الأساس، يمكنك استخدام طريقة `checkTabbablePage`. **لا** تحتاج إلى استخدام الطريقتين معًا. إذا كانت هناك بالفعل صورة خط أساس تم إنشاؤها، والتي يمكن إنشاؤها تلقائيًا من خلال توفير `autoSaveBaseline: true` عند تهيئة الخدمة،
فإن `checkTabbablePage` ستقوم أولاً بإنشاء الصورة _الفعلية_ ثم مقارنتها مع خط الأساس.

##### الخيارات

تستخدم كلتا الطريقتين نفس الخيارات مثل [`saveFullPageScreen`](https://github.com/wswebcreation/webdriver-image-comparison/blob/master/docs/OPTIONS.md#savefullpagescreen-or-savetabbablepage) أو
[`compareFullPageScreen`](https://github.com/wswebcreation/webdriver-image-comparison/blob/master/docs/OPTIONS.md#comparefullpagescreen-or-comparetabbablepage).

#### مثال

هذا مثال على كيفية عمل التبويب على [موقع خنزير غينيا](https://guinea-pig.webdriver.io/image-compare.html) الخاص بنا:

![WDIO tabbing example](/img/visual/tabbable-chrome-latest-1366x768.png)

### تحديث لقطات بصرية فاشلة تلقائيًا

قم بتحديث صور خط الأساس من خلال سطر الأوامر بإضافة الوسيطة `--update-visual-baseline`. هذا سوف

-   نسخ لقطة الشاشة الفعلية المأخوذة تلقائيًا ووضعها في مجلد خط الأساس
-   إذا كانت هناك اختلافات، فسيجعل الاختبار يمر لأنه تم تحديث خط الأساس

**الاستخدام:**

```sh
npm run test.local.desktop  --update-visual-baseline
```

عند تشغيل سجلات في وضع المعلومات/التصحيح، سترى السجلات التالية مضافة

```logs
[0-0] ..............
[0-0] #####################################################################################
[0-0]  INFO:
[0-0]  Updated the actual image to
[0-0]  /Users/wswebcreation/Git/wdio/visual-testing/localBaseline/chromel/demo-chrome-1366x768.png
[0-0] #####################################################################################
[0-0] ..........
```

## دعم TypeScript

تتضمن هذه الوحدة دعم TypeScript، مما يتيح لك الاستفادة من الإكمال التلقائي وسلامة النوع وتحسين تجربة المطور عند استخدام خدمة الاختبار البصري.

### الخطوة 1: إضافة تعريفات النوع
لضمان تعرف TypeScript على أنواع الوحدة، أضف الإدخال التالي إلى حقل الأنواع في ملف tsconfig.json الخاص بك:

```json
{
    "compilerOptions": {
        "types": ["@wdio/visual-service"]
    }
}
```

### الخطوة 2: تمكين سلامة النوع لخيارات الخدمة
لفرض التحقق من النوع على خيارات الخدمة، قم بتحديث تكوين WebdriverIO الخاص بك:

```ts
// wdio.conf.ts
import { join } from 'node:path';
// استيراد تعريف النوع
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
                // خيارات الخدمة
                baselineFolder: join(process.cwd(), './__snapshots__/'),
                formatImageName: '{tag}-{logName}-{width}x{height}',
                screenshotPath: join(process.cwd(), '.tmp/'),
            } satisfies VisualServiceOptions, // يضمن سلامة النوع
        ],
    ],
    // ...
};
```

## متطلبات النظام

### الإصدار 5 وما فوق

بالنسبة للإصدار 5 وما فوق، هذه الوحدة هي وحدة تعتمد على JavaScript بالكامل دون تبعيات نظام إضافية تتجاوز [متطلبات المشروع العامة](/docs/gettingstarted#system-requirements). تستخدم [Jimp](https://github.com/jimp-dev/jimp) وهي مكتبة معالجة صور لـ Node مكتوبة بالكامل بلغة JavaScript، بدون تبعيات أصلية.

### الإصدار 4 وما دون

بالنسبة للإصدار 4 وما دون، تعتمد هذه الوحدة على [Canvas](https://github.com/Automattic/node-canvas)، وهو تنفيذ للوحة الرسم لـ Node.js. يعتمد Canvas على [Cairo](https://cairographics.org/).

#### تفاصيل التثبيت

بشكل افتراضي، سيتم تنزيل الملفات الثنائية لنظام macOS وLinux وWindows أثناء تثبيت `npm install` لمشروعك. إذا لم يكن لديك نظام تشغيل أو بنية معالج مدعومة، فسيتم تجميع الوحدة على نظامك. هذا يتطلب العديد من التبعيات، بما في ذلك Cairo وPango.

للحصول على معلومات تثبيت مفصلة، راجع [ويكي node-canvas](https://github.com/Automattic/node-canvas/wiki/_pages). فيما يلي تعليمات التثبيت بسطر واحد لأنظمة التشغيل الشائعة. لاحظ أن `libgif/giflib` و`librsvg` و`libjpeg` اختيارية ومطلوبة فقط لدعم GIF وSVG وJPEG على التوالي. مطلوب Cairo v1.10.0 أو أحدث.

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

     باستخدام [Homebrew](https://brew.sh/):

     ```sh
     brew install pkg-config cairo pango libpng jpeg giflib librsvg pixman
     ```

    **Mac OS X v10.11+:** إذا كنت قد قمت مؤخرًا بالتحديث إلى Mac OS X v10.11+ وتواجه مشكلة عند التجميع، قم بتشغيل الأمر التالي: `xcode-select --install`. اقرأ المزيد عن المشكلة [على Stack Overflow](http://stackoverflow.com/a/32929012/148072).
    إذا كان لديك Xcode 10.0 أو أعلى مثبتًا، للبناء من المصدر تحتاج إلى NPM 6.4.1 أو أعلى.

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

    راجع [الويكي](https://github.com/Automattic/node-canvas/wiki/Installation:-Windows)

</TabItem>
<TabItem value="others">

    راجع [الويكي](https://github.com/Automattic/node-canvas/wiki)

</TabItem>
</Tabs>