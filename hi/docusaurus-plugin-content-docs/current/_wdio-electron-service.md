---
id: wdio-electron-service
title: इलेक्ट्रॉन सर्विस
custom_edit_url: https://github.com/webdriverio-community/wdio-electron-service/edit/main/README.md
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

> wdio-electron-service एक तृतीय पक्ष पैकेज है, अधिक जानकारी के लिए कृपया देखें [GitHub](https://github.com/webdriverio-community/wdio-electron-service) | [npm](https://www.npmjs.com/package/wdio-electron-service)

<a href="https://www.npmjs.com/package/wdio-electron-service" alt="NPM Version">
  <img src="https://img.shields.io/npm/v/wdio-electron-service" /></a>
<a href="https://www.npmjs.com/package/wdio-electron-service/v/lts" alt="NPM LTS Version">
  <img src="https://img.shields.io/npm/v/wdio-electron-service/lts" /></a>
<a href="https://www.npmjs.com/package/wdio-electron-service/v/next" alt="NPM Next Version">
  <img src="https://img.shields.io/npm/v/wdio-electron-service/next" /></a>
<a href="https://www.npmjs.com/package/wdio-electron-service" alt="NPM Downloads">
  <img src="https://img.shields.io/npm/dw/wdio-electron-service" /></a>

<br />

**इलेक्ट्रॉन एप्लिकेशन के परीक्षण के लिए WebdriverIO सेवा**

WebdriverIO इकोसिस्टम के माध्यम से इलेक्ट्रॉन ऐप्स के क्रॉस-प्लेटफॉर्म E2E परीक्षण को सक्षम करता है।

[Spectron](https://github.com/electron-userland/spectron) का आध्यात्मिक उत्तराधिकारी ([RIP](https://github.com/electron-userland/spectron/issues/1045))।

### विशेषताएँ

इलेक्ट्रॉन एप्लिकेशन के परीक्षण को बहुत आसान बनाता है:

- 🚗 आवश्यक Chromedriver का ऑटो-सेटअप (इलेक्ट्रॉन v26 और उससे ऊपर के लिए)
- 📦 आपके इलेक्ट्रॉन एप्लिकेशन की स्वचालित पथ पहचान
  - [Electron Forge](https://www.electronforge.io/), [Electron Builder](https://www.electron.build/) और अनपैकेज्ड ऐप्स का समर्थन करता है
- 🧩 अपने परीक्षणों में इलेक्ट्रॉन APIs का उपयोग करें
- 🕵️ Vitest जैसे API के माध्यम से इलेक्ट्रॉन APIs की मॉकिंग

## इंस्टालेशन

आपको `WebdriverIO` इंस्टॉल करने की आवश्यकता होगी, निर्देश [यहां](https://webdriver.io/docs/gettingstarted) पाए जा सकते हैं।

## क्विक स्टार्ट

जल्दी से शुरू करने और चलाने का अनुशंसित तरीका [WDIO कॉन्फिगरेशन विज़ार्ड](https://webdriver.io/docs/gettingstarted#initiate-a-webdriverio-setup) का उपयोग करना है।

### मैनुअल क्विक स्टार्ट

कॉन्फिगरेशन विज़ार्ड का उपयोग किए बिना शुरू करने के लिए, आपको सेवा और `@wdio/cli` को इंस्टॉल करने की आवश्यकता होगी:

```bash
npm install --dev @wdio/cli wdio-electron-service
```

या अपनी पसंद के पैकेज मैनेजर का उपयोग करें - pnpm, yarn, आदि।

अगला, अपनी WDIO कॉन्फिगरेशन फाइल बनाएं। यदि आपको इसके लिए कुछ प्रेरणा की आवश्यकता है, तो इस रिपॉजिटरी के [example directory](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./example/wdio.conf.ts) में एक कार्यशील कॉन्फिगरेशन है, साथ ही [WDIO कॉन्फिगरेशन रेफरेंस पेज](https://webdriver.io/docs/configuration) भी है।

आपको अपनी सेवाओं की सरणी में `electron` जोड़ने और एक इलेक्ट्रॉन क्षमता सेट करने की आवश्यकता होगी, उदाहरण के लिए:

_`wdio.conf.ts`_

```ts
export const config = {
  // ...
  services: ['electron'],
  capabilities: [
    {
      browserName: 'electron',
    },
  ],
  // ...
};
```

अंत में, अपनी कॉन्फिगरेशन फाइल का उपयोग करके [कुछ परीक्षण चलाएं](https://webdriver.io/docs/gettingstarted#run-test)।

यह आपके ऐप का एक इंस्टेंस उसी तरह से शुरू करेगा जिस तरह WDIO Chrome या Firefox जैसे ब्राउज़रों को संभालता है। यह सेवा [WDIO (समानांतर) मल्टीरिमोट](https://webdriver.io/docs/multiremote) के साथ काम करती है यदि आपको एक साथ अतिरिक्त इंस्टेंस चलाने की आवश्यकता है, जैसे आपके ऐप के कई इंस्टेंस या आपके ऐप और वेब ब्राउज़र के विभिन्न संयोजन।

यदि आप अपने ऐप को पैकेज करने के लिए [Electron Forge](https://www.electronforge.io/) या [Electron Builder](https://www.electron.build/) का उपयोग करते हैं, तो सेवा स्वचालित रूप से आपके बंडल इलेक्ट्रॉन एप्लिकेशन के पथ को खोजने का प्रयास करेगी। आप कस्टम सर्विस कैपेबिलिटीज के माध्यम से बाइनरी के लिए एक कस्टम पथ प्रदान कर सकते हैं, उदाहरण के लिए:

_`wdio.conf.ts`_

```ts
export const config = {
  // ...
  capabilities: [
    {
      'browserName': 'electron',
      'wdio:electronServiceOptions': {
        appBinaryPath: './path/to/built/electron/app.exe',
        appArgs: ['foo', 'bar=baz'],
      },
    },
  ],
  // ...
};
```

इलेक्ट्रॉन द्वारा समर्थित विभिन्न ऑपरेटिंग सिस्टम के लिए अपने `appBinaryPath` मान को कैसे खोजें, इसके लिए [कॉन्फिगरेशन डॉक](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/configuration/service-configuration.md#appbinarypath) देखें।

वैकल्पिक रूप से, आप `main.js` स्क्रिप्ट के पथ को प्रदान करके अनपैकेज्ड ऐप पर सेवा को निर्देशित कर सकते हैं। इलेक्ट्रॉन को आपके `node_modules` में इंस्टॉल करने की आवश्यकता होगी। Rollup, Parcel, Webpack आदि जैसे बंडलर का उपयोग करके अनपैकेज्ड ऐप्स को बंडल करने की सिफारिश की जाती है।

_`wdio.conf.ts`_

```ts
export const config = {
  // ...
  capabilities: [
    {
      'browserName': 'electron',
      'wdio:electronServiceOptions': {
        appEntryPoint: './path/to/bundled/electron/main.bundle.js',
        appArgs: ['foo', 'bar=baz'],
      },
    },
  ],
  // ...
};
```

## Chromedriver कॉन्फिगरेशन

**यदि आपका ऐप इलेक्ट्रॉन के ऐसे संस्करण का उपयोग करता है जो v26 से कम है, तो आपको [मैन्युअल रूप से Chromedriver कॉन्फिगर](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/configuration/chromedriver-configuration.md#user-managed) करने की आवश्यकता होगी।**

ऐसा इसलिए है क्योंकि WDIO Chromedriver डाउनलोड करने के लिए Chrome for Testing का उपयोग करता है, जो केवल v115 या उससे नए Chromedriver संस्करण प्रदान करता है।

## दस्तावेज़ीकरण

**[सर्विस कॉन्फिगरेशन](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/configuration/service-configuration.md)** \
**[Chromedriver कॉन्फिगरेशन](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/configuration/chromedriver-configuration.md)** \
**[इलेक्ट्रॉन APIs तक पहुँचना](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/electron-apis/accessing-apis.md)** \
**[इलेक्ट्रॉन APIs की मॉकिंग](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/electron-apis/mocking-apis.md)** \
**[विंडो प्रबंधन](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/window-management.md)** \
**[स्टैंडअलोन मोड](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/standalone-mode.md)** \
**[डेवलपमेंट](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/development.md)** \
**[सामान्य समस्याएँ और डिबगिंग](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/common-issues-debugging.md)**

## डेवलपमेंट

यदि आप योगदान करने में रुचि रखते हैं तो [डेवलपमेंट डॉक](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/development.md) पढ़ें।

## उदाहरण एकीकरण

हमारा [इलेक्ट्रॉन बॉयलरप्लेट](https://github.com/webdriverio/electron-boilerplate) प्रोजेक्ट देखें जो दिखाता है कि उदाहरण एप्लिकेशन में WebdriverIO को कैसे एकीकृत किया जाए। आप इस रिपॉजिटरी में [Example Apps](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./apps/) और [E2Es](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./e2e/) डायरेक्टरी भी देख सकते हैं।

## सहायता

यदि आपको सेवा के साथ WDIO चलाने में समस्याएँ आ रही हैं, तो आपको पहले प्रलेखित [सामान्य समस्याओं](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/common-issues.md) की जाँच करनी चाहिए, फिर [मुख्य WDIO फोरम](https://github.com/webdriverio/webdriverio/discussions) में एक चर्चा शुरू करें।

इलेक्ट्रॉन सेवा चर्चा फोरम WDIO वाले की तुलना में बहुत कम सक्रिय है, लेकिन अगर आप जो समस्या अनुभव कर रहे हैं वह इलेक्ट्रॉन या सेवा का उपयोग करने के लिए विशिष्ट है, तो आप [यहां](https://github.com/webdriverio-community/wdio-electron-service/discussions) चर्चा शुरू कर सकते हैं।