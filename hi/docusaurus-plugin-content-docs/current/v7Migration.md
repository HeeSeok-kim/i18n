---
id: v7-migration
title: v6 से v7 तक
---

यह ट्यूटोरियल उन लोगों के लिए है जो अभी भी WebdriverIO के `v6` का उपयोग कर रहे हैं और `v7` में माइग्रेट करना चाहते हैं। जैसा कि हमारे [रिलीज़ ब्लॉग पोस्ट](https://webdriver.io/blog/2021/02/09/webdriverio-v7-released) में उल्लेख किया गया है, परिवर्तन ज्यादातर अंदरूनी हैं और अपग्रेड करना एक सीधी प्रक्रिया होनी चाहिए।

:::info

यदि आप WebdriverIO `v5` या उससे नीचे का उपयोग कर रहे हैं, तो कृपया पहले `v6` में अपग्रेड करें। कृपया हमारे [v6 माइग्रेशन गाइड](v6-migration) को देखें।

:::

जबकि हम इसके लिए एक पूरी तरह से स्वचालित प्रक्रिया रखना चाहते हैं, वास्तविकता अलग दिखती है। हर किसी का सेटअप अलग है। हर कदम को मार्गदर्शन के रूप में देखा जाना चाहिए और कम से कम कदम-दर-कदम निर्देश के रूप में। यदि आपको माइग्रेशन के साथ समस्याएं हैं, तो [हमसे संपर्क करने](https://github.com/webdriverio/codemod/discussions/new) में संकोच न करें।

## सेटअप

अन्य माइग्रेशन के समान, हम WebdriverIO [कोडमोड](https://github.com/webdriverio/codemod) का उपयोग कर सकते हैं। इस ट्यूटोरियल के लिए हम एक समुदाय के सदस्य द्वारा जमा किए गए [बॉयलरप्लेट प्रोजेक्ट](https://github.com/WarleyGabriel/demo-webdriverio-cucumber) का उपयोग करते हैं और इसे `v6` से `v7` में पूरी तरह से माइग्रेट करते हैं।

कोडमोड इंस्टॉल करने के लिए, चलाएं:

```sh
npm install jscodeshift @wdio/codemod
```

#### कमिट्स:

- _install codemod deps_ [[6ec9e52]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/6ec9e52038f7e8cb1221753b67040b0f23a8f61a)

## WebdriverIO डिपेंडेंसीज अपग्रेड करें

चूंकि सभी WebdriverIO वर्जन एक-दूसरे से जुड़े हुए हैं, इसलिए हमेशा एक विशिष्ट टैग, जैसे `latest` पर अपग्रेड करना सबसे अच्छा है। ऐसा करने के लिए हम अपने `package.json` से सभी WebdriverIO संबंधित डिपेंडेंसीज को कॉपी करते हैं और उन्हें इस प्रकार से फिर से इंस्टॉल करते हैं:

```sh
npm i --save-dev @wdio/allure-reporter@7 @wdio/cli@7 @wdio/cucumber-framework@7 @wdio/local-runner@7 @wdio/spec-reporter@7 @wdio/sync@7 wdio-chromedriver-service@7 wdio-timeline-reporter@7 webdriverio@7
```

आमतौर पर WebdriverIO डिपेंडेंसीज डेव डिपेंडेंसीज का हिस्सा होती हैं, आपके प्रोजेक्ट के आधार पर यह अलग-अलग हो सकती हैं। इसके बाद आपका `package.json` और `package-lock.json` अपडेट हो जाना चाहिए। __नोट:__ ये डिपेंडेंसीज [उदाहरण प्रोजेक्ट](https://github.com/WarleyGabriel/demo-webdriverio-cucumber) द्वारा उपयोग की जाती हैं, आपकी अलग हो सकती हैं।

#### कमिट्स:

- _updated dependencies_ [[7097ab6]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/7097ab6297ef9f37ead0a9c2ce9fce8d0765458d)

## कॉन्फिग फाइल ट्रांसफॉर्म करें

एक अच्छा पहला कदम कॉन्फिग फाइल से शुरू करना है। WebdriverIO `v7` में हमें मैन्युअली किसी भी कंपाइलर को रजिस्टर करने की आवश्यकता नहीं है। वास्तव में उन्हें हटाने की आवश्यकता है। यह कोडमोड के साथ पूरी तरह से स्वचालित रूप से किया जा सकता है:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v7 ./wdio.conf.js
```

:::caution

कोडमोड अभी तक टाइपस्क्रिप्ट प्रोजेक्ट्स का समर्थन नहीं करता है। देखें [`@webdriverio/codemod#10`](https://github.com/webdriverio/codemod/issues/10)। हम इसका समर्थन जल्द ही लागू करने के लिए काम कर रहे हैं। यदि आप टाइपस्क्रिप्ट का उपयोग कर रहे हैं तो कृपया शामिल हों!

:::

#### कमिट्स:

- _transpile config file_ [[6015534]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/60155346a386380d8a77ae6d1107483043a43994)

## स्टेप डेफिनिशन अपडेट करें

यदि आप Jasmine या Mocha का उपयोग कर रहे हैं, तो आप यहां समाप्त कर चुके हैं। अंतिम चरण Cucumber.js इम्पोर्ट को `cucumber` से `@cucumber/cucumber` में अपडेट करना है। यह भी कोडमोड के माध्यम से स्वचालित रूप से किया जा सकता है:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v7 ./src/e2e/*
```

बस इतना ही! कोई और बदलाव आवश्यक नहीं 🎉

#### कमिट्स:

- _transpile step definitions_ [[8c97b90]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/8c97b90a8b9197c62dffe4e2954f7dad814753cc)

## निष्कर्ष

हमें आशा है कि यह ट्यूटोरियल आपको WebdriverIO `v7` में माइग्रेशन प्रक्रिया में थोड़ा मार्गदर्शन करेगा। समुदाय विभिन्न टीमों और विभिन्न संगठनों में इसका परीक्षण करते हुए कोडमोड में सुधार करना जारी रखता है। यदि आपके पास फीडबैक है तो [एक इश्यू उठाने](https://github.com/webdriverio/codemod/issues/new) में या यदि आप माइग्रेशन प्रक्रिया के दौरान संघर्ष करते हैं तो [एक चर्चा शुरू करने](https://github.com/webdriverio/codemod/discussions/new) में संकोच न करें।