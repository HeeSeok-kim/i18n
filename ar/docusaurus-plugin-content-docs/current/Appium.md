---
id: appium
title: إعداد Appium
---

باستخدام WebdriverIO يمكنك اختبار ليس فقط تطبيقات الويب في المتصفح ولكن أيضًا منصات أخرى مثل:

- 📱 تطبيقات الهاتف المحمول على iOS وAndroid وTizen
- 🖥️ تطبيقات سطح المكتب على macOS أو Windows
- 📺 وكذلك تطبيقات التلفزيون لـ Roku وtvOS وAndroid TV وSamsung

نوصي باستخدام [Appium](https://appium.io/) لمساعدتك في تسهيل هذه الأنواع من الاختبارات. يمكنك الحصول على نظرة عامة على Appium في [صفحة التوثيق الرسمية](https://appium.io/docs/en/2.0/intro/).

إعداد البيئة المناسبة ليس أمرًا سهلاً مباشرًا. لحسن الحظ، يحتوي نظام Appium على أدوات رائعة للمساعدة في ذلك. لإعداد إحدى البيئات المذكورة أعلاه، ما عليك سوى تشغيل:

```sh
$ npx appium-installer
```

سيبدأ هذا الأمر تشغيل أداة [appium-installer](https://github.com/AppiumTestDistribution/appium-installer) التي ترشدك خلال عملية الإعداد.