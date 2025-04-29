---
id: wdio-ocr-service
title: OCR परीक्षण सेवा
custom_edit_url: https://github.com/webdriverio/visual-testing/edit/main/README.md
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

> @wdio/ocr-service एक तीसरे पक्ष का पैकेज है, अधिक जानकारी के लिए कृपया [GitHub](https://github.com/webdriverio/visual-testing) | [npm](https://www.npmjs.com/package/@wdio/ocr-service) देखें

WebdriverIO के साथ दृश्य परीक्षण के लिए दस्तावेज़ीकरण के लिए, कृपया [डॉक्स](https://webdriver.io/docs/visual-testing) देखें। इस प्रोजेक्ट में WebdriverIO के साथ दृश्य परीक्षण चलाने के लिए सभी प्रासंगिक मॉड्यूल शामिल हैं। `./packages` निर्देशिका के अंदर आपको मिलेंगे:

-   `@wdio/visual-testing`: दृश्य परीक्षण को एकीकृत करने के लिए WebdriverIO सेवा
-   `webdriver-image-comparison`: एक छवि तुलना मॉड्यूल जिसका उपयोग विभिन्न NodeJS टेस्ट ऑटोमेशन फ्रेमवर्क के लिए किया जा सकता है जो WebDriver प्रोटोकॉल का समर्थन करते हैं

## स्टोरीबुक रनर (बीटा)

<details>
  <summary>स्टोरीबुक रनर बीटा के बारे में अधिक जानकारी प्राप्त करने के लिए क्लिक करें</summary>

> स्टोरीबुक रनर अभी भी बीटा में है, डॉक्स बाद में [WebdriverIO](https://webdriver.io/docs/visual-testing) दस्तावेज़ीकरण पृष्ठों पर स्थानांतरित हो जाएंगे।

यह मॉड्यूल अब एक नए विजुअल रनर के साथ स्टोरीबुक का समर्थन करता है। यह रनर स्वचालित रूप से एक स्थानीय/रिमोट स्टोरीबुक इंस्टेंस के लिए स्कैन करता है और प्रत्येक कंपोनेंट के तत्व स्क्रीनशॉट बनाएगा। यह निम्न को जोड़कर किया जा सकता है

```ts
export const config: WebdriverIO.Config = {
    // ...
    services: ["visual"],
    // ....
};
```

अपनी `services` में और `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook` को कमांड लाइन के माध्यम से चलाकर।
यह डिफ़ॉल्ट ब्राउज़र के रूप में हेडलेस मोड में Chrome का उपयोग करेगा।

> [!NOTE]
>
> -   अधिकांश विजुअल टेस्टिंग विकल्प स्टोरीबुक रनर के लिए भी काम करेंगे, देखें [WebdriverIO](https://webdriver.io/docs/visual-testing) दस्तावेज़ीकरण।
> -   स्टोरीबुक रनर आपकी सभी क्षमताओं को ओवरराइट करेगा और केवल उन ब्राउज़रों पर चल सकता है जिनका वह समर्थन करता है, देखें [`--browsers`](#browsers)।
> -   स्टोरीबुक रनर मल्टीरिमोट क्षमताओं का उपयोग करने वाले मौजूदा कॉन्फिग का समर्थन नहीं करता है और एक त्रुटि फेंकेगा।
> -   स्टोरीबुक रनर केवल डेस्कटॉप वेब का समर्थन करता है, मोबाइल वेब का नहीं।

### स्टोरीबुक रनर सेवा विकल्प

सेवा विकल्प इस प्रकार प्रदान किए जा सकते हैं

```ts
export const config: WebdriverIO.Config  = {
    // ...
    services: [
      [
        'visual',
        {
            // Some default options
            baselineFolder: join(process.cwd(), './__snapshots__/'),
            debug: true,
            // The storybook options, see cli options for the description
            storybook: {
                additionalSearchParams: new URLSearchParams({foo: 'bar', abc: 'def'}),
                clip: false,
                clipSelector: ''#some-id,
                numShards: 4,
                // `skipStories` can be a string ('example-button--secondary'),
                // an array (['example-button--secondary', 'example-button--small'])
                // or a regex which needs to be provided as as string ("/.*button.*/gm")
                skipStories: ['example-button--secondary', 'example-button--small'],
                url: 'https://www.bbc.co.uk/iplayer/storybook/',
                version: 6,
                // Optional - Allows overriding the baselines path. By default it will group the baselines by category and component (e.g. forms/input/baseline.png)
                getStoriesBaselinePath: (category, component) => `path__${category}__${component}`,
            },
        },
      ],
    ],
    // ....
}
```

### स्टोरीबुक रनर CLI विकल्प

#### `--additionalSearchParams`

-   **प्रकार:** `string`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** ''
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --additionalSearchParams="foo=bar&abc=def"`

यह स्टोरीबुक URL में अतिरिक्त खोज पैरामीटर जोड़ेगा।
अधिक जानकारी के लिए [URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) दस्तावेज़ीकरण देखें। स्ट्रिंग एक वैध URLSearchParams स्ट्रिंग होनी चाहिए।

> [!NOTE]
> डबल कोट्स की आवश्यकता है ताकि `&` को कमांड सेपरेटर के रूप में व्याख्या करने से रोका जा सके।
> उदाहरण के लिए `--additionalSearchParams="foo=bar&abc=def"` के साथ यह स्टोरी टेस्ट के लिए निम्न स्टोरीबुक URL जनरेट करेगा: `http://storybook.url/iframe.html?id=story-id&foo=bar&abc=def`।

#### `--browsers`

-   **प्रकार:** `string`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** `chrome`, आप `chrome|firefox|edge|safari` से चुन सकते हैं
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --browsers=chrome,firefox,edge,safari`
-   **नोट:** केवल CLI के माध्यम से उपलब्ध है

यह कंपोनेंट स्क्रीनशॉट लेने के लिए प्रदान किए गए ब्राउज़रों का उपयोग करेगा

> [!NOTE]
> सुनिश्चित करें कि आपने अपने स्थानीय मशीन पर उन ब्राउज़रों को इंस्टॉल किया है जिन पर आप चलाना चाहते हैं

#### `--clip`

-   **प्रकार:** `boolean`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** `true`
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --clip=false`

जब अक्षम किया जाता है तो यह व्यूपोर्ट स्क्रीनशॉट बनाएगा। जब सक्षम किया जाता है तो यह [`--clipSelector`](#clipselector) के आधार पर तत्व स्क्रीनशॉट बनाएगा जो कंपोनेंट स्क्रीनशॉट के चारों ओर खाली स्थान की मात्रा को कम करेगा और स्क्रीनशॉट आकार को कम करेगा।

#### `--clipSelector`

-   **प्रकार:** `string`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** स्टोरीबुक V7 के लिए `#storybook-root > :first-child` और स्टोरीबुक V6 के लिए `#root > :first-child:not(script):not(style)`, देखें [`--version`](#version)
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --clipSelector="#some-id"`

यह वह सेलेक्टर है जिसका उपयोग किया जाएगा:

-   स्क्रीनशॉट लेने के लिए तत्व का चयन करने के लिए
-   स्क्रीनशॉट लेने से पहले दृश्यमान होने के लिए तत्व की प्रतीक्षा करने के लिए

#### `--devices`

-   **प्रकार:** `string`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** आप [`deviceDescriptors.ts`](https://github.com/webdriverio/visual-testing/blob/main/./packages/service/src/storybook/deviceDescriptors.ts) से चुन सकते हैं
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --devices="iPhone 14 Pro Max","Pixel 3 XL"`
-   **नोट:** केवल CLI के माध्यम से उपलब्ध है

यह कंपोनेंट स्क्रीनशॉट लेने के लिए [`deviceDescriptors.ts`](https://github.com/webdriverio/visual-testing/blob/main/./packages/service/src/storybook/deviceDescriptors.ts) से मेल खाने वाले प्रदान किए गए उपकरणों का उपयोग करेगा

> [!NOTE]
>
> -   यदि आपको एक डिवाइस कॉन्फिग की कमी है, तो [फीचर अनुरोध](https://github.com/webdriverio/visual-testing/issues/new?assignees=&labels=&projects=&template=--feature-request.md) सबमिट करने के लिए स्वतंत्र महसूस करें
> -   यह केवल Chrome के साथ काम करेगा:
>     -   यदि आप `--devices` प्रदान करते हैं तो सभी Chrome इंस्टेंस **मोबाइल एमुलेशन** मोड में चलेंगे
>     -   यदि आप Chrome के अलावा अन्य ब्राउज़र भी प्रदान करते हैं, जैसे `--devices --browsers=firefox,safari,edge` तो यह स्वचालित रूप से मोबाइल एमुलेशन मोड में Chrome जोड़ देगा
> -   स्टोरीबुक रनर डिफ़ॉल्ट रूप से तत्व स्नैपशॉट बनाएगा, यदि आप पूरा मोबाइल एमुलेटेड स्क्रीनशॉट देखना चाहते हैं तो कमांड लाइन के माध्यम से `--clip=false` प्रदान करें
> -   फ़ाइल नाम उदाहरण के लिए `__snapshots__/example/button/desktop_chrome/example-button--large-local-chrome-iPhone-14-Pro-Max-430x932-dpr-3.png` जैसा दिखेगा
> -   **[SRC:](https://chromedriver.chromium.org/mobile-emulation#h.p_ID_167)** मोबाइल एमुलेशन का उपयोग करके डेस्कटॉप पर मोबाइल वेबसाइट का परीक्षण करना उपयोगी हो सकता है, लेकिन परीक्षकों को पता होना चाहिए कि कई सूक्ष्म अंतर हैं जैसे:
>     -   पूरी तरह से अलग GPU, जो बड़े प्रदर्शन परिवर्तनों की ओर ले जा सकता है;
>     -   मोबाइल UI का अनुकरण नहीं किया जाता है (विशेष रूप से, छिपाने वाला url बार पृष्ठ ऊंचाई को प्रभावित करता है);
>     -   विभेदन पॉपअप (जहां आप कुछ टच लक्ष्यों में से एक का चयन करते हैं) समर्थित नहीं है;
>     -   कई हार्डवेयर API (उदाहरण के लिए, orientationchange इवेंट) अनुपलब्ध हैं।

#### `--headless`

-   **प्रकार:** `boolean`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** `true`
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --headless=false`
-   **नोट:** केवल CLI के माध्यम से उपलब्ध है

यह टेस्ट को डिफ़ॉल्ट रूप से हेडलेस मोड में चलाएगा (जब ब्राउज़र इसका समर्थन करता है) या अक्षम किया जा सकता है

#### `--numShards`

-   **प्रकार:** `number`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** `true`
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --numShards=10`

यह उन समानांतर इंस्टेंस की संख्या होगी जिनका उपयोग स्टोरी चलाने के लिए किया जाएगा। यह आपके `wdio.conf`-फ़ाइल में `maxInstances` द्वारा सीमित होगा।

> [!IMPORTANT]
> `headless`-मोड में चलाते समय संख्या को 20 से अधिक न बढ़ाएं ताकि संसाधन प्रतिबंधों के कारण अस्थिरता को रोका जा सके

#### `--skipStories`

-   **प्रकार:** `string|regex`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** null
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --skipStories="/.*button.*/gm"`

यह हो सकता है:

-   एक स्ट्रिंग (`example-button--secondary,example-button--small`)
-   या एक रेगेक्स (`"/.*button.*/gm"`)

कुछ स्टोरी को छोड़ने के लिए। स्टोरी की `id` का उपयोग करें जो स्टोरी के URL में पाई जा सकती है। उदाहरण के लिए, इस URL `http://localhost:6006/?path=/story/example-page--logged-out` में `id` `example-page--logged-out` है

#### `--url`

-   **प्रकार:** `string`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** `http://127.0.0.1:6006`
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --url="https://example.com"`

वह URL जहां आपका स्टोरीबुक इंस्टेंस होस्ट किया गया है।

#### `--version`

-   **प्रकार:** `number`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** 7
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --version=6`

यह स्टोरीबुक का वर्शन है, यह डिफ़ॉल्ट रूप से `7` है। यह जानने के लिए आवश्यक है कि क्या V6 [`clipSelector`](#clipselector) का उपयोग किया जाना चाहिए।

### स्टोरीबुक इंटरैक्शन टेस्टिंग

स्टोरीबुक इंटरैक्शन टेस्टिंग आपको WDIO कमांड के साथ कस्टम स्क्रिप्ट बनाकर अपने कंपोनेंट के साथ इंटरैक्ट करने की अनुमति देता है ताकि एक कंपोनेंट को एक निश्चित स्थिति में सेट किया जा सके। उदाहरण के लिए, नीचे दिए गए कोड स्निपेट देखें:

```ts
import { browser, expect } from "@wdio/globals";

describe("Storybook Interaction", () => {
    it("should create screenshots for the logged in state when it logs out", async () => {
        const componentId = "example-page--logged-in";
        await browser.waitForStorybookComponentToBeLoaded({ id: componentId });

        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-in-state`
        );
        await $("button=Log out").click();
        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-out-state`
        );
    });

    it("should create screenshots for the logged out state when it logs in", async () => {
        const componentId = "example-page--logged-out";
        await browser.waitForStorybookComponentToBeLoaded({ id: componentId });

        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-out-state`
        );
        await $("button=Log in").click();
        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-in-state`
        );
    });
});
```

दो अलग-अलग कंपोनेंट पर दो परीक्षण निष्पादित किए जाते हैं। प्रत्येक परीक्षण पहले एक स्थिति सेट करता है और फिर एक स्क्रीनशॉट लेता है। आप यह भी देखेंगे कि एक नया कस्टम कमांड पेश किया गया है, जिसे [यहां](#new-custom-command) देखा जा सकता है।

उपरोक्त स्पेक फ़ाइल को एक फ़ोल्डर में सहेजा जा सकता है और निम्न कमांड के साथ कमांड लाइन में जोड़ा जा सकता है:

```sh
pnpm run test.local.desktop.storybook.localhost -- --spec='tests/specs/storybook-interaction/*.ts'
```

स्टोरीबुक रनर पहले स्वचालित रूप से आपके स्टोरीबुक इंस्टेंस को स्कैन करेगा और फिर आपके परीक्षणों को उन स्टोरी में जोड़ेगा जिन्हें तुलना करने की आवश्यकता है। यदि आप नहीं चाहते कि इंटरैक्शन परीक्षण के लिए आपके द्वारा उपयोग किए जाने वाले कंपोनेंट की दो बार तुलना की जाए, तो आप स्कैन से "डिफ़ॉल्ट" स्टोरी को हटाने के लिए [`--skipStories`](#--skipstories) फिल्टर प्रदान कर सकते हैं। यह इस तरह दिखेगा:

```sh
pnpm run test.local.desktop.storybook.localhost -- --skipStories="/example-page.*/gm" --spec='tests/specs/storybook-interaction/*.ts'
```

### नया कस्टम कमांड

एक नया कस्टम कमांड जिसे `browser.waitForStorybookComponentToBeLoaded({ id: 'componentId' })` कहा जाता है, `browser/driver`-ऑब्जेक्ट में जोड़ा जाएगा जो स्वचालित रूप से कंपोनेंट को लोड करेगा और इसके पूरा होने की प्रतीक्षा करेगा, ताकि आपको `browser.url('url.com')` विधि का उपयोग करने की आवश्यकता न हो। इसका उपयोग इस प्रकार किया जा सकता है

```ts
import { browser, expect } from "@wdio/globals";

describe("Storybook Interaction", () => {
    it("should create screenshots for the logged in state when it logs out", async () => {
        const componentId = "example-page--logged-in";
        await browser.waitForStorybookComponentToBeLoaded({ id: componentId });

        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-in-state`
        );
        await $("button=Log out").click();
        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-out-state`
        );
    });

    it("should create screenshots for the logged out state when it logs in", async () => {
        const componentId = "example-page--logged-out";
        await browser.waitForStorybookComponentToBeLoaded({ id: componentId });

        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-out-state`
        );
        await $("button=Log in").click();
        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-in-state`
        );
    });
});
```

विकल्प हैं:

#### `additionalSearchParams`

-   **प्रकार:** [`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** `new URLSearchParams()`
-   **उदाहरण:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    additionalSearchParams: new URLSearchParams({ foo: "bar", abc: "def" }),
    id: "componentId",
});
```

यह स्टोरीबुक URL में अतिरिक्त खोज पैरामीटर जोड़ेगा, उपरोक्त उदाहरण में URL `http://storybook.url/iframe.html?id=story-id&foo=bar&abc=def` होगा।
अधिक जानकारी के लिए [URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) दस्तावेज़ीकरण देखें।

#### `clipSelector`

-   **प्रकार:** `string`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** स्टोरीबुक V7 के लिए `#storybook-root > :first-child` और स्टोरीबुक V6 के लिए `#root > :first-child:not(script):not(style)`
-   **उदाहरण:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    clipSelector: "#your-selector",
    id: "componentId",
});
```

यह वह सेलेक्टर है जिसका उपयोग किया जाएगा:

-   स्क्रीनशॉट लेने के लिए तत्व का चयन करने के लिए
-   स्क्रीनशॉट लेने से पहले दृश्यमान होने के लिए तत्व की प्रतीक्षा करने के लिए

#### `id`

-   **प्रकार:** `string`
-   **अनिवार्य:** हां
-   **उदाहरण:**

```ts
await browser.waitForStorybookComponentToBeLoaded({ '#your-selector', id: 'componentId' })
```

स्टोरी की `id` का उपयोग करें जो स्टोरी के URL में पाई जा सकती है। उदाहरण के लिए, इस URL `http://localhost:6006/?path=/story/example-page--logged-out` में `id` `example-page--logged-out` है

#### `timeout`

-   **प्रकार:** `number`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** 1100 मिलीसेकंड
-   **उदाहरण:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    id: "componentId",
    timeout: 20000,
});
```

अधिकतम टाइमआउट जो हम पृष्ठ पर लोड होने के बाद एक कंपोनेंट के दृश्यमान होने की प्रतीक्षा करना चाहते हैं

#### `url`

-   **प्रकार:** `string`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** `http://127.0.0.1:6006`
-   **उदाहरण:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    id: "componentId",
    url: "https://your.url",
});
```

वह URL जहां आपका स्टोरीबुक इंस्टेंस होस्ट किया गया है।

</details>

## योगदान

### पैकेज अपडेट करना

आप एक सरल CLI टूल के साथ पैकेज अपडेट कर सकते हैं। सुनिश्चित करें कि आपने सभी निर्भरताओं को इंस्टॉल किया है, फिर आप चला सकते हैं

```sh
pnpm update.packages
```

यह एक CLI ट्रिगर करेगा जो आपसे निम्न प्रश्न पूछेगा

```logs
==========================
🤖 Package update Wizard 🧙
==========================

? Which version target would you like to update to? (Minor|Latest)
? Do you want to update the package.json files? (Y/n)
? Do you want to remove all "node_modules" and reinstall dependencies? (Y/n)
? Would you like reinstall the dependencies? (Y/n)
```

### प्रश्न

यदि आपके पास इस प्रोजेक्ट में योगदान करने के बारे में कोई प्रश्न या समस्याएं हैं, तो कृपया हमारे [Discord](https://discord.webdriver.io) सर्वर से जुड़ें। `🙏-contributing` चैनल में योगदानकर्ताओं से संपर्क करें।

### समस्याएं

यदि आपके पास प्रश्न, बग या फीचर अनुरोध हैं, तो कृपया एक समस्या दर्ज करें। समस्या सबमिट करने से पहले, कृपया डुप्लिकेट को कम करने में मदद के लिए समस्या आर्काइव खोजें, और [FAQ](https://webdriver.io/docs/visual-testing/faq/) पढ़ें।

यदि आप इसे वहां नहीं पा सकते हैं तो आप एक समस्या सबमिट कर सकते हैं जहां आप सबमिट कर सकते हैं:

-   🐛**बग रिपोर्ट**: हमें सुधार करने में मदद करने के लिए एक रिपोर्ट बनाएं
-   📖**दस्तावेज़ीकरण**: सुधार का सुझाव दें या गायब/अस्पष्ट दस्तावेज़ीकरण की रिपोर्ट करें।
-   💡**फीचर अनुरोध**: इस मॉड्यूल के लिए एक विचार का सुझाव दें।
-   💬**प्रश्न**: प्रश्न पूछें।

### विकास वर्कफ़्लो

इस प्रोजेक्ट के लिए PR बनाने और योगदान देना शुरू करने के लिए इस चरण-दर-चरण गाइड का अनुसरण करें:

-   प्रोजेक्ट को फोर्क करें।
-   प्रोजेक्ट को अपने कंप्यूटर पर कहीं क्लोन करें

    ```sh
    $ git clone https://github.com/webdriverio/visual-testing.git
    ```

-   डायरेक्टरी में जाएं और प्रोजेक्ट सेटअप करें

    ```sh
    $ cd visual-testing
    $ corepack enable
    $ pnpm pnpm.install.workaround
    ```

-   वॉच मोड चलाएं जो स्वचालित रूप से कोड को ट्रांसपाइल करेगा

    ```sh
    $ pnpm watch
    ```

    प्रोजेक्ट बनाने के लिए, चलाएं:

    ```sh
    $ pnpm build
    ```

-   सुनिश्चित करें कि आपके परिवर्तन किसी भी परीक्षण को नहीं तोड़ते हैं, चलाएं:

    ```sh
    $ pnpm test
    ```

यह प्रोजेक्ट स्वचालित रूप से चेंजलॉग और रिलीज बनाने के लिए [changesets](https://github.com/changesets/changesets) का उपयोग करता है।

### परीक्षण

मॉड्यूल का परीक्षण करने के लिए कई परीक्षण निष्पादित करने होंगे। PR जोड़ते समय सभी परीक्षणों को कम से कम स्थानीय परीक्षणों को पास करना चाहिए। प्रत्येक PR का स्वचालित रूप से Sauce Labs के खिलाफ परीक्षण किया जाता है, देखें [हमारी GitHub Actions पाइपलाइन](https://github.com/webdriverio/visual-testing/actions/workflows/tests.yml)। PR को अनुमोदित करने से पहले मुख्य योगदानकर्ता एमुलेटर/सिमुलेटर / वास्तविक उपकरणों के खिलाफ PR का परीक्षण करेंगे।

#### स्थानीय परीक्षण

सबसे पहले, एक स्थानीय बेसलाइन बनाने की आवश्यकता है। यह निम्न के साथ किया जा सकता है:

```sh
// With the webdriver protocol
$ pnpm run test.local.init
```

यह कमांड `localBaseline` नामक एक फ़ोल्डर बनाएगा जो सभी बेसलाइन छवियों को रखेगा।

फिर चलाएं:

```sh
// With the webdriver protocol
pnpm run test.local.desktop
```

यह स्थानीय मशीन पर Chrome पर सभी परीक्षण चलाएगा।

#### स्थानीय स्टोरीबुक रनर परीक्षण (बीटा)

सबसे पहले, एक स्थानीय बेसलाइन बनाने की आवश्यकता है। यह निम्न के साथ किया जा सकता है:

```sh
pnpm run test.local.desktop.storybook
```

यह हेडलेस मोड में Chrome के साथ स्टोरीबुक परीक्षण https://govuk-react.github.io/govuk-react/ पर स्थित एक डेमो स्टोरीबुक रेपो के खिलाफ चलाएगा।

अधिक ब्राउज़रों के साथ परीक्षण चलाने के लिए आप चला सकते हैं

```sh
pnpm run test.local.desktop.storybook -- --browsers=chrome,firefox,edge,safari
```

> [!NOTE]
> सुनिश्चित करें कि आपने अपने स्थानीय मशीन पर उन ब्राउज़रों को इंस्टॉल किया है जिन पर आप चलाना चाहते हैं

#### CI परीक्षण Sauce Labs के साथ (PR के लिए आवश्यक नहीं)

नीचे दिया गया कमांड GitHub Actions पर बिल्ड का परीक्षण करने के लिए उपयोग किया जाता है, इसका उपयोग केवल वहां किया जा सकता है और स्थानीय विकास के लिए नहीं।

```
$ pnpm run test.saucelabs
```

यह बहुत सारे कॉन्फिगरेशन के खिलाफ परीक्षण करेगा जिन्हें [यहां](https://github.com/webdriverio/visual-testing/blob/main/./tests/configs/wdio.saucelabs.web.conf.ts) पाया जा सकता है।
सभी PR की स्वचालित रूप से Sauce Labs के खिलाफ जांच की जाती है।

## रिलीज़िंग

ऊपर सूचीबद्ध किसी भी पैकेज के वर्शन को रिलीज करने के लिए, निम्न करें:

-   [रिलीज पाइपलाइन](https://github.com/webdriverio/visual-testing/actions/workflows/release.yml) ट्रिगर करें
-   एक रिलीज PR जनरेट किया जाता है, इसकी समीक्षा करवाएं और किसी अन्य WebdriverIO सदस्य द्वारा अनुमोदित करवाएं
-   PR को मर्ज करें
-   [रिलीज पाइपलाइन](https://github.com/webdriverio/visual-testing/actions/workflows/release.yml) को फिर से ट्रिगर करें
-   एक नया वर्शन रिलीज होना चाहिए 🎉

## क्रेडिट

`@wdio/visual-testing` [LambdaTest](https://www.lambdatest.com/) और [Sauce Labs](https://saucelabs.com/) से एक ओपन-सोर्स लाइसेंस का उपयोग करता है।