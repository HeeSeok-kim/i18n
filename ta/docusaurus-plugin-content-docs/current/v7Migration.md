---
id: v7-migration
title: v6 இல் இருந்து v7 வரை
---

இந்த பயிற்சி `v6` WebdriverIO ஐப் பயன்படுத்தும் மக்களுக்கும், `v7`க்கு மாற விரும்புவோருக்கும் உரியது. எங்களின் [வெளியீட்டு வலைப்பதிவில்](https://webdriver.io/blog/2021/02/09/webdriverio-v7-released) குறிப்பிட்டுள்ளபடி, மாற்றங்கள் பெரும்பாலும் உள் அமைப்பிலேயே உள்ளன, மேம்படுத்துவது ஒரு நேரடி செயல்முறையாக இருக்க வேண்டும்.

:::info

நீங்கள் WebdriverIO `v5` அல்லது அதற்கு கீழே பயன்படுத்தினால், முதலில் `v6`க்கு மேம்படுத்தவும். எங்கள் [v6 மாற்ற வழிகாட்டியை](v6-migration) பார்க்கவும்.

:::

இதற்கு முழுமையாக தானியங்கி செயல்முறை இருக்க வேண்டும் என்று நாங்கள் விரும்பினாலும், உண்மை வேறுவிதமாக உள்ளது. ஒவ்வொருவரும் வித்தியாசமான அமைப்பைக் கொண்டுள்ளனர். ஒவ்வொரு படியும் படிப்படியான வழிமுறையாக இல்லாமல் ஒரு வழிகாட்டியாகவே பார்க்கப்பட வேண்டும். மாற்றத்தில் சிக்கல்கள் இருந்தால், [எங்களை தொடர்பு கொள்ள](https://github.com/webdriverio/codemod/discussions/new) தயங்க வேண்டாம்.

## அமைப்பு

மற்ற மாற்றங்களைப் போலவே, WebdriverIO [codemod](https://github.com/webdriverio/codemod) ஐப் பயன்படுத்தலாம். இந்த பயிற்சிக்கு, சமூக உறுப்பினரால் சமர்ப்பிக்கப்பட்ட [boilerplate திட்டத்தை](https://github.com/WarleyGabriel/demo-webdriverio-cucumber) பயன்படுத்தி, `v6` இலிருந்து `v7`க்கு முழுமையாக மாற்றுவோம்.

codemod ஐ நிறுவ, இயக்கவும்:

```sh
npm install jscodeshift @wdio/codemod
```

#### கமிட்கள்:

- _install codemod deps_ [[6ec9e52]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/6ec9e52038f7e8cb1221753b67040b0f23a8f61a)

## WebdriverIO சார்புகளை மேம்படுத்தவும்

எல்லா WebdriverIO பதிப்புகளும் ஒன்றுக்கொன்று இறுக்கமாக இருப்பதால், எப்போதும் ஒரு குறிப்பிட்ட டேக், எ.கா. `latest`க்கு மேம்படுத்துவது சிறந்தது. அதற்கு, WebdriverIO தொடர்பான அனைத்து சார்புகளையும் `package.json` இலிருந்து நகலெடுத்து, பின்வருமாறு மீண்டும் நிறுவுவோம்:

```sh
npm i --save-dev @wdio/allure-reporter@7 @wdio/cli@7 @wdio/cucumber-framework@7 @wdio/local-runner@7 @wdio/spec-reporter@7 @wdio/sync@7 wdio-chromedriver-service@7 wdio-timeline-reporter@7 webdriverio@7
```

பொதுவாக WebdriverIO சார்புகள் டெவ் சார்புகளின் ஒரு பகுதியாகும், உங்கள் திட்டத்தைப் பொறுத்து இது மாறுபடலாம். இதற்குப் பிறகு உங்கள் `package.json` மற்றும் `package-lock.json` புதுப்பிக்கப்பட வேண்டும். __குறிப்பு:__ இவை [உதாரண திட்டத்தால்](https://github.com/WarleyGabriel/demo-webdriverio-cucumber) பயன்படுத்தப்படும் சார்புகள், உங்களுடையது வேறுபடலாம்.

#### கமிட்கள்:

- _updated dependencies_ [[7097ab6]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/7097ab6297ef9f37ead0a9c2ce9fce8d0765458d)

## கான்ஃபிக் கோப்பை மாற்றியமைத்தல்

கான்ஃபிக் கோப்பில் தொடங்குவது நல்ல முதல் படியாகும். WebdriverIO `v7` இல் எந்த கம்பைலர்களையும் கைமுறையாக பதிவு செய்ய வேண்டியதில்லை. உண்மையில் அவை அகற்றப்பட வேண்டும். இதை codemod மூலம் முழுமையாக தானியங்கி முறையில் செய்யலாம்:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v7 ./wdio.conf.js
```

:::caution

codemod இன்னும் TypeScript திட்டங்களை ஆதரிக்கவில்லை. [`@webdriverio/codemod#10`](https://github.com/webdriverio/codemod/issues/10) ஐப் பார்க்கவும். நாங்கள் விரைவில் அதற்கான ஆதரவை செயல்படுத்த முயற்சிக்கிறோம். நீங்கள் TypeScript பயன்படுத்தினால் தயவுசெய்து பங்கேற்கவும்!

:::

#### கமிட்கள்:

- _transpile config file_ [[6015534]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/60155346a386380d8a77ae6d1107483043a43994)

## ஸ்டெப் டெஃபினிஷன்களை புதுப்பித்தல்

நீங்கள் Jasmine அல்லது Mocha பயன்படுத்தினால், நீங்கள் முடித்துவிட்டீர்கள். கடைசி படி Cucumber.js இறக்குமதிகளை `cucumber` இலிருந்து `@cucumber/cucumber`க்கு புதுப்பிப்பதாகும். இதையும் codemod மூலம் தானாகவே செய்யலாம்:

```sh
npx jscodeshift -t ./node_modules/@wdio/codemod/v7 ./src/e2e/*
```

அவ்வளவுதான்! வேறு மாற்றங்கள் தேவையில்லை 🎉

#### கமிட்கள்:

- _transpile step definitions_ [[8c97b90]](https://github.com/WarleyGabriel/demo-webdriverio-cucumber/pull/11/commits/8c97b90a8b9197c62dffe4e2954f7dad814753cc)

## முடிவுரை

WebdriverIO `v7`க்கு மாறும் செயல்முறையில் இந்த பயிற்சி உங்களுக்கு சிறிது வழிகாட்டும் என்று நம்புகிறோம். சமூகம் தொடர்ந்து codemod ஐ மேம்படுத்தி, பல்வேறு நிறுவனங்களில் பல்வேறு குழுக்களுடன் சோதிக்கிறது. கருத்துக்கள் இருந்தால் [பிரச்சினையை எழுப்ப](https://github.com/webdriverio/codemod/issues/new) அல்லது நீங்கள் மாற்றுவதில் சிரமப்பட்டால் [விவாதத்தைத் தொடங்க](https://github.com/webdriverio/codemod/discussions/new) தயங்க வேண்டாம்.