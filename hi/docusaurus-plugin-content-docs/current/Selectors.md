---
id: selectors
title: सेलेक्टर्स
---

[WebDriver Protocol](https://w3c.github.io/webdriver/) एक तत्व को क्वेरी करने के लिए कई सेलेक्टर रणनीतियां प्रदान करता है। WebdriverIO उन्हें सरल बनाता है ताकि तत्वों का चयन करना आसान रहे। कृपया ध्यान दें कि यद्यपि तत्वों को क्वेरी करने का कमांड `$` और `$$` कहलाता है, उनका jQuery या [Sizzle Selector Engine](https://github.com/jquery/sizzle) से कोई संबंध नहीं है।

हालांकि कई अलग-अलग सेलेक्टर उपलब्ध हैं, उनमें से केवल कुछ ही सही तत्व को खोजने का एक लचीला तरीका प्रदान करते हैं। उदाहरण के लिए, निम्न बटन दिया गया है:

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

हम निम्नलिखित सेलेक्टर्स की __अनुशंसा करते हैं__ और __अनुशंसा नहीं करते हैं__:

| सेलेक्टर | अनुशंसित | नोट्स |
| -------- | ----------- | ----- |
| `$('button')` | 🚨 कभी नहीं | सबसे खराब - बहुत सामान्य, कोई संदर्भ नहीं। |
| `$('.btn.btn-large')` | 🚨 कभी नहीं | बुरा। स्टाइलिंग से जुड़ा हुआ। परिवर्तन के अधीन अत्यधिक। |
| `$('#main')` | ⚠️ कम-कम | बेहतर। लेकिन फिर भी स्टाइलिंग या JS इवेंट लिसनर्स से जुड़ा हुआ। |
| `$(() => document.queryElement('button'))` | ⚠️ कम-कम | प्रभावी क्वेरी, लिखने में जटिल। |
| `$('button[name="submission"]')` | ⚠️ कम-कम | `name` विशेषता से जुड़ा हुआ जिसमें HTML सिमेंटिक्स हैं। |
| `$('button[data-testid="submit"]')` | ✅ अच्छा | अतिरिक्त विशेषता की आवश्यकता है, a11y से जुड़ा नहीं है। |
| `$('aria/Submit')` या `$('button=Submit')` | ✅ हमेशा | सबसे अच्छा। इसमें दिखाया गया है कि उपयोगकर्ता पेज के साथ कैसे इंटरैक्ट करता है। यह अनुशंसित है कि आप अपने फ्रंटएंड की अनुवाद फ़ाइलों का उपयोग करें ताकि जब अनुवाद अपडेट किए जाएं तो आपके टेस्ट विफल न हों |

## CSS क्वेरी सेलेक्टर

यदि अन्यथा इंगित नहीं किया गया है, तो WebdriverIO [CSS सेलेक्टर](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors) पैटर्न का उपयोग करके तत्वों को क्वेरी करेगा, उदाहरण के लिए:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L7-L8
```

## लिंक टेक्स्ट

किसी विशेष टेक्स्ट वाले एंकर तत्व को प्राप्त करने के लिए, बराबर (`=`) चिह्न से शुरू होने वाले टेक्स्ट को क्वेरी करें।

उदाहरण के लिए:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L3
```

आप इस कमांड को कॉल करके इस तत्व को क्वेरी कर सकते हैं:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L16-L18
```

## आंशिक लिंक टेक्स्ट

ऐसा एंकर तत्व खोजने के लिए जिसका दृश्यमान टेक्स्ट आंशिक रूप से आपके खोज मूल्य से मेल खाता हो, क्वेरी स्ट्रिंग के सामने `*=` का उपयोग करके क्वेरी करें (जैसे `*=driver`)।

आप ऊपर दिए गए उदाहरण के तत्व को यह कॉल करके भी क्वेरी कर सकते हैं:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L24-L26
```

__नोट:__ आप एक सेलेक्टर में कई सेलेक्टर रणनीतियों को मिश्रित नहीं कर सकते। समान लक्ष्य तक पहुंचने के लिए कई चेन किए गए तत्व क्वेरी का उपयोग करें, उदाहरण के लिए:

```js
const elem = await $('header h1*=Welcome') // काम नहीं करता!!!
// इसके बजाय उपयोग करें
const elem = await $('header').$('*=driver')
```

## विशेष टेक्स्ट वाला तत्व

वही तकनीक तत्वों पर भी लागू की जा सकती है। इसके अतिरिक्त, क्वेरी के भीतर `.=` या `.*=` का उपयोग करके केस-इनसेंसिटिव मैचिंग करना भी संभव है।

उदाहरण के लिए, यहां "Welcome to my Page" टेक्स्ट के साथ लेवल 1 हेडिंग के लिए एक क्वेरी है:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L2
```

आप इस कमांड को कॉल करके इस तत्व को क्वेरी कर सकते हैं:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/13eddfac6f18a2a4812cc09ed7aa5e468f392060/selectors/example.js#L35C1-L38
```

या आंशिक टेक्स्ट क्वेरी का उपयोग करके:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/13eddfac6f18a2a4812cc09ed7aa5e468f392060/selectors/example.js#L44C9-L47
```

यही `id` और `class` नामों के लिए भी काम करता है:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L4
```

आप इस कमांड को कॉल करके इस तत्व को क्वेरी कर सकते हैं:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/13eddfac6f18a2a4812cc09ed7aa5e468f392060/selectors/example.js#L49-L67
```

__नोट:__ आप एक सेलेक्टर में कई सेलेक्टर रणनीतियों को मिश्रित नहीं कर सकते। समान लक्ष्य तक पहुंचने के लिए कई चेन किए गए तत्व क्वेरी का उपयोग करें, उदाहरण के लिए:

```js
const elem = await $('header h1*=Welcome') // काम नहीं करता!!!
// इसके बजाय उपयोग करें
const elem = await $('header').$('h1*=Welcome')
```

## टैग नाम

एक विशिष्ट टैग नाम वाले तत्व को क्वेरी करने के लिए, `<tag>` या `<tag />` का उपयोग करें।

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L5
```

आप इस कमांड को कॉल करके इस तत्व को क्वेरी कर सकते हैं:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L61-L62
```

## नाम विशेषता

विशिष्ट नाम विशेषता वाले तत्वों को क्वेरी करने के लिए आप या तो सामान्य CSS3 सेलेक्टर का उपयोग कर सकते हैं या [JSONWireProtocol](https://github.com/SeleniumHQ/selenium/wiki/JsonWireProtocol) से प्रदान की गई नाम रणनीति का उपयोग कर सकते हैं, जिसमें सेलेक्टर पैरामीटर के रूप में [name="some-name"] जैसा कुछ पास करते हैं:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.html#L6
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L68-L69
```

__नोट:__ यह सेलेक्टर रणनीति पुरानी है और केवल पुराने ब्राउज़र में काम करती है जो JSONWireProtocol प्रोटोकॉल द्वारा चलाए जाते हैं या Appium का उपयोग करके।

## xPath

विशिष्ट [xPath](https://developer.mozilla.org/en-US/docs/Web/XPath) के माध्यम से तत्वों को क्वेरी करना भी संभव है।

एक xPath सेलेक्टर का फॉर्मेट `//body/div[6]/div[1]/span[1]` जैसा होता है।

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/xpath.html
```

आप इस कमांड को कॉल करके दूसरे पैराग्राफ को क्वेरी कर सकते हैं:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L75-L76
```

आप xPath का उपयोग DOM ट्री में ऊपर और नीचे जाने के लिए भी कर सकते हैं:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L78-L79
```

## एक्सेसिबिलिटी नाम सेलेक्टर

तत्वों को उनके एक्सेसिबल नाम से क्वेरी करें। एक्सेसिबल नाम वह है जो स्क्रीन रीडर द्वारा घोषित किया जाता है जब उस तत्व पर फोकस होता है। एक्सेसिबल नाम का मूल्य दृश्य सामग्री या छिपे हुए टेक्स्ट विकल्प हो सकता है।

:::info

आप इस सेलेक्टर के बारे में हमारे [रिलीज़ ब्लॉग पोस्ट](/blog/2022/09/05/accessibility-selector) में अधिक पढ़ सकते हैं

:::

### `aria-label` द्वारा प्राप्त करें

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L1
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L86-L87
```

### `aria-labelledby` द्वारा प्राप्त करें

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L2-L3
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L93-L94
```

### सामग्री द्वारा प्राप्त करें

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L4
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L100-L101
```

### शीर्षक द्वारा प्राप्त करें

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L5
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L107-L108
```

### `alt` प्रॉपर्टी द्वारा प्राप्त करें

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L6
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L114-L115
```

## ARIA - रोल विशेषता

[ARIA रोल्स](https://www.w3.org/TR/html-aria/#docconformance) के आधार पर तत्वों को क्वेरी करने के लिए, आप सेलेक्टर पैरामीटर के रूप में `[role=button]` जैसे तत्व की भूमिका को सीधे निर्दिष्ट कर सकते हैं:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/aria.html#L13
```

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L131-L132
```

## ID विशेषता

लोकेटर रणनीति "id" WebDriver प्रोटोकॉल में समर्थित नहीं है, ID का उपयोग करके तत्वों को खोजने के लिए किसी को CSS या xPath सेलेक्टर रणनीतियों का उपयोग करना चाहिए।

हालांकि, कुछ ड्राइवर (जैसे [Appium You.i Engine Driver](https://github.com/YOU-i-Labs/appium-youiengine-driver#selector-strategies)) इस सेलेक्टर को [समर्थन](https://github.com/YOU-i-Labs/appium-youiengine-driver#selector-strategies) कर सकते हैं।

ID के लिए वर्तमान समर्थित सेलेक्टर सिंटैक्स हैं:

```js
//css लोकेटर
const button = await $('#someid')
//xpath लोकेटर
const button = await $('//*[@id="someid"]')
//id रणनीति
// नोट: केवल Appium या समान फ्रेमवर्क में काम करता है जो लोकेटर रणनीति "ID" का समर्थन करते हैं
const button = await $('id=resource-id/iosname')
```

## JS फंक्शन

आप वेब नेटिव API का उपयोग करके तत्वों को प्राप्त करने के लिए JavaScript फंक्शन का भी उपयोग कर सकते हैं। बेशक, आप इसे केवल वेब कॉन्टेक्स्ट के अंदर ही कर सकते हैं (जैसे, `browser`, या मोबाइल में वेब कॉन्टेक्स्ट)।

दिए गए HTML संरचना के लिए:

```html reference
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/js.html
```

आप निम्न तरीके से `#elem` के सिबलिंग तत्व को क्वेरी कर सकते हैं:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L139-L143
```

## डीप सेलेक्टर्स

:::warning

WebdriverIO के `v9` से शुरू होकर, इस विशेष सेलेक्टर की कोई आवश्यकता नहीं है क्योंकि WebdriverIO स्वचालित रूप से आपके लिए Shadow DOM के माध्यम से प्रवेश करता है। इसके सामने से `>>>` को हटाकर इस सेलेक्टर से माइग्रेट करने की सिफारिश की जाती है।

:::

कई फ्रंटएंड एप्लिकेशन [shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM) वाले तत्वों पर भारी निर्भर करते हैं। बिना वर्कअराउंड के shadow DOM के भीतर तत्वों को क्वेरी करना तकनीकी रूप से असंभव है। [`shadow$`](https://webdriver.io/docs/api/element/shadow$) और [`shadow$$`](https://webdriver.io/docs/api/element/shadow$$) ऐसे वर्कअराउंड थे जिनकी अपनी [सीमाएँ](https://github.com/Georgegriff/query-selector-shadow-dom#how-is-this-different-to-shadow) थीं। डीप सेलेक्टर के साथ अब आप सामान्य क्वेरी कमांड का उपयोग करके किसी भी shadow DOM के भीतर सभी तत्वों को क्वेरी कर सकते हैं।

मान लीजिए कि हमारे पास निम्न संरचना वाला एप्लिकेशन है:

![Chrome Example](https://github.com/Georgegriff/query-selector-shadow-dom/raw/main/Chrome-example.png "Chrome Example")

इस सेलेक्टर के साथ आप `<button />` तत्व को क्वेरी कर सकते हैं जो दूसरे shadow DOM के अंदर नेस्टेड है, उदाहरण के लिए:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/selectors/example.js#L147-L149
```

## मोबाइल सेलेक्टर्स

हाइब्रिड मोबाइल टेस्टिंग के लिए, कमांड निष्पादित करने से पहले ऑटोमेशन सर्वर को सही *कॉन्टेक्स्ट* में होना महत्वपूर्ण है। जेस्चर को ऑटोमेट करने के लिए, ड्राइवर को आदर्श रूप से नेटिव कॉन्टेक्स्ट पर सेट किया जाना चाहिए। लेकिन DOM से तत्वों का चयन करने के लिए, ड्राइवर को प्लेटफॉर्म के वेबव्यू कॉन्टेक्स्ट पर सेट किया जाना चाहिए। केवल *तब* ऊपर बताई गई विधियों का उपयोग किया जा सकता है।

नेटिव मोबाइल टेस्टिंग के लिए, कॉन्टेक्स्ट के बीच स्विचिंग नहीं होती है, क्योंकि आपको मोबाइल रणनीतियों का उपयोग करना होता है और अंतर्निहित डिवाइस ऑटोमेशन टेक्नोलॉजी का सीधे उपयोग करना होता है। यह विशेष रूप से उपयोगी होता है जब टेस्ट को तत्वों को खोजने के लिए बारीक नियंत्रण की आवश्यकता होती है।

### Android UiAutomator

Android का UI Automator फ्रेमवर्क तत्वों को खोजने के कई तरीके प्रदान करता है। आप [UI Automator API](https://developer.android.com/tools/testing-support-library/index.html#uia-apis), विशेष रूप से [UiSelector क्लास](https://developer.android.com/reference/androidx/test/uiautomator/UiSelector) का उपयोग तत्वों को ढूंढने के लिए कर सकते हैं। Appium में आप जावा कोड, स्ट्रिंग के रूप में, सर्वर को भेजते हैं, जो एप्लिकेशन के वातावरण में निष्पादित होता है और तत्व या तत्वों को वापस लौटाता है।

```js
const selector = 'new UiSelector().text("Cancel").className("android.widget.Button")'
const button = await $(`android=${selector}`)
await button.click()
```

### Android DataMatcher और ViewMatcher (केवल Espresso)

Android की DataMatcher रणनीति [Data Matcher](https://developer.android.com/reference/android/support/test/espresso/DataInteraction) द्वारा तत्वों को खोजने का एक तरीका प्रदान करती है

```js
const menuItem = await $({
  "name": "hasEntry",
  "args": ["title", "ViewTitle"]
})
await menuItem.click()
```

और इसी तरह [View Matcher](https://developer.android.com/reference/android/support/test/espresso/ViewInteraction)

```js
const menuItem = await $({
  "name": "hasEntry",
  "args": ["title", "ViewTitle"],
  "class": "androidx.test.espresso.matcher.ViewMatchers"
})
await menuItem.click()
```

### Android View Tag (केवल Espresso)

व्यू टैग रणनीति उनके [टैग](https://developer.android.com/reference/android/support/test/espresso/matcher/ViewMatchers.html#withTagValue%28org.hamcrest.Matcher%3Cjava.lang.Object%3E%29) द्वारा तत्वों को खोजने का एक सुविधाजनक तरीका प्रदान करती है।

```js
const elem = await $('-android viewtag:tag_identifier')
await elem.click()
```

### iOS UIAutomation

जब iOS एप्लिकेशन को ऑटोमेट कर रहे हों, तो Apple का [UI Automation फ्रेमवर्क](https://developer.apple.com/library/prerelease/tvos/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/UIAutomation.html) तत्वों को खोजने के लिए उपयोग किया जा सकता है।

यह JavaScript [API](https://developer.apple.com/library/ios/documentation/DeveloperTools/Reference/UIAutomationRef/index.html#//apple_ref/doc/uid/TP40009771) व्यू और उस पर सब कुछ तक पहुंचने के तरीके प्रदान करता है।

```js
const selector = 'UIATarget.localTarget().frontMostApp().mainWindow().buttons()[0]'
const button = await $(`ios=${selector}`)
await button.click()
```

आप Appium में iOS UI Automation के भीतर प्रेडिकेट सर्चिंग का भी उपयोग कर सकते हैं, जिससे तत्व चयन को और भी परिष्कृत किया जा सकता है। विवरण के लिए [यहां](https://github.com/appium/appium/blob/master/docs/en/writing-running-appium/ios/ios-predicate.md) देखें।

### iOS XCUITest प्रेडिकेट स्ट्रिंग्स और क्लास चेन्स

iOS 10 और उससे ऊपर (`XCUITest` ड्राइवर का उपयोग करके), आप [प्रेडिकेट स्ट्रिंग्स](https://github.com/facebook/WebDriverAgent/wiki/Predicate-Queries-Construction-Rules) का उपयोग कर सकते हैं:

```js
const selector = `type == 'XCUIElementTypeSwitch' && name CONTAINS 'Allow'`
const switch = await $(`-ios predicate string:${selector}`)
await switch.click()
```

और [क्लास चेन्स](https://github.com/facebook/WebDriverAgent/wiki/Class-Chain-Queries-Construction-Rules):

```js
const selector = '**/XCUIElementTypeCell[`name BEGINSWITH "D"`]/**/XCUIElementTypeButton'
const button = await $(`-ios class chain:${selector}`)
await button.click()
```

### एक्सेसिबिलिटी ID

`accessibility id` लोकेटर रणनीति को UI तत्व के लिए एक अद्वितीय पहचानकर्ता पढ़ने के लिए डिज़ाइन किया गया है। इसका लाभ यह है कि स्थानीयकरण या किसी अन्य प्रक्रिया के दौरान यह बदलता नहीं है जो टेक्स्ट को बदल सकती है। इसके अतिरिक्त, यह क्रॉस-प्लेटफॉर्म टेस्ट बनाने में मदद कर सकता है, यदि तत्व जो कार्यात्मक रूप से समान हैं, उनके पास समान एक्सेसिबिलिटी आईडी है।

- iOS के लिए यह Apple द्वारा निर्धारित `accessibility identifier` है, जो [यहां](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAccessibilityIdentification_Protocol/index.html) दिया गया है।
- Android के लिए `accessibility id` तत्व के `content-description` से मैप होता है, जैसा कि [यहां](https://developer.android.com/training/accessibility/accessible-app.html) वर्णित है।

दोनों प्लेटफॉर्म के लिए, `accessibility id` द्वारा तत्व (या कई तत्वों) को प्राप्त करना आमतौर पर सबसे अच्छी विधि है। यह डिप्रिकेटेड `name` रणनीति से बेहतर तरीका है।

```js
const elem = await $('~my_accessibility_identifier')
await elem.click()
```

### क्लास नाम

`class name` रणनीति एक `string` है जो वर्तमान व्यू पर UI तत्व का प्रतिनिधित्व करती है।

- iOS के लिए यह [UIAutomation क्लास](https://developer.apple.com/library/prerelease/tvos/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/UIAutomation.html) का पूरा नाम है, और `UIA-` से शुरू होगा, जैसे टेक्स्ट फील्ड के लिए `UIATextField`। एक पूर्ण संदर्भ [यहां](https://developer.apple.com/library/ios/navigation/#section=Frameworks&topic=UIAutomation) मिल सकता है।
- Android के लिए यह [UI Automator](https://developer.android.com/tools/testing-support-library/index.html#UIAutomator) [क्लास](https://developer.android.com/reference/android/widget/package-summary.html) का पूरी तरह से योग्य नाम है, जैसे टेक्स्ट फील्ड के लिए `android.widget.EditText`। एक पूर्ण संदर्भ [यहां](https://developer.android.com/reference/android/widget/package-summary.html) मिल सकता है।
- Youi.tv के लिए यह Youi.tv क्लास का पूरा नाम है, और `CYI-` से शुरू होगा, जैसे पुश बटन तत्व के लिए `CYIPushButtonView`। एक पूर्ण संदर्भ [You.i Engine Driver के GitHub पेज](https://github.com/YOU-i-Labs/appium-youiengine-driver) पर मिल सकता है

```js
// iOS उदाहरण
await $('UIATextField').click()
// Android उदाहरण
await $('android.widget.DatePicker').click()
// Youi.tv उदाहरण
await $('CYIPushButtonView').click()
```

## चेन सेलेक्टर्स

यदि आप अपनी क्वेरी में अधिक विशिष्ट होना चाहते हैं, तो आप सही तत्व पाने तक सेलेक्टर्स को चेन कर सकते हैं। यदि आप अपने वास्तविक कमांड से पहले `element` कॉल करते हैं, तो WebdriverIO उस तत्व से क्वेरी शुरू करता है।

उदाहरण के लिए, यदि आपके पास इस तरह की DOM संरचना है:

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

और आप प्रोडक्ट B को कार्ट में जोड़ना चाहते हैं, तो केवल CSS सेलेक्टर का उपयोग करके ऐसा करना मुश्किल होगा।

सेलेक्टर चेनिंग के साथ, यह बहुत आसान है। बस चरण दर चरण वांछित तत्व को संकुचित करें:

```js
await $('.row .entry:nth-child(2)').$('button*=Add').click()
```

### Appium इमेज सेलेक्टर

`-image` लोकेटर रणनीति का उपयोग करके, Appium को एक इमेज फाइल भेजना संभव है जो उस तत्व का प्रतिनिधित्व करती है जिसे आप एक्सेस करना चाहते हैं।

समर्थित फाइल प्रारूप `jpg,png,gif,bmp,svg`

पूर्ण संदर्भ [यहां](https://github.com/appium/appium/blob/master/packages/images-plugin/docs/find-by-image.md) पाया जा सकता है

```js
const elem = await $('./file/path/of/image/test.jpg')
await elem.click()
```

**नोट**: Appium इस सेलेक्टर के साथ जिस तरह से काम करता है, वह यह है कि यह आंतरिक रूप से एक (ऐप)स्क्रीनशॉट लेगा और प्रदान किए गए इमेज सेलेक्टर का उपयोग यह सत्यापित करने के लिए करेगा कि क्या तत्व उस (ऐप)स्क्रीनशॉट में पाया जा सकता है।

इस बात से अवगत रहें कि Appium लिए गए (ऐप)स्क्रीनशॉट को आकार बदल सकता है ताकि वह आपके (ऐप)स्क्रीन के CSS-आकार से मेल खाए (यह आईफोन पर होगा, लेकिन मैक मशीनों पर भी जिनमें रेटिना डिस्प्ले होती है क्योंकि DPR 1 से बड़ा होता है)। इसके परिणामस्वरूप मेल नहीं मिलेगा क्योंकि प्रदान किया गया इमेज सेलेक्टर मूल स्क्रीनशॉट से लिया गया हो सकता है।
आप इसे Appium सर्वर सेटिंग्स को अपडेट करके ठीक कर सकते हैं, सेटिंग्स के लिए [Appium डॉक्स](https://github.com/appium/appium/blob/master/packages/images-plugin/docs/find-by-image.md#related-settings) और विस्तृत स्पष्टीकरण के लिए [यह टिप्पणी](https://github.com/webdriverio/webdriverio/issues/6097#issuecomment-726675579) देखें।

## रिएक्ट सेलेक्टर्स

WebdriverIO कंपोनेंट नाम के आधार पर रिएक्ट कंपोनेंट्स को चुनने का एक तरीका प्रदान करता है। इसके लिए, आपके पास दो कमांड का विकल्प है: `react$` और `react$$`।

ये कमांड आपको [React VirtualDOM](https://reactjs.org/docs/faq-internals.html) से कंपोनेंट्स को चुनने और या तो एक WebdriverIO तत्व या तत्वों की एक सरणी (किस फंक्शन का उपयोग किया जाता है, उस पर निर्भर करता है) वापस करने की अनुमति देते हैं।

**नोट**: कमांड `react$` और `react$$` फंक्शनैलिटी में समान हैं, सिवाय इसके कि `react$$` WebdriverIO तत्वों की एक सरणी के रूप में *सभी* मिलान होने वाले इंस्टेंस वापस करेगा, और `react$` पहला पाया गया इंस्टेंस वापस करेगा।

#### बेसिक उदाहरण

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

उपरोक्त कोड में एप्लिकेशन के अंदर एक सरल `MyComponent` इंस्टेंस है, जिसे React `id="root"` वाले HTML तत्व के अंदर रेंडर कर रहा है।

`browser.react$` कमांड के साथ, आप `MyComponent` का एक इंस्टेंस चुन सकते हैं:

```js
const myCmp = await browser.react$('MyComponent')
```

अब जब आपके पास WebdriverIO तत्व `myCmp` वेरिएबल में स्टोर है, तो आप इसके खिलाफ तत्व कमांड निष्पादित कर सकते हैं।

#### कंपोनेंट्स को फिल्टर करना

WebdriverIO द्वारा आंतरिक रूप से उपयोग की जाने वाली लाइब्रेरी कंपोनेंट के प्रॉप्स और/या स्टेट द्वारा आपके चयन को फिल्टर करने की अनुमति देती है। ऐसा करने के लिए, आपको ब्राउज़र कमांड के लिए प्रॉप्स के लिए दूसरा आर्गुमेंट और/या स्टेट के लिए तीसरा आर्गुमेंट पास करने की आवश्यकता है।

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

यदि आप `MyComponent` के उस इंस्टेंस को चुनना चाहते हैं, जिसमें `WebdriverIO` के रूप में `name` प्रॉप है, तो आप कमांड को इस तरह निष्पादित कर सकते हैं:

```js
const myCmp = await browser.react$('MyComponent', {
    props: { name: 'WebdriverIO' }
})
```

यदि आप स्टेट द्वारा अपने चयन को फिल्टर करना चाहते थे, तो `browser` कमांड कुछ इस तरह दिखेगा:

```js
const myCmp = await browser.react$('MyComponent', {
    state: { myState: 'some value' }
})
```

#### `React.Fragment` के साथ काम करना

जब React [फ्रैग्मेंट्स](https://reactjs.org/docs/fragments.html) का चयन करने के लिए `react$` कमांड का उपयोग करते हैं, तो WebdriverIO उस कंपोनेंट के पहले चाइल्ड को कंपोनेंट के नोड के रूप में वापस करेगा। यदि आप `react$$` का उपयोग करते हैं, तो आपको सेलेक्टर से मेल खाने वाले फ्रैग्मेंट्स के अंदर सभी HTML नोड्स युक्त एक सरणी प्राप्त होगी।

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

उपरोक्त उदाहरण के दिए गए, कमांड इस प्रकार काम करेंगे:

```js
await browser.react$('MyComponent') // पहले <div /> के लिए WebdriverIO तत्व वापस करता है
await browser.react$$('MyComponent') // सरणी [<div />, <div />] के लिए WebdriverIO तत्वों को वापस करता है
```

**नोट:** यदि आपके पास `MyComponent` के कई इंस्टेंस हैं और आप इन फ्रैग्मेंट कंपोनेंट्स का चयन करने के लिए `react$$` का उपयोग करते हैं, तो आपको सभी नोड्स की एक-आयामी सरणी वापस मिलेगी। दूसरे शब्दों में, यदि आपके पास 3 `<MyComponent />` इंस्टेंस हैं, तो आपको छह WebdriverIO तत्वों की एक सरणी वापस मिलेगी।

## कस्टम सेलेक्टर रणनीतियां


यदि आपके ऐप को तत्वों को फेच करने के लिए एक विशिष्ट तरीके की आवश्यकता है, तो आप खुद एक कस्टम सेलेक्टर रणनीति परिभाषित कर सकते हैं जिसे आप `custom$` और `custom$$` के साथ उपयोग कर सकते हैं। इसके लिए अपनी रणनीति को टेस्ट की शुरुआत में एक बार रजिस्टर करें, जैसे `before` हुक में:

```js reference
https://github.com/webdriverio/example-recipes/blob/f5730428ec3605e856e90bf58be17c9c9da891de/queryElements/customStrategy.js#L2-L11
```

निम्नलिखित HTML स्निपेट दिए गए:

```html reference
https://github.com/webdriverio/example-recipes/blob/f5730428ec3605e856e90bf58be17c9c9da891de/queryElements/example.html#L8-L12
```

फिर इसका उपयोग इस प्रकार करें:

```js reference
https://github.com/webdriverio/example-recipes/blob/f5730428ec3605e856e90bf58be17c9c9da891de/queryElements/customStrategy.js#L16-L19
```

**नोट:** यह केवल वेब वातावरण में काम करता है जिसमें [`execute`](/docs/api/browser/execute) कमांड चलाया जा सकता है।