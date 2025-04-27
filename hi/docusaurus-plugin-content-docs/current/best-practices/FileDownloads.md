---
id: file-download
title: फ़ाइल डाउनलोड
---

वेब टेस्टिंग में फ़ाइल डाउनलोड्स को स्वचालित करते समय, विभिन्न ब्राउज़रों में उन्हें लगातार संभालना आवश्यक है ताकि विश्वसनीय परीक्षण निष्पादन सुनिश्चित किया जा सके।

यहां, हम फ़ाइल डाउनलोड्स के लिए सर्वोत्तम प्रथाओं को प्रदान करते हैं और **Google Chrome**, **Mozilla Firefox**, और **Microsoft Edge** के लिए डाउनलोड डायरेक्टरी को कॉन्फ़िगर करने का तरीका दिखाते हैं।

## डाउनलोड पाथ

परीक्षण स्क्रिप्ट्स में डाउनलोड पाथ को **हार्डकोड** करने से रखरखाव समस्याएँ और पोर्टेबिलिटी समस्याएँ हो सकती हैं। विभिन्न वातावरणों में पोर्टेबिलिटी और संगतता सुनिश्चित करने के लिए डाउनलोड डायरेक्टरी के लिए **सापेक्ष पाथ** का उपयोग करें।

```javascript
// 👎
// Hardcoded download path
const downloadPath = '/path/to/downloads';

// 👍
// Relative download path
const downloadPath = path.join(__dirname, 'downloads');
```

## वेट स्ट्रेटेजीज

उचित वेट स्ट्रेटेजीज को लागू करने में विफलता रेस कंडीशन्स या अविश्वसनीय परीक्षणों का कारण बन सकती है, विशेष रूप से डाउनलोड पूरा होने के लिए। फ़ाइल डाउनलोड्स के पूरा होने की प्रतीक्षा करने के लिए **स्पष्ट** वेट स्ट्रेटेजीज लागू करें, जिससे परीक्षण चरणों के बीच सिंक्रनाइजेशन सुनिश्चित हो।

```javascript
// 👎
// No explicit wait for download completion
await browser.pause(5000);

// 👍
// Wait for file download completion
await waitUntil(async ()=> await fs.existsSync(downloadPath), 5000);
```

## डाउनलोड डायरेक्टरी कॉन्फ़िगर करना

**Google Chrome**, **Mozilla Firefox**, और **Microsoft Edge** के लिए फ़ाइल डाउनलोड व्यवहार को ओवरराइड करने के लिए, WebDriverIO क्षमताओं में डाउनलोड डायरेक्टरी प्रदान करें:

<Tabs
defaultValue="chrome"
values={[
{label: 'Chrome', value: 'chrome'},
{label: 'Firefox', value: 'firefox'},
{label: 'Microsoft Edge', value: 'edge'},
]
}>

<TabItem value='chrome'>

```javascript reference title="wdio.conf.js"

https://github.com/webdriverio/example-recipes/blob/84dda93011234d0b2a34ee0cfb3cdfa2a06136a5/testDownloadBehavior/wdio.conf.js#L8-L16

```

</TabItem>

<TabItem value='firefox'>

```javascript reference title="wdio.conf.js"

https://github.com/webdriverio/example-recipes/blob/84dda93011234d0b2a34ee0cfb3cdfa2a06136a5/testDownloadBehavior/wdio.conf.js#L20-L32

```

</TabItem>

<TabItem value='edge'>

```javascript reference title="wdio.conf.js"

https://github.com/webdriverio/example-recipes/blob/84dda93011234d0b2a34ee0cfb3cdfa2a06136a5/testDownloadBehavior/wdio.conf.js#L36-L44

```

</TabItem>

</Tabs>

एक उदाहरण कार्यान्वयन के लिए, [WebdriverIO Test Download Behavior Recipe](https://github.com/webdriverio/example-recipes/tree/main/testDownloadBehavior) देखें।

## क्रोमियम ब्राउज़र डाउनलोड्स कॉन्फ़िगर करना

क्रोम डेवटूल्स तक पहुँचने के लिए WebDriverIO के `getPuppeteer` मेथड का उपयोग करके __क्रोमियम-आधारित__ ब्राउज़रों (जैसे Chrome, Edge, Brave, आदि) के लिए डाउनलोड पाथ बदलने के लिए।

```javascript
const page = await browser.getPuppeteer();
// Initiate a CDP Session:
const cdpSession = await page.target().createCDPSession();
// Set the Download Path:
await cdpSession.send('Browser.setDownloadBehavior', { behavior: 'allow', downloadPath: downloadPath });
```

## एकाधिक फ़ाइल डाउनलोड्स का प्रबंधन

एकाधिक फ़ाइल डाउनलोड्स वाले परिदृश्यों से निपटते समय, प्रत्येक डाउनलोड को प्रभावी ढंग से प्रबंधित और सत्यापित करने के लिए रणनीतियों को लागू करना आवश्यक है। निम्नलिखित दृष्टिकोणों पर विचार करें:

__अनुक्रमिक डाउनलोड हैंडलिंग:__ एक-एक करके फ़ाइलें डाउनलोड करें और अगला डाउनलोड शुरू करने से पहले प्रत्येक डाउनलोड को सत्यापित करें ताकि व्यवस्थित निष्पादन और सटीक सत्यापन सुनिश्चित हो सके।

__समानांतर डाउनलोड हैंडलिंग:__ परीक्षण निष्पादन समय को अनुकूलित करने के लिए एक साथ कई फ़ाइल डाउनलोड शुरू करने के लिए अतुल्यकालिक प्रोग्रामिंग तकनीकों का उपयोग करें। पूरा होने पर सभी डाउनलोड्स को सत्यापित करने के लिए मजबूत सत्यापन तंत्र लागू करें।

## क्रॉस-ब्राउज़र संगतता विचार

हालांकि WebDriverIO ब्राउज़र ऑटोमेशन के लिए एक एकीकृत इंटरफेस प्रदान करता है, ब्राउज़र व्यवहार और क्षमताओं में भिन्नताओं का ध्यान रखना आवश्यक है। संगतता और निरंतरता सुनिश्चित करने के लिए अपनी फ़ाइल डाउनलोड कार्यक्षमता को विभिन्न ब्राउज़रों में परीक्षण करने पर विचार करें।

__ब्राउज़र-विशिष्ट कॉन्फ़िगरेशन:__ Chrome, Firefox, Edge और अन्य समर्थित ब्राउज़रों में ब्राउज़र व्यवहार और प्राथमिकताओं में अंतर को समायोजित करने के लिए डाउनलोड पाथ सेटिंग्स और वेट स्ट्रेटेजीज को समायोजित करें।

__ब्राउज़र वर्शन संगतता:__ अपने मौजूदा परीक्षण सूट के साथ संगतता सुनिश्चित करते हुए नवीनतम सुविधाओं और सुधारों का लाभ उठाने के लिए अपने WebDriverIO और ब्राउज़र वर्शनों को नियमित रूप से अपडेट करें।