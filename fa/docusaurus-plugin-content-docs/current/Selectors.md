---
id: selectors
title: انتخابگرها
---

پروتکل [WebDriver](https://w3c.github.io/webdriver/) چندین استراتژی انتخابگر برای پرس و جوی یک عنصر ارائه می‌دهد. WebdriverIO آنها را ساده‌سازی می‌کند تا انتخاب عناصر ساده بماند. لطفاً توجه داشته باشید که حتی با وجود اینکه دستور برای پرس و جوی عناصر `$` و `$$` نامیده می‌شود، اما هیچ ارتباطی با jQuery یا [موتور انتخابگر Sizzle](https://github.com/jquery/sizzle) ندارند.

در حالی که انتخابگرهای مختلفی وجود دارند، فقط تعداد کمی از آنها روشی مقاوم برای یافتن عنصر مناسب ارائه می‌دهند. برای مثال، با توجه به دکمه زیر:

```html
<button
  id="main"
  class="btn btn-large"
  name="submission"
  role="button"
  data-testid="submit"
>
  Submit
</button>
```

ما انتخابگرهای زیر را توصیه __می‌کنیم__ و __نمی‌کنیم__:

| انتخابگر | توصیه شده | یادداشت‌ها |
| -------- | ----------- | ----- |
| `$('button')` | 🚨 هرگز | بدترین - خیلی عمومی، بدون زمینه. |
| `$('.btn.btn-large')` | 🚨 هرگز | بد. وابسته به استایل. بسیار در معرض تغییر. |
| `$('#main')` | ⚠️ به ندرت | بهتر. اما هنوز وابسته به استایل یا شنونده‌های رویداد JS است. |
| `$(() => document.queryElement('button'))` | ⚠️ به ندرت | پرس و جوی موثر، پیچیده برای نوشتن. |
| `$('button[name="submission"]')` | ⚠️ به ندرت | وابسته به ویژگی `name` که معنایی HTML دارد. |
| `$('button[data-testid="submit"]')` | ✅ خوب | نیاز به ویژگی اضافی دارد، به a11y متصل نیست. |
| `$('aria/Submit')` یا `$('button=Submit')` | ✅ همیشه | بهترین. شبیه به روشی است که کاربر با صفحه تعامل می‌کند. توصیه می‌شود از فایل‌های ترجمه فرانت‌اند خود استفاده کنید تا زمانی که ترجمه‌ها به‌روزرسانی می‌شوند آزمایش‌های شما هرگز شکست نخورند |

## انتخابگر پرس و جوی CSS

اگر به گونه‌ای دیگر مشخص نشده باشد، WebdriverIO عناصر را با استفاده از الگوی [انتخابگر CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors) پرس و جو می‌کند، به عنوان مثال:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L7-L8
```

## متن لینک

برای دریافت یک عنصر لنگر با متن خاص در آن، متن را با علامت مساوی (`=`) شروع کنید.

برای مثال:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L3
```

شما می‌توانید این عنصر را با فراخوانی:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L16-L18
```

## متن لینک جزئی

برای پیدا کردن عنصر لنگری که متن قابل مشاهده آن به طور جزئی با مقدار جستجوی شما مطابقت دارد،
آن را با استفاده از `*=` در ابتدای رشته پرس و جو (مثلاً `*=driver`) جستجو کنید.

شما می‌توانید عنصر از مثال بالا را با فراخوانی:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L24-L26
```

__نکته:__ شما نمی‌توانید چندین استراتژی انتخابگر را در یک انتخابگر ترکیب کنید. برای رسیدن به همان هدف، از چندین پرس و جوی عنصر زنجیره‌ای استفاده کنید، به عنوان مثال:

```js
const elem = await $('header h1*=Welcome') // کار نمی‌کند!!!
// به جای آن استفاده کنید
const elem = await $('header').$('*=driver')
```

## عنصر با متن خاص

همان تکنیک را می‌توان برای عناصر نیز اعمال کرد. علاوه بر این، انجام مطابقت بدون حساسیت به بزرگی و کوچکی حروف با استفاده از `.=` یا `.*=` در پرس و جو نیز امکان‌پذیر است.

برای مثال، یک پرس و جو برای یک عنوان سطح 1 با متن "Welcome to my Page":

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L2
```

شما می‌توانید این عنصر را با فراخوانی:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/13eddfac6f18a2a4812cc09ed7aa5e468f392060/selectors/example.js#L35C1-L38
```

یا با استفاده از متن جزئی پرس و جو:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/13eddfac6f18a2a4812cc09ed7aa5e468f392060/selectors/example.js#L44C9-L47
```

همین کار برای نام‌های `id` و `class` نیز کار می‌کند:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L4
```

شما می‌توانید این عنصر را با فراخوانی:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/13eddfac6f18a2a4812cc09ed7aa5e468f392060/selectors/example.js#L49-L67
```

__نکته:__ شما نمی‌توانید چندین استراتژی انتخابگر را در یک انتخابگر ترکیب کنید. برای رسیدن به همان هدف، از چندین پرس و جوی عنصر زنجیره‌ای استفاده کنید، به عنوان مثال:

```js
const elem = await $('header h1*=Welcome') // کار نمی‌کند!!!
// به جای آن استفاده کنید
const elem = await $('header').$('h1*=Welcome')
```

## نام تگ

برای پرس و جوی یک عنصر با نام تگ خاص، از `<tag>` یا `<tag />` استفاده کنید.

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L5
```

شما می‌توانید این عنصر را با فراخوانی:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L61-L62
```

## ویژگی نام

برای پرس و جوی عناصر با ویژگی نام خاص، می‌توانید از یک انتخابگر معمولی CSS3 استفاده کنید یا استراتژی نام ارائه شده از [JSONWireProtocol](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol) با ارسال چیزی مانند [name="some-name"] به عنوان پارامتر انتخابگر:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L6
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L68-L69
```

__نکته:__ این استراتژی انتخابگر منسوخ شده است و فقط در مرورگرهای قدیمی که توسط پروتکل JSONWireProtocol اجرا می‌شوند یا با استفاده از Appium کار می‌کند.

## xPath

همچنین امکان پرس و جوی عناصر از طریق [xPath](https://developer.mozilla.org/en-US/docs/Web/XPath) خاص وجود دارد.

یک انتخابگر xPath قالبی مانند `//body/div[6]/div[1]/span[1]` دارد.

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/xpath.html
```

شما می‌توانید پاراگراف دوم را با فراخوانی:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L75-L76
```

می‌توانید از xPath برای پیمایش بالا و پایین درخت DOM نیز استفاده کنید:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L78-L79
```

## انتخابگر نام دسترسی‌پذیری

عناصر را با نام قابل دسترس آنها پرس و جو کنید. نام قابل دسترس چیزی است که توسط یک صفحه‌خوان هنگامی که آن عنصر فوکوس دریافت می‌کند اعلام می‌شود. مقدار نام قابل دسترس می‌تواند هم محتوای بصری و هم متن جایگزین پنهان باشد.

:::info

شما می‌توانید درباره این انتخابگر در [پست وبلاگ انتشار](/blog/2022/09/05/accessibility-selector) ما بیشتر بخوانید

:::

### بازیابی با `aria-label`

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L1
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L86-L87
```

### بازیابی با `aria-labelledby`

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L2-L3
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L93-L94
```

### بازیابی با محتوا

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L4
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L100-L101
```

### بازیابی با عنوان

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L5
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L107-L108
```

### بازیابی با ویژگی `alt`

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L6
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L114-L115
```

## ARIA - ویژگی نقش

برای پرس و جوی عناصر بر اساس [نقش‌های ARIA](https://www.w3.org/TR/html-aria/#docconformance)، می‌توانید نقش عنصر را مستقیماً مانند `[role=button]` به عنوان پارامتر انتخابگر مشخص کنید:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L13
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L131-L132
```

## ویژگی ID

استراتژی مکان‌یاب "id" در پروتکل WebDriver پشتیبانی نمی‌شود، باید از استراتژی‌های انتخابگر CSS یا xPath برای یافتن عناصر با استفاده از ID استفاده کنید.

با این حال برخی از درایورها (مانند [راننده موتور Appium You.i](https://github.com/YOU-i-Labs/appium-youiengine-driver#selector-strategies)) ممکن است هنوز از این انتخابگر [پشتیبانی](https://github.com/YOU-i-Labs/appium-youiengine-driver#selector-strategies) کنند.

نحوهای فعلی پشتیبانی شده برای ID عبارتند از:

```js
//مکان‌یاب css
const button = await $('#someid')
//مکان‌یاب xpath
const button = await $('//*[@id="someid"]')
//استراتژی id
// نکته: فقط در Appium یا چارچوب‌های مشابه که از استراتژی مکان‌یاب "ID" پشتیبانی می‌کنند، کار می‌کند
const button = await $('id=resource-id/iosname')
```

## تابع JS

همچنین می‌توانید از توابع JavaScript برای بازیابی عناصر با استفاده از APIهای بومی وب استفاده کنید. البته، شما فقط می‌توانید این کار را در یک زمینه وب انجام دهید (به عنوان مثال، `browser` یا زمینه وب در موبایل).

با توجه به ساختار HTML زیر:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/js.html
```

شما می‌توانید عنصر خواهر `#elem` را به صورت زیر پرس و جو کنید:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L139-L143
```

## انتخابگرهای عمیق

:::warning

از `v9` WebdriverIO، نیازی به این انتخابگر خاص نیست زیرا WebdriverIO به طور خودکار از Shadow DOM عبور می‌کند. توصیه می‌شود با حذف `>>>` از جلوی آن، از این انتخابگر خارج شوید.

:::

بسیاری از برنامه‌های کاربردی فرانت‌اند به شدت به عناصری با [shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM) متکی هستند. از نظر فنی پرس و جوی عناصر در shadow DOM بدون راه‌حل‌های جایگزین غیرممکن است. [`shadow$`](https://webdriver.io/docs/api/element/shadow$) و [`shadow$$`](https://webdriver.io/docs/api/element/shadow$$) چنین راه‌حل‌هایی بوده‌اند که [محدودیت‌هایی](https://github.com/Georgegriff/query-selector-shadow-dom#how-is-this-different-to-shadow) داشتند. با انتخابگر عمیق، اکنون می‌توانید تمام عناصر درون هر shadow DOM را با استفاده از دستور پرس و جوی معمولی پرس و جو کنید.

با فرض اینکه برنامه‌ای با ساختار زیر داریم:

![مثال کروم](https://github.com/Georgegriff/query-selector-shadow-dom/raw/main/Chrome-example.png "مثال کروم")

با این انتخابگر می‌توانید عنصر `<button />` که در shadow DOM دیگری تو در تو است را پرس و جو کنید، به عنوان مثال:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L147-L149
```

## انتخابگرهای موبایل

برای آزمایش موبایل هیبریدی، مهم است که سرور اتوماسیون در *زمینه* صحیح قبل از اجرای دستورات باشد. برای اتوماسیون حرکات، درایور در حالت ایده‌آل باید روی زمینه بومی تنظیم شود. اما برای انتخاب عناصر از DOM، درایور باید روی زمینه webview پلتفرم تنظیم شود. فقط *سپس* می‌توان از روش‌های ذکر شده در بالا استفاده کرد.

برای آزمایش موبایل بومی، تغییر بین زمینه‌ها وجود ندارد، زیرا شما باید از استراتژی‌های موبایل استفاده کنید و مستقیماً از فناوری اتوماسیون دستگاه زیربنایی استفاده کنید. این به ویژه زمانی مفید است که یک آزمایش نیاز به کنترل دقیق برای یافتن عناصر دارد.

### Android UiAutomator

چارچوب UI Automator اندروید چندین روش برای یافتن عناصر ارائه می‌دهد. می‌توانید از [API UI Automator](https://developer.android.com/tools/testing-support-library/index.html#uia-apis)، به ویژه [کلاس UiSelector](https://developer.android.com/reference/androidx/test/uiautomator/UiSelector) برای پیدا کردن عناصر استفاده کنید. در Appium شما کد جاوا را به عنوان یک رشته به سرور ارسال می‌کنید، که آن را در محیط برنامه اجرا می‌کند و عنصر یا عناصر را برمی‌گرداند.

```js
const selector = 'new UiSelector().text("Cancel").className("android.widget.Button")'
const button = await $(`android=${selector}`)
await button.click()
```

### Android DataMatcher و ViewMatcher (فقط Espresso)

استراتژی DataMatcher اندروید روشی برای یافتن عناصر توسط [Data Matcher](https://developer.android.com/reference/android/support/test/espresso/DataInteraction) ارائه می‌دهد

```js
const menuItem = await $({
  "name": "hasEntry",
  "args": ["title", "ViewTitle"]
})
await menuItem.click()
```

و به طور مشابه [View Matcher](https://developer.android.com/reference/android/support/test/espresso/ViewInteraction)

```js
const menuItem = await $({
  "name": "hasEntry",
  "args": ["title", "ViewTitle"],
  "class": "androidx.test.espresso.matcher.ViewMatchers"
})
await menuItem.click()
```

### Android View Tag (فقط Espresso)

استراتژی view tag روشی مناسب برای یافتن عناصر با [tag](https://developer.android.com/reference/android/support/test/espresso/matcher/ViewMatchers.html#withTagValue%28org.hamcrest.Matcher%3Cjava.lang.Object%3E%29) آنها ارائه می‌دهد.

```js
const elem = await $('-android viewtag:tag_identifier')
await elem.click()
```

### iOS UIAutomation

هنگام اتوماسیون یک برنامه iOS، می‌توان از [چارچوب UI Automation](https://developer.apple.com/library/prerelease/tvos/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/UIAutomation.html) اپل برای یافتن عناصر استفاده کرد.

این [API](https://developer.apple.com/library/ios/documentation/DeveloperTools/Reference/UIAutomationRef/index.html#//apple_ref/doc/uid/TP40009771) جاوا اسکریپت روش‌هایی برای دسترسی به نما و همه چیز روی آن دارد.

```js
const selector = 'UIATarget.localTarget().frontMostApp().mainWindow().buttons()[0]'
const button = await $(`ios=${selector}`)
await button.click()
```

همچنین می‌توانید از جستجوی گزاره‌ای در iOS UI Automation در Appium برای پالایش بیشتر انتخاب عناصر استفاده کنید. برای جزئیات [اینجا](https://github.com/appium/appium/blob/master/docs/en/writing-running-appium/ios/ios-predicate.md) را ببینید.

### رشته‌های گزاره iOS XCUITest و زنجیره‌های کلاس

با iOS 10 و بالاتر (با استفاده از درایور `XCUITest`)، می‌توانید از [رشته‌های گزاره](https://github.com/facebook/WebDriverAgent/wiki/Predicate-Queries-Construction-Rules) استفاده کنید:

```js
const selector = `type == 'XCUIElementTypeSwitch' && name CONTAINS 'Allow'`
const switch = await $(`-ios predicate string:${selector}`)
await switch.click()
```

و [زنجیره‌های کلاس](https://github.com/facebook/WebDriverAgent/wiki/Class-Chain-Queries-Construction-Rules):

```js
const selector = '**/XCUIElementTypeCell[`name BEGINSWITH "D"`]/**/XCUIElementTypeButton'
const button = await $(`-ios class chain:${selector}`)
await button.click()
```

### شناسه دسترسی‌پذیری

استراتژی مکان‌یاب `accessibility id` برای خواندن یک شناسه منحصر به فرد برای یک عنصر UI طراحی شده است. این مزیت را دارد که در طول محلی‌سازی یا هر فرآیند دیگری که ممکن است متن را تغییر دهد، تغییر نمی‌کند. علاوه بر این، می‌تواند در ایجاد آزمایش‌های چند پلتفرمی کمک کند، اگر عناصری که از نظر عملکردی یکسان هستند، همان شناسه دسترسی‌پذیری را داشته باشند.

- برای iOS این `شناسه دسترسی‌پذیری` است که توسط Apple [اینجا](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAccessibilityIdentification_Protocol/index.html) مشخص شده است.
- برای Android، `شناسه دسترسی‌پذیری` به `توضیح-محتوا` برای عنصر نگاشت می‌شود، همانطور که [اینجا](https://developer.android.com/training/accessibility/accessible-app.html) توضیح داده شده است.

برای هر دو پلتفرم، دریافت یک عنصر (یا چندین عنصر) با `شناسه دسترسی‌پذیری` آنها معمولاً بهترین روش است. همچنین روش ترجیحی نسبت به استراتژی `name` منسوخ شده است.

```js
const elem = await $('~my_accessibility_identifier')
await elem.click()
```

### نام کلاس

استراتژی `نام کلاس` یک `رشته` است که یک عنصر UI را در نمای فعلی نشان می‌دهد.

- برای iOS، نام کامل یک [کلاس UIAutomation](https://developer.apple.com/library/prerelease/tvos/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/UIAutomation.html) است و با `UIA-` شروع می‌شود، مانند `UIATextField` برای یک فیلد متن. مرجع کامل را می‌توان [اینجا](https://developer.apple.com/library/ios/navigation/#section=Frameworks&topic=UIAutomation) یافت.
- برای Android، نام کاملاً مشخص شده یک [کلاس](https://developer.android.com/reference/android/widget/package-summary.html) [UI Automator](https://developer.android.com/tools/testing-support-library/index.html#UIAutomator) است، مانند `android.widget.EditText` برای یک فیلد متن. مرجع کامل را می‌توان [اینجا](https://developer.android.com/reference/android/widget/package-summary.html) یافت.
- برای Youi.tv، نام کامل یک کلاس Youi.tv است و با `CYI-` شروع می‌شود، مانند `CYIPushButtonView` برای یک عنصر دکمه فشاری. مرجع کامل را می‌توان در [صفحه GitHub درایور موتور You.i](https://github.com/YOU-i-Labs/appium-youiengine-driver) یافت

```js
// مثال iOS
await $('UIATextField').click()
// مثال Android
await $('android.widget.DatePicker').click()
// مثال Youi.tv
await $('CYIPushButtonView').click()
```

## انتخابگرهای زنجیره‌ای

اگر می‌خواهید در پرس و جوی خود دقیق‌تر باشید، می‌توانید انتخابگرها را زنجیر کنید تا عنصر صحیح را پیدا کنید. اگر قبل از دستور واقعی خود، `element` را فراخوانی کنید، WebdriverIO پرس و جو را از آن عنصر شروع می‌کند.

برای مثال، اگر ساختار DOM مانند زیر دارید:

```html
<div class="row">
  <div class="entry">
    <label>Product A</label>
    <button>Add to cart</button>
    <button>More Information</button>
  </div>
  <div class="entry">
    <label>Product B</label>
    <button>Add to cart</button>
    <button>More Information</button>
  </div>
  <div class="entry">
    <label>Product C</label>
    <button>Add to cart</button>
    <button>More Information</button>
  </div>
</div>
```

و می‌خواهید محصول B را به سبد خرید اضافه کنید، انجام این کار فقط با استفاده از انتخابگر CSS دشوار خواهد بود.

با زنجیره کردن انتخابگر، بسیار آسان‌تر است. به سادگی عنصر مورد نظر را قدم به قدم محدود کنید:

```js
await $('.row .entry:nth-child(2)').$('button*=Add').click()
```

### انتخابگر تصویر Appium

با استفاده از استراتژی مکان‌یاب `-image`، امکان ارسال یک فایل تصویری به Appium وجود دارد که نشان دهنده عنصری است که می‌خواهید به آن دسترسی داشته باشید.

فرمت‌های فایل پشتیبانی شده `jpg,png,gif,bmp,svg`

مرجع کامل را می‌توان [اینجا](https://github.com/appium/appium/blob/master/packages/images-plugin/docs/find-by-image.md) یافت

```js
const elem = await $('./file/path/of/image/test.jpg')
await elem.click()
```

**نکته**: نحوه کار Appium با این انتخابگر به این صورت است که داخلاً یک (برنامه)اسکرین‌شات می‌گیرد و از انتخابگر تصویر ارائه شده استفاده می‌کند
تا تأیید کند آیا عنصر در آن (برنامه)اسکرین‌شات یافت می‌شود یا خیر.

توجه داشته باشید که Appium ممکن است اندازه (برنامه)اسکرین‌شات گرفته شده را تغییر دهد تا با اندازه CSS صفحه (برنامه) شما مطابقت داشته باشد (این اتفاق
در iPhone‌ها و همچنین در دستگاه‌های Mac با نمایشگر Retina رخ می‌دهد زیرا DPR بزرگتر از 1 است). این باعث عدم یافتن تطابق می‌شود زیرا
انتخابگر تصویر ارائه شده ممکن است از اسکرین‌شات اصلی گرفته شده باشد.
می‌توانید این مشکل را با به‌روزرسانی تنظیمات سرور Appium برطرف کنید، به [اسناد Appium](https://github.com/appium/appium/blob/master/packages/images-plugin/docs/find-by-image.md#related-settings)
برای تنظیمات و [این نظر](https://github.com/webdriverio/webdriverio/issues/6097#issuecomment-726675579) برای توضیحات دقیق مراجعه کنید.

## انتخابگرهای React

WebdriverIO روشی برای انتخاب اجزای React بر اساس نام اجزا ارائه می‌دهد. برای انجام این کار، شما می‌توانید بین دو دستور انتخاب کنید: `react$` و `react$$`.

این دستورات به شما امکان می‌دهند اجزا را از [React VirtualDOM](https://reactjs.org/docs/faq-internals.html) انتخاب کنید و یا یک عنصر WebdriverIO واحد یا آرایه‌ای از عناصر را برگردانید (بسته به اینکه از کدام تابع استفاده می‌شود).

**نکته**: دستورات `react$` و `react$$` از نظر عملکرد مشابه هستند، با این تفاوت که `react$$` *تمام* موارد مطابق را به عنوان آرایه‌ای از عناصر WebdriverIO برمی‌گرداند و `react$` اولین نمونه یافت شده را برمی‌گرداند.

#### مثال پایه

```jsx
// index.jsx
import React from 'react'
import ReactDOM from 'react-dom'

function MyComponent() {
    return (
        <div>
            MyComponent
        </div>
    )
}

function App() {
    return (<MyComponent />)
}

ReactDOM.render(<App />, document.querySelector('#root'))
```

در کد بالا یک نمونه ساده از `MyComponent` درون برنامه وجود دارد، که React آن را در یک عنصر HTML با `id="root"` رندر می‌کند.

با دستور `browser.react$`، می‌توانید یک نمونه از `MyComponent` را انتخاب کنید:

```js
const myCmp = await browser.react$('MyComponent')
```

حالا که عنصر WebdriverIO را در متغیر `myCmp` ذخیره کرده‌اید، می‌توانید دستورات عنصر را روی آن اجرا کنید.

#### فیلتر کردن اجزا

کتابخانه‌ای که WebdriverIO داخلاً استفاده می‌کند، امکان فیلتر انتخاب شما را بر اساس props و/یا state اجزا فراهم می‌کند. برای انجام این کار، باید یک آرگومان دوم برای props و/یا یک آرگومان سوم برای state به دستور مرورگر ارسال کنید.

```jsx
// index.jsx
import React from 'react'
import ReactDOM from 'react-dom'

function MyComponent(props) {
    return (
        <div>
            Hello { props.name || 'World' }!
        </div>
    )
}

function App() {
    return (
        <div>
            <MyComponent name="WebdriverIO" />
            <MyComponent />
        </div>
    )
}

ReactDOM.render(<App />, document.querySelector('#root'))
```

اگر می‌خواهید نمونه‌ای از `MyComponent` را که دارای prop `name` با مقدار `WebdriverIO` است انتخاب کنید، می‌توانید دستور را به این صورت اجرا کنید:

```js
const myCmp = await browser.react$('MyComponent', {
    props: { name: 'WebdriverIO' }
})
```

اگر می‌خواهید انتخاب خود را بر اساس state فیلتر کنید، دستور `browser` به شکل زیر خواهد بود:

```js
const myCmp = await browser.react$('MyComponent', {
    state: { myState: 'some value' }
})
```

#### کار با `React.Fragment`

هنگام استفاده از دستور `react$` برای انتخاب [قطعات](https://reactjs.org/docs/fragments.html) React، WebdriverIO اولین فرزند آن اجزا را به عنوان گره اجزا برمی‌گرداند. اگر از `react$$` استفاده کنید، آرایه‌ای شامل تمام گره‌های HTML داخل قطعاتی که با انتخابگر مطابقت دارند دریافت خواهید کرد.

```jsx
// index.jsx
import React from 'react'
import ReactDOM from 'react-dom'

function MyComponent() {
    return (
        <React.Fragment>
            <div>
                MyComponent
            </div>
            <div>
                MyComponent
            </div>
        </React.Fragment>
    )
}

function App() {
    return (<MyComponent />)
}

ReactDOM.render(<App />, document.querySelector('#root'))
```

با توجه به مثال بالا، این نحوه کار دستورات است:

```js
await browser.react$('MyComponent') // عنصر WebdriverIO را برای اولین <div /> برمی‌گرداند
await browser.react$$('MyComponent') // عناصر WebdriverIO را برای آرایه [<div />, <div />] برمی‌گرداند
```

**نکته:** اگر چندین نمونه از `MyComponent` دارید و از `react$$` برای انتخاب این اجزای قطعه استفاده می‌کنید، یک آرایه یک بعدی از تمام گره‌ها به شما برگردانده می‌شود. به عبارت دیگر، اگر 3 نمونه `<MyComponent />` داشته باشید، آرایه‌ای با شش عنصر WebdriverIO به شما برگردانده می‌شود.

## استراتژی‌های انتخابگر سفارشی


اگر برنامه شما به روش خاصی برای بازیابی عناصر نیاز دارد، می‌توانید خودتان یک استراتژی انتخابگر سفارشی تعریف کنید که می‌توانید از آن با `custom$` و `custom$$` استفاده کنید. برای این کار استراتژی خود را یک بار در ابتدای آزمایش ثبت کنید، به عنوان مثال در یک هوک `before`:

```js reference
https://github.com/webdriverio/example-recipes/blob/f5730428ec3605e856e90bf58be17c9c9da891de/queryElements/customStrategy.js#L2-L11
```

با توجه به قطعه HTML زیر:

```html reference
https://github.com/webdriverio/example-recipes/blob/f5730428ec3605e856e90bf58be17c9c9da891de/queryElements/example.html#L8-L12
```

سپس با فراخوانی:

```js reference
https://github.com/webdriverio/example-recipes/blob/f5730428ec3605e856e90bf58be17c9c9da891de/queryElements/customStrategy.js#L16-L19
```

**نکته:** این فقط در محیطی وب کار می‌کند که دستور [`execute`](/docs/api/browser/execute) قابل اجرا باشد.