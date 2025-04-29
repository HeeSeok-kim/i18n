---
id: wdio-visual-service
title: छवि तुलना (विजुअल रिग्रेशन टेस्टिंग) सेवा
custom_edit_url: https://github.com/webdriverio/visual-testing/edit/main/README.md
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

> @wdio/visual-service एक तृतीय पक्ष पैकेज है, अधिक जानकारी के लिए कृपया देखें [GitHub](https://github.com/webdriverio/visual-testing) | [npm](https://www.npmjs.com/package/@wdio/visual-service)

WebdriverIO के साथ विजुअल टेस्टिंग के लिए दस्तावेज़ीकरण के लिए, कृपया [docs](https://webdriver.io/docs/visual-testing) देखें। इस प्रोजेक्ट में WebdriverIO के साथ विजुअल टेस्ट चलाने के लिए सभी प्रासंगिक मॉड्यूल शामिल हैं। `./packages` डायरेक्टरी में आपको मिलेंगे:

-   `@wdio/visual-testing`: विजुअल टेस्टिंग को एकीकृत करने के लिए WebdriverIO सेवा
-   `webdriver-image-comparison`: एक छवि तुलना मॉड्यूल जिसे विभिन्न NodeJS टेस्ट ऑटोमेशन फ्रेमवर्क के लिए उपयोग किया जा सकता है जो WebDriver प्रोटोकॉल का समर्थन करते हैं

## Storybook रनर (बीटा)

<details>
  <summary>Storybook रनर बीटा के बारे में अधिक दस्तावेज़ीकरण जानने के लिए क्लिक करें</summary>

> Storybook रनर अभी भी बीटा में है, डॉक्स बाद में [WebdriverIO](https://webdriver.io/docs/visual-testing) दस्तावेज़ीकरण पेजों पर स्थानांतरित किए जाएंगे।

यह मॉड्यूल अब एक नए विजुअल रनर के साथ Storybook का समर्थन करता है। यह रनर स्वचालित रूप से स्थानीय/रिमोट storybook इंस्टेंस के लिए स्कैन करता है और प्रत्येक कंपोनेंट के एलिमेंट स्क्रीनशॉट बनाएगा। इसे निम्न द्वारा किया जा सकता है

```ts
export const config: WebdriverIO.Config = {
    // ...
    services: ["visual"],
    // ....
};
```

अपने `services` में जोड़कर और `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook` कमांड लाइन के माध्यम से चलाकर।
यह डिफ़ॉल्ट ब्राउज़र के रूप में हेडलेस मोड में Chrome का उपयोग करेगा।

> [!NOTE]
>
> -   अधिकांश विजुअल टेस्टिंग विकल्प Storybook रनर के लिए भी काम करेंगे, [WebdriverIO](https://webdriver.io/docs/visual-testing) दस्तावेज़ीकरण देखें।
> -   Storybook रनर आपकी सभी क्षमताओं को ओवरराइट करेगा और केवल उन ब्राउज़रों पर चल सकता है जिनका वह समर्थन करता है, [`--browsers`](#browsers) देखें।
> -   Storybook रनर मल्टीरिमोट क्षमताओं का उपयोग करने वाले मौजूदा कॉन्फ़िग का समर्थन नहीं करता है और एक त्रुटि फेंकेगा।
> -   Storybook रनर केवल डेस्कटॉप वेब का समर्थन करता है, मोबाइल वेब का नहीं।

### Storybook रनर सेवा विकल्प

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

### Storybook रनर CLI विकल्प

#### `--additionalSearchParams`

-   **प्रकार:** `string`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** ''
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --additionalSearchParams="foo=bar&abc=def"`

यह Storybook URL में अतिरिक्त खोज पैरामीटर जोड़ देगा।
अधिक जानकारी के लिए [URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) दस्तावेज़ीकरण देखें। स्ट्रिंग एक वैध URLSearchParams स्ट्रिंग होनी चाहिए।

> [!NOTE]
> डबल कोट्स की आवश्यकता है ताकि `&` को कमांड सेपरेटर के रूप में व्याख्या न किया जाए।
> उदाहरण के लिए `--additionalSearchParams="foo=bar&abc=def"` के साथ यह स्टोरीज परीक्षण के लिए निम्नलिखित Storybook URL उत्पन्न करेगा: `http://storybook.url/iframe.html?id=story-id&foo=bar&abc=def`।

#### `--browsers`

-   **प्रकार:** `string`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** `chrome`, आप `chrome|firefox|edge|safari` में से चुन सकते हैं
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

जब अक्षम किया जाता है तो यह व्यूपोर्ट स्क्रीनशॉट बनाएगा। जब सक्षम किया जाता है तो यह [`--clipSelector`](#clipselector) के आधार पर एलिमेंट स्क्रीनशॉट बनाएगा जो कंपोनेंट स्क्रीनशॉट के आसपास के सफेद स्थान की मात्रा को कम करेगा और स्क्रीनशॉट आकार को कम करेगा।

#### `--clipSelector`

-   **प्रकार:** `string`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** Storybook V7 के लिए `#storybook-root > :first-child` और Storybook V6 के लिए `#root > :first-child:not(script):not(style)`, [`--version`](#version) भी देखें
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --clipSelector="#some-id"`

यह वह सिलेक्टर है जिसका उपयोग किया जाएगा:

-   स्क्रीनशॉट लेने के लिए एलिमेंट का चयन करने के लिए
-   स्क्रीनशॉट लेने से पहले एलिमेंट को दृश्यमान होने की प्रतीक्षा करने के लिए

#### `--devices`

-   **प्रकार:** `string`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** आप [`deviceDescriptors.ts`](https://github.com/webdriverio/visual-testing/blob/main/./packages/service/src/storybook/deviceDescriptors.ts) से चुन सकते हैं
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --devices="iPhone 14 Pro Max","Pixel 3 XL"`
-   **नोट:** केवल CLI के माध्यम से उपलब्ध है

यह कंपोनेंट स्क्रीनशॉट लेने के लिए [`deviceDescriptors.ts`](https://github.com/webdriverio/visual-testing/blob/main/./packages/service/src/storybook/deviceDescriptors.ts) से मेल खाने वाले प्रदान किए गए डिवाइसों का उपयोग करेगा

> [!NOTE]
>
> -   अगर आपको कोई डिवाइस कॉन्फिग नहीं मिलता है, तो [फीचर अनुरोध](https://github.com/webdriverio/visual-testing/issues/new?assignees=&labels=&projects=&template=--feature-request.md) सबमिट करने के लिए स्वतंत्र महसूस करें
> -   यह केवल Chrome के साथ काम करेगा:
>     -   अगर आप `--devices` प्रदान करते हैं तो सभी Chrome इंस्टेंस **मोबाइल एम्युलेशन** मोड में चलेंगे
>     -   अगर आप Chrome के अलावा अन्य ब्राउज़र भी प्रदान करते हैं, जैसे `--devices --browsers=firefox,safari,edge` तो यह स्वचालित रूप से मोबाइल एम्युलेशन मोड में Chrome जोड़ देगा
> -   Storybook रनर डिफ़ॉल्ट रूप से एलिमेंट स्नैपशॉट बनाएगा, अगर आप पूरा मोबाइल एम्युलेटेड स्क्रीनशॉट देखना चाहते हैं तो कमांड लाइन के माध्यम से `--clip=false` प्रदान करें
> -   फ़ाइल नाम उदाहरण के लिए ऐसा दिखेगा `__snapshots__/example/button/desktop_chrome/example-button--large-local-chrome-iPhone-14-Pro-Max-430x932-dpr-3.png`
> -   **[SRC:](https://chromedriver.chromium.org/mobile-emulation#h.p_ID_167)** मोबाइल एम्युलेशन का उपयोग करके डेस्कटॉप पर मोबाइल वेबसाइट का परीक्षण करना उपयोगी हो सकता है, लेकिन परीक्षकों को पता होना चाहिए कि कई सूक्ष्म अंतर हैं जैसे:
>     -   पूरी तरह से अलग GPU, जो बड़े प्रदर्शन परिवर्तन का कारण बन सकता है;
>     -   मोबाइल UI का अनुकरण नहीं किया जाता है (विशेष रूप से, हाइडिंग url बार पेज की ऊंचाई को प्रभावित करता है);
>     -   विभेदन पॉपअप (जहां आप कुछ टच टारगेट में से एक का चयन करते हैं) समर्थित नहीं है;
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

यह समानांतर इंस्टेंसेस की संख्या होगी जिनका उपयोग स्टोरीज चलाने के लिए किया जाएगा। यह आपकी `wdio.conf`-फ़ाइल में `maxInstances` द्वारा सीमित होगा।

> [!IMPORTANT]
> `headless`-मोड में चलते समय संसाधन प्रतिबंधों के कारण अस्थिरता को रोकने के लिए संख्या को 20 से अधिक न बढ़ाएं

#### `--skipStories`

-   **प्रकार:** `string|regex`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** null
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --skipStories="/.*button.*/gm"`

यह हो सकता है:

-   एक स्ट्रिंग (`example-button--secondary,example-button--small`)
-   या एक रेगेक्स (`"/.*button.*/gm"`)

कुछ स्टोरीज को छोड़ने के लिए। स्टोरी के `id` का उपयोग करें जिसे स्टोरी के URL में पाया जा सकता है। उदाहरण के लिए, इस URL `http://localhost:6006/?path=/story/example-page--logged-out` में `id` `example-page--logged-out` है

#### `--url`

-   **प्रकार:** `string`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** `http://127.0.0.1:6006`
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --url="https://example.com"`

वह URL जहां आपका Storybook इंस्टेंस होस्ट किया गया है।

#### `--version`

-   **प्रकार:** `number`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** 7
-   **उदाहरण:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --version=6`

यह Storybook का वर्शन है, यह डिफ़ॉल्ट रूप से `7` है। यह जानने के लिए आवश्यक है कि क्या V6 [`clipSelector`](#clipselector) का उपयोग किया जाना चाहिए।

### Storybook इंटरैक्शन टेस्टिंग

Storybook इंटरैक्शन टेस्टिंग आपको WDIO कमांड्स के साथ कस्टम स्क्रिप्ट बनाकर अपने कंपोनेंट के साथ इंटरैक्ट करने की अनुमति देता है, जिससे कंपोनेंट को एक निश्चित स्थिति में सेट किया जा सकता है। उदाहरण के लिए, नीचे दिए गए कोड स्निपेट को देखें:

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

दो अलग-अलग कंपोनेंट्स पर दो टेस्ट चलाए जाते हैं। प्रत्येक टेस्ट पहले एक स्थिति सेट करता है और फिर स्क्रीनशॉट लेता है। आप यह भी नोटिस करेंगे कि एक नया कस्टम कमांड पेश किया गया है, जिसे [यहां](#new-custom-command) पाया जा सकता है।

उपरोक्त स्पेक फाइल को एक फोल्डर में सहेजा जा सकता है और निम्न कमांड के साथ कमांड लाइन में जोड़ा जा सकता है:

```sh
pnpm run test.local.desktop.storybook.localhost -- --spec='tests/specs/storybook-interaction/*.ts'
```

Storybook रनर पहले स्वचालित रूप से आपके Storybook इंस्टेंस को स्कैन करेगा और फिर आपके टेस्ट को उन स्टोरीज में जोड़ देगा जिन्हें तुलना करने की आवश्यकता है। अगर आप नहीं चाहते कि वे कंपोनेंट्स जिन्हें आप इंटरैक्शन टेस्टिंग के लिए उपयोग करते हैं, दो बार तुलना की जाए, तो आप [`--skipStories`](#--skipstories) फिल्टर प्रदान करके स्कैन से "डिफ़ॉल्ट" स्टोरीज को हटाने के लिए एक फिल्टर जोड़ सकते हैं। यह इस तरह दिखेगा:

```sh
pnpm run test.local.desktop.storybook.localhost -- --skipStories="/example-page.*/gm" --spec='tests/specs/storybook-interaction/*.ts'
```

### नया कस्टम कमांड

एक नया कस्टम कमांड जिसे `browser.waitForStorybookComponentToBeLoaded({ id: 'componentId' })` कहा जाता है, `browser/driver`-ऑब्जेक्ट में जोड़ा जाएगा जो स्वचालित रूप से कंपोनेंट को लोड करेगा और इसके पूरा होने का इंतजार करेगा, इसलिए आपको `browser.url('url.com')` विधि का उपयोग करने की आवश्यकता नहीं है। इसका उपयोग इस प्रकार किया जा सकता है

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

यह Storybook URL में अतिरिक्त खोज पैरामीटर जोड़ देगा, ऊपर के उदाहरण में URL `http://storybook.url/iframe.html?id=story-id&foo=bar&abc=def` होगा।
अधिक जानकारी के लिए [URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) दस्तावेज़ीकरण देखें।

#### `clipSelector`

-   **प्रकार:** `string`
-   **अनिवार्य:** नहीं
-   **डिफ़ॉल्ट:** Storybook V7 के लिए `#storybook-root > :first-child` और Storybook V6 के लिए `#root > :first-child:not(script):not(style)`
-   **उदाहरण:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    clipSelector: "#your-selector",
    id: "componentId",
});
```

यह वह सिलेक्टर है जिसका उपयोग किया जाएगा:

-   स्क्रीनशॉट लेने के लिए एलिमेंट का चयन करने के लिए
-   स्क्रीनशॉट लेने से पहले एलिमेंट को दृश्यमान होने की प्रतीक्षा करने के लिए

#### `id`

-   **प्रकार:** `string`
-   **अनिवार्य:** हां
-   **उदाहरण:**

```ts
await browser.waitForStorybookComponentToBeLoaded({ '#your-selector', id: 'componentId' })
```

स्टोरी के `id` का उपयोग करें जिसे स्टोरी के URL में पाया जा सकता है। उदाहरण के लिए, इस URL `http://localhost:6006/?path=/story/example-page--logged-out` में `id` `example-page--logged-out` है

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

अधिकतम टाइमआउट जितने हम पेज पर लोडिंग के बाद एक कंपोनेंट के दृश्यमान होने का इंतजार करना चाहते हैं

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

वह URL जहां आपका Storybook इंस्टेंस होस्ट किया गया है।

</details>

## योगदान

### पैकेज अपडेट करना

आप एक सरल CLI टूल के साथ पैकेज अपडेट कर सकते हैं। सुनिश्चित करें कि आपने सभी निर्भरताओं को इंस्टॉल किया है, फिर आप चला सकते हैं

```sh
pnpm update.packages
```

यह एक CLI ट्रिगर करेगा जो आपसे निम्नलिखित प्रश्न पूछेगा

```logs
==========================
🤖 Package update Wizard 🧙
==========================

? Which version target would you like to update to? (Minor|Latest)
? Do you want to update the package.json files? (Y/n)
? Do you want to remove all "node_modules" and reinstall dependencies? (Y/n)
? Would you like reinstall the dependencies? (Y/n)
```

इसके परिणामस्वरूप निम्नलिखित लॉग होंगे

<details>
    <summary>लॉग का एक उदाहरण देखने के लिए खोलें</summary>
    
```logs
==========================
🤖 Package update Wizard 🧙
==========================

? Which version target would you like to update to? Minor
? Do you want to update the package.json files? yes
Updating root 'package.json' for minor updates...
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/package.json
[====================] 38/38 100%

@typescript-eslint/eslint-plugin ^8.7.0 → ^8.8.0
@typescript-eslint/parser ^8.7.0 → ^8.8.0
@typescript-eslint/utils ^8.7.0 → ^8.8.0
@vitest/coverage-v8 ^2.1.1 → ^2.1.2
vitest ^2.1.1 → ^2.1.2

Run pnpm install to install new versions.
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing/packages/ocr-service...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/packages/ocr-service/package.json
[====================] 11/11 100%

All dependencies match the minor package versions :)
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing/packages/visual-reporter...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/packages/visual-reporter/package.json
[====================] 11/11 100%

eslint-config-next 14.2.13 → 14.2.14
next 14.2.13 → 14.2.14

Run pnpm install to install new versions.
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing/packages/visual-service...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/packages/visual-service/package.json
[====================] 5/5 100%

All dependencies match the minor package versions :)
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing/packages/webdriver-image-comparison...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/packages/webdriver-image-comparison/package.json
[====================] 8/8 100%

All dependencies match the minor package versions :)
? Do you want to remove all "node_modules" and reinstall dependencies? yes
Removing root dependencies in /Users/wswebcreation/Git/wdio/visual-testing...
Removing dependencies in ocr-service...
Removing dependencies in visual-reporter...
Removing dependencies in visual-service...
Removing dependencies in webdriver-image-comparison...
? Would you like reinstall the dependencies? yes
Installing dependencies in /Users/wswebcreation/Git/wdio/visual-testing...

> @wdio/visual-testing-monorepo@ pnpm.install.workaround /Users/wswebcreation/Git/wdio/visual-testing
> pnpm install --shamefully-hoist

Scope: all 5 workspace projects
Lockfile is up to date, resolution step is skipped
Packages: +1274
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Progress: resolved 1274, reused 1265, downloaded 0, added 1274, done

dependencies:

-   @wdio/ocr-service 2.0.0 <- packages/ocr-service
-   @wdio/visual-service 6.0.0 <- packages/visual-service

devDependencies:

-   @changesets/cli 2.27.8
-   @inquirer/prompts 5.5.0
-   @tsconfig/node20 20.1.4
-   @types/eslint 9.6.1
-   @types/jsdom 21.1.7
-   @types/node 20.16.4
-   @types/react 18.3.5
-   @types/react-dom 18.3.0
-   @types/xml2js 0.4.14
-   @typescript-eslint/eslint-plugin 8.8.0
-   @typescript-eslint/parser 8.8.0
-   @typescript-eslint/utils 8.8.0
-   @vitest/coverage-v8 2.1.2
-   @wdio/appium-service 9.1.2
-   @wdio/cli 9.1.2
-   @wdio/globals 9.1.2
-   @wdio/local-runner 9.1.2
-   @wdio/mocha-framework 9.1.2
-   @wdio/sauce-service 9.1.2
-   @wdio/shared-store-service 9.1.2
-   @wdio/spec-reporter 9.1.2
-   @wdio/types 9.1.2
-   eslint 9.11.1
-   eslint-plugin-import 2.30.0
-   eslint-plugin-unicorn 55.0.0
-   eslint-plugin-wdio 9.0.8
-   husky 9.1.6
-   jsdom 25.0.1
-   pnpm-run-all2 6.2.3
-   release-it 17.6.0
-   rimraf 6.0.1
-   saucelabs 8.0.0
-   ts-node 10.9.2
-   typescript 5.6.2
-   vitest 2.1.2
-   webdriverio 9.1.2

. prepare$ husky
└─ Done in 204ms
Done in 9.5s
All packages updated!

````

</details>

### प्रश्न

कृपया इस प्रोजेक्ट में योगदान देने के बारे में किसी भी प्रश्न या समस्या के लिए हमारे [Discord](https://discord.webdriver.io) सर्वर से जुड़ें। हमें योगदानकर्ताओं को `🙏-contributing` चैनल में पा सकते हैं।

### समस्याएँ

अगर आपके प्रश्न, बग या फीचर अनुरोध हैं, तो कृपया एक मुद्दा दर्ज करें। इश्यू जमा करने से पहले, कृपया डुप्लिकेट को कम करने में मदद के लिए इश्यू आर्काइव खोजें और [FAQ](https://webdriver.io/docs/visual-testing/faq/) पढ़ें।

अगर आप इसे वहां नहीं पा सकते हैं तो आप एक इश्यू सबमिट कर सकते हैं जहां आप सबमिट कर सकते हैं:

-   🐛**बग रिपोर्ट**: हमें सुधार करने में मदद करने के लिए एक रिपोर्ट बनाएं
-   📖**दस्तावेज़ीकरण**: सुधार का सुझाव दें या गायब/अस्पष्ट दस्तावेज़ीकरण की रिपोर्ट करें।
-   💡**फीचर अनुरोध**: इस मॉड्यूल के लिए एक विचार सुझाएं।
-   💬**प्रश्न**: प्रश्न पूछें।

### विकास वर्कफ़्लो

इस प्रोजेक्ट के लिए एक PR बनाने और योगदान शुरू करने के लिए इस चरण-दर-चरण गाइड का पालन करें:

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

    प्रोजेक्ट को बिल्ड करने के लिए, चलाएँ:

    ```sh
    $ pnpm build
    ```

-   सुनिश्चित करें कि आपके परिवर्तन किसी भी टेस्ट को तोड़ते नहीं हैं, चलाएँ:

    ```sh
    $ pnpm test
    ```

यह प्रोजेक्ट [changesets](https://github.com/changesets/changesets) का उपयोग करता है ताकि स्वचालित रूप से चेंजलॉग और रिलीज बनाए जा सकें।

### टेस्टिंग

मॉड्यूल का परीक्षण करने के लिए कई परीक्षण निष्पादित करने की आवश्यकता होती है। PR जोड़ते समय सभी परीक्षणों को कम से कम स्थानीय परीक्षणों को पास करना होगा। प्रत्येक PR का स्वचालित रूप से Sauce Labs के खिलाफ परीक्षण किया जाता है, [हमारी GitHub Actions पाइपलाइन](https://github.com/webdriverio/visual-testing/actions/workflows/tests.yml) देखें। PR को अनुमोदित करने से पहले मुख्य योगदानकर्ता PR का परीक्षण एमुलेटर/सिमुलेटर / वास्तविक डिवाइसों के खिलाफ करेंगे।

#### स्थानीय परीक्षण

सबसे पहले, एक स्थानीय बेसलाइन बनाने की आवश्यकता है। यह इसके साथ किया जा सकता है:

```sh
// With the webdriver protocol
$ pnpm run test.local.init
````

यह कमांड `localBaseline` नामक एक फोल्डर बनाएगा जो सभी बेसलाइन छवियों को रखेगा।

फिर चलाएँ:

```sh
// With the webdriver protocol
pnpm run test.local.desktop
```

यह Chrome पर स्थानीय मशीन पर सभी परीक्षण चलाएगा।

#### स्थानीय Storybook रनर परीक्षण (बीटा)

सबसे पहले, एक स्थानीय बेसलाइन बनाने की आवश्यकता है। यह इसके साथ किया जा सकता है:

```sh
pnpm run test.local.desktop.storybook
```

यह हेडलेस मोड में Chrome के साथ Storybook टेस्ट https://govuk-react.github.io/govuk-react/ पर स्थित एक डेमो Storybook रेपो के खिलाफ चलाएगा।

अधिक ब्राउज़रों के साथ टेस्ट चलाने के लिए आप चला सकते हैं

```sh
pnpm run test.local.desktop.storybook -- --browsers=chrome,firefox,edge,safari
```

> [!NOTE]
> सुनिश्चित करें कि आपने अपने स्थानीय मशीन पर उन ब्राउज़रों को इंस्टॉल किया है जिन पर आप चलाना चाहते हैं

#### Sauce Labs के साथ CI टेस्टिंग (PR के लिए आवश्यक नहीं)

नीचे दिए गए कमांड का उपयोग GitHub Actions पर बिल्ड का परीक्षण करने के लिए किया जाता है, इसका उपयोग केवल वहां किया जा सकता है और स्थानीय विकास के लिए नहीं।

```
$ pnpm run test.saucelabs
```

यह कई कॉन्फ़िगरेशन के खिलाफ परीक्षण करेगा जिन्हें [यहां](https://github.com/webdriverio/visual-testing/blob/main/./tests/configs/wdio.saucelabs.web.conf.ts) पाया जा सकता है।
सभी PR को स्वचालित रूप से Sauce Labs के खिलाफ जांचा जाता है।

## रिलीज़िंग

ऊपर सूचीबद्ध किसी भी पैकेज के एक वर्शन को रिलीज़ करने के लिए, निम्नलिखित करें:

-   [रिलीज़ पाइपलाइन](https://github.com/webdriverio/visual-testing/actions/workflows/release.yml) ट्रिगर करें
-   एक रिलीज़ PR जेनरेट किया जाता है, इसकी समीक्षा करवाएं और किसी अन्य WebdriverIO सदस्य द्वारा अनुमोदित करवाएं
-   PR को मर्ज करें
-   [रिलीज़ पाइपलाइन](https://github.com/webdriverio/visual-testing/actions/workflows/release.yml) फिर से ट्रिगर करें
-   एक नया वर्शन रिलीज़ किया जाना चाहिए 🎉

## आभार

`@wdio/visual-testing` [LambdaTest](https://www.lambdatest.com/) और [Sauce Labs](https://saucelabs.com/) से एक ओपन-सोर्स लाइसेंस का उपयोग करता है।