---
id: wdio-vscode-service
title: VSCode நீட்டிப்பு சோதனை சேவை
custom_edit_url: https://github.com/webdriverio-community/wdio-vscode-service/edit/main/README.md
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

> wdio-vscode-service என்பது ஒரு 3வது தரப்பு தொகுப்பாகும், மேலும் தகவலுக்கு [GitHub](https://github.com/webdriverio-community/wdio-vscode-service) | [npm](https://www.npmjs.com/package/wdio-vscode-service) பார்க்கவும்

சோதிக்கப்பட்டது:

[![VSCode Version](https://img.shields.io/badge/VSCode%20Version-insiders%20/%20stable%20/%20v1.86.0%20/%20web-brightgreen)](https://github.com/webdriverio-community/wdio-vscode-service/actions/workflows/ci.yml) [![CI Status](https://img.shields.io/badge/Platform-windows%20%2F%20macos%20%2F%20ubuntu-brightgreen)](https://github.com/webdriverio-community/wdio-vscode-service/actions/workflows/ci.yml)

> VSCode நீட்டிப்புகளை சோதிப்பதற்கான WebdriverIO சேவை.

இந்த WebdriverIO சேவை, உங்கள் VSCode நீட்டிப்புகளை VSCode Desktop IDE அல்லது வலை நீட்டிப்பாக ஒருங்கிணைந்த முறையில் சோதிக்க அனுமதிக்கிறது. நீங்கள் உங்கள் நீட்டிப்பிற்கான பாதையை வழங்க வேண்டும், சேவை பின்வருவனவற்றை செய்வதன் மூலம் மீதியை கவனித்துக் கொள்கிறது:

- 🏗️ VSCode நிறுவுதல் (ஒன்று `stable`, `insiders` அல்லது குறிப்பிட்ட பதிப்பு)
- ⬇️ குறிப்பிட்ட VSCode பதிப்பிற்கான குறிப்பிட்ட Chromedriver பதிவிறக்கம்
- 🚀 உங்கள் சோதனைகளில் இருந்து VSCode API அணுக உங்களை அனுமதிக்கிறது
- 🖥️ தனிப்பயன் பயனர் அமைப்புகளுடன் VSCode தொடங்குதல் (உபுண்டு, MacOS மற்றும் விண்டோஸில் VSCode ஆதரவு உட்பட)
- 🌐 அல்லது VSCode ஐ ஒரு சேவையகத்திலிருந்து சேவைச் செய்கிறது, [வலை நீட்டிப்புகளை](https://code.visualstudio.com/api/extension-guides/web-extensions) சோதிப்பதற்காக எந்த உலாவியிலும் அணுகலாம்
- 📔 உங்கள் VSCode பதிப்புடன் பொருந்தும் இருப்பிட காட்டிகளுடன் பக்க பொருள்களை முன்தொடக்குதல்

இந்த திட்டம் [vscode-extension-tester](https://www.npmjs.com/package/vscode-extension-tester) திட்டத்தால் பெரிதும் ஊக்கமளிக்கப்பட்டது, இது செலீனியம் அடிப்படையிலானது. இந்த தொகுப்பு கருத்தை எடுத்து அதை WebdriverIO க்கு தழுவுகிறது.

VSCode v1.86 முதல், எந்த கட்டமைப்பும் இல்லாமல் Chromedriver ஐ நிறுவ `webdriverio` v8.14 அல்லது அதற்கு பிந்தைய பதிப்பு தேவைப்படுகிறது. VSCode இன் முந்தைய பதிப்புகளை சோதிக்க வேண்டுமானால், கீழே உள்ள [Chromedriver கட்டமைப்பு](#chromedriver) பிரிவைப் பார்க்கவும்.

## நிறுவல்

ஒரு புதிய WebdriverIO திட்டத்தைத் தொடங்க, இயக்கவும்:

```bash
npm create wdio ./
```

ஒரு நிறுவல் வழிகாட்டி செயல்முறை வழியாக உங்களை வழிநடத்தும். TypeScript ஐ கம்பைலராகத் தேர்ந்தெடுத்து, இந்தத் திட்டம் தேவையான அனைத்து பக்க பொருட்களுடனும் வருவதால் அது உங்களுக்காக பக்க பொருட்களை உருவாக்காமல் இருப்பதை உறுதிப்படுத்தவும். பின்னர் சேவைகளின் பட்டியலில் `vscode` ஐ தேர்ந்தெடுக்க உறுதிப்படுத்தவும்:

![Install Demo](https://raw.githubusercontent.com/webdriverio-community/wdio-vscode-service/main/.github/assets/demo.gif "Install Demo")

`WebdriverIO` எவ்வாறு நிறுவுவது பற்றிய கூடுதல் தகவலுக்கு, [திட்ட ஆவணங்களை](https://webdriver.io/docs/gettingstarted) சரிபார்க்கவும்.

## உதாரண கட்டமைப்பு

சேவையைப் பயன்படுத்த, உங்கள் சேவைகளின் பட்டியலில் `vscode` ஐ சேர்க்க வேண்டும், விருப்பமாக ஒரு கட்டமைப்பு பொருளைத் தொடர்ந்து. இது WebdriverIO கொடுக்கப்பட்ட VSCode பைனரிகளையும் பொருத்தமான Chromedriver பதிப்பையும் பதிவிறக்க செய்கிறது:

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

நீங்கள் `vscode` தவிர வேறு எந்த `browserName` உடன் `wdio:vscodeOptions` ஐ வரையறுத்தால், எ.கா. `chrome`, சேவை நீட்டிப்பை வலை நீட்டிப்பாக வழங்கும். நீங்கள் Chrome இல் சோதித்தால், கூடுதல் டிரைவர் சேவை தேவையில்லை, எ.கா.:

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

_குறிப்பு:_ வலை நீட்டிப்புகளை சோதிக்கும் போது, நீங்கள் `browserVersion` ஆக `stable` அல்லது `insiders` இடையே மட்டுமே தேர்வு செய்ய முடியும்.

### TypeScript அமைப்பு

உங்கள் `tsconfig.json` இல், உங்கள் வகைகளின் பட்டியலில் `wdio-vscode-service` சேர்க்க உறுதிப்படுத்தவும்:

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

## பயன்பாடு

பின்னர் நீங்கள் `getWorkbench` முறையைப் பயன்படுத்தி, உங்கள் விரும்பிய VSCode பதிப்புடன் பொருந்தும் இருப்பிட காட்டிகளுக்கான பக்க பொருட்களை அணுகலாம்:

```ts
describe('WDIO VSCode Service', () => {
    it('should be able to load VSCode', async () => {
        const workbench = await browser.getWorkbench()
        expect(await workbench.getTitleBar().getTitle())
            .toBe('[Extension Development Host] - README.md - wdio-vscode-service - Visual Studio Code')
    })
})
```

### VSCode API களை அணுகுதல்

குறிப்பிட்ட தானியக்கத்தை [VSCode API](https://code.visualstudio.com/api/references/vscode-api) மூலம் செயல்படுத்த விரும்பினால், தனிப்பயன் `executeWorkbench` கட்டளை மூலம் தொலைதூர கட்டளைகளை இயக்கலாம். இந்த கட்டளை உங்கள் சோதனையில் இருந்து VSCode சூழலில் தொலைதூரமாக குறியீட்டை செயல்படுத்த அனுமதிக்கிறது மற்றும் VSCode API ஐ அணுக உங்களை அனுமதிக்கிறது. நீங்கள் தன்னிச்சையான அளவுருக்களை செயல்பாட்டில் அனுப்பலாம், அவை செயல்பாட்டில் பரப்பப்படும். `vscode` பொருள் எப்போதும் வெளிப்புற செயல்பாட்டு அளவுருக்களைத் தொடர்ந்து முதல் அளவுருவாக அனுப்பப்படும். callback தொலைதூரமாக செயல்படுத்தப்படுவதால் செயல்பாட்டு நோக்கத்திற்கு வெளியே உள்ள மாறிகளை நீங்கள் அணுக முடியாது என்பதை கவனிக்கவும். இங்கு ஒரு உதாரணம்:

```ts
const workbench = await browser.getWorkbench()
await browser.executeWorkbench((vscode, param1, param2) => {
    vscode.window.showInformationMessage(`I am an ${param1} ${param2}!`)
}, 'API', 'call')

const notifs = await workbench.getNotifications()
console.log(await notifs[0].getMessage()) // outputs: "I am an API call!"
```

முழு பக்க பொருள் ஆவணங்களுக்கு, [ஆவணங்களை](https://webdriverio-community.github.io/wdio-vscode-service/modules.html) சரிபார்க்கவும். பல்வேறு பயன்பாட்டு உதாரணங்களை இந்த [திட்டத்தின் சோதனை தொகுப்பு](https://github.com/webdriverio-community/wdio-vscode-service/blob/main/test/specs) இல் காணலாம்.

## கட்டமைப்பு

சேவை கட்டமைப்பு மூலம், நீங்கள் VSCode பதிப்பு மற்றும் VSCode க்கான பயனர் அமைப்புகளை நிர்வகிக்கலாம்:

### சேவை விருப்பங்கள்

சேவை விருப்பங்கள் என்பவை சோதனை சூழலை அமைக்க சேவைக்குத் தேவையான விருப்பங்கள்.

#### `cachePath`

VS Code தொகுப்புகளை மீண்டும் பதிவிறக்குவதைத் தவிர்க்க ஒரு தற்காலிக கோப்பகப் பாதையை வரையறுக்கவும். ஒவ்வொரு சோதனை ஓட்டத்திற்கும் VSCode ஐ மீண்டும் பதிவிறக்குவதைத் தவிர்க்க CI/CD க்கு இது பயனுள்ளதாக இருக்கும்.

வகை: `string`<br />
இயல்பு: `process.cwd()`

### VSCode திறன்கள் (`wdio:vscodeOptions`)

VSCode மூலம் சோதனைகளை இயக்க, `browserName` ஆக `vscode` ஐ வரையறுக்க வேண்டும். `browserVersion` திறனை வழங்குவதன் மூலம் VSCode பதிப்பைக் குறிப்பிடலாம். தனிப்பயன் VSCode விருப்பங்கள் பின்னர் தனிப்பயன் `wdio:vscodeOptions` திறனுக்குள் வரையறுக்கப்படுகின்றன. விருப்பங்கள் பின்வருமாறு:

#### `binary`

உள்ளூரில் நிறுவப்பட்ட VSCode நிறுவலுக்கான பாதை. விருப்பம் வழங்கப்படவில்லை என்றால், சேவை கொடுக்கப்பட்ட `browserVersion` (அல்லது கொடுக்கப்படவில்லை என்றால் `stable`) அடிப்படையில் VSCode ஐப் பதிவிறக்கும்.

வகை: `string`

#### `extensionPath`

நீங்கள் சோதிக்க விரும்பும் நீட்டிப்பிற்கான அடைவை வரையறுக்கவும்.

வகை: `string`

#### `storagePath`

VS Code அதன் அனைத்து தரவையும் சேமிக்க ஒரு தனிப்பயன் இடத்தை வரையறுக்கவும். இது VSCode இன் உள் அடைவுகளுக்கான வேர் ஆகும் (பகுதி பட்டியல்)
* **user-data-dir**: அனைத்து பயனர் அமைப்புகள் (உலகளாவிய அமைப்புகள்), நீட்டிப்பு பதிவுகள் போன்றவை சேமிக்கப்படும் அடைவு.
* **extension-install-dir**: VS Code நீட்டிப்புகள் நிறுவப்பட்ட அடைவு.

வழங்கப்படவில்லை என்றால், ஒரு தற்காலிக கோப்பகம் பயன்படுத்தப்படுகிறது.

வகை: `string`

#### `userSettings`

VSCode க்கு பயன்படுத்தப்படும் தனிப்பயன் பயனர் அமைப்புகளை வரையறுக்கவும்.

வகை: `Record<string, number | string | object | boolean>`<br />
இயல்பு: `{}`

#### `workspacePath`

குறிப்பிட்ட பணியிடத்திற்கான VSCode ஐத் திறக்கிறது. வழங்கப்படவில்லை என்றால், VSCode ஒரு பணியிடம் திறக்காமல் தொடங்குகிறது.

வகை: `string`

#### `filePath`

குறிப்பிட்ட கோப்பு திறக்கப்பட்ட VSCode ஐத் திறக்கிறது.

வகை: `string`

#### `vscodeArgs`

ஒரு பொருளாக கூடுதல் தொடக்க அளவுருக்கள், எ.கா.

```ts
vscodeArgs: { fooBar: true, 'bar-foo': '/foobar' }
```

பின்வருமாறு அனுப்பப்படும்:

```ts
--foo-bar --fooBar --bar-foo=/foobar
```

வகை: `Record<string, string | boolean>`<br />
இயல்பு: [`constants.ts#L5-L14`](https://github.com/webdriverio-community/wdio-vscode-service/blob/196a69be3ac2fb82d9c7e4f19a2a1c8ccbaec1e2/src/constants.ts#L5-L14) பார்க்கவும்

#### `verboseLogging`

சரியென அமைக்கப்பட்டால், சேவை நீட்டிப்பு ஹோஸ்ட் மற்றும் கன்சோல் API இல் இருந்து VSCode வெளியீட்டைப் பதிவு செய்கிறது.

வகை: `boolean`<br />
இயல்பு: `false`

#### `vscodeProxyOptions`

VSCode API ப்ராக்ஸி கட்டமைப்புகள் VSCode API ஐ அணுக WebdriverIO எவ்வாறு VSCode workbench உடன் இணைகிறது என்பதை வரையறுக்கிறது.

வகை: `VSCodeProxyOptions`<br />
இயல்பு:

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

VSCode v1.86 முதல், எந்த கட்டமைப்பும் இல்லாமல் Chromedriver ஐ நிறுவ `webdriverio` v8.14 அல்லது அதற்கு பிந்தைய பதிப்பு தேவைப்படுகிறது. [எளிமைப்படுத்தப்பட்ட உலாவி தானியக்க அமைப்பு](https://webdriver.io/blog/2023/07/31/driver-management) எல்லாவற்றையும் உங்களுக்காக கையாளுகிறது.

VS Code இன் முந்தைய பதிப்புகளை சோதிக்க, பதிவுகளில் இருந்து எதிர்பார்க்கப்படும் Chromedriver பதிப்பைக் கண்டறிந்து, [Chromedriver](https://chromedriver.chromium.org/downloads) ஐப் பதிவிறக்கி, பாதையை உள்ளமைக்கவும். உதாரணமாக:

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

## உங்கள் சொந்த PageObjects உருவாக்கவும்

இந்த சேவையில் பயன்படுத்தப்படும் கூறுகளை உங்கள் சொந்த சீராய்வு பக்க பொருட்களுக்குப் பயன்படுத்தலாம். அதற்காக முதலில் உங்கள் அனைத்து தேர்வுகளையும் வரையறுக்கும் ஒரு கோப்பை உருவாக்கவும், எ.கா.:

```ts
// e.g. in /test/pageobjects/locators.ts
export const componentA = {
    elem: 'form', // component container element
    submit: 'button[type="submit"]', // submit button
    username: 'input.username', // username input
    password: 'input.password' // password input
}
```

இப்போது நீங்கள் பின்வருமாறு ஒரு பக்க பொருளை உருவாக்கலாம்:

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

இப்போது உங்கள் சோதனையில், உங்கள் பக்க பொருளை பின்வருமாறு பயன்படுத்தலாம்:

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

## TypeScript ஆதரவு

நீங்கள் WebdriverIO ஐ TypeScript உடன் பயன்படுத்தினால், உங்கள் `tsconfig.json` இல் உள்ள `types` இல் `wdio-vscode-service` சேர்க்க உறுதிப்படுத்தவும், எ.கா.:

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

## ப்ராக்ஸி ஆதரவு

இந்த சேவையின் துவக்கத்தின் போது, ஒரு ChromeDriver மற்றும் VSCode விநியோகம் பதிவிறக்கப்படுகிறது. சுற்றுச்சூழல் மாறி `HTTPS_PROXY` அல்லது `https_proxy` ஐ அமைப்பதன் மூலம் இந்த கோரிக்கைகளை ஒரு ப்ராக்ஸி வழியாக அனுப்பலாம். எ.கா.:

```bash
HTTPS_PROXY=http://127.0.0.1:1080 npm run wdio
```

## குறிப்புகள்

பின்வரும் VS Code நீட்டிப்புகள் `wdio-vscode-service` ஐப் பயன்படுத்துகின்றன:

- [Marquee](https://marketplace.visualstudio.com/items?itemName=stateful.marquee) (27k பதிவிறக்கங்கள்)
- [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) (27.8m பதிவிறக்கங்கள்)
- [DVC Extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=Iterative.dvc) (11.2k பதிவிறக்கங்கள்)
- [Nx Console](https://marketplace.visualstudio.com/items?itemName=nrwl.angular-console) (1.2m பதிவிறக்கங்கள்)
- [inlang – i18n supercharged](https://marketplace.visualstudio.com/items?itemName=inlang.vs-code-extension) (3k பதிவிறக்கங்கள்)

## பங்களிப்பு

ஒரு இழு கோரிக்கையை அனுப்புவதற்கு முன்பு, பின்வருவனவற்றை இயக்கவும்:

1. `git clone git@github.com:webdriverio-community/wdio-vscode-service.git`
1. `cd wdio-vscode-service`
1. `npm install`
1. `npm run build`
1. `npm run test` (அல்லது `npm run ci`)

## மேலும் அறிக

VSCode நீட்டிப்புகளை சோதிப்பது பற்றி மேலும் அறிய விரும்பினால், [OpenJS World 2022](https://www.youtube.com/watch?v=PhGNTioBUiU) இல் [Christian Bromann's](https://twitter.com/bromann) உரையைப் பார்க்கவும்:

[![Testing VSCode Extensions at OpenJS World 2022](https://img.youtube.com/vi/PhGNTioBUiU/sddefault.jpg)](https://www.youtube.com/watch?v=PhGNTioBUiU)

---

WebdriverIO பற்றிய மேலும் தகவலுக்கு திட்ட [முகப்புப்பக்கத்தை](https://webdriver.io) பார்க்கவும்.