---
id: selectors
title: தேர்வுக்கருவிகள்
---

The [WebDriver Protocol](https://w3c.github.io/webdriver/) provides several selector strategies to query an element. WebdriverIO simplifies them to keep selecting elements simple. Please note that even though the command to query elements is called `$` and `$$`, they have nothing to do with jQuery or the [Sizzle Selector Engine](https://github.com/jquery/sizzle).

While there are so many different selectors available, only a few of them provide a resilient way to find the right element. For example, given the following button:

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

நாங்கள் __பரிந்துரைக்கும்__ மற்றும் __பரிந்துரைக்காத__ தேர்வுக்கருவிகள் பின்வருமாறு:

| தேர்வுக்கருவி | பரிந்துரைக்கப்படுகிறதா? | குறிப்புகள் |
| -------- | ----------- | ----- |
| `$('button')` | 🚨 ஒருபோதும் | மிகவும் மோசமானது - மிகவும் பொதுவானது, சூழலில்லை. |
| `$('.btn.btn-large')` | 🚨 ஒருபோதும் | மோசமானது. பாணியுடன் இணைக்கப்பட்டுள்ளது. மாற்றங்களுக்கு உள்ளாகக்கூடியது. |
| `$('#main')` | ⚠️ சிறிதளவு | சிறப்பானது. ஆனால் இன்னும் பாணி அல்லது JS நிகழ்வு கேட்பவர்களுடன் இணைந்துள்ளது. |
| `$(() => document.queryElement('button'))` | ⚠️ சிறிதளவு | திறமையான வினவல், எழுத சிக்கலானது. |
| `$('button[name="submission"]')` | ⚠️ சிறிதளவு | HTML மாற்றங்களுடன் தொடர்புடைய `name` பண்புடன் இணைக்கப்பட்டுள்ளது. |
| `$('button[data-testid="submit"]')` | ✅ நல்லது | கூடுதல் பண்பு தேவைப்படுகிறது, a11y உடன் தொடர்பில்லை. |
| `$('aria/Submit')` அல்லது `$('button=Submit')` | ✅ எப்போதும் | சிறந்தது. பயனர் பக்கத்துடன் எவ்வாறு தொடர்புகொள்கிறார் என்பதைப் போன்றது. உங்கள் முன்முனையின் மொழிபெயர்ப்பு கோப்புகளைப் பயன்படுத்துவது பரிந்துரைக்கப்படுகிறது, இதனால் மொழிபெயர்ப்புகள் புதுப்பிக்கப்படும்போது உங்கள் சோதனைகள் ஒருபோதும் தோல்வியடையாது |

## CSS வினவல் தேர்வுக்கருவி

வேறு விதமாகக் குறிப்பிடப்படாவிட்டால், WebdriverIO [CSS தேர்வுக்கருவி](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors) முறையைப் பயன்படுத்தி உறுப்புகளை வினவும், எ.கா.:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L7-L8
```

## இணைப்பு உரை

குறிப்பிட்ட உரையுடன் கூடிய நங்கூர உறுப்பைப் பெற, சமக்குறி (`=`) குறியுடன் தொடங்கும் உரையை வினவுங்கள்.

எடுத்துக்காட்டாக:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L3
```

இந்த உறுப்பை பின்வருமாறு அழைத்து வினவலாம்:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L16-L18
```

## பகுதி இணைப்பு உரை

உங்கள் தேடல் மதிப்புடன் தெரியும் உரை ஓரளவு பொருந்தும் நங்கூர உறுப்பைக் கண்டறிய, வினவல் சரத்திற்கு முன்னால் `*=` ஐப் பயன்படுத்தி வினவுங்கள் (எ.கா. `*=driver`).

மேலே உள்ள எடுத்துக்காட்டில் உள்ள உறுப்பை இவ்வாறு அழைத்தும் வினவலாம்:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L24-L26
```

__குறிப்பு:__ ஒரு தேர்வுக்கருவியில் பல தேர்வுக்கருவி உத்திகளை கலக்க முடியாது. அதே இலக்கை அடைய பல சங்கிலி உறுப்பு வினவல்களைப் பயன்படுத்தவும், எ.கா.:

```js
const elem = await $('header h1*=Welcome') // வேலை செய்யாது!!!
// இதற்கு பதிலாக பயன்படுத்தவும்
const elem = await $('header').$('*=driver')
```

## குறிப்பிட்ட உரையுடன் கூடிய உறுப்பு

அதே நுட்பத்தை உறுப்புகளுக்கும் பயன்படுத்தலாம். கூடுதலாக, வினவலுக்குள் `.=` அல்லது `.*=` ஐப் பயன்படுத்தி பெரிய-சிறிய எழுத்துகளை பொருட்படுத்தாத பொருத்தத்தை செய்வதும் சாத்தியமாகும்.

எடுத்துக்காட்டாக, "Welcome to my Page" என்ற உரையுடன் கூடிய நிலை 1 தலைப்புக்கான வினவல் இங்கே உள்ளது:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L2
```

இந்த உறுப்பை பின்வருமாறு அழைத்து வினவலாம்:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/13eddfac6f18a2a4812cc09ed7aa5e468f392060/selectors/example.js#L35C1-L38
```

அல்லது பகுதி உரையை வினவும் பயன்படுத்தி:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/13eddfac6f18a2a4812cc09ed7aa5e468f392060/selectors/example.js#L44C9-L47
```

இதே `id` மற்றும் `class` பெயர்களுக்கும் செயல்படும்:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L4
```

இந்த உறுப்பை பின்வருமாறு அழைத்து வினவலாம்:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/13eddfac6f18a2a4812cc09ed7aa5e468f392060/selectors/example.js#L49-L67
```

__குறிப்பு:__ ஒரு தேர்வுக்கருவியில் பல தேர்வுக்கருவி உத்திகளை கலக்க முடியாது. அதே இலக்கை அடைய பல சங்கிலி உறுப்பு வினவல்களைப் பயன்படுத்தவும், எ.கா.:

```js
const elem = await $('header h1*=Welcome') // வேலை செய்யாது!!!
// இதற்கு பதிலாக பயன்படுத்தவும்
const elem = await $('header').$('h1*=Welcome')
```

## குறி பெயர்

குறிப்பிட்ட குறி பெயருடன் ஒரு உறுப்பை வினவ, `<tag>` அல்லது `<tag />` ஐப் பயன்படுத்தவும்.

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L5
```

இந்த உறுப்பை பின்வருமாறு அழைத்து வினவலாம்:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L61-L62
```

## பெயர் பண்பு

குறிப்பிட்ட பெயர் பண்புகொண்ட உறுப்புகளை வினவ, நீங்கள் ஒரு சாதாரண CSS3 தேர்வுக்கருவியைப் பயன்படுத்தலாம் அல்லது தேர்வுக்கருவி அளவுருவாக [name="some-name"] போன்றவற்றை [JSONWireProtocol](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol) இலிருந்து வழங்கப்பட்ட பெயர் உத்தியைப் பயன்படுத்தலாம்:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L6
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L68-L69
```

__குறிப்பு:__ இந்த தேர்வுக்கருவி உத்தி காலாவதியானது மற்றும் JSONWireProtocol நெறிமுறையால் இயக்கப்படும் பழைய உலாவிகளில் மட்டுமே அல்லது Appium ஐப் பயன்படுத்தி வேலை செய்யும்.

## xPath

குறிப்பிட்ட [xPath](https://developer.mozilla.org/en-US/docs/Web/XPath) மூலம் உறுப்புகளை வினவுவதும் சாத்தியமாகும்.

ஒரு xPath தேர்வுக்கருவி `//body/div[6]/div[1]/span[1]` போன்ற வடிவத்தைக் கொண்டுள்ளது.

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/xpath.html
```

இரண்டாவது பத்தியை பின்வருமாறு அழைத்து வினவலாம்:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L75-L76
```

DOM மரத்தை மேலும் கீழும் செல்லவும் xPath ஐப் பயன்படுத்தலாம்:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L78-L79
```

## அணுகல் பெயர் தேர்வுக்கருவி

அவற்றின் அணுகக்கூடிய பெயரால் உறுப்புகளை வினவுங்கள். அந்த உறுப்புக்கு கவனம் கிடைக்கும்போது திரை வாசிப்பாளரால் அறிவிக்கப்படுவதே அணுகக்கூடிய பெயராகும். அணுகக்கூடிய பெயரின் மதிப்பு காட்சி உள்ளடக்கம் அல்லது மறைக்கப்பட்ட உரை மாற்றுகள் இரண்டும் இருக்கலாம்.

:::info

இந்த தேர்வுக்கருவி பற்றி எங்கள் [வெளியீட்டு வலைப்பதிவில்](/blog/2022/09/05/accessibility-selector) மேலும் படிக்கலாம்

:::

### `aria-label` மூலம் பெறுதல்

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L1
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L86-L87
```

### `aria-labelledby` மூலம் பெறுதல்

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L2-L3
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L93-L94
```

### உள்ளடக்கத்தால் பெறுதல்

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L4
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L100-L101
```

### தலைப்பால் பெறுதல்

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L5
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L107-L108
```

### `alt` பண்பால் பெறுதல்

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L6
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L114-L115
```

## ARIA - Role பண்பு

[ARIA பங்குகள்](https://www.w3.org/TR/html-aria/#docconformance) அடிப்படையில் உறுப்புகளை வினவ, நீங்கள் நேரடியாக `[role=button]` போன்ற உறுப்பின் பங்கை தேர்வுக்கருவி அளவுருவாக குறிப்பிடலாம்:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L13
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L131-L132
```

## ID பண்பு

"id" இருப்பிட உத்தி WebDriver நெறிமுறையில் ஆதரிக்கப்படவில்லை, ID ஐப் பயன்படுத்தி உறுப்புகளைக் கண்டறிய CSS அல்லது xPath தேர்வுக்கருவி உத்திகளைப் பயன்படுத்த வேண்டும்.

எனினும் சில இயக்கிகள் (எ.கா. [Appium You.i Engine Driver](https://github.com/YOU-i-Labs/appium-youiengine-driver#selector-strategies)) இன்னும் இந்த தேர்வுக்கருவியை [ஆதரிக்கலாம்](https://github.com/YOU-i-Labs/appium-youiengine-driver#selector-strategies).

தற்போது ஆதரிக்கப்படும் ID தொடரியல்கள்:

```js
//css locator
const button = await $('#someid')
//xpath locator
const button = await $('//*[@id="someid"]')
//id strategy
// Note: works only in Appium or similar frameworks which supports locator strategy "ID"
const button = await $('id=resource-id/iosname')
```

## JS செயல்பாடு

உறுப்புகளைப் பெற வலை இயல்பான API களைப் பயன்படுத்தி JavaScript செயல்பாடுகளையும் நீங்கள் பயன்படுத்தலாம். நிச்சயமாக, இதை ஒரு வலை சூழலில் மட்டுமே (எ.கா., `browser`, அல்லது மொபைலில் வலை சூழல்) செய்ய முடியும்.

பின்வரும் HTML அமைப்பைக் கருத்தில் கொண்டு:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/js.html
```

`#elem` இன் உடன்பிறப்பு உறுப்பை பின்வருமாறு வினவலாம்:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L139-L143
```

## ஆழமான தேர்வுக்கருவிகள்

:::warning

WebdriverIO `v9` முதல் இந்த சிறப்பு தேர்வுக்கருவி தேவையில்லை, ஏனெனில் WebdriverIO தானாகவே Shadow DOM வழியாக ஊடுருவுகிறது. `>>>` ஐ அதன் முன்னால் நீக்குவதன் மூலம் இந்த தேர்வுக்கருவியிலிருந்து விலகுவது பரிந்துரைக்கப்படுகிறது.

:::

பல முன்முனை பயன்பாடுகள் [shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM) கொண்ட உறுப்புகளை அதிகம் நம்பியுள்ளன. இழுவைத் தீர்வுகள் இல்லாமல் shadow DOM க்குள் உறுப்புகளை வினவுவது தொழில்நுட்ப ரீதியாக சாத்தியமற்றது. [`shadow$`](https://webdriver.io/docs/api/element/shadow$) மற்றும் [`shadow$$`](https://webdriver.io/docs/api/element/shadow$$) ஆகியவை அத்தகைய இழுவைத் தீர்வுகளாக இருந்தன, அவற்றுக்கு [வரம்புகள்](https://github.com/Georgegriff/query-selector-shadow-dom#how-is-this-different-to-shadow) இருந்தன. ஆழமான தேர்வுக்கருவியுடன், நீங்கள் இப்போது பொதுவான வினவல் கட்டளையைப் பயன்படுத்தி எந்த shadow DOM க்குள்ளும் உள்ள அனைத்து உறுப்புகளையும் வினவலாம்.

பின்வரும் அமைப்பைக் கொண்ட ஒரு பயன்பாட்டை நாங்கள் கொண்டிருந்தால்:

![Chrome Example](https://github.com/Georgegriff/query-selector-shadow-dom/raw/main/Chrome-example.png "Chrome Example")

இந்த தேர்வுக்கருவியுடன், நீங்கள் மற்றொரு shadow DOM க்குள் உள்ளிடப்பட்ட `<button />` உறுப்பை வினவலாம், எ.கா.:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L147-L149
```

## மொபைல் தேர்வுக்கருவிகள்

ஹைப்ரிட் மொபைல் சோதனைக்கு, கட்டளைகளை செயல்படுத்துவதற்கு முன் தானியங்கி சேவையகம் சரியான *சூழலில்* இருப்பது முக்கியம். சைகைகளை தானியங்குபடுத்த, இயக்கி இயல்பான சூழலுக்கு அமைக்கப்பட வேண்டும். ஆனால் DOM இலிருந்து உறுப்புகளைத் தேர்ந்தெடுக்க, இயக்கி தளத்தின் webview சூழலுக்கு அமைக்கப்பட வேண்டும். *அப்போது* மட்டுமே மேலே குறிப்பிடப்பட்ட முறைகளைப் பயன்படுத்தலாம்.

இயல்பான மொபைல் சோதனைக்கு, சூழல்களுக்கு இடையே மாறுவது இல்லை, ஏனெனில் நீங்கள் மொபைல் உத்திகளைப் பயன்படுத்தி, அடிப்படை சாதன தானியங்கி தொழில்நுட்பத்தை நேரடியாகப் பயன்படுத்த வேண்டும். சோதனைக்கு உறுப்புகளைக் கண்டறிவதில் சிறப்பான கட்டுப்பாடு தேவைப்படும்போது இது குறிப்பாக பயனுள்ளதாக இருக்கும்.

### Android UiAutomator

Android இன் UI Automator கட்டமைப்பு உறுப்புகளைக் கண்டறிவதற்கான பல வழிகளை வழங்குகிறது. நீங்கள் [UI Automator API](https://developer.android.com/tools/testing-support-library/index.html#uia-apis) ஐப் பயன்படுத்தலாம், குறிப்பாக உறுப்புகளை கண்டறிய [UiSelector வகுப்பு](https://developer.android.com/reference/androidx/test/uiautomator/UiSelector) ஐப் பயன்படுத்தலாம். Appium இல் நீங்கள் Java குறியீட்டை ஒரு சரமாக சேவையகத்திற்கு அனுப்புகிறீர்கள், அது பயன்பாட்டின் சூழலில் அதை இயக்குகிறது, உறுப்பு அல்லது உறுப்புகளைத் திருப்பித் தருகிறது.

```js
const selector = 'new UiSelector().text("Cancel").className("android.widget.Button")'
const button = await $(`android=${selector}`)
await button.click()
```

### Android DataMatcher மற்றும் ViewMatcher (Espresso மட்டும்)

Android இன் DataMatcher உத்தி [Data Matcher](https://developer.android.com/reference/android/support/test/espresso/DataInteraction) மூலம் உறுப்புகளைக் கண்டறிய ஒரு வழியை வழங்குகிறது

```js
const menuItem = await $({
  "name": "hasEntry",
  "args": ["title", "ViewTitle"]
})
await menuItem.click()
```

மற்றும் அதேபோல் [View Matcher](https://developer.android.com/reference/android/support/test/espresso/ViewInteraction)

```js
const menuItem = await $({
  "name": "hasEntry",
  "args": ["title", "ViewTitle"],
  "class": "androidx.test.espresso.matcher.ViewMatchers"
})
await menuItem.click()
```

### Android View Tag (Espresso மட்டும்)

காட்சி குறிச்சொல் உத்தி அவற்றின் [tag](https://developer.android.com/reference/android/support/test/espresso/matcher/ViewMatchers.html#withTagValue%28org.hamcrest.Matcher%3Cjava.lang.Object%3E%29) மூலம் உறுப்புகளைக் கண்டறிய ஒரு வசதியான வழியை வழங்குகிறது.

```js
const elem = await $('-android viewtag:tag_identifier')
await elem.click()
```

### iOS UIAutomation

iOS பயன்பாட்டை தானியங்குபடுத்தும்போது, உறுப்புகளைக் கண்டறிய Apple இன் [UI Automation framework](https://developer.apple.com/library/prerelease/tvos/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/UIAutomation.html) ஐப் பயன்படுத்தலாம்.

இந்த JavaScript [API](https://developer.apple.com/library/ios/documentation/DeveloperTools/Reference/UIAutomationRef/index.html#//apple_ref/doc/uid/TP40009771) காட்சியையும் அதில் உள்ள அனைத்தையும் அணுகுவதற்கான முறைகளைக் கொண்டுள்ளது.

```js
const selector = 'UIATarget.localTarget().frontMostApp().mainWindow().buttons()[0]'
const button = await $(`ios=${selector}`)
await button.click()
```

உறுப்பு தேர்வை மேலும் சுத்திகரிக்க Appium இல் iOS UI Automation க்குள் பிரிடிகேட் தேடலையும் பயன்படுத்தலாம். விவரங்களுக்கு [இங்கே](https://github.com/appium/appium/blob/master/docs/en/writing-running-appium/ios/ios-predicate.md) பார்க்கவும்.

### iOS XCUITest பிரிடிகேட் சரங்கள் மற்றும் வகுப்பு சங்கிலிகள்

iOS 10 மற்றும் அதற்கு மேற்பட்டவற்றுடன் (`XCUITest` இயக்கியைப் பயன்படுத்தி), [பிரிடிகேட் சரங்களைப்](https://github.com/facebook/WebDriverAgent/wiki/Predicate-Queries-Construction-Rules) பயன்படுத்தலாம்:

```js
const selector = `type == 'XCUIElementTypeSwitch' && name CONTAINS 'Allow'`
const switch = await $(`-ios predicate string:${selector}`)
await switch.click()
```

மற்றும் [வகுப்பு சங்கிலிகள்](https://github.com/facebook/WebDriverAgent/wiki/Class-Chain-Queries-Construction-Rules):

```js
const selector = '**/XCUIElementTypeCell[`name BEGINSWITH "D"`]/**/XCUIElementTypeButton'
const button = await $(`-ios class chain:${selector}`)
await button.click()
```

### Accessibility ID

`accessibility id` இருப்பிட உத்தி UI உறுப்புக்கான தனித்துவமான அடையாளங்காட்டியைப் படிக்க வடிவமைக்கப்பட்டுள்ளது. இது மொழிபெயர்ப்பு அல்லது உரையை மாற்றக்கூடிய வேறு எந்த செயல்முறையின் போதும் மாறாமல் இருக்கும் என்ற நன்மையைக் கொண்டுள்ளது. கூடுதலாக, செயல்பாட்டு ரீதியாக ஒரே மாதிரியான உறுப்புகள் ஒரே அணுகல் id ஐக் கொண்டிருந்தால், குறுக்கு-தளம் சோதனைகளை உருவாக்குவதற்கு இது உதவியாக இருக்கலாம்.

- iOS க்கு இது [இங்கே](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAccessibilityIdentification_Protocol/index.html) Apple ஆல் வடிவமைக்கப்பட்ட `accessibility identifier` ஆகும்.
- Android க்கு `accessibility id` [இங்கே](https://developer.android.com/training/accessibility/accessible-app.html) விவரிக்கப்பட்டுள்ளபடி, உறுப்புக்கான `content-description` உடன் பொருந்துகிறது.

இரண்டு தளங்களுக்கும், அவற்றின் `accessibility id` மூலம் ஒரு உறுப்பைப் பெறுவது (அல்லது பல உறுப்புகள்) பொதுவாக சிறந்த முறையாகும். இது காலாவதியான `name` உத்திக்கு மேலாக விரும்பப்படும் வழியாகும்.

```js
const elem = await $('~my_accessibility_identifier')
await elem.click()
```

### Class Name

`class name` உத்தி தற்போதைய காட்சியில் உள்ள UI உறுப்பைக் குறிக்கும் ஒரு `string` ஆகும்.

- iOS க்கு இது [UIAutomation வகுப்பின்](https://developer.apple.com/library/prerelease/tvos/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/UIAutomation.html) முழு பெயராகும், மேலும் இது `UIA-` உடன் தொடங்கும், எ.கா. உரை புலத்திற்கு `UIATextField`. முழு குறிப்பு [இங்கே](https://developer.apple.com/library/ios/navigation/#section=Frameworks&topic=UIAutomation) காணலாம்.
- Android க்கு இது ஒரு [UI Automator](https://developer.android.com/tools/testing-support-library/index.html#UIAutomator) [வகுப்பின்](https://developer.android.com/reference/android/widget/package-summary.html) முழுமையான பெயராகும், எ.கா. உரை புலத்திற்கு `android.widget.EditText`. முழு குறிப்பு [இங்கே](https://developer.android.com/reference/android/widget/package-summary.html) காணலாம்.
- Youi.tv க்கு இது Youi.tv வகுப்பின் முழு பெயர், மற்றும் இது `CYI-` உடன் தொடங்கும், எ.கா. அழுத்த பொத்தான் உறுப்புக்கு `CYIPushButtonView`. முழு குறிப்பு [You.i Engine Driver's GitHub பக்கத்தில்](https://github.com/YOU-i-Labs/appium-youiengine-driver) காணலாம்

```js
// iOS example
await $('UIATextField').click()
// Android example
await $('android.widget.DatePicker').click()
// Youi.tv example
await $('CYIPushButtonView').click()
```

## சங்கிலி தேர்வுக்கருவிகள்

உங்கள் வினவலில் மேலும் குறிப்பிட்டதாக இருக்க விரும்பினால், சரியான உறுப்பைக் கண்டுபிடிக்கும் வரை தேர்வுக்கருவிகளை சங்கிலியாக்கலாம். உங்கள் உண்மையான கட்டளைக்கு முன் `element` ஐ அழைத்தால், WebdriverIO அந்த உறுப்பிலிருந்து வினவலைத் தொடங்குகிறது.

எடுத்துக்காட்டாக, உங்களிடம் பின்வரும் DOM அமைப்பு இருந்தால்:

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

மற்றும் நீங்கள் பொருள் B ஐ வண்டியில் சேர்க்க விரும்பினால், CSS தேர்வுக்கருவியைப் பயன்படுத்தி மட்டுமே அதைச் செய்வது கடினமாக இருக்கும்.

தேர்வுக்கருவி சங்கிலிப்படுத்தலுடன், இது மிகவும் எளிதானது. வேண்டிய உறுப்பைப் படிப்படியாக குறுக்கவும்:

```js
await $('.row .entry:nth-child(2)').$('button*=Add').click()
```

### Appium Image தேர்வுக்கருவி

`-image` இருப்பிட உத்தியைப் பயன்படுத்தி, நீங்கள் அணுக விரும்பும் உறுப்பைக் குறிக்கும் படக் கோப்பை Appium க்கு அனுப்ப முடியும்.

ஆதரிக்கப்படும் கோப்பு வடிவங்கள் `jpg,png,gif,bmp,svg`

முழு குறிப்பு [இங்கே](https://github.com/appium/appium/blob/master/packages/images-plugin/docs/find-by-image.md) காணலாம்

```js
const elem = await $('./file/path/of/image/test.jpg')
await elem.click()
```

**குறிப்பு**: Appium இந்த தேர்வுக்கருவியுடன் எவ்வாறு செயல்படுகிறது என்பது, உள்ளகமாக ஒரு (app)screenshot ஐ எடுத்து, வழங்கப்பட்ட படத் தேர்வுக்கருவியைப் பயன்படுத்தி அந்த (app)screenshot இல் உறுப்பைக் கண்டுபிடிக்க முடியுமா என்பதை சரிபார்க்கும்.

Appium எடுத்த (app)screenshot ஐ உங்கள் (app)திரையின் CSS-அளவுடன் பொருந்துவதற்கு அளவுமாற்றம் செய்யலாம் என்பதை கவனத்தில் கொள்ளுங்கள் (iPhone களில் இது நிகழும், ஆனால் DPR 1 ஐ விட பெரியதாக இருப்பதால் Retina திரையுடன் Mac இயந்திரங்களிலும் இது நிகழும்). வழங்கப்பட்ட படத் தேர்வுக்கருவி அசல் screenshot இலிருந்து எடுக்கப்பட்டிருக்கலாம் என்பதால் இது பொருத்தத்தைக் கண்டறியாமல் இருக்கும்.
Appium சேவையக அமைப்புகளைப் புதுப்பித்து இதைச் சரிசெய்யலாம், [Appium ஆவணங்களைப்](https://github.com/appium/appium/blob/master/packages/images-plugin/docs/find-by-image.md#related-settings) பார்க்கவும் அமைப்புகளுக்கு மற்றும் [இந்த கருத்து](https://github.com/webdriverio/webdriverio/issues/6097#issuecomment-726675579) விரிவான விளக்கத்திற்கு.

## React தேர்வுக்கருவிகள்

WebdriverIO, கூறு பெயரை அடிப்படையாகக் கொண்டு React கூறுகளைத் தேர்ந்தெடுக்க ஒரு வழியை வழங்குகிறது. இதைச் செய்ய, உங்களுக்கு இரண்டு கட்டளைகளில் தேர்வு உள்ளது: `react$` மற்றும் `react$$`.

இந்த கட்டளைகள் [React VirtualDOM](https://reactjs.org/docs/faq-internals.html) இலிருந்து கூறுகளைத் தேர்ந்தெடுக்க அனுமதிக்கின்றன, மேலும் ஒரு WebdriverIO உறுப்பு அல்லது உறுப்புகளின் வரிசையை (எந்த செயல்பாடு பயன்படுத்தப்படுகிறது என்பதைப் பொறுத்து) திருப்பித் தருகின்றன.

**குறிப்பு**: `react$` மற்றும் `react$$` கட்டளைகள் செயல்பாட்டில் ஒத்திருக்கின்றன, இருப்பினும் `react$$` *அனைத்து* பொருந்தும் நிகழ்வுகளை WebdriverIO உறுப்புகளின் வரிசையாக திருப்பித் தரும், மற்றும் `react$` முதலில் கண்டுபிடிக்கப்பட்ட நிகழ்வை மட்டுமே திருப்பித் தரும்.

#### அடிப்படை எடுத்துக்காட்டு

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

மேலே உள்ள குறியீட்டில், பயன்பாட்டில் ஒரு எளிய `MyComponent` நிகழ்வு உள்ளது, அதை React `id="root"` உடன் HTML உறுப்புக்குள் ரெண்டர் செய்கிறது.

`browser.react$` கட்டளையைப் பயன்படுத்தி, `MyComponent` இன் ஒரு நிகழ்வைத் தேர்ந்தெடுக்கலாம்:

```js
const myCmp = await browser.react$('MyComponent')
```

இப்போது `myCmp` மாறியில் WebdriverIO உறுப்பு சேமிக்கப்பட்டுள்ளது, அதற்கு எதிராக உறுப்பு கட்டளைகளை நீங்கள் செயல்படுத்தலாம்.

#### கூறுகளை வடிகட்டுதல்

WebdriverIO உள்ளகமாகப் பயன்படுத்தும் நூலகம் கூறுவின் props மற்றும்/அல்லது நிலையால் உங்கள் தேர்வை வடிகட்ட அனுமதிக்கிறது. இதைச் செய்ய, நீங்கள் props க்கு இரண்டாவது அளவுருவையும், நிலைக்கு மூன்றாவது அளவுருவையும் உலாவி கட்டளைக்கு அனுப்ப வேண்டும்.

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

`WebdriverIO` என்ற `name` prop கொண்ட `MyComponent` நிகழ்வைத் தேர்ந்தெடுக்க விரும்பினால், கட்டளையை இவ்வாறு செயல்படுத்தலாம்:

```js
const myCmp = await browser.react$('MyComponent', {
    props: { name: 'WebdriverIO' }
})
```

நீங்கள் நிலையால் உங்கள் தேர்வை வடிகட்ட விரும்பினால், `browser` கட்டளை இவ்வாறு இருக்கும்:

```js
const myCmp = await browser.react$('MyComponent', {
    state: { myState: 'some value' }
})
```

#### `React.Fragment` உடன் கையாளுதல்

React [fragments](https://reactjs.org/docs/fragments.html) ஐத் தேர்ந்தெடுக்க `react$` கட்டளையைப் பயன்படுத்தும்போது, WebdriverIO அந்த கூறுவின் முதல் குழந்தையை கூறுவின் node ஆக திருப்பித் தரும். நீங்கள் `react$$` ஐப் பயன்படுத்தினால், தேர்வுக்கருவியுடன் பொருந்தும் fragments க்குள் உள்ள அனைத்து HTML node களையும் கொண்ட வரிசையைப் பெறுவீர்கள்.

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

மேலே உள்ள எடுத்துக்காட்டைப் பொறுத்த வரையில், கட்டளைகள் இவ்வாறு செயல்படும்:

```js
await browser.react$('MyComponent') // முதல் <div /> க்கான WebdriverIO உறுப்பைத் திருப்பித் தருகிறது
await browser.react$$('MyComponent') // வரிசை [<div />, <div />] க்கான WebdriverIO உறுப்புகளைத் திருப்பித் தருகிறது
```

**குறிப்பு:** உங்களிடம் பல `MyComponent` நிகழ்வுகள் இருந்து, இந்த fragment கூறுகளைத் தேர்ந்தெடுக்க `react$$` ஐப் பயன்படுத்தினால், அனைத்து node களின் ஒரு-பரிமாண வரிசையைப் பெறுவீர்கள். அதாவது, உங்களிடம் 3 `<MyComponent />` நிகழ்வுகள் இருந்தால், ஆறு WebdriverIO உறுப்புகளுடன் கூடிய வரிசையைப் பெறுவீர்கள்.

## தனிப்பயன் தேர்வுக்கருவி உத்திகள்


உங்கள் பயன்பாட்டிற்கு உறுப்புகளைப் பெறுவதற்கு ஒரு குறிப்பிட்ட வழி தேவைப்பட்டால், `custom$` மற்றும் `custom$$` உடன் பயன்படுத்த நீங்களே ஒரு தனிப்பயன் தேர்வுக்கருவி உத்தியை வரையறுக்கலாம். அதற்காக சோதனையின் தொடக்கத்தில் ஒருமுறை உங்கள் உத்தியைப் பதிவு செய்யுங்கள், எ.கா. ஒரு `before` hook இல்:

```js reference
https://github.com/webdriverio/example-recipes/blob/f5730428ec3605e856e90bf58be17c9c9da891de/queryElements/customStrategy.js#L2-L11
```

பின்வரும் HTML துணுக்கு கொடுக்கப்பட்டுள்ளது:

```html reference
https://github.com/webdriverio/example-recipes/blob/f5730428ec3605e856e90bf58be17c9c9da891de/queryElements/example.html#L8-L12
```

பின்னர் இவ்வாறு அழைத்துப் பயன்படுத்தவும்:

```js reference
https://github.com/webdriverio/example-recipes/blob/f5730428ec3605e856e90bf58be17c9c9da891de/queryElements/customStrategy.js#L16-L19
```

**குறிப்பு:** இது [`execute`](/docs/api/browser/execute) கட்டளையை இயக்கக்கூடிய வலை சூழலில் மட்டுமே செயல்படும்.