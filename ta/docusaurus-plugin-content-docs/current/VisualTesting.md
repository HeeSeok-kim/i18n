---
id: visual-testing
title: காட்சி சோதனை
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

## இது என்ன செய்யக்கூடும்?

WebdriverIO திரைகள், கூறுகள் அல்லது முழு பக்கத்திற்கான படப் ஒப்பீடுகளை வழங்குகிறது

-   🖥️ டெஸ்க்டாப் உலாவிகள் (Chrome / Firefox / Safari / Microsoft Edge)
-   📱 மொபைல் / டேப்லெட் உலாவிகள் (Android எமுலேட்டர்களில் Chrome / iOS சிமுலேட்டர்களில் Safari / சிமுலேட்டர்கள் / உண்மையான சாதனங்கள்) Appium மூலம்
-   📱 நேட்டிவ் ஆப்ஸ் (Android எமுலேட்டர்கள் / iOS சிமுலேட்டர்கள் / உண்மையான சாதனங்கள்) Appium மூலம் (🌟 **புதிய** 🌟)
-   📳 ஹைப்ரிட் ஆப்ஸ் Appium மூலம்

இவை [`@wdio/visual-service`](https://www.npmjs.com/package/@wdio/visual-service) மூலம் வழங்கப்படுகின்றன, இது ஒரு லேசான WebdriverIO சேவையாகும்.

இது உங்களை பின்வருவனவற்றை செய்ய அனுமதிக்கிறது:

-   **திரைகள்/கூறுகள்/முழு-பக்க** திரைகளை அடிப்படை நிலையுடன் சேமிக்கலாம் அல்லது ஒப்பிடலாம்
-   அடிப்படை இல்லாதபோது தானாகவே **அடிப்படை உருவாக்கலாம்**
-   **தனிப்பயன் பகுதிகளை தடுக்கலாம்** மற்றும் ஒப்பீட்டின் போது நிலை மற்றும் கருவிப் பட்டிகளை (மொபைல் மட்டும்) **தானாகவே விலக்கலாம்**
-   கூறு பரிமாண ஸ்கிரீன்ஷாட்களை அதிகரிக்கலாம்
-   இணையதள ஒப்பீட்டின் போது **உரையை மறைக்கலாம்**:
    -   **நிலைத்தன்மையை மேம்படுத்த** மற்றும் எழுத்துரு வழங்கல் நிலையற்ற தன்மையைத் தடுக்க
    -   ஒரு இணையதளத்தின் **லேஅவுட்** மீது மட்டுமே கவனம் செலுத்த
-   **வெவ்வேறு ஒப்பீட்டு முறைகளையும்** சிறந்த படிக்கக்கூடிய சோதனைகளுக்கான **கூடுதல் போருத்திகளின்** தொகுப்பையும் பயன்படுத்தலாம்
-   உங்கள் இணையதளம் **உங்கள் விசைப்பலகையுடன் டேப்பிங் ஆதரவை எவ்வாறு வழங்கும்** என்பதை சரிபார்க்கலாம், [வலைத்தளத்தில் டேப்பிங்](#tabbing-through-a-website) ஐயும் பார்க்கவும்
-   மேலும் பல, [சேவை](./visual-testing/service-options) மற்றும் [முறை](./visual-testing/method-options) விருப்பங்களைப் பார்க்கவும்

இந்த சேவை அனைத்து உலாவிகள்/சாதனங்களுக்கும் தேவையான தரவு மற்றும் ஸ்கிரீன்ஷாட்களைப் பெறுவதற்கான ஒரு லேசான மாட்யூல் ஆகும். ஒப்பீட்டு சக்தி [ResembleJS](https://github.com/Huddle/Resemble.js) இலிருந்து வருகிறது. நீங்கள் படங்களை ஆன்லைனில் ஒப்பிட விரும்பினால், [ஆன்லைன் கருவியைப்](http://rsmbl.github.io/Resemble.js/) பார்க்கலாம்.

:::info குறிப்பு நேட்டிவ்/ஹைப்ரிட் ஆப்ஸ்க்கு
`saveScreen`, `saveElement`, `checkScreen`, `checkElement` முறைகள் மற்றும் `toMatchScreenSnapshot` மற்றும் `toMatchElementSnapshot` போன்ற போருத்திகள் நேட்டிவ் ஆப்ஸ்/கான்டெக்ஸ்ட்க்கு பயன்படுத்தப்படலாம்.

நீங்கள் ஹைப்ரிட் ஆப்ஸ்க்கு பயன்படுத்த விரும்பினால், உங்கள் சேவை அமைப்புகளில் `isHybridApp:true` பண்பை பயன்படுத்தவும்.
:::

## நிறுவல்

எளிதான வழி `@wdio/visual-service` ஐ உங்கள் `package.json` இல் ஒரு dev-dependency ஆக வைத்திருப்பது:

```sh
npm install --save-dev @wdio/visual-service
```

## பயன்பாடு

`@wdio/visual-service` ஐ ஒரு சாதாரண சேவையாகப் பயன்படுத்தலாம். நீங்கள் அதை உங்கள் கட்டமைப்பு கோப்பில் பின்வருமாறு அமைக்கலாம்:

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
                // Some options, see the docs for more
                baselineFolder: path.join(process.cwd(), "tests", "baseline"),
                formatImageName: "{tag}-{logName}-{width}x{height}",
                screenshotPath: path.join(process.cwd(), "tmp"),
                savePerInstance: true,
                // ... more options
            },
        ],
    ],
    // ...
};
```

மேலும் சேவை விருப்பங்களை [இங்கே](/docs/visual-testing/service-options) காணலாம்.

WebdriverIO கட்டமைப்பில் அமைத்த பிறகு, [உங்கள் சோதனைகளில்](/docs/visual-testing/writing-tests) காட்சி உறுதிப்படுத்தல்களைச் சேர்க்கலாம்.

### திறன்கள்
Visual Testing மாட்யூலைப் பயன்படுத்த, **உங்கள் திறன்களுக்கு கூடுதல் விருப்பங்களைச் சேர்க்க வேண்டியதில்லை**. எனினும், சில சந்தர்ப்பங்களில், `logName` போன்ற கூடுதல் மெட்டாடேட்டாவை உங்கள் விஷுவல் டெஸ்ட்களுக்குச் சேர்க்க விரும்பலாம்.

`logName` ஒவ்வொரு திறனுக்கும் ஒரு தனிப்பயன் பெயரை நியமிக்க அனுமதிக்கிறது, இது பிறகு படக் கோப்பு பெயர்களில் சேர்க்கப்படலாம். இது வெவ்வேறு உலாவிகள், சாதனங்கள் அல்லது கட்டமைப்புகளில் எடுக்கப்பட்ட ஸ்கிரீன்ஷாட்களை வேறுபடுத்துவதற்கு குறிப்பாக பயனுள்ளதாக இருக்கும்.

இதை இயக்க, நீங்கள் `capabilities` பிரிவில் `logName` ஐ வரையறுக்கலாம் மற்றும் Visual Testing சேவையில் `formatImageName` விருப்பம் அதைக் குறிப்பிடுவதை உறுதிசெய்யவும். இதை எவ்வாறு அமைப்பது:

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
                logName: 'chrome-mac-15', // Custom log name for Chrome
            },
        }
        {
            browserName: 'firefox',
            'wdio-ics:options': {
                logName: 'firefox-mac-15', // Custom log name for Firefox
            },
        }
    ],
    services: [
        [
            "visual",
            {
                // Some options, see the docs for more
                baselineFolder: path.join(process.cwd(), "tests", "baseline"),
                screenshotPath: path.join(process.cwd(), "tmp"),
                // The format below will use the `logName` from capabilities
                formatImageName: "{tag}-{logName}-{width}x{height}",
                // ... more options
            },
        ],
    ],
    // ...
};
```

#### இது எப்படி வேலை செய்கிறது
1. `logName` ஐ அமைத்தல்:

    - `capabilities` பிரிவில், ஒவ்வொரு உலாவி அல்லது சாதனத்திற்கும் ஒரு தனித்துவமான `logName` ஐ ஒதுக்கவும். எடுத்துக்காட்டாக, `chrome-mac-15` macOS பதிப்பு 15 இல் Chrome இல் இயங்கும் சோதனைகளை அடையாளம் காட்டுகிறது.

2. தனிப்பயன் படப் பெயரிடல்:

    - `formatImageName` விருப்பம் ஸ்கிரீன்ஷாட் கோப்பு பெயர்களில் `logName` ஐ ஒருங்கிணைக்கிறது. எடுத்துக்காட்டாக, `tag` ஹோம்பேஜ் மற்றும் தீர்மானம் `1920x1080` என்றால், கிடைக்கும் கோப்புப் பெயர் இவ்வாறு இருக்கலாம்:

        `homepage-chrome-mac-15-1920x1080.png`

3. தனிப்பயன் பெயரிடலின் நன்மைகள்:

    - வெவ்வேறு உலாவிகள் அல்லது சாதனங்களிலிருந்து ஸ்கிரீன்ஷாட்களை வேறுபடுத்துவது மிகவும் எளிதாகிறது, குறிப்பாக அடிப்படைகளை நிர்வகிக்கும் போது மற்றும் முரண்பாடுகளை பிழைதிருத்தும் போது.

4. இயல்புநிலைகள் பற்றிய குறிப்பு:

    - `capabilities` இல் `logName` அமைக்கப்படவில்லை என்றால், `formatImageName` விருப்பம் அதை கோப்பு பெயர்களில் வெற்று சரமாகக் காட்டும் (`homepage--15-1920x1080.png`)

### WebdriverIO MultiRemote

நாங்கள் [MultiRemote](https://webdriver.io/docs/multiremote/) ஐயும் ஆதரிக்கிறோம். இதை சரியாக செயல்பட, நீங்கள் கீழே காணும்படி உங்கள் திறன்களில் `wdio-ics:options` ஐச் சேர்ப்பதை உறுதிசெய்யவும். இது ஒவ்வொரு ஸ்கிரீன்ஷாட்டிற்கும் அதன் சொந்த தனித்துவமான பெயர் இருப்பதை உறுதிசெய்யும்.

[உங்கள் சோதனைகளை எழுதுதல்](/docs/visual-testing/writing-tests) [testrunner](https://webdriver.io/docs/testrunner) ஐப் பயன்படுத்துவதுடன் ஒப்பிடும்போது வேறுபடாது

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

### நிரல் ரீதியாக இயக்குதல்

இங்கே `remote` விருப்பங்கள் மூலம் `@wdio/visual-service` ஐப் பயன்படுத்துவதற்கான ஒரு குறைந்தபட்ச எடுத்துக்காட்டு உள்ளது:

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

// "Start" the service to add the custom commands to the `browser`
visualService.remoteSetup(browser);

await browser.url("https://webdriver.io/");

// or use this for ONLY saving a screenshot
await browser.saveFullPageScreen("examplePaged", {});

// or use this for validating. Both methods don't need to be combined, see the FAQ
await browser.checkFullPageScreen("examplePaged", {});

await browser.deleteSession();
```

### இணையதளத்தில் டேப்பிங்

நீங்கள் விசைப்பலகை <kbd>TAB</kbd>-விசையைப் பயன்படுத்தி ஒரு இணையதளம் அணுகக்கூடியதா என்பதை சரிபார்க்கலாம். அணுகல்தன்மையின் இந்த பகுதியை சோதிப்பது எப்போதும் நேரம் எடுக்கும் (கையேடு) வேலையாக இருந்தது மற்றும் ஆட்டோமேஷன் மூலம் செய்வது மிகவும் கடினம்.
`saveTabbablePage` மற்றும் `checkTabbablePage` முறைகளுடன், டேப்பிங் வரிசையை சரிபார்க்க உங்கள் இணையதளத்தில் கோடுகள் மற்றும் புள்ளிகளை இப்போது வரையலாம்.

இது டெஸ்க்டாப் உலாவிகளுக்கு மட்டுமே பயனுள்ளதாக இருக்கும் என்பதையும், மொபைல் சாதனங்களுக்கு **இல்லை\*\*** என்பதையும் கவனத்தில் கொள்ளவும். அனைத்து டெஸ்க்டாப் உலாவிகளும் இந்த அம்சத்தை ஆதரிக்கின்றன.

:::note

இந்த வேலை [Viv Richards](https://github.com/vivrichards600) அவரது ["AUTOMATING PAGE TABABILITY (IS THAT A WORD?) WITH VISUAL TESTING"](https://vivrichards.co.uk/accessibility/automating-page-tab-flows-using-visual-testing-and-javascript) பற்றிய வலைப்பதிவால் ஈர்க்கப்பட்டது.

டேப்பில் செய்யக்கூடிய கூறுகள் தேர்ந்தெடுக்கப்படும் முறை [tabbable](https://github.com/davidtheclark/tabbable) மாட்யூலை அடிப்படையாகக் கொண்டது. டேப்பிங் தொடர்பான சிக்கல்கள் ஏதேனும் இருந்தால், [README.md](https://github.com/davidtheclark/tabbable/blob/master/README.md) மற்றும் குறிப்பாக [More Details](https://github.com/davidtheclark/tabbable/blob/master/README.md#more-details) பிரிவைப் பார்க்கவும்.

:::

#### இது எவ்வாறு செயல்படுகிறது

இரண்டு முறைகளும் உங்கள் இணையதளத்தில் ஒரு `canvas` கூறை உருவாக்கி, ஒரு இறுதிப் பயனர் அதைப் பயன்படுத்தினால் உங்கள் TAB எங்கு செல்லும் என்பதைக் காட்ட கோடுகள் மற்றும் புள்ளிகளை வரையும். அதன் பிறகு, பாய்வின் நல்ல கண்ணோட்டத்தை உங்களுக்கு வழங்க ஒரு முழுப் பக்க ஸ்கிரீன்ஷாட்டை உருவாக்கும்.

:::important

**ஸ்கிரீன்ஷாட்டை உருவாக்க வேண்டும் மற்றும் **அடிப்படை** படத்துடன் ஒப்பிட **விரும்பவில்லை** என்றால் மட்டுமே `saveTabbablePage` ஐப் பயன்படுத்தவும்.\*\*\*\*

:::

நீங்கள் டேப்பிங் பாய்வை ஒரு அடிப்படையுடன் ஒப்பிட விரும்பினால், `checkTabbablePage`-முறையைப் பயன்படுத்தலாம். இரண்டு முறைகளையும் சேர்த்துப் பயன்படுத்த **வேண்டாம்**. சேவையை உருவாக்கும் போது `autoSaveBaseline: true` வழங்குவதன் மூலம் தானாகவே செய்யக்கூடிய அடிப்படைப் படம் ஏற்கனவே உருவாக்கப்பட்டிருந்தால், `checkTabbablePage` முதலில் _உண்மையான_ படத்தை உருவாக்கும், பின்னர் அதை அடிப்படையுடன் ஒப்பிடும்.

##### விருப்பங்கள்

இரண்டு முறைகளும் [`saveFullPageScreen`](https://github.com/wswebcreation/webdriver-image-comparison/blob/master/docs/OPTIONS.md#savefullpagescreen-or-savetabbablepage) அல்லது [`compareFullPageScreen`](https://github.com/wswebcreation/webdriver-image-comparison/blob/master/docs/OPTIONS.md#comparefullpagescreen-or-comparetabbablepage) போன்ற அதே விருப்பங்களைப் பயன்படுத்துகின்றன.

#### எடுத்துக்காட்டு

எங்கள் [கினியா பிக் இணையதளத்தில்](https://guinea-pig.webdriver.io/image-compare.html) டேப்பிங் எவ்வாறு செயல்படுகிறது என்பதற்கான எடுத்துக்காட்டு இது:

![WDIO tabbing example](/img/visual/tabbable-chrome-latest-1366x768.png)

### தோல்வியுற்ற விஷுவல் ஸ்னாப்ஷாட்களை தானாகவே புதுப்பிக்கவும்

கட்டளை வரியில் `--update-visual-baseline` வாதத்தைச் சேர்ப்பதன் மூலம் அடிப்படைப் படங்களைப் புதுப்பிக்கவும். இது

-   உண்மையில் ஸ்கிரீன்ஷாட்டை தானாகவே நகலெடுத்து அடிப்படை கோப்புறையில் வைக்கும்
-   வேறுபாடுகள் இருந்தால், அடிப்படை புதுப்பிக்கப்பட்டதால் சோதனையை கடக்க அனுமதிக்கும்

**பயன்பாடு:**

```sh
npm run test.local.desktop  --update-visual-baseline
```

பதிவுகளை தகவல்/பிழைத்திருத்த முறையில் இயக்கும்போது, பின்வரும் பதிவுகள் சேர்க்கப்படுவதைக் காணலாம்

```logs
[0-0] ..............
[0-0] #####################################################################################
[0-0]  INFO:
[0-0]  Updated the actual image to
[0-0]  /Users/wswebcreation/Git/wdio/visual-testing/localBaseline/chromel/demo-chrome-1366x768.png
[0-0] #####################################################################################
[0-0] ..........
```

## Typescript ஆதரவு

இந்த மாட்யூல் TypeScript ஆதரவை உள்ளடக்கியது, இது நீங்கள் Visual Testing சேவையைப் பயன்படுத்தும்போது தானியங்கி முடிவு, வகை பாதுகாப்பு மற்றும் மேம்படுத்தப்பட்ட டெவலப்பர் அனுபவத்தைப் பெற உதவுகிறது.

### படி 1: வகை வரையறைகளைச் சேர்க்கவும்
TypeScript மாட்யூல் வகைகளை அங்கீகரிப்பதை உறுதிசெய்ய, உங்கள் tsconfig.json இல் வகைகள் புலத்தில் பின்வரும் உள்ளீட்டைச் சேர்க்கவும்:

```json
{
    "compilerOptions": {
        "types": ["@wdio/visual-service"]
    }
}
```

### படி 2: சேவை விருப்பங்களுக்கான வகை பாதுகாப்பை இயக்கவும்
சேவை விருப்பங்களில் வகை சரிபார்ப்பை அமல்படுத்த, உங்கள் WebdriverIO கட்டமைப்பை புதுப்பிக்கவும்:

```ts
// wdio.conf.ts
import { join } from 'node:path';
// Import the type definition
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
                // Service options
                baselineFolder: join(process.cwd(), './__snapshots__/'),
                formatImageName: '{tag}-{logName}-{width}x{height}',
                screenshotPath: join(process.cwd(), '.tmp/'),
            } satisfies VisualServiceOptions, // Ensures type safety
        ],
    ],
    // ...
};
```

## கணினி தேவைகள்

### பதிப்பு 5 மற்றும் அதற்கு மேல்

பதிப்பு 5 மற்றும் அதற்கு மேல், இந்த மாட்யூல் பொதுவான [திட்டத் தேவைகளுக்கு](/docs/gettingstarted#system-requirements) அப்பால் கூடுதல் கணினி சார்புகள் இல்லாத முற்றிலும் JavaScript அடிப்படையிலான மாட்யூல் ஆகும். இது [Jimp](https://github.com/jimp-dev/jimp) ஐப் பயன்படுத்துகிறது, இது பூஜ்ஜிய நேட்டிவ் சார்புகளுடன், முற்றிலும் JavaScript இல் எழுதப்பட்ட Node.js க்கான படச் செயலாக்க நூலகம்.

### பதிப்பு 4 மற்றும் குறைவானவை

பதிப்பு 4 மற்றும் குறைவானவற்றிற்கு, இந்த மாட்யூல் Node.js க்கான கேன்வாஸ் அமலாக்கமான [Canvas](https://github.com/Automattic/node-canvas) ஐ நம்பியுள்ளது. Canvas [Cairo](https://cairographics.org/) ஐ சார்ந்துள்ளது.

#### நிறுவல் விவரங்கள்

இயல்பாக, macOS, Linux மற்றும் Windows க்கான பைனரிகள் உங்கள் திட்டத்தின் `npm install` போது பதிவிறக்கப்படும். உங்களிடம் ஆதரிக்கப்படும் இயக்க முறைமை அல்லது செயலி கட்டமைப்பு இல்லை என்றால், மாட்யூல் உங்கள் கணினியில் தொகுக்கப்படும். இதற்கு Cairo மற்றும் Pango உள்ளிட்ட பல சார்புகள் தேவைப்படுகிறது.

விரிவான நிறுவல் தகவலுக்கு, [node-canvas wiki](https://github.com/Automattic/node-canvas/wiki/_pages) ஐப் பார்க்கவும். கீழே பொதுவான இயக்க முறைமைகளுக்கான ஒரு வரி நிறுவல் வழிமுறைகள் உள்ளன. `libgif/giflib`, `librsvg`, மற்றும் `libjpeg` விருப்பமானவை மற்றும் GIF, SVG, மற்றும் JPEG ஆதரவிற்கு மட்டுமே தேவைப்படுகின்றன என்பதை நினைவில் கொள்ளவும். Cairo v1.10.0 அல்லது அதற்குப் பிறகு தேவை.

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

     [Homebrew](https://brew.sh/) பயன்படுத்தி:

     ```sh
     brew install pkg-config cairo pango libpng jpeg giflib librsvg pixman
     ```

    **Mac OS X v10.11+:** நீங்கள் சமீபத்தில் Mac OS X v10.11+ க்கு புதுப்பித்து, தொகுக்கும்போது சிரமத்தை அனுபவித்தால், பின்வரும் கட்டளையை இயக்கவும்: `xcode-select --install`. [Stack Overflow](http://stackoverflow.com/a/32929012/148072) இல் பிரச்சினை பற்றி மேலும் படிக்கவும்.
    Xcode 10.0 அல்லது அதற்கு மேற்பட்ட பதிப்பு நிறுவப்பட்டிருந்தால், மூலத்திலிருந்து உருவாக்க NPM 6.4.1 அல்லது அதற்கு மேற்பட்டது தேவை.

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

    [Wiki](https://github.com/Automattic/node-canvas/wiki/Installation:-Windows) ஐப் பார்க்கவும்

</TabItem>
<TabItem value="others">

    [Wiki](https://github.com/Automattic/node-canvas/wiki) ஐப் பார்க்கவும்

</TabItem>
</Tabs>