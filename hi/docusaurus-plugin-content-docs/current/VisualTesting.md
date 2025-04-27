---
id: visual-testing
title: विजुअल टेस्टिंग
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

## यह क्या कर सकता है?

WebdriverIO निम्नलिखित के लिए स्क्रीन, एलिमेंट्स या पूरे पेज पर इमेज कंपैरिज़न प्रदान करता है

-   🖥️ डेस्कटॉप ब्राउज़र (Chrome / Firefox / Safari / Microsoft Edge)
-   📱 मोबाइल / टैबलेट ब्राउज़र (Android एमुलेटर पर Chrome / iOS सिम्युलेटर पर Safari / सिम्युलेटर / वास्तविक डिवाइस) Appium के माध्यम से
-   📱 नेटिव ऐप्स (Android एमुलेटर / iOS सिम्युलेटर / वास्तविक डिवाइस) Appium के माध्यम से (🌟 **नया** 🌟)
-   📳 हाइब्रिड ऐप्स Appium के माध्यम से

[`@wdio/visual-service`](https://www.npmjs.com/package/@wdio/visual-service) के माध्यम से जो एक लाइटवेट WebdriverIO सर्विस है।

इससे आप निम्न कार्य कर सकते हैं:

-   **स्क्रीन/एलिमेंट्स/पूरे-पेज** स्क्रीन को बेसलाइन के खिलाफ सेव या तुलना करें
-   जब कोई बेसलाइन नहीं है तो स्वचालित रूप से **बेसलाइन बनाएं**
-   **कस्टम क्षेत्रों को ब्लॉक करें** और यहां तक कि तुलना के दौरान स्टेटस और टूलबार (केवल मोबाइल) को **स्वचालित रूप से बाहर रखें**
-   एलिमेंट डायमेंशन स्क्रीनशॉट बढ़ाएं
-   वेबसाइट कंपैरिज़न के दौरान **टेक्स्ट छिपाएं**:
    -   **स्थिरता बढ़ाएं** और फॉन्ट रेंडरिंग की अस्थिरता को रोकें
    -   केवल वेबसाइट के **लेआउट** पर ध्यान केंद्रित करें
-   बेहतर पठनीय परीक्षणों के लिए **विभिन्न तुलना विधियों** और **अतिरिक्त मैचर्स** का सेट उपयोग करें
-   सत्यापित करें कि आपकी वेबसाइट **कीबोर्ड के साथ टैबिंग का समर्थन कैसे करेगी**, देखें [वेबसाइट पर टैबिंग](#tabbing-through-a-website)
-   और बहुत कुछ, देखें [सर्विस](./visual-testing/service-options) और [मेथड](./visual-testing/method-options) विकल्प

यह सर्विस सभी ब्राउज़र/डिवाइसेस के लिए आवश्यक डेटा और स्क्रीनशॉट प्राप्त करने के लिए एक लाइटवेट मॉड्यूल है। तुलना शक्ति [ResembleJS](https://github.com/Huddle/Resemble.js) से आती है। यदि आप ऑनलाइन छवियों की तुलना करना चाहते हैं तो आप [ऑनलाइन टूल](http://rsmbl.github.io/Resemble.js/) देख सकते हैं।

:::info नेटिव/हाइब्रिड ऐप्स के लिए नोट
`saveScreen`, `saveElement`, `checkScreen`, `checkElement` मेथड्स और `toMatchScreenSnapshot` और `toMatchElementSnapshot` मैचर्स का उपयोग नेटिव ऐप्स/कॉन्टेक्स्ट के लिए किया जा सकता है।

कृपया हाइब्रिड ऐप्स के लिए उपयोग करते समय अपनी सर्विस सेटिंग्स में `isHybridApp:true` प्रॉपर्टी का उपयोग करें।
:::

## इंस्टॉलेशन

सबसे आसान तरीका है `@wdio/visual-service` को अपने `package.json` में एक डेव-डिपेंडेंसी के रूप में रखना, इस प्रकार:

```sh
npm install --save-dev @wdio/visual-service
```

## उपयोग

`@wdio/visual-service` का उपयोग एक सामान्य सर्विस के रूप में किया जा सकता है। आप इसे अपनी कॉन्फिगरेशन फाइल में निम्न प्रकार से सेट कर सकते हैं:

```js
import path from "node:path";

// wdio.conf.ts
export const config = {
    // ...
    // =====
    // Setup
    // =====
    services: [
        [
            "visual",
            {
                // कुछ विकल्प, अधिक जानकारी के लिए डॉक्स देखें
                baselineFolder: path.join(process.cwd(), "tests", "baseline"),
                formatImageName: "{tag}-{logName}-{width}x{height}",
                screenshotPath: path.join(process.cwd(), "tmp"),
                savePerInstance: true,
                // ... अधिक विकल्प
            },
        ],
    ],
    // ...
};
```

अधिक सर्विस विकल्प [यहां](/docs/visual-testing/service-options) पाए जा सकते हैं।

एक बार आपके WebdriverIO कॉन्फिगरेशन में सेट होने के बाद, आप आगे बढ़ सकते हैं और [अपने टेस्ट](/docs/visual-testing/writing-tests) में विजुअल असर्शन जोड़ सकते हैं।

### कैपेबिलिटीज
विजुअल टेस्टिंग मॉड्यूल का उपयोग करने के लिए, **आपको अपनी कैपेबिलिटीज में कोई अतिरिक्त विकल्प जोड़ने की आवश्यकता नहीं है**। हालांकि, कुछ मामलों में, आप अपने विजुअल टेस्ट में अतिरिक्त मेटाडेटा जोड़ना चाह सकते हैं, जैसे `logName`।

`logName` आपको प्रत्येक कैपेबिलिटी को एक कस्टम नाम असाइन करने की अनुमति देता है, जिसे फिर इमेज फाइलनेम में शामिल किया जा सकता है। यह विशेष रूप से विभिन्न ब्राउज़रों, डिवाइसों, या कॉन्फिगरेशन पर लिए गए स्क्रीनशॉट को अलग करने के लिए उपयोगी है।

इसे सक्षम करने के लिए, आप `capabilities` सेक्शन में `logName` को परिभाषित कर सकते हैं और यह सुनिश्चित कर सकते हैं कि विजुअल टेस्टिंग सर्विस में `formatImageName` विकल्प इसका संदर्भ देता है। यहां बताया गया है कि आप इसे कैसे सेट कर सकते हैं:

```js
import path from "node:path";

// wdio.conf.ts
export const config = {
    // ...
    // =====
    // Setup
    // =====
    capabilities: [
        {
            browserName: 'chrome',
            'wdio-ics:options': {
                logName: 'chrome-mac-15', // Chrome के लिए कस्टम लॉग नाम
            },
        }
        {
            browserName: 'firefox',
            'wdio-ics:options': {
                logName: 'firefox-mac-15', // Firefox के लिए कस्टम लॉग नाम
            },
        }
    ],
    services: [
        [
            "visual",
            {
                // कुछ विकल्प, अधिक जानकारी के लिए डॉक्स देखें
                baselineFolder: path.join(process.cwd(), "tests", "baseline"),
                screenshotPath: path.join(process.cwd(), "tmp"),
                // नीचे दिया गया फॉर्मेट कैपेबिलिटीज से `logName` का उपयोग करेगा
                formatImageName: "{tag}-{logName}-{width}x{height}",
                // ... अधिक विकल्प
            },
        ],
    ],
    // ...
};
```

#### यह कैसे काम करता है
1. `logName` सेट करना:

    - `capabilities` सेक्शन में, प्रत्येक ब्राउज़र या डिवाइस को एक अद्वितीय `logName` असाइन करें। उदाहरण के लिए, `chrome-mac-15` macOS संस्करण 15 पर Chrome पर चलने वाले टेस्ट की पहचान करता है।

2. कस्टम इमेज नामिंग:

    - `formatImageName` विकल्प स्क्रीनशॉट फाइलनेम में `logName` को एकीकृत करता है। उदाहरण के लिए, यदि `tag` होमपेज है और रेज़ोल्यूशन `1920x1080` है, तो परिणामी फाइलनेम इस प्रकार दिख सकता है:

        `homepage-chrome-mac-15-1920x1080.png`

3. कस्टम नामिंग के लाभ:

    - विभिन्न ब्राउज़रों या डिवाइसों से स्क्रीनशॉट के बीच अंतर करना बहुत आसान हो जाता है, विशेष रूप से बेसलाइन को प्रबंधित करते समय और विसंगतियों को डीबग करते समय।

4. डिफ़ॉल्ट पर नोट:

    - यदि `logName` क्षमताओं में सेट नहीं है, तो `formatImageName` विकल्प इसे फाइलनेम में एक खाली स्ट्रिंग के रूप में दिखाएगा (`homepage--15-1920x1080.png`)

### WebdriverIO MultiRemote

हम [MultiRemote](https://webdriver.io/docs/multiremote/) का भी समर्थन करते हैं। इसे ठीक से काम करने के लिए सुनिश्चित करें कि आप अपनी
क्षमताओं में `wdio-ics:options` जोड़ें जैसा कि आप नीचे देख सकते हैं। यह सुनिश्चित करेगा कि प्रत्येक स्क्रीनशॉट का अपना अद्वितीय नाम होगा।

[अपने टेस्ट लिखना](/docs/visual-testing/writing-tests) [टेस्टरनर](https://webdriver.io/docs/testrunner) का उपयोग करने की तुलना में अलग नहीं होगा।

```js
// wdio.conf.js
export const config = {
    capabilities: {
        chromeBrowserOne: {
            capabilities: {
                browserName: "chrome",
                "goog:chromeOptions": {
                    args: ["disable-infobars"],
                },
                // THIS!!!
                "wdio-ics:options": {
                    logName: "chrome-latest-one",
                },
            },
        },
        chromeBrowserTwo: {
            capabilities: {
                browserName: "chrome",
                "goog:chromeOptions": {
                    args: ["disable-infobars"],
                },
                // THIS!!!
                "wdio-ics:options": {
                    logName: "chrome-latest-two",
                },
            },
        },
    },
};
```

### प्रोग्रामेटिक रूप से चलाना

यहां `remote` विकल्पों के माध्यम से `@wdio/visual-service` का उपयोग करने का एक न्यूनतम उदाहरण है:

```js
import { remote } from "webdriverio";
import VisualService from "@wdio/visual-service";

let visualService = new VisualService({
    autoSaveBaseline: true,
});

const browser = await remote({
    logLevel: "silent",
    capabilities: {
        browserName: "chrome",
    },
});

// `browser` में कस्टम कमांड जोड़ने के लिए सर्विस को "शुरू" करें
visualService.remoteSetup(browser);

await browser.url("https://webdriver.io/");

// या केवल स्क्रीनशॉट सेव करने के लिए इसका उपयोग करें
await browser.saveFullPageScreen("examplePaged", {});

// या वैलिडेशन के लिए इसका उपयोग करें। दोनों मेथड्स को संयोजित करने की आवश्यकता नहीं है, FAQ देखें
await browser.checkFullPageScreen("examplePaged", {});

await browser.deleteSession();
```

### वेबसाइट पर टैबिंग

आप कीबोर्ड <kbd>TAB</kbd>-की का उपयोग करके यह जांच सकते हैं कि क्या वेबसाइट एक्सेसिबल है। एक्सेसिबिलिटी के इस भाग का परीक्षण हमेशा समय लेने वाला (मैनुअल) काम रहा है और ऑटोमेशन के माध्यम से करना काफी कठिन है।
`saveTabbablePage` और `checkTabbablePage` विधियों के साथ, आप अब अपनी वेबसाइट पर लाइनें और डॉट्स ड्रा कर सकते हैं ताकि टैबिंग क्रम को सत्यापित कर सकें।

इस बात का ध्यान रखें कि यह केवल डेस्कटॉप ब्राउज़रों के लिए उपयोगी है और **मोबाइल डिवाइसों के लिए नहीं है**। सभी डेस्कटॉप ब्राउज़र इस सुविधा का समर्थन करते हैं।

:::note

यह काम [Viv Richards](https://github.com/vivrichards600) के ब्लॉग पोस्ट ["AUTOMATING PAGE TABABILITY (IS THAT A WORD?) WITH VISUAL TESTING"](https://vivrichards.co.uk/accessibility/automating-page-tab-flows-using-visual-testing-and-javascript) से प्रेरित है।

तरीका जिससे टैबेबल एलिमेंट्स का चयन किया जाता है, वह [tabbable](https://github.com/davidtheclark/tabbable) मॉड्यूल पर आधारित है। यदि टैबिंग के संबंध में कोई समस्या है, तो कृपया [README.md](https://github.com/davidtheclark/tabbable/blob/master/README.md) और विशेष रूप से [अधिक विवरण](https://github.com/davidtheclark/tabbable/blob/master/README.md#more-details) सेक्शन देखें।

:::

#### यह कैसे काम करता है

दोनों मेथड्स आपकी वेबसाइट पर एक `canvas` एलिमेंट बनाएंगे और लाइनें और बिंदु खींचेंगे ताकि आपको दिखाएं कि अगर एक अंतिम उपयोगकर्ता इसका उपयोग करेगा तो आपका TAB कहां जाएगा। इसके बाद, यह फ्लो का अच्छा अवलोकन देने के लिए एक पूर्ण-पृष्ठ स्क्रीनशॉट बनाएगा।

:::important

**`saveTabbablePage` का उपयोग केवल तब करें जब आपको स्क्रीनशॉट बनाने की आवश्यकता हो और **उसे **बेसलाइन** इमेज के साथ तुलना **नहीं** करनी हो।**

:::

जब आप टैबिंग फ्लो की तुलना बेसलाइन से करना चाहते हैं, तो आप `checkTabbablePage`-मेथड का उपयोग कर सकते हैं। आपको दो विधियों का एक साथ उपयोग करने की **आवश्यकता नहीं** है। यदि पहले से ही एक बेसलाइन इमेज बनाई गई है, जो स्वचालित रूप से सर्विस को इंस्टेंशिएट करते समय `autoSaveBaseline: true` प्रदान करके किया जा सकता है,
`checkTabbablePage` पहले _एक्चुअल_ इमेज बनाएगा और फिर उसकी तुलना बेसलाइन से करेगा।

##### विकल्प

दोनों मेथड्स [`saveFullPageScreen`](https://github.com/wswebcreation/webdriver-image-comparison/blob/master/docs/OPTIONS.md#savefullpagescreen-or-savetabbablepage) या
[`compareFullPageScreen`](https://github.com/wswebcreation/webdriver-image-comparison/blob/master/docs/OPTIONS.md#comparefullpagescreen-or-comparetabbablepage) के समान विकल्पों का उपयोग करते हैं।

#### उदाहरण

यह एक उदाहरण है कि हमारी [गिनी पिग वेबसाइट](https://guinea-pig.webdriver.io/image-compare.html) पर टैबिंग कैसे काम करती है:

![WDIO tabbing example](/img/visual/tabbable-chrome-latest-1366x768.png)

### विजुअल स्नैपशॉट को स्वचालित रूप से अपडेट करें

कमांड लाइन में `--update-visual-baseline` आर्गुमेंट जोड़कर बेसलाइन इमेज अपडेट करें। यह

-   स्वचालित रूप से वास्तविक स्क्रीनशॉट को कॉपी करेगा और इसे बेसलाइन फोल्डर में रखेगा
-   यदि अंतर हैं तो यह परीक्षण को पास कर देगा क्योंकि बेसलाइन अपडेट किया गया है

**उपयोग:**

```sh
npm run test.local.desktop  --update-visual-baseline
```

लॉग्स इन्फो/डीबग मोड चलाते समय आप निम्न लॉग्स देखेंगे

```logs
[0-0] ..............
[0-0] #####################################################################################
[0-0]  INFO:
[0-0]  Updated the actual image to
[0-0]  /Users/wswebcreation/Git/wdio/visual-testing/localBaseline/chromel/demo-chrome-1366x768.png
[0-0] #####################################################################################
[0-0] ..........
```

## टाइपस्क्रिप्ट सपोर्ट

इस मॉड्यूल में टाइपस्क्रिप्ट सपोर्ट शामिल है, जो आपको विजुअल टेस्टिंग सर्विस का उपयोग करते समय ऑटो-कंप्लीशन, टाइप सेफ्टी और बेहतर डेवलपर अनुभव का लाभ उठाने की अनुमति देता है।

### चरण 1: टाइप डेफिनिशन जोड़ें
यह सुनिश्चित करने के लिए कि टाइपस्क्रिप्ट मॉड्यूल टाइप्स को पहचानता है, अपने tsconfig.json में types फील्ड में निम्न एंट्री जोड़ें:

```json
{
    "compilerOptions": {
        "types": ["@wdio/visual-service"]
    }
}
```

### चरण 2: सर्विस विकल्पों के लिए टाइप सेफ्टी सक्षम करें
सर्विस विकल्पों पर टाइप चेकिंग लागू करने के लिए, अपने WebdriverIO कॉन्फिगरेशन को अपडेट करें:

```ts
// wdio.conf.ts
import { join } from 'node:path';
// टाइप डेफिनिशन इम्पोर्ट करें
import type { VisualServiceOptions } from '@wdio/visual-service';

export const config = {
    // ...
    // =====
    // Setup
    // =====
    services: [
        [
            "visual",
            {
                // सर्विस विकल्प
                baselineFolder: join(process.cwd(), './__snapshots__/'),
                formatImageName: '{tag}-{logName}-{width}x{height}',
                screenshotPath: join(process.cwd(), '.tmp/'),
            } satisfies VisualServiceOptions, // टाइप सेफ्टी सुनिश्चित करता है
        ],
    ],
    // ...
};
```

## सिस्टम आवश्यकताएँ

### संस्करण 5 और ऊपर

संस्करण 5 और उससे ऊपर के लिए, यह मॉड्यूल सामान्य [प्रोजेक्ट आवश्यकताओं](/docs/gettingstarted#system-requirements) के अलावा कोई अतिरिक्त सिस्टम निर्भरता के बिना एक पूर्ण रूप से जावास्क्रिप्ट-आधारित मॉड्यूल है। यह [Jimp](https://github.com/jimp-dev/jimp) का उपयोग करता है जो नोड के लिए एक इमेज प्रोसेसिंग लाइब्रेरी है जो पूरी तरह से जावास्क्रिप्ट में लिखी गई है, बिना किसी नेटिव निर्भरता के।

### संस्करण 4 और निचला

संस्करण 4 और निचले के लिए, यह मॉड्यूल [Canvas](https://github.com/Automattic/node-canvas) पर निर्भर करता है, जो Node.js के लिए एक कैनवास कार्यान्वयन है। Canvas [Cairo](https://cairographics.org/) पर निर्भर करता है।

#### इंस्टॉलेशन विवरण

डिफ़ॉल्ट रूप से, आपके प्रोजेक्ट के `npm install` के दौरान macOS, Linux और Windows के लिए बाइनरीज़ डाउनलोड की जाएंगी। यदि आपके पास समर्थित OS या प्रोसेसर आर्किटेक्चर नहीं है, तो मॉड्यूल को आपके सिस्टम पर कंपाइल किया जाएगा। इसके लिए कई निर्भरताएं आवश्यक हैं, जिनमें Cairo और Pango शामिल हैं।

विस्तृत इंस्टॉलेशन जानकारी के लिए, [node-canvas wiki](https://github.com/Automattic/node-canvas/wiki/_pages) देखें। नीचे सामान्य ऑपरेटिंग सिस्टम के लिए एक-पंक्ति इंस्टॉलेशन निर्देश दिए गए हैं। ध्यान दें कि `libgif/giflib`, `librsvg`, और `libjpeg` वैकल्पिक हैं और केवल क्रमशः GIF, SVG, और JPEG समर्थन के लिए आवश्यक हैं। Cairo v1.10.0 या बाद का आवश्यक है।

<Tabs
defaultValue="osx"
values={[
{label: 'OS', value: 'osx'},
{label: 'Ubuntu', value: 'ubuntu'},
{label: 'Fedora', value: 'fedora'},
{label: 'Solaris', value: 'solaris'},
{label: 'OpenBSD', value: 'openbsd'},
{label: 'Window', value: 'windows'},
{label: 'Others', value: 'others'},
]}

> <TabItem value="osx">

     [Homebrew](https://brew.sh/) का उपयोग करके:

     ```sh
     brew install pkg-config cairo pango libpng jpeg giflib librsvg pixman
     ```

    **Mac OS X v10.11+:** यदि आपने हाल ही में Mac OS X v10.11+ में अपडेट किया है और कंपाइल करते समय परेशानी का अनुभव कर रहे हैं, तो निम्न कमांड चलाएं: `xcode-select --install`। समस्या के बारे में [स्टैक ओवरफ्लो पर](http://stackoverflow.com/a/32929012/148072) अधिक पढ़ें।
    यदि आपके पास Xcode 10.0 या उच्चतर इंस्टॉल है, तो सोर्स से बिल्ड करने के लिए आपको NPM 6.4.1 या उच्चतर की आवश्यकता है।

</TabItem>
<TabItem value="ubuntu">

    ```sh
    sudo apt-get install build-essential libcairo2-dev libpango1.0-dev libjpeg-dev libgif-dev librsvg2-dev
    ```

</TabItem>
<TabItem value="fedora">

    ```sh
    sudo yum install gcc-c++ cairo-devel pango-devel libjpeg-turbo-devel giflib-devel
    ```

</TabItem>
<TabItem value="solaris">

    ```sh
    pkgin install cairo pango pkg-config xproto renderproto kbproto xextproto
    ```

</TabItem>
<TabItem value="openbsd">

    ```sh
    doas pkg_add cairo pango png jpeg giflib
    ```

</TabItem>
<TabItem value="windows">

    [विकी](https://github.com/Automattic/node-canvas/wiki/Installation:-Windows) देखें

</TabItem>
<TabItem value="others">

    [विकी](https://github.com/Automattic/node-canvas/wiki) देखें

</TabItem>
</Tabs>
