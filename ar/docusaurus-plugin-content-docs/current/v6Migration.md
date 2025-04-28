---
id: v6-migration
title: من الإصدار الخامس إلى الإصدار السادس
---

هذا البرنامج التعليمي مخصص للأشخاص الذين ما زالوا يستخدمون `v5` من WebdriverIO ويرغبون في الترقية إلى `v6` أو إلى أحدث إصدار من WebdriverIO. كما ذكرنا في [منشور مدونة الإصدار](https://webdriver.io/blog/2020/03/26/webdriverio-v6-released) يمكن تلخيص التغييرات لترقية هذا الإصدار على النحو التالي:

- قمنا بتوحيد المعلمات لبعض الأوامر (مثل `newWindow`, `react$`, `react$$`, `waitUntil`, `dragAndDrop`, `moveTo`, `waitForDisplayed`, `waitForEnabled`, `waitForExist`) ونقلنا جميع المعلمات الاختيارية إلى كائن واحد، على سبيل المثال:

    ```js
    // v5
    browser.newWindow(
        'https://webdriver.io',
        'WebdriverIO window',
        'width=420,height=230,resizable,scrollbars=yes,status=1'
    )
    // v6
    browser.newWindow('https://webdriver.io', {
        windowName: 'WebdriverIO window',
        windowFeature: 'width=420,height=230,resizable,scrollbars=yes,status=1'
    })
    ```

- انتقلت إعدادات الخدمات إلى قائمة الخدمات، على سبيل المثال:

    ```js
    // v5
    exports.config = {
        services: ['sauce'],
        sauceConnect: true,
        sauceConnectOpts: { foo: 'bar' },
    }
    // v6
    exports.config = {
        services: [['sauce', {
            sauceConnect: true,
            sauceConnectOpts: { foo: 'bar' }
        }]],
    }
    ```

- تمت إعادة تسمية بعض خيارات الخدمة لأغراض التبسيط
- قمنا بإعادة تسمية الأمر `launchApp` إلى `launchChromeApp` لجلسات Chrome WebDriver

:::info

إذا كنت تستخدم WebdriverIO `v4` أو أقل، يرجى الترقية إلى `v5` أولاً.

:::

في حين أننا نود أن يكون لدينا عملية آلية بالكامل لهذا، إلا أن الواقع يبدو مختلفًا. لكل شخص إعداد مختلف. يجب اعتبار كل خطوة كدليل وليس كتعليمات خطوة بخطوة. إذا واجهت مشاكل في عملية الترقية، فلا تتردد في [الاتصال بنا](https://github.com/webdriverio/codemod/discussions/new).

## الإعداد

على غرار عمليات الترقية الأخرى، يمكننا استخدام [codemod](https://github.com/webdriverio/codemod) من WebdriverIO. لتثبيت codemod، قم بتشغيل:

```sh
npm install jscodeshift @wdio/codemod
```

## ترقية تبعيات WebdriverIO

نظرًا لأن جميع إصدارات WebdriverIO مرتبطة ببعضها البعض، فمن الأفضل دائمًا الترقية إلى علامة محددة، مثل `6.12.0`. إذا قررت الترقية من `v5` مباشرةً إلى `v7`، يمكنك ترك العلامة وتثبيت أحدث إصدارات من جميع الحزم. للقيام بذلك، نقوم بنسخ جميع التبعيات المتعلقة بـ WebdriverIO من ملف `package.json` ونعيد تثبيتها عبر:

```sh
npm i --save-dev @wdio/allure-reporter@6 @wdio/cli@6 @wdio/cucumber-framework@6 @wdio/local-runner@6 @wdio/spec-reporter@6 @wdio/sync@6 wdio-chromedriver-service@6 webdriverio@6
```

عادة ما تكون تبعيات WebdriverIO جزءًا من تبعيات التطوير، اعتمادًا على مشروعك يمكن أن يختلف هذا رغم ذلك. بعد هذا، يجب تحديث ملفات `package.json` و`package-lock.json`. __ملاحظة:__ هذه تبعيات مثالية، قد تختلف التبعيات الخاصة بك. تأكد من العثور على أحدث إصدار v6 من خلال الاتصال، على سبيل المثال:

```sh
npm show webdriverio versions
```

حاول تثبيت أحدث إصدار 6 متاح لجميع حزم WebdriverIO الأساسية. بالنسبة لحزم المجتمع، يمكن أن يختلف هذا من حزمة إلى أخرى. نوصي هنا بالتحقق من سجل التغييرات للحصول على معلومات حول الإصدار الذي لا يزال متوافقًا مع v6.

## تحويل ملف التكوين

خطوة جيدة للبداية هي البدء بملف التكوين. يمكن حل جميع التغييرات الجذرية باستخدام codemod تلقائيًا بالكامل:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v6 ./wdio.conf.js
```

:::caution

لا يدعم codemod بعد مشاريع TypeScript. انظر [`@webdriverio/codemod#10`](https://github.com/webdriverio/codemod/issues/10). نحن نعمل على تنفيذ الدعم له قريبًا. إذا كنت تستخدم TypeScript، يرجى المشاركة!

:::

## تحديث ملفات المواصفات وكائنات الصفحة

لتحديث جميع تغييرات الأوامر، قم بتشغيل codemod على جميع ملفات e2e الخاصة بك التي تحتوي على أوامر WebdriverIO، على سبيل المثال:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v6 ./e2e/*
```

هذا كل شيء! لا مزيد من التغييرات الضرورية 🎉

## الخلاصة

نأمل أن يرشدك هذا البرنامج التعليمي قليلاً خلال عملية الترقية إلى WebdriverIO `v6`. نوصي بشدة بمواصلة الترقية إلى أحدث إصدار نظرًا لأن الترقية إلى `v7` أمر بسيط بسبب عدم وجود تغييرات جذرية تقريبًا. يرجى الاطلاع على دليل الترقية [للترقية إلى v7](v7-migration).

يستمر المجتمع في تحسين codemod أثناء اختباره مع فرق مختلفة في منظمات مختلفة. لا تتردد في [رفع مشكلة](https://github.com/webdriverio/codemod/issues/new) إذا كان لديك تعليقات أو [بدء مناقشة](https://github.com/webdriverio/codemod/discussions/new) إذا كنت تواجه صعوبة أثناء عملية الترقية.