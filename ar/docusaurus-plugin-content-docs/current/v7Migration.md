---
id: v7-migration
title: من v6 إلى v7
---

هذا الدليل مخصص للأشخاص الذين ما زالوا يستخدمون `v6` من WebdriverIO ويرغبون في الترقية إلى `v7`. كما ذكرنا في [منشور المدونة الخاص بالإصدار](https://webdriver.io/blog/2021/02/09/webdriverio-v7-released)، فإن التغييرات هي في الغالب تحت الغطاء وينبغي أن تكون عملية الترقية مباشرة.

:::info

إذا كنت تستخدم WebdriverIO `v5` أو أقل، يرجى الترقية إلى `v6` أولاً. يرجى الاطلاع على [دليل الترقية إلى v6](v6-migration).

:::

بينما نود أن نقدم عملية آلية بالكامل لهذا، إلا أن الواقع مختلف. كل شخص لديه إعداد مختلف. يجب اعتبار كل خطوة كدليل وليس كتعليمات خطوة بخطوة. إذا واجهت مشاكل في عملية الترقية، لا تتردد في [الاتصال بنا](https://github.com/webdriverio/codemod/discussions/new).

## الإعداد

مشابه للترقيات الأخرى، يمكننا استخدام [codemod](https://github.com/webdriverio/codemod) الخاص بـ WebdriverIO. في هذا الدليل، سنستخدم [مشروع نموذجي](https://github.com/WarleyGabriel/demo-webdriverio-cucumber) مقدم من أحد أعضاء المجتمع وسنقوم بترقيته بالكامل من `v6` إلى `v7`.

لتثبيت codemod، قم بتشغيل:

```sh
npm install jscodeshift @wdio/codemod
```

#### الالتزامات:

- _تثبيت تبعيات codemod_ [[6ec9e52]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/6ec9e52038f7e8cb1221753b67040b0f23a8f61a)

## ترقية تبعيات WebdriverIO

نظرًا لأن جميع إصدارات WebdriverIO مرتبطة ببعضها البعض، فإنه من الأفضل دائمًا الترقية إلى علامة محددة، مثل `latest`. للقيام بذلك، نقوم بنسخ جميع التبعيات المتعلقة بـ WebdriverIO من ملف `package.json` الخاص بنا وإعادة تثبيتها عبر:

```sh
npm i --save-dev @wdio/allure-reporter@7 @wdio/cli@7 @wdio/cucumber-framework@7 @wdio/local-runner@7 @wdio/spec-reporter@7 @wdio/sync@7 wdio-chromedriver-service@7 wdio-timeline-reporter@7 webdriverio@7
```

عادة ما تكون تبعيات WebdriverIO جزءًا من تبعيات التطوير، اعتمادًا على مشروعك قد يختلف هذا. بعد ذلك، يجب تحديث ملفات `package.json` و `package-lock.json` الخاصة بك. __ملاحظة:__ هذه هي التبعيات المستخدمة في [المشروع النموذجي](https://github.com/WarleyGabriel/demo-webdriverio-cucumber)، قد تختلف التبعيات الخاصة بك.

#### الالتزامات:

- _تحديث التبعيات_ [[7097ab6]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/7097ab6297ef9f37ead0a9c2ce9fce8d0765458d)

## تحويل ملف التكوين

خطوة جيدة للبدء هي البدء بملف التكوين. في WebdriverIO `v7` لا نحتاج إلى تسجيل أي من المترجمات يدويًا بعد الآن. في الواقع، يجب إزالتها. يمكن القيام بذلك باستخدام codemod بشكل تلقائي بالكامل:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v7 ./wdio.conf.js
```

:::caution

لا يدعم codemod بعد مشاريع TypeScript. انظر [`@webdriverio/codemod#10`](https://github.com/webdriverio/codemod/issues/10). نحن نعمل على تنفيذ دعم لها قريبًا. إذا كنت تستخدم TypeScript، يرجى المشاركة!

:::

#### الالتزامات:

- _ترجمة ملف التكوين_ [[6015534]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/60155346a386380d8a77ae6d1107483043a43994)

## تحديث تعريفات الخطوات

إذا كنت تستخدم Jasmine أو Mocha، فقد انتهيت هنا. الخطوة الأخيرة هي تحديث استيرادات Cucumber.js من `cucumber` إلى `@cucumber/cucumber`. يمكن أيضًا القيام بذلك عبر codemod تلقائيًا:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v7 ./src/e2e/*
```

هذا كل شيء! لا حاجة لمزيد من التغييرات 🎉

#### الالتزامات:

- _ترجمة تعريفات الخطوات_ [[8c97b90]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/8c97b90a8b9197c62dffe4e2954f7dad814753cc)

## الخلاصة

نأمل أن يوجهك هذا الدليل قليلاً خلال عملية الترقية إلى WebdriverIO `v7`. يواصل المجتمع تحسين codemod أثناء اختباره مع فرق مختلفة في منظمات متنوعة. لا تتردد في [إثارة مشكلة](https://github.com/webdriverio/codemod/issues/new) إذا كان لديك تعليقات أو [بدء مناقشة](https://github.com/webdriverio/codemod/discussions/new) إذا كنت تواجه صعوبة أثناء عملية الترقية.