---
id: customreporter
title: مراسل مخصص
---

يمكنك كتابة مراسل مخصص لمشغّل اختبارات WDIO مصمم خصيصًا لاحتياجاتك. وهذا أمر سهل!

كل ما عليك فعله هو إنشاء وحدة نمطية تَرِث من حزمة `@wdio/reporter`، حتى تتمكن من استقبال الرسائل من الاختبار.

الإعداد الأساسي يجب أن يبدو كالتالي:

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    constructor(options) {
        /*
         * جعل المراسل يكتب إلى دفق الإخراج بشكل افتراضي
         */
        options = Object.assign(options, { stdout: true })
        super(options)
    }

    onTestPass(test) {
        this.write(`Congratulations! Your test "${test.title}" passed 👏`)
    }
}
```

لاستخدام هذا المراسل، كل ما عليك فعله هو تعيينه إلى خاصية `reporter` في ملف التكوين الخاص بك.

يجب أن يبدو ملف `wdio.conf.js` الخاص بك كالتالي:

```js
import CustomReporter from './reporter/my.custom.reporter'

export const config = {
    // ...
    reporters: [
        /**
         * استخدام فئة المراسل المستوردة
         */
        [CustomReporter, {
            someOption: 'foobar'
        }],
        /**
         * استخدام المسار المطلق للمراسل
         */
        ['/path/to/reporter.js', {
            someOption: 'foobar'
        }]
    ],
    // ...
}
```

يمكنك أيضًا نشر المراسل على NPM حتى يتمكن الجميع من استخدامه. قم بتسمية الحزمة مثل المراسلين الآخرين `wdio-<reportername>-reporter`، وضع العلامات عليها بكلمات مفتاحية مثل `wdio` أو `wdio-reporter`.

## معالج الأحداث

يمكنك تسجيل معالج أحداث للعديد من الأحداث التي يتم إطلاقها أثناء الاختبار. ستتلقى جميع المعالجات التالية حمولات مع معلومات مفيدة عن الحالة والتقدم الحالي.

تعتمد بنية كائنات الحمولة هذه على الحدث، وهي موحدة عبر أطر العمل (Mocha و Jasmine و Cucumber). بمجرد تنفيذ مراسل مخصص، يجب أن يعمل لجميع أطر العمل.

تحتوي القائمة التالية على جميع الطرق الممكنة التي يمكنك إضافتها إلى فئة المراسل الخاصة بك:

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

أسماء الطرق واضحة بذاتها.

لطباعة شيء ما عند حدث معين، استخدم طريقة `this.write(...)`، التي توفرها فئة `WDIOReporter` الأصلية. فهي إما تنقل المحتوى إلى `stdout`، أو إلى ملف سجل (حسب خيارات المراسل).

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    onTestPass(test) {
        this.write(`Congratulations! Your test "${test.title}" passed 👏`)
    }
}
```

لاحظ أنه لا يمكنك تأجيل تنفيذ الاختبار بأي شكل من الأشكال.

يجب أن تنفذ جميع معالجات الأحداث إجراءات متزامنة (وإلا ستواجه حالات تنافس).

تأكد من الاطلاع على [قسم الأمثلة](https://github.com/webdriverio/webdriverio/tree/main/examples/wdio) حيث يمكنك العثور على مثال لمراسل مخصص يطبع اسم الحدث لكل حدث.

إذا قمت بتنفيذ مراسل مخصص يمكن أن يكون مفيدًا للمجتمع، فلا تتردد في إجراء طلب سحب حتى نتمكن من جعل المراسل متاحًا للجمهور!

أيضًا، إذا قمت بتشغيل مشغل اختبار WDIO عبر واجهة `Launcher`، فلا يمكنك تطبيق مراسل مخصص كدالة على النحو التالي:

```js
import Launcher from '@wdio/cli'

import CustomReporter from './reporter/my.custom.reporter'

const launcher = new Launcher('/path/to/config.file.js', {
    // هذا لن يعمل، لأن CustomReporter غير قابل للتسلسل
    reporters: ['dot', CustomReporter]
})
```

## الانتظار حتى `isSynchronised`

إذا كان المراسل الخاص بك يحتاج إلى تنفيذ عمليات غير متزامنة للإبلاغ عن البيانات (على سبيل المثال، تحميل ملفات السجل أو الأصول الأخرى)، يمكنك تجاوز طريقة `isSynchronised` في المراسل المخصص الخاص بك للسماح لمشغل WebdriverIO بالانتظار حتى تنتهي من حساب كل شيء. يمكن رؤية مثال على ذلك في [`@wdio/sumologic-reporter`](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-sumologic-reporter/src/index.ts):

```js
export default class SumoLogicReporter extends WDIOReporter {
    constructor (options) {
        // ...
        this.unsynced = []
        this.interval = setInterval(::this.sync, this.options.syncInterval)
        // ...
    }

    /**
     * تجاوز طريقة isSynchronised
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
             * إزالة السجلات المنقولة من سلة السجلات
             */
            this.unsynced.splice(0, MAX_LINES)
            // ...
        }
    }
}
```

بهذه الطريقة سينتظر المشغل حتى يتم تحميل جميع معلومات السجل.

## نشر المراسل على NPM

لجعل المراسل أسهل للاستخدام والاكتشاف من قبل مجتمع WebdriverIO، يرجى اتباع هذه التوصيات:

* يجب أن تستخدم الخدمات اتفاقية التسمية هذه: `wdio-*-reporter`
* استخدم كلمات NPM المفتاحية: `wdio-plugin` و `wdio-reporter`
* يجب أن يقوم إدخال `main` بتصدير مثيل المراسل
* مثال على المراسل: [`@wdio/dot-service`](https://github.com/webdriverio/webdriverio/tree/main/packages/wdio-dot-reporter)

اتباع نمط التسمية الموصى به يسمح بإضافة الخدمات بالاسم:

```js
// إضافة wdio-custom-reporter
export const config = {
    // ...
    reporter: ['custom'],
    // ...
}
```

### إضافة الخدمة المنشورة إلى واجهة سطر أوامر WDIO والوثائق

نحن نقدر حقًا كل مكون إضافي جديد يمكن أن يساعد الآخرين على إجراء اختبارات أفضل! إذا كنت قد أنشأت مثل هذا المكون الإضافي، فيرجى التفكير في إضافته إلى واجهة سطر الأوامر والوثائق الخاصة بنا لجعله أسهل في العثور عليه.

يرجى رفع طلب سحب مع التغييرات التالية:

- أضف خدمتك إلى قائمة [المراسلين المدعومين](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-cli/src/constants.ts#L74-L91)) في وحدة واجهة سطر الأوامر
- قم بتحسين [قائمة المراسلين](https://github.com/webdriverio/webdriverio/blob/main/scripts/docs-generation/3rd-party/reporters.json) لإضافة وثائقك إلى صفحة Webdriver.io الرسمية