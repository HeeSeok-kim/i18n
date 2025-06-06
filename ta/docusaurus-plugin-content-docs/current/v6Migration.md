---
id: v6-migration
title: v5 இல் இருந்து v6 க்கு
---

இந்த பயிற்சி, WebdriverIO இன் `v5` பதிப்பை பயன்படுத்துபவர்களுக்கும், `v6` அல்லது WebdriverIO இன் சமீபத்திய பதிப்புக்கு மாற விரும்புபவர்களுக்கும் உரியது. எங்கள் [வெளியீட்டு வலைப்பதிவில்](https://webdriver.io/blog/2020/03/26/webdriverio-v6-released) குறிப்பிட்டுள்ளபடி, இந்த பதிப்பு மேம்படுத்தலுக்கான மாற்றங்களை பின்வருமாறு சுருக்கமாக கூறலாம்:

- சில கட்டளைகளுக்கான அளவுருக்களை நாங்கள் ஒருங்கிணைந்தோம் (எ.கா. `newWindow`, `react$`, `react$$`, `waitUntil`, `dragAndDrop`, `moveTo`, `waitForDisplayed`, `waitForEnabled`, `waitForExist`) மற்றும் அனைத்து விருப்ப அளவுருக்களையும் ஒரு ஒற்றை பொருளாக மாற்றினோம், எ.கா.

    ```js
    // v5
    browser.newWindow(
        'https://webdriver.io',
        'WebdriverIO window',
        'width=420,height=230,resizable,scrollbars=yes,status=1'
    )
    // v6
    browser.newWindow('https://webdriver.io', {
        windowName: 'WebdriverIO window',
        windowFeature: 'width=420,height=230,resizable,scrollbars=yes,status=1'
    })
    ```

- சேவைகளுக்கான கட்டமைப்புகள் சேவை பட்டியலுக்கு மாற்றப்பட்டன, எ.கா.

    ```js
    // v5
    exports.config = {
        services: ['sauce'],
        sauceConnect: true,
        sauceConnectOpts: { foo: 'bar' },
    }
    // v6
    exports.config = {
        services: [['sauce', {
            sauceConnect: true,
            sauceConnectOpts: { foo: 'bar' }
        }]],
    }
    ```

- சில சேவை விருப்பங்கள் எளிமைப்படுத்தும் நோக்கங்களுக்காக மறுபெயரிடப்பட்டன
- Chrome WebDriver அமர்வுகளுக்கான `launchApp` கட்டளையை `launchChromeApp` என மறுபெயரிட்டோம்

:::info

நீங்கள் WebdriverIO `v4` அல்லது அதற்கு கீழ் பயன்படுத்துகிறீர்கள் என்றால், முதலில் `v5` க்கு மேம்படுத்தவும்.

:::

இதற்கான முழுமையான தானியங்கி செயல்முறையை நாங்கள் விரும்பினாலும், உண்மை வேறுவிதமாக உள்ளது. அனைவரும் வெவ்வேறு அமைப்பைக் கொண்டுள்ளனர். ஒவ்வொரு படியும் படிப்படியான வழிமுறைகளைக் காட்டிலும் வழிகாட்டுதலாக கருதப்பட வேண்டும். மாற்றத்தின் போது சிக்கல்கள் இருந்தால், [எங்களை தொடர்பு கொள்ள](https://github.com/webdriverio/codemod/discussions/new) தயங்க வேண்டாம்.

## அமைப்பு

மற்ற மாற்றங்களை போலவே, WebdriverIO [codemod](https://github.com/webdriverio/codemod) ஐ நாம் பயன்படுத்தலாம். codemod ஐ நிறுவ, இயக்கவும்:

```sh
npm install jscodeshift @wdio/codemod
```

## WebdriverIO சார்புகளை மேம்படுத்துதல்

அனைத்து WebdriverIO பதிப்புகளும் ஒன்றோடொன்று இணைக்கப்பட்டிருப்பதால், எப்போதும் ஒரு குறிப்பிட்ட குறிச்சொல்லுக்கு, உதாரணமாக `6.12.0`, மேம்படுத்துவது சிறந்தது. நீங்கள் `v5` இலிருந்து நேரடியாக `v7` க்கு மேம்படுத்த முடிவு செய்தால், குறிச்சொல்லை விட்டுவிட்டு அனைத்து தொகுப்புகளின் சமீபத்திய பதிப்புகளை நிறுவலாம். இதைச் செய்ய, அனைத்து WebdriverIO தொடர்பான சார்புகளையும் நமது `package.json` இலிருந்து நகலெடுத்து, பின்வருமாறு மீண்டும் நிறுவலாம்:

```sh
npm i --save-dev @wdio/allure-reporter@6 @wdio/cli@6 @wdio/cucumber-framework@6 @wdio/local-runner@6 @wdio/spec-reporter@6 @wdio/sync@6 wdio-chromedriver-service@6 webdriverio@6
```

பொதுவாக WebdriverIO சார்புகள் dev சார்புகளின் ஒரு பகுதியாக உள்ளன, உங்கள் திட்டத்தைப் பொறுத்து இது மாறுபடலாம். இதற்குப் பிறகு உங்கள் `package.json` மற்றும் `package-lock.json` புதுப்பிக்கப்பட வேண்டும். __குறிப்பு:__ இவை உதாரண சார்புகள், உங்களுடையது வேறுபடலாம். அழைப்பதன் மூலம் சமீபத்திய v6 பதிப்பைக் கண்டறியவும், எ.கா.:

```sh
npm show webdriverio versions
```

அனைத்து முக்கிய WebdriverIO தொகுப்புகளுக்கும் கிடைக்கும் சமீபத்திய பதிப்பு 6 ஐ நிறுவ முயற்சிக்கவும். சமூக தொகுப்புகளுக்கு இது தொகுப்புக்கு தொகுப்பு வேறுபடலாம். இங்கே, எந்த பதிப்பு v6 உடன் இன்னும் இணக்கமாக உள்ளது என்பதற்கான தகவலுக்கு changelog ஐ சரிபார்க்க பரிந்துரைக்கிறோம்.

## கட்டமைப்பு கோப்பை மாற்றுதல்

ஒரு நல்ல முதல் படி கட்டமைப்பு கோப்புடன் தொடங்குவதாகும். அனைத்து முறிவு மாற்றங்களையும் codemod ஐப் பயன்படுத்தி முழுமையாக தானியங்கி முறையில் தீர்க்கலாம்:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v6 ./wdio.conf.js
```

:::caution

codemod இன்னும் TypeScript திட்டங்களை ஆதரிக்கவில்லை. [`@webdriverio/codemod#10`](https://github.com/webdriverio/codemod/issues/10) ஐப் பார்க்கவும். விரைவில் அதற்கான ஆதரவை செயல்படுத்த நாங்கள் பணியாற்றி வருகிறோம். நீங்கள் TypeScript பயன்படுத்துகிறீர்கள் என்றால் தயவுசெய்து ஈடுபடுங்கள்!

:::

## Spec கோப்புகளையும் Page Objects ஐயும் புதுப்பித்தல்

அனைத்து கட்டளை மாற்றங்களையும் புதுப்பிக்க, WebdriverIO கட்டளைகளைக் கொண்ட உங்கள் அனைத்து e2e கோப்புகளிலும் codemod ஐ இயக்கவும், எ.கா.:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v6 ./e2e/*
```

அவ்வளவுதான்! மேலும் மாற்றங்கள் தேவையில்லை 🎉

## முடிவுரை

WebdriverIO `v6` க்கான இந்த மாற்றச் செயல்முறையில் இந்த பயிற்சி உங்களுக்கு சிறிது வழிகாட்டும் என்று நம்புகிறோம். `v7` க்கு மேம்படுத்துவது, அதன் குறைந்த எண்ணிக்கையிலான முறிவு மாற்றங்கள் காரணமாக எளிதானது என்பதால், சமீபத்திய பதிப்புக்கு தொடர்ந்து மேம்படுத்துவதை நாங்கள் வலுவாக பரிந்துரைக்கிறோம். [v7 க்கு மேம்படுத்துவதற்கான](v7-migration) மாற்ற வழிகாட்டியை பார்க்கவும்.

பல்வேறு அமைப்புகளில் உள்ள பல்வேறு குழுக்களுடன் பரிசோதிக்கும்போது, codemod ஐ தொடர்ந்து மேம்படுத்த சமூகம் உதவுகிறது. கருத்து தெரிவிக்க [ஒரு சிக்கலை உருவாக்க](https://github.com/webdriverio/codemod/issues/new) அல்லது மாற்றச் செயல்முறையின் போது சிரமப்படுகிறீர்கள் என்றால் [ஒரு விவாதத்தைத் தொடங்க](https://github.com/webdriverio/codemod/discussions/new) தயங்க வேண்டாம்.