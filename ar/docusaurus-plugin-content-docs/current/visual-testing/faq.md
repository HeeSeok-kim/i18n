---
id: faq
title: الأسئلة المتكررة
---

### هل يجب علي استخدام طرق `save(Screen/Element/FullPageScreen)` عندما أريد تشغيل `check(Screen/Element/FullPageScreen)`؟

لا، لست بحاجة إلى القيام بذلك. الطريقة `check(Screen/Element/FullPageScreen)` ستقوم بذلك تلقائيًا نيابة عنك.

### تفشل اختبارات العرض الخاصة بي بسبب وجود اختلاف، كيف يمكنني تحديث خط الأساس الخاص بي؟

يمكنك تحديث صور خط الأساس من خلال سطر الأوامر بإضافة المعامل `--update-visual-baseline`. هذا سوف

-   ينسخ تلقائيًا لقطة الشاشة الفعلية ويضعها في مجلد خط الأساس
-   إذا كانت هناك اختلافات سيسمح للاختبار بالنجاح لأن خط الأساس قد تم تحديثه

**الاستخدام:**

```sh
npm run test.local.desktop  --update-visual-baseline
```

عند تشغيل سجلات المعلومات/التصحيح ستشاهد السجلات التالية مضافة

```logs
[0-0] ..............
[0-0] #####################################################################################
[0-0]  INFO:
[0-0]  Updated the actual image to
[0-0]  /Users/wswebcreation/Git/wdio/visual-testing/localBaseline/chromel/demo-chrome-1366x768.png
[0-0] #####################################################################################
[0-0] ..........
```

### لا يمكن أن يكون العرض والارتفاع بقيم سالبة

قد يتم إلقاء الخطأ `Width and height cannot be negative`. في 9 من أصل 10 حالات، يكون هذا متعلقًا بإنشاء صورة لعنصر غير موجود في العرض. يرجى التأكد دائمًا من أن العنصر موجود في العرض قبل محاولة إنشاء صورة للعنصر.

### فشل تثبيت Canvas على ويندوز مع سجلات Node-Gyp

إذا واجهت مشاكل في تثبيت Canvas على ويندوز بسبب أخطاء Node-Gyp، يرجى ملاحظة أن هذا ينطبق فقط على الإصدار 4 وما قبله. لتجنب هذه المشاكل، ننصح بالتحديث إلى الإصدار 5 أو أعلى، والذي لا يحتوي على هذه التبعيات ويستخدم [Jimp](https://github.com/jimp-dev/jimp) لمعالجة الصور.

إذا كنت لا تزال بحاجة إلى حل المشاكل مع الإصدار 4، يرجى التحقق من:

-   قسم Node Canvas في دليل [البدء](/docs/visual-testing#system-requirements)
-   [هذا المنشور](https://spin.atomicobject.com/2019/03/27/node-gyp-windows/) لإصلاح مشاكل Node-Gyp على ويندوز. (شكراً لـ [IgorSasovets](https://github.com/IgorSasovets))