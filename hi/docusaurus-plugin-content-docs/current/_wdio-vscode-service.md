---
id: wdio-vscode-service
title: VSCode एक्सटेंशन टेस्टिंग सर्विस
custom_edit_url: https://github.com/webdriverio-community/wdio-vscode-service/edit/main/README.md
---


> wdio-vscode-service एक तृतीय पक्ष पैकेज है, अधिक जानकारी के लिए कृपया देखें [GitHub](https://github.com/webdriverio-community/wdio-vscode-service) | [npm](https://www.npmjs.com/package/wdio-vscode-service)

इस पर परीक्षण किया गया:

[![VSCode Version](https://img.shields.io/badge/VSCode%20Version-insiders%20/%20stable%20/%20v1.86.0%20/%20web-brightgreen)](https://github.com/webdriverio-community/wdio-vscode-service/actions/workflows/ci.yml) [![CI Status](https://img.shields.io/badge/Platform-windows%20%2F%20macos%20%2F%20ubuntu-brightgreen)](https://github.com/webdriverio-community/wdio-vscode-service/actions/workflows/ci.yml)

> VSCode एक्सटेंशन के परीक्षण के लिए WebdriverIO सेवा।

यह WebdriverIO सेवा आपको VSCode डेस्कटॉप IDE या वेब एक्सटेंशन के रूप में अपने VSCode एक्सटेंशन का सहज ढंग से एंड-टू-एंड परीक्षण करने की अनुमति देती है। आपको केवल अपने एक्सटेंशन का पथ प्रदान करना होगा और सेवा निम्न द्वारा बाकी काम करती है:

- 🏗️ VSCode इंस्टॉल करना (`stable`, `insiders` या निर्दिष्ट संस्करण)
- ⬇️ दिए गए VSCode संस्करण के लिए विशिष्ट Chromedriver डाउनलोड करना
- 🚀 आपको अपने परीक्षणों से VSCode API तक पहुँचने में सक्षम बनाता है
- 🖥️ VSCode को कस्टम उपयोगकर्ता सेटिंग्स के साथ शुरू करना (Ubuntu, MacOS और Windows पर VSCode के लिए समर्थन सहित)
- 🌐 या VSCode को सर्वर से परोसता है ताकि [वेब एक्सटेंशन](https://code.visualstudio.com/api/extension-guides/web-extensions) के परीक्षण के लिए किसी भी ब्राउज़र द्वारा एक्सेस किया जा सके
- 📔 आपके VSCode संस्करण से मेल खाने वाले लोकेटर्स के साथ पेज ऑब्जेक्ट्स को बूटस्ट्रैप करना

यह प्रोजेक्ट [vscode-extension-tester](https://www.npmjs.com/package/vscode-extension-tester) प्रोजेक्ट से अत्यधिक प्रेरित है जो Selenium पर आधारित है। यह पैकेज उस आइडिया को लेकर WebdriverIO के लिए अनुकूलित करता है।

VSCode v1.86 से शुरू करके, बिना किसी कॉन्फ़िगरेशन के Chromedriver इंस्टॉल करने के लिए `webdriverio` v8.14 या उससे नए संस्करण का उपयोग करना आवश्यक है। यदि आपको VSCode के पहले संस्करणों का परीक्षण करने की आवश्यकता है, तो नीचे दिए गए [Chromedriver कॉन्फ़िगरेशन](#chromedriver) अनुभाग देखें।

## इंस्टालेशन

एक नया WebdriverIO प्रोजेक्ट शुरू करने के लिए, चलाएँ:

```bash
npm create wdio ./
```

एक इंस्टालेशन विज़ार्ड आपको प्रक्रिया के माध्यम से मार्गदर्शन करेगा। सुनिश्चित करें कि आप कंपाइलर के रूप में TypeScript का चयन करें और इसे आपके लिए पेज ऑब्जेक्ट्स उत्पन्न न करने दें, क्योंकि यह प्रोजेक्ट सभी आवश्यक पेज ऑब्जेक्ट्स के साथ आता है। फिर सुनिश्चित करें कि आप सेवाओं की सूची में `vscode` का चयन करें:

![Install Demo](https://raw.githubusercontent.com/webdriverio-community/wdio-vscode-service/main/.github/assets/demo.gif "Install Demo")

`WebdriverIO` को इंस्टॉल करने के तरीके के बारे में अधिक जानकारी के लिए, कृपया [प्रोजेक्ट डॉक्स](https://webdriver.io/docs/gettingstarted) देखें।

## उदाहरण कॉन्फ़िगरेशन

सेवा का उपयोग करने के लिए आपको अपनी सेवाओं की सूची में `vscode` जोड़ना होगा, इसके बाद वैकल्पिक रूप से एक कॉन्फ़िगरेशन ऑब्जेक्ट जोड़ना होगा। यह WebdriverIO को दिए गए VSCode बाइनरीज़ और उपयुक्त Chromedriver संस्करण डाउनलोड करने के लिए प्रेरित करेगा:

```js
// wdio.conf.ts
export const config = {
    outputDir: 'trace',
    // ...
    capabilities: [{
        browserName: 'vscode',
        browserVersion: '1.86.0', // "insiders" or "stable" for latest VSCode version
        'wdio:vscodeOptions': {
            extensionPath: __dirname,
            userSettings: {
                "editor.fontSize": 14
            }
        }
    }],
    services: ['vscode'],
    /**
     * Optionally define the path WebdriverIO stores all VSCode binaries, e.g.:
     * services: [['vscode', { cachePath: __dirname }]]
     */
    // ...
};
```

यदि आप `wdio:vscodeOptions` को `vscode` के अलावा किसी अन्य `browserName` के साथ परिभाषित करते हैं, जैसे `chrome`, तो सेवा एक्सटेंशन को वेब एक्सटेंशन के रूप में परोसेगी। यदि आप Chrome पर परीक्षण करते हैं तो कोई अतिरिक्त ड्राइवर सेवा की आवश्यकता नहीं है, उदाहरण के लिए:

```js
// wdio.conf.ts
export const config = {
    outputDir: 'trace',
    // ...
    capabilities: [{
        browserName: 'chrome',
        'wdio:vscodeOptions': {
            extensionPath: __dirname
        }
    }],
    services: ['vscode'],
    // ...
};
```

_नोट:_ वेब एक्सटेंशन का परीक्षण करते समय आप केवल `browserVersion` के रूप में `stable` या `insiders` के बीच चुन सकते हैं।

### TypeScript सेटअप

अपने `tsconfig.json` में सुनिश्चित करें कि आपने अपने प्रकारों की सूची में `wdio-vscode-service` जोड़ दिया है:

```json
{
    "compilerOptions": {
        "types": [
            "node",
            "webdriverio/async",
            "@wdio/mocha-framework",
            "expect-webdriverio",
            "wdio-vscode-service"
        ],
        "target": "es2019",
        "moduleResolution": "node",
        "esModuleInterop": true
    }
}
```

## उपयोग

आप फिर अपने वांछित VSCode संस्करण से मेल खाने वाले लोकेटर्स के लिए पेज ऑब्जेक्ट्स तक पहुंचने के लिए `getWorkbench` विधि का उपयोग कर सकते हैं:

```ts
describe('WDIO VSCode Service', () => {
    it('should be able to load VSCode', async () => {
        const workbench = await browser.getWorkbench()
        expect(await workbench.getTitleBar().getTitle())
            .toBe('[Extension Development Host] - README.md - wdio-vscode-service - Visual Studio Code')
    })
})
```

### VSCode API तक पहुंच

यदि आप [VSCode API](https://code.visualstudio.com/api/references/vscode-api) के माध्यम से कुछ स्वचालन निष्पादित करना चाहते हैं, तो आप कस्टम `executeWorkbench` कमांड के माध्यम से रिमोट कमांड चला सकते हैं। यह कमांड आपको अपने परीक्षण से VSCode वातावरण के अंदर दूरस्थ रूप से कोड निष्पादित करने की अनुमति देता है और आपको VSCode API तक पहुंचने में सक्षम बनाता है। आप फ़ंक्शन में मनमाने पैरामीटर पास कर सकते हैं जो फिर फ़ंक्शन में प्रचारित किए जाएंगे। `vscode` ऑब्जेक्ट हमेशा बाहरी फ़ंक्शन पैरामीटर के बाद पहले आर्गुमेंट के रूप में पास किया जाएगा। ध्यान दें कि आप फ़ंक्शन स्कोप के बाहर चर तक नहीं पहुंच सकते क्योंकि कॉलबैक दूरस्थ रूप से निष्पादित किया जाता है। यहां एक उदाहरण है:

```ts
const workbench = await browser.getWorkbench()
await browser.executeWorkbench((vscode, param1, param2) => {
    vscode.window.showInformationMessage(`I am an ${param1} ${param2}!`)
}, 'API', 'call')

const notifs = await workbench.getNotifications()
console.log(await notifs[0].getMessage()) // outputs: "I am an API call!"
```

पूर्ण पेज ऑब्जेक्ट दस्तावेज़ीकरण के लिए, [डॉक्स](https://webdriverio-community.github.io/wdio-vscode-service/modules.html) देखें। आप इस [प्रोजेक्ट के परीक्षण सूट](https://github.com/webdriverio-community/wdio-vscode-service/blob/main/test/specs) में विभिन्न उपयोग उदाहरण पा सकते हैं।

## कॉन्फ़िगरेशन

सेवा कॉन्फ़िगरेशन के माध्यम से, आप VSCode संस्करण के साथ-साथ VSCode के लिए उपयोगकर्ता सेटिंग्स को प्रबंधित कर सकते हैं:

### सेवा विकल्प

सेवा विकल्प वे विकल्प हैं जो परीक्षण वातावरण स्थापित करने के लिए सेवा के लिए आवश्यक हैं।

#### `cachePath`

VS Code बंडल को फिर से डाउनलोड करने से बचने के लिए कैश पथ को परिभाषित करें। यह CI/CD के लिए उपयोगी है ताकि हर परीक्षण रन के लिए VSCode को फिर से डाउनलोड करने से बचा जा सके।

प्रकार: `string`<br />
डिफ़ॉल्ट: `process.cwd()`

### VSCode क्षमताएँ (`wdio:vscodeOptions`)

VSCode के माध्यम से परीक्षण चलाने के लिए आपको `browserName` के रूप में `vscode` को परिभाषित करना होगा। आप `browserVersion` क्षमता प्रदान करके VSCode संस्करण निर्दिष्ट कर सकते हैं। कस्टम VSCode विकल्प तब कस्टम `wdio:vscodeOptions` क्षमता के भीतर परिभाषित किए जाते हैं। विकल्प निम्नलिखित हैं:

#### `binary`

स्थानीय रूप से इंस्टॉल किए गए VSCode इंस्टालेशन का पथ। यदि विकल्प प्रदान नहीं किया गया है तो सेवा दिए गए `browserVersion` (या यदि नहीं दिया गया है तो `stable`) के आधार पर VSCode डाउनलोड करेगी।

प्रकार: `string`

#### `extensionPath`

उस एक्सटेंशन के लिए डायरेक्टरी को परिभाषित करें जिसका आप परीक्षण करना चाहते हैं।

प्रकार: `string`

#### `storagePath`

VS Code के लिए अपने सभी डेटा को संग्रहीत करने के लिए एक कस्टम स्थान परिभाषित करें। यह आंतरिक VS Code डायरेक्टरीज के लिए रूट है जैसे (आंशिक सूची)
* **user-data-dir**: वह डायरेक्टरी जहां सभी उपयोगकर्ता सेटिंग्स (वैश्विक सेटिंग्स), एक्सटेंशन लॉग आदि संग्रहीत हैं।
* **extension-install-dir**: वह डायरेक्टरी जहां VS Code एक्सटेंशन इंस्टॉल किए गए हैं।

यदि प्रदान नहीं किया गया है, तो एक अस्थायी डायरेक्टरी का उपयोग किया जाता है।

प्रकार: `string`

#### `userSettings`

VSCode पर लागू करने के लिए कस्टम उपयोगकर्ता सेटिंग्स को परिभाषित करें।

प्रकार: `Record<string, number | string | object | boolean>`<br />
डिफ़ॉल्ट: `{}`

#### `workspacePath`

एक विशिष्ट वर्कस्पेस के लिए VSCode खोलता है। यदि प्रदान नहीं किया गया है तो VSCode बिना किसी वर्कस्पेस के खुलता है।

प्रकार: `string`

#### `filePath`

एक विशिष्ट फ़ाइल के साथ खुले VSCode खोलता है।

प्रकार: `string`

#### `vscodeArgs`

एक ऑब्जेक्ट के रूप में अतिरिक्त स्टार्ट-अप आर्गुमेंट्स, उदाहरण के लिए

```ts
vscodeArgs: { fooBar: true, 'bar-foo': '/foobar' }
```

इस प्रकार पास किया जाएगा:

```ts
--foo-bar --fooBar --bar-foo=/foobar
```

प्रकार: `Record<string, string | boolean>`<br />
डिफ़ॉल्ट: देखें [`constants.ts#L5-L14`](https://github.com/webdriverio-community/wdio-vscode-service/blob/196a69be3ac2fb82d9c7e4f19a2a1c8ccbaec1e2/src/constants.ts#L5-L14)

#### `verboseLogging`

यदि true पर सेट है, तो सेवा एक्सटेंशन होस्ट और कंसोल API से VSCode आउटपुट लॉग करता है।

प्रकार: `boolean`<br />
डिफ़ॉल्ट: `false`

#### `vscodeProxyOptions`

VSCode API प्रॉक्सी कॉन्फ़िगरेशन्स परिभाषित करती हैं कि WebdriverIO VSCode API तक पहुंच प्रदान करने के लिए VSCode वर्कबेंच से कैसे जुड़ता है।

प्रकार: `VSCodeProxyOptions`<br />
डिफ़ॉल्ट:

```ts
{
    /**
     * If set to true, the service tries to establish a connection with the
     * VSCode workbench to enable access to the VSCode API
     */
    enable: true,
    /**
     * Port of the WebSocket connection used to connect to the workbench.
     * By default set to an available port in your operating system.
     */
    // port?: number
    /**
     * Timeout for connecting to WebSocket inside of VSCode
     */
    connectionTimeout: 5000,
    /**
     * Timeout for command to be executed within VSCode
     */
    commandTimeout: 5000
}
```

### Chromedriver

VSCode v1.86 से शुरू करके, बिना किसी कॉन्फ़िगरेशन के Chromedriver इंस्टॉल करने के लिए `webdriverio` v8.14 या उससे नए संस्करण का उपयोग करना आवश्यक है। [सरलीकृत ब्राउज़र ऑटोमेशन सेटअप](https://webdriver.io/blog/2023/07/31/driver-management) आपके लिए सब कुछ संभालता है।

VS Code के पहले संस्करणों का परीक्षण करने के लिए, लॉग से अपेक्षित Chromedriver संस्करण का पता लगाएं, [Chromedriver](https://chromedriver.chromium.org/downloads) डाउनलोड करें, और पथ कॉन्फ़िगर करें। उदाहरण के लिए:

```
[0-0] ERROR webdriver: Failed downloading chromedriver v108: Download failed: ...
```

```ts
    capabilities: [{
        browserName: 'vscode',
        browserVersion: '1.80.0',
        'wdio:chromedriverOptions': {
            binary: path.join(cacheDir, 'chromedriver-108.0.5359.71')
```

## अपने खुद के PageObjects बनाएं

आप अपने स्वयं के समीक्षा पेज ऑब्जेक्ट्स के लिए इस सेवा में उपयोग किए गए कंपोनेंट्स का पुन: उपयोग कर सकते हैं। इसके लिए पहले एक फ़ाइल बनाएं जो आपके सभी सेलेक्टर्स को परिभाषित करती है, उदाहरण के लिए:

```ts
// e.g. in /test/pageobjects/locators.ts
export const componentA = {
    elem: 'form', // component container element
    submit: 'button[type="submit"]', // submit button
    username: 'input.username', // username input
    password: 'input.password' // password input
}
```

अब आप निम्नानुसार एक पेज ऑब्जेक्ट बना सकते हैं:

```ts
// e.g. in /test/pageobjects/loginForm.ts
import { PageDecorator, IPageDecorator, BasePage } from 'wdio-vscode-service'
import * as locatorMap, { componentA as componentALocators } from './locators'
export interface LoginForm extends IPageDecorator<typeof componentALocators> {}
@PageDecorator(componentALocators)
export class LoginForm extends BasePage<typeof componentALocators, typeof locatorMap> {
    /**
     * @private locator key to identify locator map (see locators.ts)
     */
    public locatorKey = 'componentA' as const

    public login (username: string, password: string) {
        await this.username$.setValue(username)
        await this.password$.setValue(password)
        await this.submit$.click()
    }
}
```

अब अपने परीक्षण में, आप अपने पेज ऑब्जेक्ट का निम्न प्रकार से उपयोग कर सकते हैं:

```ts
import { LoginForm } from '../pageobjects/loginForm'
import * as locatorMap from '../locators'

// e.g. in /test/specs/example.e2e.ts
describe('my extension', () => {
    it('should login', async () => {
        const loginForm = new LoginForm(locatorMap)
        await loginForm.login('admin', 'test123')

        // you can also use page object elements directly via `[selector]$`
        // or `[selector]$$`, e.g.:
        await loginForm.submit$.click()

        // or access locators directly
        console.log(loginForm.locators.username)
        // outputs: "input.username"
    })
})
```

## TypeScript समर्थन

यदि आप WebdriverIO को TypeScript के साथ उपयोग करते हैं, तो सुनिश्चित करें कि आपने अपने `tsconfig.json` में `types` में `wdio-vscode-service` को जोड़ दिया है, उदाहरण के लिए:

```json
{
    "compilerOptions": {
        "moduleResolution": "node",
        "types": [
            "webdriverio/async",
            "@wdio/mocha-framework",
            "expect-webdriverio",
            // add this service to your types
            "wdio-devtools-service"
        ],
        "target": "es2019"
    }
}
```

## प्रॉक्सी समर्थन

इस सेवा की शुरुआत के दौरान, एक ChromeDriver और VSCode वितरण डाउनलोड किया जाता है। आप `HTTPS_PROXY` या `https_proxy` पर्यावरण चर सेट करके प्रॉक्सी के माध्यम से इन अनुरोधों को टनल कर सकते हैं। उदाहरण के लिए:

```bash
HTTPS_PROXY=http://127.0.0.1:1080 npm run wdio
```

## संदर्भ

निम्नलिखित VS Code एक्सटेंशन `wdio-vscode-service` का उपयोग करते हैं:

- [Marquee](https://marketplace.visualstudio.com/items?itemName=stateful.marquee) (27k डाउनलोड)
- [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) (27.8m डाउनलोड)
- [DVC Extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=Iterative.dvc) (11.2k डाउनलोड)
- [Nx Console](https://marketplace.visualstudio.com/items?itemName=nrwl.angular-console) (1.2m डाउनलोड)
- [inlang – i18n supercharged](https://marketplace.visualstudio.com/items?itemName=inlang.vs-code-extension) (3k डाउनलोड)

## योगदान

पुल रिक्वेस्ट पोस्ट करने से पहले, कृपया निम्न चलाएँ:

1. `git clone git@github.com:webdriverio-community/wdio-vscode-service.git`
1. `cd wdio-vscode-service`
1. `npm install`
1. `npm run build`
1. `npm run test` (या `npm run ci`)

## अधिक जानें

यदि आप VSCode एक्सटेंशन के परीक्षण के बारे में अधिक जानना चाहते हैं, तो [OpenJS World 2022](https://www.youtube.com/watch?v=PhGNTioBUiU) में [Christian Bromann](https://twitter.com/bromann) की बात देखें:

[![Testing VSCode Extensions at OpenJS World 2022](https://img.youtube.com/vi/PhGNTioBUiU/sddefault.jpg)](https://www.youtube.com/watch?v=PhGNTioBUiU)

---

WebdriverIO के बारे में अधिक जानकारी के लिए प्रोजेक्ट [होमपेज](https://webdriver.io) देखें।