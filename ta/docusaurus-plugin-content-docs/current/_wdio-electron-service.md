---
id: wdio-electron-service
title: எலெக்ட்ரான் சேவை
custom_edit_url: https://github.com/webdriverio-community/wdio-electron-service/edit/main/README.md
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

> wdio-electron-service is a 3rd party package, for more information please see [GitHub](https://github.com/webdriverio-community/wdio-electron-service) | [npm](https://www.npmjs.com/package/wdio-electron-service)

<a href="https://www.npmjs.com/package/wdio-electron-service" alt="NPM Version">
  <img src="https://img.shields.io/npm/v/wdio-electron-service" /></a>
<a href="https://www.npmjs.com/package/wdio-electron-service/v/lts" alt="NPM LTS Version">
  <img src="https://img.shields.io/npm/v/wdio-electron-service/lts" /></a>
<a href="https://www.npmjs.com/package/wdio-electron-service/v/next" alt="NPM Next Version">
  <img src="https://img.shields.io/npm/v/wdio-electron-service/next" /></a>
<a href="https://www.npmjs.com/package/wdio-electron-service" alt="NPM Downloads">
  <img src="https://img.shields.io/npm/dw/wdio-electron-service" /></a>

<br />

**எலெக்ட்ரான் பயன்பாடுகளை சோதிப்பதற்கான WebdriverIO சேவை**

விரிவான WebdriverIO சூழலமைப்பின் மூலம் எலெக்ட்ரான் பயன்பாடுகளின் குறுக்கு-தளம் E2E சோதனையை செயல்படுத்துகிறது.

[Spectron](https://github.com/electron-userland/spectron) ([RIP](https://github.com/electron-userland/spectron/issues/1045)) இன் ஆன்மீக வாரிசு.

### அம்சங்கள்

எலெக்ட்ரான் பயன்பாடுகளை சோதிப்பதை எளிதாக்குகிறது:

- 🚗 தேவையான Chromedriver-ஐ தானாகவே அமைத்தல் (எலெக்ட்ரான் v26 மற்றும் அதற்கு மேற்பட்டவற்றிற்கு)
- 📦 உங்கள் எலெக்ட்ரான் பயன்பாட்டின் தானியங்கி பாதை கண்டறிதல்
  - [Electron Forge](https://www.electronforge.io/), [Electron Builder](https://www.electron.build/) மற்றும் கட்டவிழ்க்கப்படாத பயன்பாடுகளை ஆதரிக்கிறது
- 🧩 உங்கள் சோதனைகளில் எலெக்ட்ரான் API-களை அணுகவும்
- 🕵️ Vitest போன்ற API மூலம் எலெக்ட்ரான் API-களை மாற்றீடு செய்தல்

## நிறுவல்

நீங்கள் `WebdriverIO`-ஐ நிறுவ வேண்டும், அறிவுறுத்தல்களை [இங்கே](https://webdriver.io/docs/gettingstarted) காணலாம்.

## விரைவு தொடக்கம்

விரைவாக தொடங்குவதற்கான பரிந்துரைக்கப்பட்ட வழி [WDIO உள்ளமைவு வழிகாட்டி](https://webdriver.io/docs/gettingstarted#initiate-a-webdriverio-setup) ஐ பயன்படுத்துவதாகும்.

### கைமுறை விரைவு தொடக்கம்

உள்ளமைவு வழிகாட்டியைப் பயன்படுத்தாமல் தொடங்க, நீங்கள் சேவையையும் `@wdio/cli` ஐயும் நிறுவ வேண்டும்:

```bash
npm install --dev @wdio/cli wdio-electron-service
```

அல்லது உங்கள் விருப்பமான தொகுப்பு மேலாளரைப் பயன்படுத்தவும் - pnpm, yarn போன்றவை.

அடுத்து, உங்கள் WDIO உள்ளமைவு கோப்பை உருவாக்கவும். இதற்கு ஏதேனும் உதவி தேவைப்பட்டால், இந்த ரெப்போசிட்டரியின் [எடுத்துக்காட்டு டைரக்டரியில்](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./example/wdio.conf.ts) வேலை செய்யும் உள்ளமைவு உள்ளது, மேலும் [WDIO உள்ளமைவு குறிப்பு பக்கமும்](https://webdriver.io/docs/configuration) உள்ளது.

நீங்கள் உங்கள் சேவைகள் வரிசையில் `electron` ஐச் சேர்க்க வேண்டும் மற்றும் ஒரு எலெக்ட்ரான் திறன்களை அமைக்க வேண்டும், எ.கா.:

_`wdio.conf.ts`_

```ts
export const config = {
  // ...
  services: ['electron'],
  capabilities: [
    {
      browserName: 'electron',
    },
  ],
  // ...
};
```

இறுதியாக, உங்கள் உள்ளமைவு கோப்பைப் பயன்படுத்தி [சில சோதனைகளை இயக்கவும்](https://webdriver.io/docs/gettingstarted#run-test).

இது WDIO Chrome அல்லது Firefox போன்ற உலாவிகளை கையாளும் அதே வழியில் உங்கள் பயன்பாட்டின் ஒரு நிகழ்வை உருவாக்கும். உங்கள் பயன்பாட்டின் பல நிகழ்வுகளை அல்லது உங்கள் பயன்பாடு மற்றும் இணைய உலாவிகளின் வெவ்வேறு கலவைகளை ஒரே நேரத்தில் இயக்க வேண்டும் என்றால், இந்த சேவை [WDIO (இணையான) மல்டிரிமோட்](https://webdriver.io/docs/multiremote) உடன் செயல்படுகிறது.

உங்கள் ஆப்பை கட்டுப்படுத்த [Electron Forge](https://www.electronforge.io/) அல்லது [Electron Builder](https://www.electron.build/) ஐப் பயன்படுத்தினால், உங்கள் கட்டப்பட்ட எலெக்ட்ரான் பயன்பாட்டிற்கான பாதையைக் கண்டறிய சேவை தானாகவே முயற்சிக்கும். தனிப்பயன் சேவை திறன்கள் மூலம் இயங்குநிரலுக்கான தனிப்பயன் பாதையை வழங்கலாம், எ.கா.:

_`wdio.conf.ts`_

```ts
export const config = {
  // ...
  capabilities: [
    {
      'browserName': 'electron',
      'wdio:electronServiceOptions': {
        appBinaryPath: './path/to/built/electron/app.exe',
        appArgs: ['foo', 'bar=baz'],
      },
    },
  ],
  // ...
};
```

எலெக்ட்ரானால் ஆதரிக்கப்படும் வெவ்வேறு இயக்க முறைமைகளுக்கான `appBinaryPath` மதிப்பைக் கண்டறிவதற்கான [உள்ளமைவு டாக்](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/configuration/service-configuration.md#appbinarypath) ஐப் பார்க்கவும்.

மாற்றாக, நீங்கள் `main.js` ஸ்கிரிப்டிற்கான பாதையை வழங்குவதன் மூலம் கட்டவிழ்க்கப்படாத பயன்பாட்டிற்கு சேவையை இயக்கலாம். எலெக்ட்ரான் உங்கள் `node_modules` இல் நிறுவப்பட வேண்டும். கட்டவிழ்க்கப்படாத பயன்பாடுகளை Rollup, Parcel, Webpack போன்ற ஒரு கட்டுபவரைப் பயன்படுத்தி கட்டுவது பரிந்துரைக்கப்படுகிறது.

_`wdio.conf.ts`_

```ts
export const config = {
  // ...
  capabilities: [
    {
      'browserName': 'electron',
      'wdio:electronServiceOptions': {
        appEntryPoint: './path/to/bundled/electron/main.bundle.js',
        appArgs: ['foo', 'bar=baz'],
      },
    },
  ],
  // ...
};
```

## Chromedriver உள்ளமைவு

**உங்கள் பயன்பாடு v26 ஐ விட குறைவான எலெக்ட்ரான் பதிப்பைப் பயன்படுத்தினால், நீங்கள் [கைமுறையாக Chromedriver ஐ உள்ளமைக்க](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/configuration/chromedriver-configuration.md#user-managed) வேண்டும்.**

இதற்கு காரணம் WDIO, Chromedriver ஐ பதிவிறக்க Chrome for Testing ஐப் பயன்படுத்துகிறது, இது v115 அல்லது அதற்கு மேற்பட்ட Chromedriver பதிப்புகளை மட்டுமே வழங்குகிறது.

## ஆவணங்கள்

**[சேவை உள்ளமைவு](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/configuration/service-configuration.md)** \
**[Chromedriver உள்ளமைவு](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/configuration/chromedriver-configuration.md)** \
**[எலெக்ட்ரான் API-களை அணுகுதல்](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/electron-apis/accessing-apis.md)** \
**[எலெக்ட்ரான் API-களை மாற்றீடு செய்தல்](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/electron-apis/mocking-apis.md)** \
**[சாளர மேலாண்மை](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/window-management.md)** \
**[தனித்த பயன்முறை](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/standalone-mode.md)** \
**[மேம்பாடு](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/development.md)** \
**[பொதுவான சிக்கல்கள் & பிழைத்திருத்தம்](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/common-issues-debugging.md)**

## மேம்பாடு

நீங்கள் பங்களிக்க விரும்பினால் [மேம்பாட்டு டாக்](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/development.md) ஐப் படிக்கவும்.

## எடுத்துக்காட்டு ஒருங்கிணைப்புகள்

எடுத்துக்காட்டு பயன்பாட்டில் WebdriverIO ஐ எவ்வாறு ஒருங்கிணைப்பது என்பதைக் காட்டும் எங்கள் [எலெக்ட்ரான் பாய்லர்பிளேட்](https://github.com/webdriverio/electron-boilerplate) திட்டத்தைப் பார்க்கவும். நீங்கள் இந்த ரெப்போசிட்டரியில் உள்ள [எடுத்துக்காட்டு பயன்பாடுகள்](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./apps/) மற்றும் [E2Es](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./e2e/) டைரக்டரிகளையும் பார்க்கலாம்.

## ஆதரவு

சேவையுடன் WDIO ஐ இயக்குவதில் சிக்கல்கள் இருந்தால், முதலில் ஆவணப்படுத்தப்பட்ட [பொதுவான சிக்கல்களைப்](https://github.com/webdriverio-community/wdio-electron-service/blob/main/./docs/common-issues.md) பார்க்க வேண்டும், பின்னர் [முக்கிய WDIO ஃபோரத்தில்](https://github.com/webdriverio/webdriverio/discussions) ஒரு விவாதத்தைத் தொடங்கவும்.

எலெக்ட்ரான் சேவை விவாத மன்றம் WDIO ஐ விட மிகவும் குறைவான செயல்பாட்டைக் கொண்டுள்ளது, ஆனால் நீங்கள் அனுபவிக்கும் சிக்கல் எலெக்ட்ரான் அல்லது சேவையைப் பயன்படுத்துவதற்கு குறிப்பிட்டதாக இருந்தால், நீங்கள் [இங்கே](https://github.com/webdriverio-community/wdio-electron-service/discussions) ஒரு விவாதத்தைத் தொடங்கலாம்.