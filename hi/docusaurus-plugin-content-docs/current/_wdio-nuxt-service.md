---
id: wdio-nuxt-service
title: नक्स्ट सर्विस सर्विस
custom_edit_url: https://github.com/webdriverio-community/wdio-nuxt-service/edit/main/README.md
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

> wdio-nuxt-service एक तृतीय पक्ष पैकेज है, अधिक जानकारी के लिए कृपया देखें [GitHub](https://github.com/webdriverio-community/wdio-nuxt-service) | [npm](https://www.npmjs.com/package/wdio-nuxt-service)

यह सेवा आपके एप्लिकेशन को लॉन्च करने में मदद करती है जब आप [Nuxt](https://nuxt.com/) का उपयोग बिल्ड टूल के रूप में करते हैं। यह परीक्षण शुरू करने से पहले आपके `nuxt.conf.js` का उपयोग करके स्वचालित रूप से Nuxt सर्वर शुरू करता है।

## इंस्टालेशन

यदि आप WebdriverIO के साथ शुरुआत कर रहे हैं तो आप सब कुछ सेट करने के लिए कॉन्फ़िगरेशन विज़ार्ड का उपयोग कर सकते हैं:

```sh
npm init wdio@latest .
```

यह आपके प्रोजेक्ट को Nuxt प्रोजेक्ट के रूप में पहचानेगा और आपके लिए सभी आवश्यक प्लगइन्स इंस्टॉल करेगा। यदि आप इस सेवा को मौजूदा सेटअप पर जोड़ रहे हैं, तो आप इसे हमेशा इस प्रकार इंस्टॉल कर सकते हैं:

```bash
npm install wdio-nuxt-service --save-dev
```

## कॉन्फ़िगरेशन

सेवा को सक्षम करने के लिए, बस इसे अपने `wdio.conf.js` फ़ाइल में अपनी `services` सूची में जोड़ें, उदाहरण के लिए:

```js
// wdio.conf.js
export const config = {
    // ...
    services: ['nuxt'],
    // ...
};
```

आप एक कॉन्फ़िग ऑब्जेक्ट के साथ एक ऐरे पास करके सेवा विकल्प लागू कर सकते हैं, उदाहरण के लिए:

```js
// wdio.conf.js
export const config = {
    // ...
    services: [
        ['nuxt', {
            rootDir: './packages/nuxt'
        }]
    ],
    // ...
};
```

## उपयोग

यदि आपका कॉन्फ़िग तदनुसार सेट किया गया है, तो सेवा [`baseUrl`](https://webdriver.io/docs/configuration#baseurl) विकल्प को आपके एप्लिकेशन की ओर इंगित करने के लिए सेट करेगी। आप [`url`](https://webdriver.io/docs/api/browser/url) कमांड के माध्यम से इस पर नेविगेट कर सकते हैं, उदाहरण के लिए:

```ts
await browser.url('/')
await expect(browser).toHaveTitle('Welcome to Nuxt!')
await expect($('aria/Welcome to Nuxt!')).toBePresent()
```

## विकल्प

### `rootDir`

प्रोजेक्ट की रूट डायरेक्टरी।

प्रकार: `string`<br />
डिफ़ॉल्ट: `process.cwd()`

### `dotenv`

सर्वर शुरू होने से पहले लोड किया जाने वाला एन्वायरनमेंट फ़ाइल।

प्रकार: `string`<br />
डिफ़ॉल्ट: `.env`

### `hostname`

सर्वर शुरू करने के लिए होस्टनेम।

प्रकार: `string`<br />
डिफ़ॉल्ट: `localhost`

### `port`

सर्वर शुरू करने के लिए पोर्ट।

प्रकार: `number`<br />
डिफ़ॉल्ट: `process.env.NUXT_PORT || config.devServer.port`

### `https`

यदि टेस्ट सर्वर https पर शुरू किया जाना चाहिए तो true पर सेट करें (सर्टिफिकेट्स को Nuxt कॉन्फ़िग में कॉन्फ़िगर किया जाना चाहिए)।

प्रकार: `boolean`<br />
डिफ़ॉल्ट: `false`

### `sslCert`

https पर सर्वर शुरू करने के लिए उपयोग किया जाने वाला SSL सर्टिफिकेट।

प्रकार: `string`

### `sslKey`

https पर सर्वर शुरू करने के लिए उपयोग की जाने वाली SSL कुंजी।

प्रकार: `string`

----

WebdriverIO पर अधिक जानकारी के लिए [होमपेज](https://webdriver.io) देखें।