---
id: vscode-extensions
title: VS Code एक्सटेंशन टेस्टिंग
---

WebdriverIO आपको आसानी से [VS Code](https://code.visualstudio.com/) एक्सटेंशन का एंड-टू-एंड टेस्ट VS Code डेस्कटॉप IDE में या वेब एक्सटेंशन के रूप में करने की अनुमति देता है। आपको केवल अपने एक्सटेंशन का पथ प्रदान करना होगा और फ्रेमवर्क बाकी सब कुछ करता है। [`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service) के साथ सब कुछ का ध्यान रखा जाता है और बहुत कुछ:

- 🏗️ VSCode इंस्टॉल करना (स्थिर, इनसाइडर्स या निर्दिष्ट संस्करण)
- ⬇️ दिए गए VSCode संस्करण के लिए विशिष्ट Chromedriver डाउनलोड करना
- 🚀 आपके टेस्ट से VSCode API तक पहुंचने की सुविधा देता है
- 🖥️ कस्टम उपयोगकर्ता सेटिंग्स के साथ VSCode शुरू करना (Ubuntu, MacOS और Windows पर VSCode के लिए समर्थन सहित)
- 🌐 या वेब एक्सटेंशन के परीक्षण के लिए किसी भी ब्राउज़र द्वारा एक्सेस किए जाने के लिए सर्वर से VSCode सर्व करता है
- 📔 आपके VSCode संस्करण से मेल खाने वाले लोकेटर्स के साथ पेज ऑब्जेक्ट बूटस्ट्रैपिंग

## शुरुआत करना

एक नया WebdriverIO प्रोजेक्ट शुरू करने के लिए, चलाएँ:

```sh
npm create wdio@latest ./
```

एक इंस्टॉलेशन विज़ार्ड आपको प्रक्रिया के माध्यम से मार्गदर्शन करेगा। सुनिश्चित करें कि जब आपसे पूछा जाए कि आप किस प्रकार की टेस्टिंग करना चाहते हैं, तो आप _"VS Code Extension Testing"_ का चयन करें, उसके बाद केवल डिफ़ॉल्ट रखें या अपनी प्राथमिकता के अनुसार संशोधित करें।

## उदाहरण कॉन्फ़िगरेशन

सेवा का उपयोग करने के लिए आपको अपनी सेवाओं की सूची में `vscode` जोड़ना होगा, वैकल्पिक रूप से इसके बाद एक कॉन्फ़िगरेशन ऑब्जेक्ट। यह WebdriverIO को दिए गए VSCode बाइनरी और उपयुक्त Chromedriver संस्करण डाउनलोड करने के लिए मजबूर करेगा:

```js
// wdio.conf.ts
export const config = {
    outputDir: 'trace',
    // ...
    capabilities: [{
        browserName: 'vscode',
        browserVersion: '1.71.0', // "insiders" या "stable" नवीनतम VSCode संस्करण के लिए
        'wdio:vscodeOptions': {
            extensionPath: __dirname,
            userSettings: {
                "editor.fontSize": 14
            }
        }
    }],
    services: ['vscode'],
    /**
     * वैकल्पिक रूप से आप WebdriverIO के लिए पथ परिभाषित कर सकते हैं जहां सभी
     * VSCode और Chromedriver बाइनरी संग्रहीत हैं, उदाहरण:
     * services: [['vscode', { cachePath: __dirname }]]
     */
    // ...
};
```

यदि आप `wdio:vscodeOptions` को `vscode` के अलावा किसी अन्य `browserName` के साथ परिभाषित करते हैं, उदाहरण के लिए `chrome`, तो सेवा एक्सटेंशन को वेब एक्सटेंशन के रूप में सर्व करेगी। यदि आप Chrome पर परीक्षण करते हैं तो कोई अतिरिक्त ड्राइवर सेवा की आवश्यकता नहीं है, उदाहरण के लिए:

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

अपने `tsconfig.json` में सुनिश्चित करें कि आपके टाइप्स की सूची में `wdio-vscode-service` जोड़ें:

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
        "target": "es2020",
        "moduleResolution": "node16"
    }
}
```

## उपयोग

फिर आप अपने वांछित VSCode संस्करण से मेल खाने वाले लोकेटर्स के लिए पेज ऑब्जेक्ट्स तक पहुंचने के लिए `getWorkbench` विधि का उपयोग कर सकते हैं:

```ts
describe('WDIO VSCode Service', () => {
    it('should be able to load VSCode', async () => {
        const workbench = await browser.getWorkbench()
        expect(await workbench.getTitleBar().getTitle())
            .toBe('[Extension Development Host] - README.md - wdio-vscode-service - Visual Studio Code')
    })
})
```

वहां से आप सही पेज ऑब्जेक्ट विधियों का उपयोग करके सभी पेज ऑब्जेक्ट्स तक पहुंच सकते हैं। सभी उपलब्ध पेज ऑब्जेक्ट्स और उनकी विधियों के बारे में [पेज ऑब्जेक्ट डॉक्स](https://webdriverio-community.github.io/wdio-vscode-service/) में अधिक जानकारी प्राप्त करें।

### VSCode API तक पहुंच

यदि आप [VSCode API](https://code.visualstudio.com/api/references/vscode-api) के माध्यम से कुछ ऑटोमेशन करना चाहते हैं, तो आप कस्टम `executeWorkbench` कमांड के माध्यम से रिमोट कमांड चला सकते हैं। यह कमांड आपके टेस्ट से VSCode वातावरण के अंदर कोड रिमोट एक्जीक्यूट करने की अनुमति देता है और VSCode API तक पहुंचने की सुविधा देता है। आप फ़ंक्शन में मनमाने पैरामीटर पास कर सकते हैं जो तब फ़ंक्शन में प्रचारित होंगे। `vscode` ऑब्जेक्ट हमेशा बाहरी फ़ंक्शन पैरामीटर के बाद पहले आर्गुमेंट के रूप में पास किया जाएगा। ध्यान दें कि आप फ़ंक्शन स्कोप के बाहर वेरिएबल्स तक नहीं पहुंच सकते क्योंकि कॉलबैक रिमोट रूप से एक्जीक्यूट किया जाता है। यहां एक उदाहरण है:

```ts
const workbench = await browser.getWorkbench()
await browser.executeWorkbench((vscode, param1, param2) => {
    vscode.window.showInformationMessage(`I am an ${param1} ${param2}!`)
}, 'API', 'call')

const notifs = await workbench.getNotifications()
console.log(await notifs[0].getMessage()) // आउटपुट: "I am an API call!"
```

पूर्ण पेज ऑब्जेक्ट दस्तावेज़ीकरण के लिए, [डॉक्स](https://webdriverio-community.github.io/wdio-vscode-service/modules.html) देखें। आप इस [प्रोजेक्ट के टेस्ट सूट](https://github.com/webdriverio-community/wdio-vscode-service/blob/main/test/specs) में विभिन्न उपयोग उदाहरण पा सकते हैं।

## अधिक जानकारी

आप [`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service) को कैसे कॉन्फ़िगर करें और कस्टम पेज ऑब्जेक्ट्स कैसे बनाएं इसके बारे में [सेवा डॉक्स](/docs/wdio-vscode-service) में अधिक जान सकते हैं। आप [Christian Bromann](https://twitter.com/bromann) द्वारा निम्नलिखित वार्ता भी देख सकते हैं: [_Testing Complex VSCode Extensions With the Power of Web Standards_](https://www.youtube.com/watch?v=PhGNTioBUiU):

<LiteYouTubeEmbed
    id="PhGNTioBUiU"
    title="Testing Complex VSCode Extensions With the Power of Web Standards"
/>