---
id: file-download
title: تنزيل الملفات
---

عند أتمتة تنزيل الملفات في اختبار الويب، من الضروري التعامل معها بشكل متسق عبر مختلف المتصفحات لضمان تنفيذ اختبار موثوق.

هنا، نقدم أفضل الممارسات لتنزيل الملفات ونوضح كيفية تكوين أدلة التنزيل لـ **جوجل كروم**، **موزيلا فايرفوكس**، و **مايكروسوفت إيدج**.

## مسارات التنزيل

**كتابة** مسارات التنزيل مباشرة في نصوص الاختبار يمكن أن تؤدي إلى مشاكل في الصيانة ومشاكل في النقل. استخدم **المسارات النسبية** لأدلة التنزيل لضمان قابلية النقل والتوافق عبر البيئات المختلفة.

```javascript
// 👎
// مسار تنزيل مكتوب مباشرة
const downloadPath = '/path/to/downloads';

// 👍
// مسار تنزيل نسبي
const downloadPath = path.join(__dirname, 'downloads');
```

## استراتيجيات الانتظار

الفشل في تنفيذ استراتيجيات انتظار مناسبة يمكن أن يؤدي إلى تنافس الشروط أو اختبارات غير موثوقة، خاصة لاكتمال التنزيل. قم بتنفيذ استراتيجيات انتظار **صريحة** للانتظار حتى اكتمال تنزيل الملفات، مما يضمن التزامن بين خطوات الاختبار.

```javascript
// 👎
// لا يوجد انتظار صريح لاكتمال التنزيل
await browser.pause(5000);

// 👍
// انتظر اكتمال تنزيل الملف
await waitUntil(async ()=> await fs.existsSync(downloadPath), 5000);
```

## تكوين أدلة التنزيل

لتجاوز سلوك تنزيل الملفات لـ **جوجل كروم**، **موزيلا فايرفوكس**، و **مايكروسوفت إيدج**، قم بتوفير دليل التنزيل في إمكانيات WebDriverIO:

<Tabs
defaultValue="chrome"
values={[
{label: 'Chrome', value: 'chrome'},
{label: 'Firefox', value: 'firefox'},
{label: 'Microsoft Edge', value: 'edge'},
]
}>

<TabItem value='chrome'>

```javascript reference title="wdio.conf.js"

https://github.com/webdriverio/example-recipes/blob/84dda93011234d0b2a34ee0cfb3cdfa2a06136a5/testDownloadBehavior/wdio.conf.js#L8-L16

```

</TabItem>

<TabItem value='firefox'>

```javascript reference title="wdio.conf.js"

https://github.com/webdriverio/example-recipes/blob/84dda93011234d0b2a34ee0cfb3cdfa2a06136a5/testDownloadBehavior/wdio.conf.js#L20-L32

```

</TabItem>

<TabItem value='edge'>

```javascript reference title="wdio.conf.js"

https://github.com/webdriverio/example-recipes/blob/84dda93011234d0b2a34ee0cfb3cdfa2a06136a5/testDownloadBehavior/wdio.conf.js#L36-L44

```

</TabItem>

</Tabs>

للحصول على مثال تنفيذي، راجع [وصفة اختبار سلوك التنزيل في WebdriverIO](https://github.com/webdriverio/example-recipes/tree/main/testDownloadBehavior).

## تكوين تنزيلات متصفحات كروميوم

لتغيير مسار التنزيل لمتصفحات __Chromium-based__ (مثل Chrome، و Edge، و Brave، إلخ) باستخدام طريقة `getPuppeteer` الخاصة بـ WebDriverIO للوصول إلى Chrome DevTools.

```javascript
const page = await browser.getPuppeteer();
// بدء جلسة CDP:
const cdpSession = await page.target().createCDPSession();
// تعيين مسار التنزيل:
await cdpSession.send('Browser.setDownloadBehavior', { behavior: 'allow', downloadPath: downloadPath });
```

## التعامل مع تنزيلات ملفات متعددة

عند التعامل مع سيناريوهات تتضمن تنزيلات ملفات متعددة، من الضروري تنفيذ استراتيجيات لإدارة والتحقق من كل تنزيل بشكل فعال. خذ بعين الاعتبار النهج التالية:

__معالجة التنزيل المتسلسل:__ قم بتنزيل الملفات واحدًا تلو الآخر والتحقق من كل تنزيل قبل بدء التنزيل التالي لضمان التنفيذ المنظم والتحقق الدقيق.

__معالجة التنزيل المتوازي:__ استخدم تقنيات البرمجة غير المتزامنة لبدء تنزيلات ملفات متعددة في وقت واحد، مما يحسن وقت تنفيذ الاختبار. قم بتنفيذ آليات تحقق قوية للتحقق من جميع التنزيلات عند الاكتمال.

## اعتبارات التوافق بين المتصفحات

في حين أن WebDriverIO يوفر واجهة موحدة لأتمتة المتصفح، من الضروري مراعاة الاختلافات في سلوك المتصفح وقدراته. فكر في اختبار وظيفة تنزيل الملفات عبر متصفحات مختلفة لضمان التوافق والاتساق.

__التكوينات الخاصة بالمتصفح:__ قم بضبط إعدادات مسار التنزيل واستراتيجيات الانتظار لاستيعاب الاختلافات في سلوك المتصفح والتفضيلات عبر Chrome و Firefox و Edge والمتصفحات المدعومة الأخرى.

__توافق إصدار المتصفح:__ قم بتحديث WebDriverIO وإصدارات المتصفح بانتظام للاستفادة من أحدث الميزات والتحسينات مع ضمان التوافق مع مجموعة الاختبار الحالية الخاصة بك.