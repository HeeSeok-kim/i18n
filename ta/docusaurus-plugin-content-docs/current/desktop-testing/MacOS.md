---
id: macos
title: MacOS
---

WebdriverIO [Appium](https://appium.io/docs/en/2.0/) பயன்படுத்தி எந்தவொரு MacOS பயன்பாட்டையும் தானியக்கமாக்க முடியும். உங்கள் கணினியில் [XCode](https://developer.apple.com/xcode/) நிறுவப்பட்டிருக்க வேண்டும், Appium மற்றும் [Mac2 Driver](https://github.com/appium/appium-mac2-driver) சார்புகளாக நிறுவப்பட்டிருக்க வேண்டும் மற்றும் சரியான திறன்களை அமைக்க வேண்டும்.

## தொடங்குதல்

புதிய WebdriverIO திட்டத்தைத் தொடங்க, இயக்கவும்:

```sh
npm create wdio@latest ./
```

ஒரு நிறுவல் வழிகாட்டி உங்களை செயல்முறையின் மூலம் வழிநடத்தும். நீங்கள் எந்த வகையான சோதனையைச் செய்ய விரும்புகிறீர்கள் என்று கேட்கும்போது _"Desktop Testing - of MacOS Applications"_ என்பதைத் தேர்ந்தெடுப்பதை உறுதிப்படுத்தவும். பின்னர் இயல்புநிலை அமைப்புகளை வைத்திருக்கவும் அல்லது உங்கள் விருப்பப்படி மாற்றவும்.

கட்டமைப்பு வழிகாட்டி தேவையான அனைத்து Appium தொகுப்புகளையும் நிறுவி, MacOS இல் சோதிக்கத் தேவையான கட்டமைப்புகளுடன் ஒரு `wdio.conf.js` அல்லது `wdio.conf.ts` உருவாக்கும். சில சோதனை கோப்புகளை தானாகவே உருவாக்க ஒப்புக்கொண்டிருந்தால், நீங்கள் `npm run wdio` மூலம் உங்கள் முதல் சோதனையை இயக்கலாம்.

<CreateMacOSProjectAnimation />

அவ்வளவுதான் 🎉

## உதாரணம்

கால்குலேட்டர் பயன்பாட்டைத் திறக்கும், ஒரு கணக்கீட்டைச் செய்து, அதன் முடிவை சரிபார்க்கும் ஒரு எளிய சோதனை இப்படி இருக்கும்:

```js
describe('My Login application', () => {
    it('should set a text to a text view', async function () {
        await $('//XCUIElementTypeButton[@label="seven"]').click()
        await $('//XCUIElementTypeButton[@label="multiply"]').click()
        await $('//XCUIElementTypeButton[@label="six"]').click()
        await $('//XCUIElementTypeButton[@title="="]').click()
        await expect($('//XCUIElementTypeStaticText[@label="main display"]')).toHaveText('42')
    });
})
```

__குறிப்பு:__ `'appium:bundleId': 'com.apple.calculator'` என்பது திறனாக வரையறுக்கப்பட்டதால் கால்குலேட்டர் பயன்பாடு அமர்வின் தொடக்கத்தில் தானாகவே திறக்கப்பட்டது. நீங்கள் அமர்வின் போது எந்த நேரத்திலும் பயன்பாடுகளை மாற்றலாம்.

## மேலும் தகவல்

MacOS இல் சோதிப்பது தொடர்பான குறிப்பிட்ட தகவல்களுக்கு, [Appium Mac2 Driver](https://github.com/appium/appium-mac2-driver) திட்டத்தைப் பார்க்க பரிந்துரைக்கிறோம்.