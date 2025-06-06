---
id: emulation
title: المحاكاة
---

مع WebdriverIO يمكنك محاكاة واجهات برمجة التطبيقات (APIs) للويب باستخدام أمر [`emulate`](/docs/api/browser/emulate) لإرجاع قيم مخصصة تساعدك في محاكاة سلوكيات معينة للمتصفح. لاحظ أن هذا يتطلب أن يستخدم تطبيقك هذه الواجهات البرمجية بشكل صريح.

<LiteYouTubeEmbed
    id="2bQXzIB_97M"
    title="WebdriverIO Tutorials: The Emulate Command - Emulate Web APIs at Runtime with WebdriverIO"
/>

:::info

تتطلب هذه الميزة دعم WebDriver Bidi للمتصفح. بينما تحتوي الإصدارات الحديثة من Chrome وEdge وFirefox على مثل هذا الدعم، فإن Safari __لا يدعم__ هذه الميزة. للحصول على التحديثات تابع [wpt.fyi](https://wpt.fyi/results/webdriver/tests/bidi/script/add_preload_script/add_preload_script.py?label=experimental&label=master&aligned). علاوة على ذلك، إذا كنت تستخدم مزودًا سحابيًا لتشغيل المتصفحات، فتأكد من أن المزود الخاص بك يدعم أيضًا WebDriver Bidi.

لتمكين WebDriver Bidi لاختبارك، تأكد من ضبط `webSocketUrl: true` في إمكانياتك.

:::

## الموقع الجغرافي

تغيير الموقع الجغرافي للمتصفح إلى منطقة محددة، على سبيل المثال:

```ts
await browser.emulate('geolocation', {
    latitude: 52.52,
    longitude: 13.39,
    accuracy: 100
})
await browser.url('https://www.google.com/maps')
await browser.$('aria/Show Your Location').click()
await browser.pause(5000)
console.log(await browser.getUrl()) // outputs: "https://www.google.com/maps/@52.52,13.39,16z?entry=ttu"
```

هذا سيقوم بتعديل كيفية عمل [`navigator.geolocation.getCurrentPosition`](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation/getCurrentPosition) ويعيد الموقع الذي قمت بتوفيره.

## نظام الألوان

تغيير إعداد نظام الألوان الافتراضي للمتصفح عبر:

```ts
await browser.emulate('colorScheme', 'light')
await browser.url('https://webdriver.io')
const backgroundColor = await browser.$('nav').getCSSProperty('background-color')
console.log(backgroundColor.parsed.hex) // outputs: "#efefef"

await browser.emulate('colorScheme', 'dark')
await browser.url('https://webdriver.io')
const backgroundColor = await browser.$('nav').getCSSProperty('background-color')
console.log(backgroundColor.parsed.hex) // outputs: "#000000"
```

هذا سيقوم بتعديل كيفية عمل [`window.matchMedia`](https://developer.mozilla.org/en-US/docs/Web/API/Window/matchMedia) عندما تستعلم عن نظام الألوان عبر `(prefers-color-scheme: dark)`.

## وكيل المستخدم

تغيير وكيل المستخدم للمتصفح إلى سلسلة مختلفة عبر:

```ts
await browser.emulate('userAgent', 'Chrome/1.2.3.4 Safari/537.36')
```

هذا سيغير قيمة [`navigator.userAgent`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/userAgent). لاحظ أن مطوري المتصفحات يقومون تدريجياً بإلغاء وكيل المستخدم.

## خاصية onLine

تغيير حالة الاتصال بالإنترنت للمتصفح عبر:

```ts
await browser.emulate('onLine', false)
```

هذا __لن__ يوقف حركة الشبكة بين المتصفح والإنترنت ويغير فقط قيمة الإرجاع من [`navigator.onLine`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/onLine). إذا كنت مهتمًا بتعديل قدرات الشبكة للمتصفح، فانظر إلى أمر [`throttleNetwork`](/docs/api/browser/throttleNetwork).

## الساعة

يمكنك تعديل ساعة نظام المتصفح باستخدام أمر [`emulate`](/docs/emulation). يقوم بتجاوز الدوال العالمية الأصلية المتعلقة بالوقت مما يسمح بالتحكم فيها بشكل متزامن عبر `clock.tick()` أو كائن الساعة الناتج. هذا يشمل التحكم في:

- `setTimeout`
- `clearTimeout`
- `setInterval`
- `clearInterval`
- `Date Objects`

تبدأ الساعة في بداية عصر يونكس (طابع زمني 0). هذا يعني أنه عندما تقوم بإنشاء كائن Date جديد في تطبيقك، سيكون له وقت 1 يناير 1970 إذا لم تمرر أي خيارات أخرى إلى أمر `emulate`.

##### مثال

عند استدعاء `browser.emulate('clock', { ... })` سيقوم مباشرة بإعادة كتابة الدوال العالمية للصفحة الحالية وكذلك لجميع الصفحات التالية، على سبيل المثال:

```ts
const clock = await browser.emulate('clock', { now: new Date(1989, 7, 4) })

console.log(await browser.execute(() => (new Date()).toString()))
// returns "Fri Aug 04 1989 00:00:00 GMT-0700 (Pacific Daylight Time)"

await browser.url('https://webdriverio')
console.log(await browser.execute(() => (new Date()).toString()))
// returns "Fri Aug 04 1989 00:00:00 GMT-0700 (Pacific Daylight Time)"

await clock.restore()

console.log(await browser.execute(() => (new Date()).toString()))
// returns "Thu Aug 01 2024 17:59:59 GMT-0700 (Pacific Daylight Time)"

await browser.url('https://guinea-pig.webdriver.io/pointer.html')
console.log(await browser.execute(() => (new Date()).toString()))
// returns "Thu Aug 01 2024 17:59:59 GMT-0700 (Pacific Daylight Time)"
```

يمكنك تعديل وقت النظام عن طريق استدعاء [`setSystemTime`](/docs/api/clock/setSystemTime) أو [`tick`](/docs/api/clock/tick).

يمكن أن يحتوي كائن `FakeTimerInstallOpts` على الخصائص التالية:

```ts
interface FakeTimerInstallOpts {
    // Installs fake timers with the specified unix epoch
    // @default: 0
    now?: number | Date | undefined;

    // An array with names of global methods and APIs to fake. By default, WebdriverIO
    // does not replace `nextTick()` and `queueMicrotask()`. For instance,
    // `browser.emulate('clock', { toFake: ['setTimeout', 'nextTick'] })` will fake only
    // `setTimeout()` and `nextTick()`
    toFake?: FakeMethod[] | undefined;

    // The maximum number of timers that will be run when calling runAll() (default: 1000)
    loopLimit?: number | undefined;

    // Tells WebdriverIO to increment mocked time automatically based on the real system
    // time shift (e.g. the mocked time will be incremented by 20ms for every 20ms change
    // in the real system time)
    // @default false
    shouldAdvanceTime?: boolean | undefined;

    // Relevant only when using with shouldAdvanceTime: true. increment mocked time by
    // advanceTimeDelta ms every advanceTimeDelta ms change in the real system time
    // @default: 20
    advanceTimeDelta?: number | undefined;

    // Tells FakeTimers to clear 'native' (i.e. not fake) timers by delegating to their
    // respective handlers. These are not cleared by default, leading to potentially
    // unexpected behavior if timers existed prior to installing FakeTimers.
    // @default: false
    shouldClearNativeTimers?: boolean | undefined;
}
```

## الجهاز

أمر `emulate` يدعم أيضًا محاكاة جهاز محمول أو جهاز سطح مكتب معين من خلال تغيير منفذ العرض، وعامل مقياس الجهاز، ووكيل المستخدم. لا ينبغي، بأي حال من الأحوال، استخدام هذا للاختبار على الهاتف المحمول حيث تختلف محركات متصفح سطح المكتب عن المحمول. يجب استخدام هذا فقط إذا كان تطبيقك يقدم سلوكًا محددًا لأحجام منفذ العرض الأصغر.

على سبيل المثال، للتبديل إلى وكيل المستخدم ومنفذ العرض الخاص بجهاز iPhone 15، ما عليك سوى تشغيل:

```ts
const restore = await browser.emulate('device', 'iPhone 15')
// test your application ...

// reset to original viewport and user agent
await restore()
```

يحتفظ WebdriverIO بقائمة ثابتة من [جميع الأجهزة المحددة](https://github.com/webdriverio/webdriverio/blob/main/packages/webdriverio/src/deviceDescriptorsSource.ts).