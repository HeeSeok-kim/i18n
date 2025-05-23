---
id: ocr-testing
title: ओसीआर टेस्टिंग
---

मोबाइल नेटिव ऐप्स और डेस्कटॉप साइट्स पर स्वचालित परीक्षण (ऑटोमेटेड टेस्टिंग) विशेष रूप से चुनौतीपूर्ण हो सकता है जब ऐसे तत्वों से निपटना हो जिनमें अद्वितीय पहचानकर्ता न हों। मानक [WebdriverIO सिलेक्टर्स](https://webdriver.io/docs/selectors) हमेशा आपकी मदद नहीं कर सकते। `@wdio/ocr-service` की दुनिया में प्रवेश करें, एक शक्तिशाली सेवा जो OCR ([ऑप्टिकल कैरेक्टर रिकग्निशन](https://en.wikipedia.org/wiki/Optical_character_recognition)) का उपयोग करके स्क्रीन पर मौजूद तत्वों को उनके **दिखाई देने वाले टेक्स्ट** के आधार पर खोजने, प्रतीक्षा करने और उनसे इंटरैक्ट करने के लिए करती है।

निम्नलिखित कस्टम कमांड्स प्रदान की जाएंगी और `browser/driver` ऑब्जेक्ट में जोड़ी जाएंगी ताकि आपको अपना काम करने के लिए सही टूलसेट मिल सके।

-   [`await browser.ocrGetText`](./ocr-get-text.md)
-   [`await browser.ocrGetElementPositionByText`](./ocr-get-element-position-by-text.md)
-   [`await browser.ocrWaitForTextDisplayed`](./ocr-wait-for-text-displayed.md)
-   [`await browser.ocrClickOnText`](./ocr-click-on-text.md)
-   [`await browser.ocrSetValue`](./ocr-set-value.md)

### यह कैसे काम करता है

यह सेवा:

1. आपके स्क्रीन/डिवाइस का स्क्रीनशॉट बनाएगी। (यदि आवश्यक हो तो आप एक हेस्टैक प्रदान कर सकते हैं, जो एक तत्व या एक आयत वस्तु हो सकती है, जिससे किसी विशिष्ट क्षेत्र को इंगित किया जा सके। प्रत्येक कमांड के लिए दस्तावेज़ीकरण देखें।)
1. OCR के लिए परिणाम को अनुकूलित करेगी, स्क्रीनशॉट को उच्च कंट्रास्ट वाले काले/सफेद स्क्रीनशॉट में बदलकर (उच्च कंट्रास्ट की आवश्यकता बहुत अधिक इमेज बैकग्राउंड नॉइज़ को रोकने के लिए है। इसे प्रति कमांड अनुकूलित किया जा सकता है।)
1. [Tesseract.js](https://github.com/naptha/tesseract.js)/[Tesseract](https://github.com/tesseract-ocr/tesseract) से [ऑप्टिकल कैरेक्टर रिकग्निशन](https://en.wikipedia.org/wiki/Optical_character_recognition) का उपयोग करके स्क्रीन से सभी टेक्स्ट प्राप्त करेगी और इमेज पर सभी मिले हुए टेक्स्ट को हाइलाइट करेगी। यह कई भाषाओं का समर्थन कर सकती है जिन्हें [यहां](https://tesseract-ocr.github.io/tessdoc/Data-Files-in-different-versions.html) पाया जा सकता है।
1. [Fuse.js](https://fusejs.io/) से फज़ी लॉजिक का उपयोग करके ऐसे स्ट्रिंग्स को खोजेगी जो दिए गए पैटर्न के _लगभग समान_ हों (बिल्कुल समान होने के बजाय)। इसका मतलब है कि उदाहरण के लिए, खोज मूल्य `Username` टेक्स्ट `Usename` को भी खोज सकता है या इसके विपरीत।
1. आपके टर्मिनल के माध्यम से अपनी इमेजेस को वैलिडेट करने और टेक्स्ट प्राप्त करने के लिए एक CLI विज़ार्ड (`npx ocr-service`) प्रदान करेगी

चरण 1, 2 और 3 का एक उदाहरण इस इमेज में पाया जा सकता है

![प्रोसेसिंग स्टेप्स](/img/ocr/processing-steps.jpg)

यह **शून्य** सिस्टम निर्भरताओं (WebdriverIO द्वारा उपयोग किए जाने वाले के अलावा) के साथ काम करता है, लेकिन यदि आवश्यक हो तो यह [Tesseract](https://tesseract-ocr.github.io/tessdoc/) के स्थानीय इंस्टॉलेशन के साथ भी काम कर सकता है जो निष्पादन समय को नाटकीय रूप से कम कर देगा! (अपने परीक्षणों को तेज़ करने के तरीके के लिए [टेस्ट एक्जीक्यूशन ऑप्टिमाइजेशन](#test-execution-optimization) भी देखें।)

उत्साहित हैं? [गेटिंग स्टार्टेड](./getting-started) गाइड का पालन करके आज ही इसका उपयोग शुरू करें।

:::caution महत्वपूर्ण
कई कारण हो सकते हैं जिनसे आपको Tesseract से अच्छी गुणवत्ता वाला आउटपुट नहीं मिल सकता। एक बड़ा कारण जो आपके ऐप और इस मॉड्यूल से संबंधित हो सकता है, वह यह है कि खोजे जाने वाले टेक्स्ट और बैकग्राउंड के बीच उचित रंग अंतर नहीं है। उदाहरण के लिए, डार्क बैकग्राउंड पर सफेद टेक्स्ट _आसानी से_ पाया जा सकता है, लेकिन सफेद बैकग्राउंड पर हल्के टेक्स्ट या डार्क बैकग्राउंड पर डार्क टेक्स्ट शायद ही पाया जा सकता है।

Tesseract से अधिक जानकारी के लिए [इस पेज](https://tesseract-ocr.github.io/tessdoc/ImproveQuality) को भी देखें।

[FAQ](./ocr-faq) पढ़ना भी न भूलें।
:::