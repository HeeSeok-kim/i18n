---
id: gettingstarted
title: தொடங்குதல்
---

WebdriverIO ஆவணத்திற்கு வரவேற்கிறோம். இது உங்களை விரைவாக தொடங்க உதவும். நீங்கள் சிக்கல்களை சந்திக்கும் போது, எங்கள் [Discord ஆதரவு சர்வர்](https://discord.webdriver.io) இல் உதவி மற்றும் பதில்களைக் காணலாம் அல்லது என்னுடன் [Twitter](https://twitter.com/webdriverio) இல் தொடர்பு கொள்ளலாம்.

:::info
இவை WebdriverIO இன் சமீபத்திய பதிப்பிற்கான ஆவணங்கள் (__>=9.x__). நீங்கள் இன்னும் பழைய பதிப்பைப் பயன்படுத்திக் கொண்டிருந்தால், [பழைய ஆவண இணையதளங்களைப்](/versions) பார்வையிடவும்!
:::

<LiteYouTubeEmbed
    id="rA4IFNyW54c"
    title="Getting Started with WebdriverIO"
/>

:::tip அதிகாரப்பூர்வ YouTube சேனல் 🎥

நீங்கள் WebdriverIO பற்றிய மேலும் வீடியோக்களை [அதிகாரப்பூர்வ YouTube சேனலில்](https://youtube.com/@webdriverio) காணலாம். நீங்கள் சந்தா செய்வதை உறுதிசெய்யுங்கள்!

:::

## WebdriverIO அமைப்பை தொடங்குதல்

ஏற்கனவே உள்ள அல்லது புதிய திட்டத்திற்கு முழு WebdriverIO அமைப்பை [WebdriverIO ஸ்டார்டர் டூல்கிட்](https://www.npmjs.com/package/create-wdio) பயன்படுத்தி சேர்க்க, இயக்கவும்:

நீங்கள் ஏற்கனவே உள்ள திட்டத்தின் ரூட் டைரக்டரியில் இருந்தால், இயக்கவும்:

<Tabs
  defaultValue="npm"
  values={[
    {label: 'NPM', value: 'npm'},
    {label: 'Yarn', value: 'yarn'},
    {label: 'pnpm', value: 'pnpm'},
    {label: 'bun', value: 'bun'},
  ]
}>
<TabItem value="npm">

```sh
npm init wdio@latest .
```

அல்லது நீங்கள் ஒரு புதிய திட்டத்தை உருவாக்க விரும்பினால்:

```sh
npm init wdio@latest ./path/to/new/project
```

</TabItem>
<TabItem value="yarn">

```sh
yarn create wdio .
```

அல்லது நீங்கள் ஒரு புதிய திட்டத்தை உருவாக்க விரும்பினால்:

```sh
yarn create wdio ./path/to/new/project
```

</TabItem>
<TabItem value="pnpm">

```sh
pnpm create wdio@latest .
```

அல்லது நீங்கள் ஒரு புதிய திட்டத்தை உருவாக்க விரும்பினால்:

```sh
pnpm create wdio@latest ./path/to/new/project
```

</TabItem>
<TabItem value="bun">

```sh
bun create wdio@latest .
```

அல்லது நீங்கள் ஒரு புதிய திட்டத்தை உருவாக்க விரும்பினால்:

```sh
bun create wdio@latest ./path/to/new/project
```

</TabItem>
</Tabs>

இந்த ஒற்றை கட்டளை WebdriverIO CLI கருவியைப் பதிவிறக்கி, உங்கள் சோதனை தொகுப்பை உள்ளமைக்க உதவும் கட்டமைப்பு வழிகாட்டியை இயக்குகிறது.

<CreateProjectAnimation />

வழிகாட்டி உங்களை அமைப்பு மூலம் வழிநடத்தும் ஒரு தொகுப்பு கேள்விகளைக் கேட்கும். [Page Object](https://martinfowler.com/bliki/PageObject.html) முறையைப் பயன்படுத்தி Chrome மூலம் Mocha ஐப் பயன்படுத்தும் இயல்புநிலை அமைப்பை தேர்வு செய்ய நீங்கள் `--yes` அளவுருவை அனுப்பலாம்.

<Tabs
  defaultValue="npm"
  values={[
    {label: 'NPM', value: 'npm'},
    {label: 'Yarn', value: 'yarn'},
    {label: 'pnpm', value: 'pnpm'},
    {label: 'bun', value: 'bun'},
  ]
}>
<TabItem value="npm">

```sh
npm init wdio@latest . -- --yes
```

</TabItem>
<TabItem value="yarn">

```sh
yarn create wdio . --yes
```

</TabItem>
<TabItem value="pnpm">

```sh
pnpm create wdio@latest . --yes
```

</TabItem>
<TabItem value="bun">

```sh
bun create wdio@latest . --yes
```

</TabItem>
</Tabs>

## CLI ஐ கைமுறையாக நிறுவுதல்

நீங்கள் CLI தொகுப்பை உங்கள் திட்டத்திற்கு கைமுறையாகவும் சேர்க்கலாம்:

```sh
npm i --save-dev @wdio/cli
npx wdio --version # prints e.g. `8.13.10`

# run configuration wizard
npx wdio config
```

## சோதனையை இயக்குதல்

நீங்கள் `run` கட்டளையைப் பயன்படுத்தி, நீங்கள் உருவாக்கிய WebdriverIO கட்டமைப்பைக் குறிப்பிடுவதன் மூலம் உங்கள் சோதனை தொகுப்பை தொடங்கலாம்:

```sh
npx wdio run ./wdio.conf.js
```

நீங்கள் குறிப்பிட்ட சோதனை கோப்புகளை இயக்க விரும்பினால் `--spec` அளவுருவைச் சேர்க்கலாம்:

```sh
npx wdio run ./wdio.conf.js --spec example.e2e.js
```

அல்லது உங்கள் கட்டமைப்பு கோப்பில் தொகுப்புகளை வரையறுத்து, ஒரு தொகுப்பில் வரையறுக்கப்பட்ட சோதனை கோப்புகளை மட்டும் இயக்கலாம்:

```sh
npx wdio run ./wdio.conf.js --suite exampleSuiteName
```

## ஸ்கிரிப்டில் இயக்கவும்

நீங்கள் WebdriverIO ஐ [தனித்து இயங்கும் முறையில்](/docs/setuptypes#standalone-mode) Node.JS ஸ்கிரிப்டில் தானியங்கி இயந்திரமாகப் பயன்படுத்த விரும்பினால், நீங்கள் WebdriverIO ஐ நேரடியாக நிறுவி, அதை ஒரு தொகுப்பாகப் பயன்படுத்தலாம், எ.கா. ஒரு இணையதளத்தின் திரைப்பிடிப்பை உருவாக்க:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/fc362f2f8dd823d294b9bb5f92bd5991339d4591/getting-started/run-in-script.js#L2-L19
```

__குறிப்பு:__ அனைத்து WebdriverIO கட்டளைகளும் ஒத்திசைவற்றவை மற்றும் [`async/await`](https://javascript.info/async-await) பயன்படுத்தி சரியாக கையாளப்பட வேண்டும்.

## சோதனைகளை பதிவு செய்தல்

WebdriverIO உங்கள் சோதனை செயல்களை திரையில் பதிவு செய்து WebdriverIO சோதனை ஸ்கிரிப்ட்களை தானாகவே உருவாக்க உதவும் கருவிகளை வழங்குகிறது. மேலும் தகவலுக்கு [Chrome DevTools Recorder உடன் பதிவாளர் சோதனைகள்](/docs/record) பார்க்கவும்.

## கணினி தேவைகள்

உங்களுக்கு [Node.js](http://nodejs.org) நிறுவப்பட்டிருக்க வேண்டும்.

- இது மிகப் பழைய செயலில் உள்ள LTS பதிப்பாக இருப்பதால், குறைந்தது v18.20.0 அல்லது அதற்கு மேற்பட்டதை நிறுவவும்
- LTS வெளியீடுகளாக உள்ள அல்லது ஆகப்போகும் வெளியீடுகள் மட்டுமே அதிகாரப்பூர்வமாக ஆதரிக்கப்படுகின்றன

உங்கள் கணினியில் Node தற்போது நிறுவப்படவில்லை என்றால், பல செயலில் உள்ள Node.js பதிப்புகளை நிர்வகிப்பதற்கு [NVM](https://github.com/creationix/nvm) அல்லது [Volta](https://volta.sh/) போன்ற கருவியைப் பயன்படுத்த பரிந்துரைக்கிறோம். NVM ஒரு பிரபலமான தேர்வாகும், அதே நேரத்தில் Volta கூட ஒரு நல்ல மாற்றாகும்.