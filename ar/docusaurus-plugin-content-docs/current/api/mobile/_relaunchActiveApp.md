---
id: relaunchActiveApp
title: إعادة تشغيل التطبيق النشط
custom_edit_url: https://github.com/webdriverio/webdriverio/edit/main/packages/webdriverio/src/commands/mobile/relaunchActiveApp.ts
---

يقوم بإعادة تشغيل التطبيق الأصلي النشط عن طريق:

- إنهاء التطبيق النشط
- إطلاق التطبيق النشط السابق

:::important
سيقوم هذا الأمر بإعادة تشغيل (إنهاء/إغلاق وإطلاق/بدء) التطبيق النشط الحالي ولن يعيد تعيين حالة التطبيق. لا يمكن لـ Appium إجراء إعادة تعيين كاملة للتطبيق إلا في حالة:

- بدء جلسة جديدة وقيام معالج الجلسة بإزالة حالة التطبيق/تنظيف الجهاز
- وجود باب خلفي في تطبيقك لإعادة تعيين حالة التطبيق ويمكن لـ Appium استدعاء هذا الباب الخلفي

إذا كنت ترغب في إعادة تعيين حالة التطبيق لنظامي Android أو iOS، فأنت بحاجة إلى إنشاء آلية/أمر إعادة تعيين خاص بك في البرنامج النصي. قد تكون الخيارات:

- Android: استخدم أمر `adb` لمسح بيانات التطبيق: `adb shell pm clear <appPackage>`
- iOS: أعد تثبيت التطبيق باستخدام أمر `mobile: installApp`
- ....
- عدم استخدام هذا الأمر

تعتمد الخيارات المتاحة لك على المنصة والتطبيق والموقع (محلي مع إمكانية الوصول الكاملة إلى الجهاز في معظم الأوقات، أو في السحابة مع وصول أقل) الذي تختبره.

:::

##### مثال

```js title="restart.app.js"
it('should restart the app with default options', async () => {
    await browser.relaunchActiveApp()
})
```