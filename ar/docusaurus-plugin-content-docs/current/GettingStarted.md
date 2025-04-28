---
id: gettingstarted
title: البدء
---

مرحبًا بك في توثيق WebdriverIO. سيساعدك هذا على البدء بسرعة. إذا واجهت مشاكل، يمكنك العثور على المساعدة والإجابات على [خادم الدعم Discord](https://discord.webdriver.io) أو يمكنك التواصل معي على [Twitter](https://twitter.com/webdriverio).

:::info
هذه هي الوثائق للإصدار الأحدث (__>=9.x__) من WebdriverIO. إذا كنت لا تزال تستخدم إصدارًا أقدم، يرجى زيارة [مواقع التوثيق القديمة](/versions)!
:::

<LiteYouTubeEmbed
    id="rA4IFNyW54c"
    title="Getting Started with WebdriverIO"
/>

:::tip القناة الرسمية على YouTube 🎥

يمكنك العثور على المزيد من مقاطع الفيديو حول WebdriverIO على [القناة الرسمية على YouTube](https://youtube.com/@webdriverio). تأكد من الاشتراك!

:::

## بدء إعداد WebdriverIO

لإضافة إعداد كامل لـ WebdriverIO إلى مشروع موجود أو جديد باستخدام [مجموعة أدوات بدء WebdriverIO](https://www.npmjs.com/package/create-wdio)، قم بتشغيل:

إذا كنت في الدليل الجذر لمشروع موجود، قم بتشغيل:

<Tabs
  defaultValue="npm"
  values={[
    {label: 'NPM', value: 'npm'},
    {label: 'Yarn', value: 'yarn'},
    {label: 'pnpm', value: 'pnpm'},
    {label: 'bun', value: 'bun'},
  ]
}>
<TabItem value="npm">

```sh
npm init wdio@latest .
```

أو إذا كنت ترغب في إنشاء مشروع جديد:

```sh
npm init wdio@latest ./path/to/new/project
```

</TabItem>
<TabItem value="yarn">

```sh
yarn create wdio .
```

أو إذا كنت ترغب في إنشاء مشروع جديد:

```sh
yarn create wdio ./path/to/new/project
```

</TabItem>
<TabItem value="pnpm">

```sh
pnpm create wdio@latest .
```

أو إذا كنت ترغب في إنشاء مشروع جديد:

```sh
pnpm create wdio@latest ./path/to/new/project
```

</TabItem>
<TabItem value="bun">

```sh
bun create wdio@latest .
```

أو إذا كنت ترغب في إنشاء مشروع جديد:

```sh
bun create wdio@latest ./path/to/new/project
```

</TabItem>
</Tabs>

هذا الأمر الواحد يقوم بتنزيل أداة سطر أوامر WebdriverIO ويشغل معالج التكوين الذي يساعدك على تكوين مجموعة الاختبار الخاصة بك.

<CreateProjectAnimation />

سيطرح المعالج مجموعة من الأسئلة التي ترشدك خلال الإعداد. يمكنك تمرير المعلمة `--yes` لاختيار إعداد افتراضي والذي سيستخدم Mocha مع Chrome باستخدام نمط [Page Object](https://martinfowler.com/bliki/PageObject.html).

<Tabs
  defaultValue="npm"
  values={[
    {label: 'NPM', value: 'npm'},
    {label: 'Yarn', value: 'yarn'},
    {label: 'pnpm', value: 'pnpm'},
    {label: 'bun', value: 'bun'},
  ]
}>
<TabItem value="npm">

```sh
npm init wdio@latest . -- --yes
```

</TabItem>
<TabItem value="yarn">

```sh
yarn create wdio . --yes
```

</TabItem>
<TabItem value="pnpm">

```sh
pnpm create wdio@latest . --yes
```

</TabItem>
<TabItem value="bun">

```sh
bun create wdio@latest . --yes
```

</TabItem>
</Tabs>

## تثبيت CLI يدويًا

يمكنك أيضًا إضافة حزمة CLI إلى مشروعك يدويًا عبر:

```sh
npm i --save-dev @wdio/cli
npx wdio --version # prints e.g. `8.13.10`

# run configuration wizard
npx wdio config
```

## تشغيل الاختبار

يمكنك بدء مجموعة الاختبار الخاصة بك باستخدام الأمر `run` والإشارة إلى تكوين WebdriverIO الذي أنشأته للتو:

```sh
npx wdio run ./wdio.conf.js
```

إذا كنت ترغب في تشغيل ملفات اختبار محددة، يمكنك إضافة معلمة `--spec`:

```sh
npx wdio run ./wdio.conf.js --spec example.e2e.js
```

أو تحديد مجموعات في ملف التكوين الخاص بك وتشغيل ملفات الاختبار المحددة فقط في مجموعة:

```sh
npx wdio run ./wdio.conf.js --suite exampleSuiteName
```

## التشغيل في سكريبت

إذا كنت ترغب في استخدام WebdriverIO كمحرك أتمتة في [الوضع المستقل](/docs/setuptypes#standalone-mode) ضمن سكريبت Node.JS، يمكنك أيضًا تثبيت WebdriverIO مباشرة واستخدامه كحزمة، على سبيل المثال لإنشاء لقطة شاشة لموقع ويب:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/fc362f2f8dd823d294b9bb5f92bd5991339d4591/getting-started/run-in-script.js#L2-L19
```

__ملاحظة:__ جميع أوامر WebdriverIO غير متزامنة وتحتاج إلى معالجة بشكل صحيح باستخدام [`async/await`](https://javascript.info/async-await).

## تسجيل الاختبارات

يوفر WebdriverIO أدوات لمساعدتك على البدء من خلال تسجيل إجراءات الاختبار الخاصة بك على الشاشة وإنشاء نصوص اختبار WebdriverIO تلقائيًا. راجع [تسجيل الاختبارات باستخدام مسجل Chrome DevTools](/docs/record) للحصول على مزيد من المعلومات.

## متطلبات النظام

ستحتاج إلى تثبيت [Node.js](http://nodejs.org).

- قم بتثبيت الإصدار v18.20.0 على الأقل أو أعلى حيث أن هذا هو أقدم إصدار LTS نشط
- فقط الإصدارات التي هي أو ستصبح إصدار LTS مدعومة رسميًا

إذا لم يكن Node مثبتًا حاليًا على نظامك، نقترح استخدام أداة مثل [NVM](https://github.com/creationix/nvm) أو [Volta](https://volta.sh/) للمساعدة في إدارة إصدارات Node.js النشطة المتعددة. NVM هو اختيار شائع، بينما Volta هو أيضًا بديل جيد.