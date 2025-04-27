---
id: vscode-extensions
title: VS Code நீட்சி சோதனை
---

WebdriverIO உங்கள் [VS Code](https://code.visualstudio.com/) நீட்சிகளை VS Code Desktop IDE அல்லது வலை நீட்சியாக எளிதாக சோதிக்க உதவுகிறது. நீங்கள் உங்கள் நீட்சிக்கான பாதையை வழங்க வேண்டியது மட்டுமே, மற்றும் கட்டமைப்பு மீதியை கவனித்துக் கொள்ளும். [`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service) மூலம் அனைத்தும் கவனிக்கப்படுகிறது மற்றும் அதற்கும் மேலாக:

- 🏗️ VSCode நிறுவுதல் (நிலையான, insiders அல்லது குறிப்பிட்ட பதிப்பு)
- ⬇️ குறிப்பிட்ட VSCode பதிப்புக்கான Chromedriver பதிவிறக்கம்
- 🚀 உங்கள் சோதனைகளில் இருந்து VSCode API அணுக உதவுகிறது
- 🖥️ தனிப்பயன் பயனர் அமைப்புகளுடன் VSCode தொடங்குதல் (Ubuntu, MacOS மற்றும் Windows இல் VSCode ஆதரவு உட்பட)
- 🌐 அல்லது வலை நீட்சிகளை சோதிக்க எந்த உலாவியும் அணுகக்கூடிய வகையில் VSCode-ஐ சேவையாக வழங்குதல்
- 📔 உங்கள் VSCode பதிப்புடன் பொருந்தும் locators கொண்ட page objects உருவாக்குதல்

## தொடங்குதல்

புதிய WebdriverIO திட்டத்தை தொடங்க, இயக்கவும்:

```sh
npm create wdio@latest ./
```

ஒரு நிறுவல் வழிகாட்டி உங்களை வழிநடத்தும். "VS Code Extension Testing" என்பதை நீங்கள் எந்த வகை சோதனை செய்ய விரும்புகிறீர்கள் என்று கேட்கும்போது தேர்ந்தெடுக்கவும், பின்னர் இயல்புநிலை விருப்பங்களை வைத்திருக்கவும் அல்லது உங்கள் விருப்பத்திற்கேற்ப மாற்றவும்.

## உதாரண கட்டமைப்பு

சேவையைப் பயன்படுத்த, `vscode`-ஐ உங்கள் சேவைகள் பட்டியலில் சேர்க்க வேண்டும், விருப்பமாக கட்டமைப்பு பொருளால் தொடரப்படலாம். இது WebdriverIO குறிப்பிட்ட VSCode பைனரிகளையும் பொருத்தமான Chromedriver பதிப்பையும் பதிவிறக்க செய்யும்:

```js
// wdio.conf.ts
export const config = {
    outputDir: 'trace',
    // ...
    capabilities: [{
        browserName: 'vscode',
        browserVersion: '1.71.0', // "insiders" or "stable" for latest VSCode version
        'wdio:vscodeOptions': {
            extensionPath: __dirname,
            userSettings: {
                "editor.fontSize": 14
            }
        }
    }],
    services: ['vscode'],
    /**
     * optionally you can define the path WebdriverIO stores all
     * VSCode and Chromedriver binaries, e.g.:
     * services: [['vscode', { cachePath: __dirname }]]
     */
    // ...
};
```

நீங்கள் `vscode` தவிர வேறு `browserName` உடன் `wdio:vscodeOptions` வரையறுத்தால், எ.கா. `chrome`, சேவை நீட்சியை வலை நீட்சியாக வழங்கும். Chrome இல் சோதிக்கும்போது கூடுதல் டிரைவர் சேவை தேவையில்லை, எ.கா.:

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

_குறிப்பு:_ வலை நீட்சிகளை சோதிக்கும்போது `browserVersion` ஆக `stable` அல்லது `insiders` மட்டுமே தேர்வு செய்ய முடியும்.

### TypeScript அமைப்பு

உங்கள் `tsconfig.json` இல் வகைகளின் பட்டியலில் `wdio-vscode-service` சேர்க்க உறுதிசெய்யவும்:

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

## பயன்பாடு

உங்கள் விரும்பிய VSCode பதிப்புக்கு பொருந்தும் locators க்கான page objects அணுக `getWorkbench` முறையை பயன்படுத்தலாம்:

```ts
describe('WDIO VSCode Service', () => {
    it('should be able to load VSCode', async () => {
        const workbench = await browser.getWorkbench()
        expect(await workbench.getTitleBar().getTitle())
            .toBe('[Extension Development Host] - README.md - wdio-vscode-service - Visual Studio Code')
    })
})
```

அங்கிருந்து சரியான page object முறைகளைப் பயன்படுத்தி அனைத்து page objects அணுகலாம். கிடைக்கக்கூடிய அனைத்து page objects மற்றும் அவற்றின் முறைகளைப் பற்றி [page object docs](https://webdriverio-community.github.io/wdio-vscode-service/) இல் மேலும் அறியவும்.

### VSCode APIs அணுகுதல்

[VSCode API](https://code.visualstudio.com/api/references/vscode-api) மூலம் சில தானியங்கி செயல்களை நிறைவேற்ற விரும்பினால், தனிப்பயன் `executeWorkbench` கட்டளை மூலம் தொலைநிலை கட்டளைகளை இயக்கலாம். இந்த கட்டளை உங்கள் சோதனையிலிருந்து VSCode சூழலில் தொலைநிலையில் குறியீட்டை இயக்க அனுமதிக்கிறது மற்றும் VSCode API அணுக உதவுகிறது. நீங்கள் விருப்பமான அளவுருக்களை செயல்பாட்டிற்குள் அனுப்பலாம், அவை பின்னர் வெளிப்புற செயல்பாட்டு அளவுருக்களாக பரப்பப்படும். `vscode` பொருள் எப்போதும் முதல் argument ஆக அனுப்பப்படும். செயல்பாட்டு வரம்புக்கு வெளியே உள்ள மாறிகளை அணுக முடியாது ஏனெனில் callback தொலைநிலையில் இயக்கப்படுகிறது. இதோ ஒரு உதாரணம்:

```ts
const workbench = await browser.getWorkbench()
await browser.executeWorkbench((vscode, param1, param2) => {
    vscode.window.showInformationMessage(`I am an ${param1} ${param2}!`)
}, 'API', 'call')

const notifs = await workbench.getNotifications()
console.log(await notifs[0].getMessage()) // outputs: "I am an API call!"
```

முழு page object ஆவணங்களுக்கு, [docs](https://webdriverio-community.github.io/wdio-vscode-service/modules.html) ஐப் பார்க்கவும். இந்த [திட்டத்தின் சோதனை தொகுப்பில்](https://github.com/webdriverio-community/wdio-vscode-service/blob/main/test/specs) பல்வேறு பயன்பாட்டு உதாரணங்களைக் காணலாம்.

## மேலும் தகவல்

[`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service) கட்டமைக்கும் முறை மற்றும் தனிப்பயன் page objects உருவாக்குவது பற்றி [சேவை ஆவணங்களில்](/docs/wdio-vscode-service) மேலும் அறியலாம். [Christian Bromann](https://twitter.com/bromann) வழங்கிய [_Testing Complex VSCode Extensions With the Power of Web Standards_](https://www.youtube.com/watch?v=PhGNTioBUiU) என்ற உரையையும் நீங்கள் பார்க்கலாம்:

<LiteYouTubeEmbed
    id="PhGNTioBUiU"
    title="Testing Complex VSCode Extensions With the Power of Web Standards"
/>