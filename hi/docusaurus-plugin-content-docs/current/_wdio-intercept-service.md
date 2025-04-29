---
id: wdio-intercept-service
title: इंटरसेप्ट सर्विस
custom_edit_url: https://github.com/webdriverio-community/wdio-intercept-service/edit/main/README.md
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

> wdio-intercept-service एक तृतीय पक्ष पैकेज है, अधिक जानकारी के लिए कृपया देखें [GitHub](https://github.com/webdriverio-community/wdio-intercept-service) | [npm](https://www.npmjs.com/package/wdio-intercept-service)

🕸 [webdriver.io](http://webdriver.io/) में HTTP ajax कॉल्स को कैप्चर और असर्ट करें

[![Tests](https://github.com/webdriverio-community/wdio-intercept-service/actions/workflows/test.yaml/badge.svg)](https://github.com/webdriverio-community/wdio-intercept-service/actions/workflows/test.yaml) [![Join the chat on Discord](https://img.shields.io/discord/1097401827202445382?logo=discord&logoColor=FFFFFF&color=5865F2)](https://discord.webdriver.io/)

यह [webdriver.io](http://webdriver.io/) के लिए एक प्लगइन है। अगर आप इसे अभी तक नहीं जानते हैं, तो इसे चेक करें, यह काफी अच्छा है।

हालांकि selenium और webdriver का उपयोग e2e और विशेष रूप से UI टेस्टिंग के लिए किया जाता है, आप अपने क्लाइंट कोड द्वारा किए गए HTTP अनुरोधों का आकलन करना चाह सकते हैं (जैसे जब आपके पास तत्काल UI फीडबैक नहीं होता है, जैसे मेट्रिक्स या ट्रैकिंग कॉल्स में)। wdio-intercept-service के साथ आप किसी उपयोगकर्ता क्रिया से शुरू किए गए ajax HTTP कॉल्स (जैसे बटन प्रेस, आदि) को इंटरसेप्ट कर सकते हैं और बाद में अनुरोध और संबंधित प्रतिक्रियाओं के बारे में दावे कर सकते हैं।

हालांकि एक बात ध्यान देने योग्य है: आप उन HTTP कॉल्स को इंटरसेप्ट नहीं कर सकते जो पेज लोड होने पर शुरू होते हैं (जैसे अधिकांश SPA में), क्योंकि इसके लिए कुछ सेटअप कार्य करना आवश्यक है जो केवल पेज लोड होने के बाद ही किया जा सकता है (selenium में सीमाओं के कारण)। **इसका मतलब है कि आप केवल उन अनुरोधों को कैप्चर कर सकते हैं जो परीक्षण के अंदर शुरू किए गए थे।** अगर आप इससे ठीक हैं, तो यह प्लगइन आपके लिए हो सकता है, इसलिए पढ़ते रहें।

## आवश्यकताएँ

* webdriver.io **v5.x** या उससे नया।

**ध्यान दें! अगर आप अभी भी webdriver.io v4 का उपयोग कर रहे हैं, तो कृपया इस प्लगइन की v2.x शाखा का उपयोग करें!**

## इंस्टालेशन

```shell
npm install wdio-intercept-service -D
```

## उपयोग

### WebDriver CLI के साथ उपयोग

यह आपके `wdio.conf.js` में wdio-intercept-service को जोड़ने के रूप में आसान होना चाहिए:

```javascript
exports.config = {
  // ...
  services: ['intercept']
  // ...
};
```

और आप सेट हो गए हैं।

### WebDriver Standalone के साथ उपयोग

WebdriverIO Standalone का उपयोग करते समय, `before` और `beforeTest` / `beforeScenario` फंक्शन को मैन्युअल रूप से कॉल करने की आवश्यकता होती है।

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

एक बार इनीशियलाइज़ होने के बाद, कुछ संबंधित फंक्शन आपकी ब्राउज़र कमांड चेन में जोड़े जाते हैं (देखें [API](#api))।

## क्विकस्टार्ट

उपयोग का उदाहरण:

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

अनुरोधों के बारे में विवरण प्राप्त करें:

```javascript
browser.url('http://foo.bar')
browser.setupInterceptor();
browser.click('#button')
browser.pause(1000);

var request = browser.getRequest(0);
assert.equal(request.method, 'GET');
assert.equal(request.response.headers['content-length'], '42');
```

## समर्थित ब्राउज़र

यह सभी ब्राउज़रों के कुछ नए संस्करणों के साथ काम करना चाहिए। यदि यह आपके साथ काम नहीं करता है तो कृपया एक मुद्दा रिपोर्ट करें।

## API

पूर्ण सिंटैक्स के लिए TypeScript डिक्लेरेशन फाइल देखें जो WebdriverIO ब्राउज़र ऑब्जेक्ट में जोड़े गए कस्टम कमांड्स के लिए है। सामान्य तौर पर, कोई भी विधि जो पैरामीटर के रूप में "options" ऑब्जेक्ट लेती है, उसे डिफ़ॉल्ट व्यवहार प्राप्त करने के लिए उस पैरामीटर के बिना कॉल किया जा सकता है। इन "वैकल्पिक विकल्प" ऑब्जेक्ट्स के बाद `?: = {}` होता है और प्रत्येक विधि के लिए अनुमानित डिफ़ॉल्ट मान का वर्णन किया गया है।

### विकल्प विवरण

यह लाइब्रेरी कमांड जारी करते समय थोड़ी सी कॉन्फ़िगरेशन प्रदान करती है। कई विधियों द्वारा उपयोग किए जाने वाले कॉन्फ़िगरेशन विकल्पों का यहां वर्णन किया गया है (प्रत्येक विधि परिभाषा देखें ताकि विशिष्ट समर्थन निर्धारित किया जा सके)।

* `orderBy` (`'START' | 'END'`): यह विकल्प इंटरसेप्टर द्वारा कैप्चर किए गए अनुरोधों के क्रम को नियंत्रित करता है, जब आपके परीक्षण को वापस किया जाता है। इस लाइब्रेरी के मौजूदा संस्करणों के साथ बैकवर्ड कंपैटिबिलिटी के लिए, डिफ़ॉल्ट क्रम `'END'` है, जो उस समय से मेल खाता है जब अनुरोध पूरा किया गया था। यदि आप `orderBy` विकल्प को `'START'` पर सेट करते हैं, तो अनुरोधों को उस समय के अनुसार क्रमबद्ध किया जाएगा जब वे शुरू किए गए थे।
* `includePending` (`boolean`): यह विकल्प नियंत्रित करता है कि क्या अभी तक पूरा नहीं हुए अनुरोधों को वापस किया जाएगा। इस लाइब्रेरी के मौजूदा संस्करणों के साथ बैकवर्ड कंपैटिबिलिटी के लिए, डिफ़ॉल्ट मान `false` है, और केवल पूर्ण अनुरोधों को वापस किया जाएगा।

### browser.setupInterceptor()

ब्राउज़र में ajax कॉल्स को कैप्चर करता है। बाद में अनुरोधों का आकलन करने के लिए आपको हमेशा सेटअप फंक्शन को कॉल करना होगा।

### browser.disableInterceptor()

ब्राउज़र में ajax कॉल्स के आगे कैप्चर को रोकता है। सभी कैप्चर किए गए अनुरोध की जानकारी हटा दी जाती है। अधिकांश उपयोगकर्ताओं को इंटरसेप्टर को अक्षम करने की आवश्यकता नहीं होगी, लेकिन यदि कोई परीक्षण विशेष रूप से लंबे समय तक चलता है या सत्र स्टोरेज क्षमता से अधिक हो जाता है, तो इंटरसेप्टर को अक्षम करना सहायक हो सकता है।

### `browser.excludeUrls(urlRegexes: (string | RegExp)[])`

कुछ URL से अनुरोधों को रिकॉर्ड किए जाने से बाहर रखता है। यह स्ट्रिंग्स या रेगुलर एक्सप्रेशन्स की एक सरणी लेता है। स्टोरेज में लिखने से पहले,
अनुरोध के URL को प्रत्येक स्ट्रिंग या रेगेक्स के खिलाफ जांचता है। यदि ऐसा होता है, तो अनुरोध को स्टोरेज में नहीं लिखा जाता है। disableInterceptor की तरह, यह 
सत्र स्टोरेज क्षमता से अधिक होने की समस्याओं के साथ चलने पर सहायक हो सकता है।

### browser.expectRequest(method: string, url: string, statusCode: number)

परीक्षण के दौरान शुरू होने वाले ajax अनुरोधों के बारे में अपेक्षाएँ बनाएँ। चेन (और करना चाहिए) किया जा सकता है। अपेक्षाओं का क्रम किए जा रहे अनुरोधों के क्रम से मेल खाना चाहिए।

* `method` (`String`): http विधि जिसकी अपेक्षा की जाती है। कुछ भी हो सकता है जो `xhr.open()` पहले तर्क के रूप में स्वीकार करता है।
* `url` (`String`|`RegExp`): सटीक URL जो अनुरोध में स्ट्रिंग के रूप में या RegExp के रूप में मिलान करने के लिए कॉल किया जाता है
* `statusCode` (`Number`): प्रतिक्रिया का अपेक्षित स्टेटस कोड

### browser.getExpectations()

हेल्पर मेथड। उस बिंदु तक आपके द्वारा की गई सभी अपेक्षाओं को वापस करता है

### browser.resetExpectations()

हेल्पर मेथड। उस बिंदु तक आपके द्वारा की गई सभी अपेक्षाओं को रीसेट करता है

### `browser.assertRequests({ orderBy?: 'START' | 'END' }?: = {})`

जब सभी अपेक्षित ajax अनुरोध समाप्त हो जाते हैं तो इस विधि को कॉल करें। यह अपेक्षाओं की तुलना वास्तविक किए गए अनुरोधों से करता है और निम्नलिखित का दावा करता है:

- किए गए अनुरोधों की गिनती
- अनुरोधों का क्रम
- विधि, URL और स्टेटस कोड प्रत्येक अनुरोध के लिए मेल खाना चाहिए
- विकल्प ऑब्जेक्ट डिफॉल्ट रूप से `{ orderBy: 'END' }` होता है, अर्थात जब अनुरोध पूरे हो गए थे, v4.1.10 और पहले के व्यवहार के साथ संगत होने के लिए। जब `orderBy` विकल्प `'START'` पर सेट किया जाता है, तो अनुरोधों को उस समय के अनुसार क्रमबद्ध किया जाएगा जब वे पृष्ठ द्वारा शुरू किए गए थे।

### `browser.assertExpectedRequestsOnly({ inOrder?: boolean, orderBy?: 'START' | 'END' }?: = {})`

`browser.assertRequests` के समान, लेकिन केवल उन अनुरोधों को मान्य करता है जिन्हें आप अपने `expectRequest` निर्देशों में निर्दिष्ट करते हैं, उसके आसपास हो सकने वाले सभी नेटवर्क अनुरोधों को मैप करने की आवश्यकता के बिना। यदि `inOrder` विकल्प `true` (डिफॉल्ट) है, तो अनुरोधों को उसी क्रम में पाया जाने की उम्मीद की जाती है जैसे वे `expectRequest` के साथ सेटअप किए गए थे।

### `browser.getRequest(index: number, { includePending?: boolean, orderBy?: 'START' | 'END' }?: = {})`

किसी विशिष्ट अनुरोध के बारे में अधिक परिष्कृत दावे करने के लिए आप किसी विशिष्ट अनुरोध के लिए विवरण प्राप्त कर सकते हैं। आपको उस अनुरोध का 0-आधारित इंडेक्स प्रदान करना होगा जिसे आप एक्सेस करना चाहते हैं, उस क्रम में जिसमें अनुरोध पूरे किए गए थे (डिफॉल्ट), या शुरू किए गए थे (विकल्प `orderBy: 'START'` पास करके)।

* `index` (`number`): उस अनुरोध की संख्या जिसे आप एक्सेस करना चाहते हैं
* `options` (`object`): कॉन्फ़िगरेशन विकल्प
* `options.includePending` (`boolean`): क्या अभी तक पूरा न किए गए अनुरोधों को वापस किया जाना चाहिए। डिफ़ॉल्ट रूप से, यह false है, v4.1.10 और पहले के लाइब्रेरी के व्यवहार से मेल खाने के लिए।
* `options.orderBy` (`'START' | 'END'`): अनुरोधों को कैसे क्रमबद्ध किया जाना चाहिए। डिफॉल्ट रूप से, यह `'END'` है, v4.1.10 और पहले के लाइब्रेरी के व्यवहार से मेल खाने के लिए। यदि `'START'` है, तो अनुरोधों को अनुरोध पूर्णता के समय के बजाय प्रारंभ के समय के अनुसार क्रमबद्ध किया जाएगा। (चूंकि एक लंबित अनुरोध अभी तक पूरा नहीं हुआ है, जब `'END'` द्वारा क्रमित किया जाता है तो सभी लंबित अनुरोध सभी पूर्ण अनुरोधों के बाद आएंगे।)

**रिटर्न** `request` ऑब्जेक्ट:

* `request.url`: अनुरोधित URL
* `request.method`: उपयोग की गई HTTP विधि
* `request.body`: अनुरोध में उपयोग किया गया पेलोड/बॉडी डेटा
* `request.headers`: अनुरोध http हेडर्स JS ऑब्जेक्ट के रूप में
* `request.pending`: इस बात के लिए बूलियन फ्लैग कि क्या यह अनुरोध पूर्ण है (अर्थात एक `response` प्रॉपर्टी है), या इन-फ्लाइट है।
* `request.response`: एक JS ऑब्जेक्ट जो केवल तभी मौजूद होता है जब अनुरोध पूरा हो जाता है (अर्थात `request.pending === false`), जिसमें प्रतिक्रिया के बारे में डेटा होता है।
* `request.response?.headers`: प्रतिक्रिया http हेडर्स JS ऑब्जेक्ट के रूप में
* `request.response?.body`: प्रतिक्रिया बॉडी (यदि संभव हो तो JSON के रूप में पार्स किया जाएगा)
* `request.response?.statusCode`: प्रतिक्रिया स्टेटस कोड

**`request.body` पर एक नोट:** wdio-intercept-service अनुरोध बॉडी को निम्नानुसार पार्स करने का प्रयास करेगा:

* string: बस स्ट्रिंग वापस करें (`'value'`)
* JSON: `JSON.parse()` का उपयोग करके JSON ऑब्जेक्ट को पार्स करें (`({ key: value })`)
* FormData: FormData को `{ key: [value1, value2, ...] }` प्रारूप में आउटपुट करेगा
* ArrayBuffer: बफर को स्ट्रिंग में बदलने का प्रयास करेगा (प्रयोगात्मक)
* कुछ भी और: आपके डेटा पर एक क्रूर `JSON.stringify()` का उपयोग करेगा। शुभकामनाएँ!

**`fetch` API के लिए, हम केवल स्ट्रिंग और JSON डेटा का समर्थन करते हैं!**

### `browser.getRequests({ includePending?: boolean, orderBy?: 'START' | 'END' }?: = {})`

सभी कैप्चर किए गए अनुरोधों को एक सरणी के रूप में प्राप्त करें, `getRequest` के समान वैकल्पिक विकल्पों का समर्थन करता है।

**रिटर्न** `request` ऑब्जेक्ट्स की सरणी।

### browser.hasPendingRequests()

एक उपयोगिता विधि जो जांचती है कि क्या कोई HTTP अनुरोध अभी भी लंबित हैं। परीक्षणों द्वारा यह सुनिश्चित करने के लिए उपयोग किया जा सकता है कि सभी अनुरोध उचित समय के भीतर पूरे हो गए हैं, या यह सत्यापित करने के लिए कि `getRequests()` या `assertRequests()` के लिए कॉल में सभी वांछित HTTP अनुरोध शामिल होंगे।

**रिटर्न** बूलियन

## TypeScript समर्थन

यह प्लगइन अपने TS टाइप्स प्रदान करता है। बस अपने tsconfig को टाइप एक्सटेंशन की ओर इंगित करें जैसा कि [यहां](https://webdriver.io/docs/typescript.html#framework-types) उल्लेख किया गया है:

```
"compilerOptions": {
    // ..
    "types": ["node", "webdriverio", "wdio-intercept-service"]
},
```

## परीक्षण चलाना

परीक्षणों को स्थानीय रूप से चलाने के लिए Chrome और Firefox के हालिया संस्करण की आवश्यकता है। आपको अपने सिस्टम पर इंस्टॉल किए गए संस्करण से मेल खाने के लिए `chromedriver` और `geckodriver` निर्भरताओं को अपडेट करने की आवश्यकता हो सकती है।

```shell
npm test
```

## योगदान

मैं हर योगदान के लिए खुश हूं। बस एक मुद्दा खोलें या सीधे एक PR फाइल करें।  
कृपया ध्यान दें कि यह इंटरसेप्टर लाइब्रेरी इंटरनेट एक्सप्लोरर जैसे लीगेसी ब्राउज़र के साथ काम करने के लिए लिखी गई है। इस प्रकार, `lib/interceptor.js` में उपयोग किया गया कोई भी कोड कम से कम इंटरनेट एक्सप्लोरर के जावास्क्रिप्ट रनटाइम द्वारा पार्स किया जा सकता होना चाहिए।

## लाइसेंस

MIT