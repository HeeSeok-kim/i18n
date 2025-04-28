---
id: customreporter
title: مُبلغ مخصص
---

يمكنك كتابة تقرير مخصص خاص بك لمشغل اختبار WDIO يتناسب مع احتياجاتك. وهذا سهل!

كل ما عليك فعله هو إنشاء وحدة نود تَرِث من حزمة `@wdio/reporter`، بحيث يمكنها استقبال رسائل من الاختبار.

الإعداد الأساسي يجب أن يبدو مثل:

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    constructor(options) {
        /*
         * جعل المُبلغ يكتب إلى مجرى الإخراج بشكل افتراضي
         */
        options = Object.assign(options, { stdout: true })
        super(options)
    }

    onTestPass(test) {
        this.write(`Congratulations! Your test "${test.title}" passed 👏`)
    }
}
```

لاستخدام هذا المُبلغ، كل ما عليك فعله هو تعيينه إلى خاصية `reporter` في التكوين الخاص بك.

يجب أن يبدو ملف `wdio.conf.js` الخاص بك كما يلي:

```js
import CustomReporter from './reporter/my.custom.reporter'

export const config = {
    // ...
    reporters: [
        /**
         * استخدام فئة المُبلغ المستوردة
         */
        [CustomReporter, {
            someOption: 'foobar'
        }],
        /**
         * استخدام المسار المطلق للمُبلغ
         */
        ['/path/to/reporter.js', {
            someOption: 'foobar'
        }]
    ],
    // ...
}
```

يمكنك أيضًا نشر المُبلغ على NPM حتى يتمكن الجميع من استخدامه. قم بتسمية الحزمة مثل المُبلغين الآخرين `wdio-<reportername>-reporter` ، وقم بوضع علامات عليها بكلمات مفتاحية مثل `wdio` أو `wdio-reporter`.

## معالج الأحداث

يمكنك تسجيل معالج حدث للعديد من الأحداث التي يتم تشغيلها أثناء الاختبار. ستتلقى جميع المعالجات التالية البيانات مع معلومات مفيدة حول الحالة والتقدم الحاليين.

تعتمد بنية كائنات البيانات هذه على الحدث، وهي موحدة عبر الأطر (Mocha وJasmine وCucumber). بمجرد تنفيذ مُبلغ مخصص، يجب أن يعمل لجميع الأطر.

تحتوي القائمة التالية على جميع الطرق الممكنة التي يمكنك إضافتها إلى فئة المُبلغ الخاصة بك:

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    onRunnerStart() {}
    onBeforeCommand() {}
    onAfterCommand() {}
    onSuiteStart() {}
    onHookStart() {}
    onHookEnd() {}
    onTestStart() {}
    onTestPass() {}
    onTestFail() {}
    onTestSkip() {}
    onTestEnd() {}
    onSuiteEnd() {}
    onRunnerEnd() {}
}
```

أسماء الطرق واضحة للغاية.

لطباعة شيء ما في حدث معين، استخدم طريقة `this.write(...)`، التي توفرها فئة `WDIOReporter` الأصلية. إما أن تقوم بتدفق المحتوى إلى `stdout` أو إلى ملف سجل (اعتمادًا على خيارات المُبلغ).

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    onTestPass(test) {
        this.write(`Congratulations! Your test "${test.title}" passed 👏`)
    }
}
```

لاحظ أنه لا يمكنك تأجيل تنفيذ الاختبار بأي شكل من الأشكال.

يجب أن تنفذ جميع معالجات الأحداث الإجراءات المتزامنة (أو ستواجه حالات سباق).

تأكد من التحقق من [قسم الأمثلة](https://github.com/webdriverio/webdriverio/tree/main/examples/wdio) حيث يمكنك العثور على مثال لمُبلغ مخصص يطبع اسم الحدث لكل حدث.

إذا قمت بتنفيذ مُبلغ مخصص يمكن أن يكون مفيدًا للمجتمع، فلا تتردد في إجراء طلب سحب حتى نتمكن من جعل المُبلغ متاحًا للجمهور!

أيضًا، إذا قمت بتشغيل مشغل اختبار WDIO عبر واجهة `Launcher`، فلا يمكنك تطبيق مُبلغ مخصص كدالة كما يلي:

```js
import Launcher from '@wdio/cli'

import CustomReporter from './reporter/my.custom.reporter'

const launcher = new Launcher('/path/to/config.file.js', {
    // هذا لن يعمل، لأن CustomReporter غير قابل للتسلسل
    reporters: ['dot', CustomReporter]
})
```

## انتظر حتى `isSynchronised`

إذا كان المُبلغ الخاص بك يحتاج إلى تنفيذ عمليات غير متزامنة للإبلاغ عن البيانات (مثل تحميل ملفات السجل أو الأصول الأخرى)، يمكنك الكتابة فوق طريقة `isSynchronised` في المُبلغ المخصص الخاص بك للسماح لمشغل WebdriverIO بالانتظار حتى تكون قد حسبت كل شيء. يمكن رؤية مثال على هذا في [`@wdio/sumologic-reporter`](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-sumologic-reporter/src/index.ts):

```js
export default class SumoLogicReporter extends WDIOReporter {
    constructor (options) {
        // ...
        this.unsynced = []
        this.interval = setInterval(::this.sync, this.options.syncInterval)
        // ...
    }

    /**
     * الكتابة فوق طريقة isSynchronised
     */
    get isSynchronised () {
        return this.unsynced.length === 0
    }

    /**
     * مزامنة ملفات السجل
     */
    sync () {
        // ...
        request({
            method: 'POST',
            uri: this.options.sourceAddress,
            body: logLines
        }, (err, resp) => {
            // ...
            /**
             * إزالة السجلات المنقولة من دلو السجل
             */
            this.unsynced.splice(0, MAX_LINES)
            // ...
        }
    }
}
```

بهذه الطريقة سينتظر المشغل حتى يتم تحميل جميع معلومات السجل.

## نشر المُبلغ على NPM

لجعل المُبلغ أسهل في الاستهلاك والاكتشاف من قبل مجتمع WebdriverIO، يرجى اتباع هذه التوصيات:

* يجب أن تستخدم الخدمات اتفاقية التسمية هذه: `wdio-*-reporter`
* استخدم كلمات مفتاحية NPM: `wdio-plugin`، `wdio-reporter`
* يجب أن يقوم إدخال `main` بـ`export` مثيل المُبلغ
* مثال على المُبلغ: [`@wdio/dot-service`](https://github.com/webdriverio/webdriverio/tree/main/packages/wdio-dot-reporter)

يسمح اتباع نمط التسمية الموصى به بإضافة الخدمات حسب الاسم:

```js
// إضافة wdio-custom-reporter
export const config = {
    // ...
    reporter: ['custom'],
    // ...
}
```

### إضافة الخدمة المنشورة إلى WDIO CLI والتوثيق

نحن نقدر حقًا كل إضافة جديدة يمكن أن تساعد الآخرين على إجراء اختبارات أفضل! إذا قمت بإنشاء مثل هذه الإضافة، فيرجى التفكير في إضافتها إلى واجهة سطر الأوامر (CLI) والمستندات الخاصة بنا لتسهيل العثور عليها.

يرجى تقديم طلب سحب مع التغييرات التالية:

- أضف خدمتك إلى قائمة [المُبلغين المدعومين](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-cli/src/constants.ts#L74-L91)) في وحدة CLI
- تحسين [قائمة المُبلغين](https://github.com/webdriverio/webdriverio/blob/main/scripts/docs-generation/3rd-party/reporters.json) لإضافة المستندات الخاصة بك إلى صفحة Webdriver.io الرسمية