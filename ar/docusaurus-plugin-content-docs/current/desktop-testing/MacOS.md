---
id: macos
title: نظام تشغيل ماك
---

يمكن لـ WebdriverIO أتمتة تطبيقات MacOS العشوائية باستخدام [Appium](https://appium.io/docs/en/2.0/). كل ما تحتاجه هو تثبيت [XCode](https://developer.apple.com/xcode/) على نظامك، وتثبيت Appium و[Mac2 Driver](https://github.com/appium/appium-mac2-driver) كتبعية وضبط الإمكانيات الصحيحة.

## البدء

لبدء مشروع WebdriverIO جديد، قم بتشغيل:

```sh
npm create wdio@latest ./
```

سيرشدك معالج التثبيت خلال العملية. تأكد من اختيار _"Desktop Testing - of MacOS Applications"_ عندما يسألك عن نوع الاختبار الذي ترغب في القيام به. بعد ذلك، احتفظ بالإعدادات الافتراضية أو قم بتعديلها حسب تفضيلاتك.

سيقوم معالج التكوين بتثبيت جميع حزم Appium المطلوبة وينشئ ملف `wdio.conf.js` أو `wdio.conf.ts` مع التكوين اللازم للاختبار على MacOS. إذا وافقت على إنشاء بعض ملفات الاختبار تلقائيًا، يمكنك تشغيل اختبارك الأول عبر `npm run wdio`.

<CreateMacOSProjectAnimation />

هذا كل شيء 🎉

## مثال

هكذا يمكن أن يبدو اختبار بسيط يفتح تطبيق الآلة الحاسبة، ويجري عملية حسابية ويتحقق من نتيجتها:

```js
describe('My Login application', () => {
    it('should set a text to a text view', async function () {
        await $('//XCUIElementTypeButton[@label="seven"]').click()
        await $('//XCUIElementTypeButton[@label="multiply"]').click()
        await $('//XCUIElementTypeButton[@label="six"]').click()
        await $('//XCUIElementTypeButton[@title="="]').click()
        await expect($('//XCUIElementTypeStaticText[@label="main display"]')).toHaveText('42')
    });
})
```

__ملاحظة:__ تم فتح تطبيق الآلة الحاسبة تلقائيًا في بداية الجلسة لأن `'appium:bundleId': 'com.apple.calculator'` تم تحديده كخيار للإمكانيات. يمكنك التبديل بين التطبيقات خلال الجلسة في أي وقت.

## مزيد من المعلومات

للحصول على معلومات حول خصوصيات الاختبار على MacOS، نوصي بالاطلاع على مشروع [Appium Mac2 Driver](https://github.com/appium/appium-mac2-driver).