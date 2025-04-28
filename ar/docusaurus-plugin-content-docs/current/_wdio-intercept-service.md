---
id: wdio-intercept-service
title: خدمة الاعتراض
custom_edit_url: https://github.com/webdriverio-community/wdio-intercept-service/edit/main/README.md
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

> wdio-intercept-service هي حزمة خارجية، لمزيد من المعلومات يرجى زيارة [GitHub](https://github.com/webdriverio-community/wdio-intercept-service) | [npm](https://www.npmjs.com/package/wdio-intercept-service)

🕸 التقاط وتأكيد مكالمات HTTP ajax في [webdriver.io](http://webdriver.io/)

[![Tests](https://github.com/webdriverio-community/wdio-intercept-service/actions/workflows/test.yaml/badge.svg)](https://github.com/webdriverio-community/wdio-intercept-service/actions/workflows/test.yaml) [![Join the chat on Discord](https://img.shields.io/discord/1097401827202445382?logo=discord&logoColor=FFFFFF&color=5865F2)](https://discord.webdriver.io/)

هذا هو ملحق لـ [webdriver.io](http://webdriver.io/). إذا لم تكن تعرفه بعد، تحقق منه، إنه رائع جدًا.

على الرغم من أن selenium و webdriver يستخدمان للاختبار الشامل e2e وخاصة اختبار واجهة المستخدم، قد ترغب في تقييم طلبات HTTP التي يقوم بها كود العميل الخاص بك (على سبيل المثال عندما لا يكون لديك تغذية راجعة فورية من واجهة المستخدم، مثل المقاييس أو تتبع المكالمات). مع wdio-intercept-service يمكنك اعتراض مكالمات HTTP ajax التي بدأت بواسطة إجراء المستخدم (مثل الضغط على زر، إلخ) وإجراء تأكيدات حول الطلب والاستجابات المقابلة لاحقًا.

هناك عائق واحد رغم ذلك: لا يمكنك اعتراض مكالمات HTTP التي يتم بدؤها عند تحميل الصفحة (كما هو الحال في معظم SPAs)، لأنها تتطلب بعض أعمال الإعداد التي لا يمكن القيام بها إلا بعد تحميل الصفحة (بسبب القيود في selenium). **هذا يعني أنه يمكنك فقط التقاط الطلبات التي تم بدؤها داخل اختبار.** إذا كنت على ما يرام مع ذلك، فقد يكون هذا الملحق مناسبًا لك، لذا تابع القراءة.

## المتطلبات المسبقة

* webdriver.io **v5.x** أو أحدث.

**انتبه! إذا كنت لا تزال تستخدم webdriver.io v4، يرجى استخدام فرع v2.x من هذا الملحق!**

## التثبيت

```shell
npm install wdio-intercept-service -D
```

## الاستخدام

### الاستخدام مع WebDriver CLI

يجب أن يكون الأمر سهلاً مثل إضافة wdio-intercept-service إلى ملف `wdio.conf.js` الخاص بك:

```javascript
exports.config = {
  // ...
  services: ['intercept']
  // ...
};
```

وأنت جاهز.

### الاستخدام مع WebDriver Standalone

عند استخدام WebdriverIO Standalone، يجب استدعاء وظائف `before` و`beforeTest` / `beforeScenario` يدويًا.

```javascript
import { remote } from 'webdriverio';
import WebdriverAjax from 'wdio-intercept-service'

const WDIO_OPTIONS = {
  port: 9515,
  path: '/',
  capabilities: {
    browserName: 'chrome'
  },
}

let browser;
const interceptServiceLauncher = WebdriverAjax();

beforeAll(async () => {
  browser = await remote(WDIO_OPTIONS)
  interceptServiceLauncher.before(null, null, browser)
})

beforeEach(async () => {
  interceptServiceLauncher.beforeTest()
})

afterAll(async () => {
  await client.deleteSession()
});

describe('', async () => {
  ... // See example usage
});
```

بمجرد التهيئة، تتم إضافة بعض الوظائف ذات الصلة إلى سلسلة أوامر المتصفح الخاص بك (انظر [API](#api)).

## البدء السريع

مثال على الاستخدام:

```javascript
browser.url('http://foo.bar');
browser.setupInterceptor(); // capture ajax calls
browser.expectRequest('GET', '/api/foo', 200); // expect GET request to /api/foo with 200 statusCode
browser.expectRequest('POST', '/api/foo', 400); // expect POST request to /api/foo with 400 statusCode
browser.expectRequest('GET', /\/api\/foo/, 200); // can validate a URL with regex, too
browser.click('#button'); // button that initiates ajax request
browser.pause(1000); // maybe wait a bit until request is finished
browser.assertRequests(); // validate the requests
```

الحصول على تفاصيل حول الطلبات:

```javascript
browser.url('http://foo.bar')
browser.setupInterceptor();
browser.click('#button')
browser.pause(1000);

var request = browser.getRequest(0);
assert.equal(request.method, 'GET');
assert.equal(request.response.headers['content-length'], '42');
```

## المتصفحات المدعومة

يجب أن تعمل مع إصدارات حديثة نوعًا ما من جميع المتصفحات. يرجى الإبلاغ عن مشكلة إذا لم يبدو أنها تعمل مع المتصفح الخاص بك.

## API

راجع ملف إعلان TypeScript للحصول على الصيغة الكاملة للأوامر المخصصة المضافة إلى كائن متصفح WebdriverIO. بشكل عام، يمكن استدعاء أي طريقة تأخذ كائن "options" كمعلمة بدون تلك المعلمة للحصول على السلوك الافتراضي. هذه الكائنات "options الاختيارية" متبوعة بـ `?: = {}` والقيم الافتراضية المستنتجة موصوفة لكل طريقة.

### وصف الخيارات

توفر هذه المكتبة قدرًا صغيرًا من التكوين عند إصدار الأوامر. يتم وصف خيارات التكوين التي تستخدمها طرق متعددة هنا (انظر تعريف كل طريقة لتحديد الدعم المحدد).

* `orderBy` (`'START' | 'END'`): يتحكم هذا الخيار في ترتيب الطلبات التي تم التقاطها بواسطة المعترض، عند إرجاعها إلى اختبارك. للتوافق مع الإصدارات الحالية من هذه المكتبة، الترتيب الافتراضي هو `'END'`، وهو ما يتوافق مع وقت اكتمال الطلب. إذا قمت بتعيين خيار `orderBy` إلى `'START'`، فسيتم ترتيب الطلبات وفقًا للوقت الذي تم بدؤها فيه.
* `includePending` (`boolean`): يتحكم هذا الخيار في ما إذا كانت الطلبات التي لم تكتمل بعد سيتم إرجاعها. للتوافق مع الإصدارات الحالية من هذه المكتبة، القيمة الافتراضية هي `false`، وسيتم إرجاع الطلبات المكتملة فقط.

### browser.setupInterceptor()

يلتقط مكالمات ajax في المتصفح. يجب عليك دائمًا استدعاء وظيفة الإعداد من أجل تقييم الطلبات لاحقًا.

### browser.disableInterceptor()

يمنع المزيد من التقاط مكالمات ajax في المتصفح. تتم إزالة جميع معلومات الطلب الملتقطة. لن يحتاج معظم المستخدمين إلى تعطيل المعترض، ولكن إذا كان الاختبار يستمر لفترة طويلة بشكل خاص أو يتجاوز سعة تخزين الجلسة، فإن تعطيل المعترض يمكن أن يكون مفيدًا.

### `browser.excludeUrls(urlRegexes: (string | RegExp)[])`

يستبعد الطلبات من عناوين URL معينة من التسجيل. يأخذ مصفوفة من السلاسل أو التعبيرات العادية. قبل الكتابة في التخزين،
يختبر عنوان URL للطلب ضد كل سلسلة أو تعبير عادي. إذا كان كذلك، فلن يتم كتابة الطلب في التخزين. مثل disableInterceptor، يمكن أن يكون هذا مفيدًا
إذا كنت تواجه مشاكل تتعلق بتجاوز سعة تخزين الجلسة.

### browser.expectRequest(method: string, url: string, statusCode: number)

قم بإجراء توقعات حول طلبات ajax التي سيتم بدؤها أثناء الاختبار. يمكن (ويجب) ربطها. يجب أن يتطابق ترتيب التوقعات مع ترتيب الطلبات التي يتم إجراؤها.

* `method` (`String`): طريقة http المتوقعة. يمكن أن تكون أي شيء تقبله `xhr.open()` كوسيطة أولى.
* `url` (`String`|`RegExp`): عنوان URL الدقيق الذي يتم استدعاؤه في الطلب كسلسلة أو تعبير منتظم للمطابقة
* `statusCode` (`Number`): رمز الحالة المتوقع للاستجابة

### browser.getExpectations()

طريقة مساعدة. تعيد جميع التوقعات التي قمت بها حتى تلك النقطة

### browser.resetExpectations()

طريقة مساعدة. تعيد تعيين جميع التوقعات التي قمت بها حتى تلك النقطة

### `browser.assertRequests({ orderBy?: 'START' | 'END' }?: = {})`

استدعِ هذه الطريقة عندما تنتهي جميع طلبات ajax المتوقعة. تقارن التوقعات بالطلبات الفعلية المقدمة وتؤكد ما يلي:

- عدد الطلبات التي تم تقديمها
- ترتيب الطلبات
- يجب أن تتطابق الطريقة وعنوان URL ورمز الحالة لكل طلب تم إجراؤه
- كائن الخيارات يكون افتراضيًا `{ orderBy: 'END' }`، أي عندما تم الانتهاء من الطلبات، ليكون متسقًا مع سلوك الإصدار 4.1.10 والإصدارات السابقة. عندما يتم تعيين خيار `orderBy` إلى `'START'`، سيتم ترتيب الطلبات حسب وقت بدئها من قبل الصفحة.

### `browser.assertExpectedRequestsOnly({ inOrder?: boolean, orderBy?: 'START' | 'END' }?: = {})`

مشابهة لـ `browser.assertRequests`، لكنها تتحقق فقط من الطلبات التي تحددها في توجيهات `expectRequest` الخاصة بك، دون الحاجة إلى رسم خريطة لجميع طلبات الشبكة التي قد تحدث حول ذلك. إذا كان خيار `inOrder` هو `true` (افتراضي)، فمن المتوقع أن يتم العثور على الطلبات بنفس الترتيب الذي تم إعدادها به باستخدام `expectRequest`.

### `browser.getRequest(index: number, { includePending?: boolean, orderBy?: 'START' | 'END' }?: = {})`

لإجراء تأكيدات أكثر تعقيدًا حول طلب محدد، يمكنك الحصول على تفاصيل لطلب محدد. يجب عليك توفير فهرس الطلب المستند إلى 0 الذي تريد الوصول إليه، بالترتيب الذي تم فيه الانتهاء من الطلبات (افتراضي)، أو بدأت (عن طريق تمرير خيار `orderBy: 'START'`).

* `index` (`number`): رقم الطلب الذي تريد الوصول إليه
* `options` (`object`): خيارات التكوين
* `options.includePending` (`boolean`): ما إذا كان يجب إرجاع الطلبات التي لم تكتمل بعد. بشكل افتراضي، هذا هو false، لمطابقة سلوك المكتبة في الإصدار 4.1.10 والإصدارات السابقة.
* `options.orderBy` (`'START' | 'END'`): كيف يجب ترتيب الطلبات. بشكل افتراضي، هذا هو `'END'`، لمطابقة سلوك المكتبة في الإصدار 4.1.10 والإصدارات السابقة. إذا كان `'START'`، فسيتم ترتيب الطلبات حسب وقت البدء، بدلاً من وقت اكتمال الطلب. (نظرًا لأن الطلب المعلق لم يكتمل بعد، عند الترتيب حسب `'END'` ستأتي جميع الطلبات المعلقة بعد جميع الطلبات المكتملة.)

**يعيد** كائن `request`:

* `request.url`: عنوان URL المطلوب
* `request.method`: طريقة HTTP المستخدمة
* `request.body`: بيانات الحمولة/النص المستخدمة في الطلب
* `request.headers`: رؤوس http للطلب ككائن JS
* `request.pending`: علامة منطقية لما إذا كان هذا الطلب مكتملاً (أي له خاصية `response`)، أو قيد التنفيذ.
* `request.response`: كائن JS يكون موجودًا فقط إذا اكتمل الطلب (أي `request.pending === false`)، ويحتوي على بيانات حول الاستجابة.
* `request.response?.headers`: رؤوس http للاستجابة ككائن JS
* `request.response?.body`: نص الاستجابة (سيتم تحليله كـ JSON إذا أمكن)
* `request.response?.statusCode`: رمز حالة الاستجابة

**ملاحظة حول `request.body`:** ستحاول wdio-intercept-service تحليل نص الطلب على النحو التالي:

* سلسلة: فقط إرجاع السلسلة (`'value'`)
* JSON: تحليل كائن JSON باستخدام `JSON.parse()` (`({ key: value })`)
* FormData: سيخرج FormData بالتنسيق `{ key: [value1, value2, ...] }`
* ArrayBuffer: سيحاول تحويل المخزن المؤقت إلى سلسلة (تجريبي)
* أي شيء آخر: سيستخدم `JSON.stringify()` بشكل قاسي على البيانات الخاصة بك. حظاً سعيداً!

**بالنسبة لواجهة برمجة التطبيقات `fetch`، نحن ندعم فقط بيانات السلاسل و JSON!**

### `browser.getRequests({ includePending?: boolean, orderBy?: 'START' | 'END' }?: = {})`

احصل على جميع الطلبات الملتقطة كمصفوفة، مع دعم نفس الخيارات الاختيارية مثل `getRequest`.

**يعيد** مصفوفة من كائنات `request`.

### browser.hasPendingRequests()

طريقة مساعدة تتحقق مما إذا كانت أي طلبات HTTP لا تزال معلقة. يمكن استخدامها من قبل الاختبارات للتأكد من اكتمال جميع الطلبات خلال فترة زمنية معقولة، أو للتحقق من أن استدعاء `getRequests()` أو `assertRequests()` سيتضمن جميع طلبات HTTP المرغوبة.

**يعيد** قيمة منطقية

## دعم TypeScript

يوفر هذا الملحق أنواع TS الخاصة به. ما عليك سوى توجيه tsconfig الخاص بك إلى امتدادات النوع كما هو مذكور [هنا](https://webdriver.io/docs/typescript.html#framework-types):

```
"compilerOptions": {
    // ..
    "types": ["node", "webdriverio", "wdio-intercept-service"]
},
```

## تشغيل الاختبارات

تتطلب إصدارات حديثة من Chrome و Firefox لتشغيل الاختبارات محليًا. قد تحتاج إلى تحديث تبعيات `chromedriver` و `geckodriver` لتتطابق مع الإصدار المثبت على نظامك.

```shell
npm test
```

## المساهمة

أنا سعيد بكل مساهمة. فقط افتح مشكلة أو قم بتقديم PR مباشرة.  
يرجى ملاحظة أن مكتبة الاعتراض هذه مكتوبة للعمل مع المتصفحات القديمة مثل Internet Explorer. على هذا النحو، يجب أن يكون أي رمز مستخدم في `lib/interceptor.js` قابلاً للتحليل على الأقل بواسطة وقت تشغيل JavaScript في Internet Explorer.

## الترخيص

MIT