---
id: file-download
title: تنزيل الملفات
---

عند أتمتة تنزيلات الملفات في اختبارات الويب، من الضروري التعامل معها بشكل متسق عبر المتصفحات المختلفة لضمان تنفيذ اختبارات موثوقة.

هنا، نقدم أفضل الممارسات لتنزيل الملفات ونوضح كيفية تكوين أدلة التنزيل لمتصفحات **Google Chrome** و**Mozilla Firefox** و**Microsoft Edge**.

## مسارات التنزيل

**تحديد** مسارات التنزيل بشكل ثابت في نصوص الاختبار يمكن أن يؤدي إلى مشاكل في الصيانة ومشاكل في قابلية النقل. استخدم **مسارات نسبية** لأدلة التنزيل لضمان قابلية النقل والتوافق عبر البيئات المختلفة.

```javascript
// 👎
// مسار تنزيل ثابت
const downloadPath = '/path/to/downloads';

// 👍
// مسار تنزيل نسبي
const downloadPath = path.join(__dirname, 'downloads');
```

## استراتيجيات الانتظار

عدم تنفيذ استراتيجيات انتظار مناسبة يمكن أن يؤدي إلى حالات السباق أو اختبارات غير موثوقة، خاصة لاكتمال التنزيل. قم بتنفيذ استراتيجيات انتظار **صريحة** للانتظار حتى تكتمل تنزيلات الملفات، مما يضمن التزامن بين خطوات الاختبار.

```javascript
// 👎
// لا يوجد انتظار صريح لاكتمال التنزيل
await browser.pause(5000);

// 👍
// انتظار اكتمال تنزيل الملف
await waitUntil(async ()=> await fs.existsSync(downloadPath), 5000);
```

## تكوين أدلة التنزيل

لتجاوز سلوك تنزيل الملفات لمتصفحات **Google Chrome** و**Mozilla Firefox** و**Microsoft Edge**، قم بتوفير دليل التنزيل في إمكانيات WebDriverIO:

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

للاطلاع على تنفيذ مثالي، راجع [وصفة اختبار سلوك التنزيل في WebdriverIO](https://github.com/webdriverio/example-recipes/tree/main/testDownloadBehavior).

## تكوين تنزيلات متصفح Chromium

لتغيير مسار التنزيل لمتصفحات __Chromium__ (مثل Chrome وEdge وBrave وغيرها) باستخدام طريقة `getPuppeteer` من WebDriverIO للوصول إلى Chrome DevTools.

```javascript
const page = await browser.getPuppeteer();
// بدء جلسة CDP:
const cdpSession = await page.target().createCDPSession();
// تعيين مسار التنزيل:
await cdpSession.send('Browser.setDownloadBehavior', { behavior: 'allow', downloadPath: downloadPath });
```

## التعامل مع تنزيلات ملفات متعددة

عند التعامل مع سيناريوهات تتضمن تنزيلات ملفات متعددة، من الضروري تنفيذ استراتيجيات لإدارة والتحقق من كل تنزيل بشكل فعال. ضع في اعتبارك المناهج التالية:

__معالجة التنزيل التسلسلي:__ قم بتنزيل الملفات واحداً تلو الآخر وتحقق من كل تنزيل قبل بدء التنزيل التالي لضمان التنفيذ المنظم والتحقق الدقيق.

__معالجة التنزيل المتوازي:__ استخدم تقنيات البرمجة غير المتزامنة لبدء تنزيلات ملفات متعددة في وقت واحد، مما يحسن وقت تنفيذ الاختبار. قم بتنفيذ آليات تحقق قوية للتحقق من جميع التنزيلات عند الانتهاء.

## اعتبارات التوافق عبر المتصفحات

بينما يوفر WebDriverIO واجهة موحدة لأتمتة المتصفح، من الضروري مراعاة الاختلافات في سلوك المتصفح وقدراته. ضع في اعتبارك اختبار وظيفة تنزيل الملفات عبر متصفحات مختلفة لضمان التوافق والاتساق.

__تكوينات خاصة بالمتصفح:__ اضبط إعدادات مسار التنزيل واستراتيجيات الانتظار لاستيعاب الاختلافات في سلوك المتصفح وتفضيلاته عبر Chrome وFirefox وEdge والمتصفحات المدعومة الأخرى.

__توافق إصدار المتصفح:__ قم بتحديث WebDriverIO وإصدارات المتصفح بانتظام للاستفادة من أحدث الميزات والتحسينات مع ضمان التوافق مع مجموعة الاختبارات الحالية.