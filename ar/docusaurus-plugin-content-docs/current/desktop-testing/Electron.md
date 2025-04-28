---
id: electron
title: إلكترون
---

إلكترون هو إطار عمل لبناء تطبيقات سطح المكتب باستخدام JavaScript وHTML وCSS. من خلال تضمين Chromium وNode.js في الملف الثنائي، يسمح لك إلكترون بالحفاظ على قاعدة شفرة JavaScript واحدة وإنشاء تطبيقات عبر المنصات المختلفة تعمل على Windows وmacOS وLinux - لا تتطلب خبرة في التطوير الأصلي.

يوفر WebdriverIO خدمة متكاملة تبسط التفاعل مع تطبيق إلكترون الخاص بك وتجعل اختباره سهلاً للغاية. مزايا استخدام WebdriverIO لاختبار تطبيقات إلكترون هي:

- 🚗 إعداد تلقائي لـ Chromedriver المطلوب
- 📦 اكتشاف تلقائي لمسار تطبيق إلكترون الخاص بك - يدعم [Electron Forge](https://www.electronforge.io/) و[Electron Builder](https://www.electron.build/)
- 🧩 الوصول إلى واجهات برمجة تطبيقات إلكترون داخل اختباراتك
- 🕵️ محاكاة واجهات برمجة تطبيقات إلكترون عبر واجهة برمجة تشبه Vitest

تحتاج فقط إلى بضع خطوات بسيطة للبدء. شاهد هذا البرنامج التعليمي البسيط للبدء خطوة بخطوة من قناة [WebdriverIO YouTube](https://www.youtube.com/@webdriverio):

<LiteYouTubeEmbed
    id="iQNxTdWedk0"
    title="Getting Started with ElectronJS Testing in WebdriverIO"
/>

أو اتبع الدليل في القسم التالي.

## البدء

لبدء مشروع WebdriverIO جديد، قم بتشغيل:

```sh
npm create wdio@latest ./
```

سيرشدك معالج التثبيت خلال العملية. تأكد من تحديد _"Desktop Testing - of Electron Applications"_ عندما يسألك عن نوع الاختبار الذي ترغب في القيام به. بعد ذلك، قم بتوفير المسار إلى تطبيق إلكترون المجمع الخاص بك، مثل `./dist`، ثم احتفظ بالإعدادات الافتراضية أو قم بتعديلها حسب تفضيلك.

سيقوم معالج التكوين بتثبيت جميع الحزم المطلوبة وإنشاء ملف `wdio.conf.js` أو `wdio.conf.ts` مع التكوين اللازم لاختبار تطبيقك. إذا وافقت على إنشاء بعض ملفات الاختبار تلقائيًا، يمكنك تشغيل أول اختبار لك عبر `npm run wdio`.

## الإعداد اليدوي

إذا كنت تستخدم بالفعل WebdriverIO في مشروعك، يمكنك تخطي معالج التثبيت وإضافة التبعيات التالية فقط:

```sh
npm install --save-dev wdio-electron-service
```

ثم يمكنك استخدام التكوين التالي:

```ts
// wdio.conf.ts
export const config: WebdriverIO.Config = {
    // ...
    services: [['electron', {
        appEntryPoint: './path/to/bundled/electron/main.bundle.js',
        appArgs: [/** ... */],
    }]]
}
```

هذا كل شيء 🎉

تعرف على المزيد حول كيفية [تكوين خدمة إلكترون](/docs/desktop-testing/electron/configuration)، [كيفية محاكاة واجهات برمجة تطبيقات إلكترون](/docs/desktop-testing/electron/mocking) و[كيفية الوصول إلى واجهات برمجة تطبيقات إلكترون](/docs/desktop-testing/electron/api).