---
id: vscode-extensions
title: اختبار امتداد VS Code
---

يسمح لك WebdriverIO باختبار امتدادات [VS Code](https://code.visualstudio.com/) بشكل سلس من طرف إلى طرف في بيئة VS Code سطح المكتب أو كامتداد ويب. كل ما تحتاجه هو توفير مسار لامتدادك والإطار يتولى الباقي. مع [`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service) يتم التعامل مع كل شيء وأكثر:

- 🏗️ تثبيت VSCode (إما المستقر أو الداخلي أو إصدار محدد)
- ⬇️ تنزيل Chromedriver الخاص بإصدار VSCode المحدد
- 🚀 يمكّنك من الوصول إلى واجهة برمجة تطبيقات VSCode من اختباراتك
- 🖥️ بدء تشغيل VSCode بإعدادات مستخدم مخصصة (بما في ذلك دعم VSCode على Ubuntu و MacOS و Windows)
- 🌐 أو استضافة VSCode من خادم ليتم الوصول إليه من أي متصفح لاختبار امتدادات الويب
- 📔 إعداد page objects مع محددات المواقع المتوافقة مع إصدار VSCode الخاص بك

## البدء

لبدء مشروع WebdriverIO جديد، قم بتشغيل:

```sh
npm create wdio@latest ./
```

سيرشدك معالج التثبيت خلال العملية. تأكد من اختيار _"VS Code Extension Testing"_ عندما يسألك عن نوع الاختبار الذي ترغب في القيام به، بعد ذلك احتفظ بالإعدادات الافتراضية أو قم بتعديلها حسب تفضيلاتك.

## مثال التكوين

لاستخدام الخدمة، تحتاج إلى إضافة `vscode` إلى قائمة الخدمات لديك، يتبعها اختيارياً كائن تكوين. هذا سيجعل WebdriverIO يقوم بتنزيل ملفات VSCode الثنائية المحددة وإصدار Chromedriver المناسب:

```js
// wdio.conf.ts
export const config = {
    outputDir: 'trace',
    // ...
    capabilities: [{
        browserName: 'vscode',
        browserVersion: '1.71.0', // "insiders" أو "stable" لأحدث إصدار من VSCode
        'wdio:vscodeOptions': {
            extensionPath: __dirname,
            userSettings: {
                "editor.fontSize": 14
            }
        }
    }],
    services: ['vscode'],
    /**
     * اختيارياً يمكنك تحديد المسار الذي يخزن فيه WebdriverIO جميع
     * ملفات VSCode و Chromedriver الثنائية، على سبيل المثال:
     * services: [['vscode', { cachePath: __dirname }]]
     */
    // ...
};
```

إذا قمت بتعريف `wdio:vscodeOptions` مع أي `browserName` آخر غير `vscode`، مثل `chrome`، فستقوم الخدمة بتقديم الامتداد كامتداد ويب. إذا كنت تختبر على Chrome فلا حاجة لخدمة تشغيل إضافية، على سبيل المثال:

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

_ملاحظة:_ عند اختبار امتدادات الويب يمكنك فقط الاختيار بين `stable` أو `insiders` كـ `browserVersion`.

### إعداد TypeScript

في ملف `tsconfig.json` الخاص بك، تأكد من إضافة `wdio-vscode-service` إلى قائمة الأنواع:

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

## الاستخدام

يمكنك بعد ذلك استخدام طريقة `getWorkbench` للوصول إلى page objects للمحددات المتطابقة مع إصدار VSCode المطلوب:

```ts
describe('WDIO VSCode Service', () => {
    it('should be able to load VSCode', async () => {
        const workbench = await browser.getWorkbench()
        expect(await workbench.getTitleBar().getTitle())
            .toBe('[Extension Development Host] - README.md - wdio-vscode-service - Visual Studio Code')
    })
})
```

من هناك يمكنك الوصول إلى جميع page objects باستخدام طرق page object المناسبة. اكتشف المزيد عن جميع page objects المتاحة وطرقها في [وثائق page object](https://webdriverio-community.github.io/wdio-vscode-service/).

### الوصول إلى واجهات برمجة تطبيقات VSCode

إذا كنت ترغب في تنفيذ أتمتة معينة من خلال [واجهة برمجة تطبيقات VSCode](https://code.visualstudio.com/api/references/vscode-api)، يمكنك القيام بذلك عن طريق تشغيل أوامر عن بُعد عبر الأمر المخصص `executeWorkbench`. يتيح لك هذا الأمر تنفيذ رمز عن بُعد من اختبارك داخل بيئة VSCode ويتيح الوصول إلى واجهة برمجة تطبيقات VSCode. يمكنك تمرير معلمات عشوائية إلى الدالة والتي سيتم نقلها بعد ذلك إلى الدالة. سيتم دائمًا تمرير الكائن `vscode` كوسيطة أولى متبوعًا بمعلمات الدالة الخارجية. لاحظ أنه لا يمكنك الوصول إلى المتغيرات خارج نطاق الدالة لأن رد الاتصال يتم تنفيذه عن بُعد. إليك مثال:

```ts
const workbench = await browser.getWorkbench()
await browser.executeWorkbench((vscode, param1, param2) => {
    vscode.window.showInformationMessage(`I am an ${param1} ${param2}!`)
}, 'API', 'call')

const notifs = await workbench.getNotifications()
console.log(await notifs[0].getMessage()) // يخرج: "I am an API call!"
```

للحصول على وثائق page object الكاملة، راجع [الوثائق](https://webdriverio-community.github.io/wdio-vscode-service/modules.html). يمكنك العثور على أمثلة استخدام متنوعة في [مجموعة اختبارات هذا المشروع](https://github.com/webdriverio-community/wdio-vscode-service/blob/main/test/specs).

## مزيد من المعلومات

يمكنك معرفة المزيد حول كيفية تكوين [`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service) وكيفية إنشاء page objects مخصصة في [وثائق الخدمة](/docs/wdio-vscode-service). يمكنك أيضًا مشاهدة المحادثة التالية من [Christian Bromann](https://twitter.com/bromann) حول [_اختبار امتدادات VSCode المعقدة بقوة معايير الويب_](https://www.youtube.com/watch?v=PhGNTioBUiU):

<LiteYouTubeEmbed
    id="PhGNTioBUiU"
    title="Testing Complex VSCode Extensions With the Power of Web Standards"
/>