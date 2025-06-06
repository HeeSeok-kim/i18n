---
id: bamboo
title: பாம்பூ
---

WebdriverIO [பாம்பூ](https://www.atlassian.com/software/bamboo) போன்ற CI அமைப்புகளுடன் இறுக்கமான ஒருங்கிணைப்பை வழங்குகிறது. [JUnit](https://webdriver.io/docs/junit-reporter.html) அல்லது [Allure](https://webdriver.io/docs/allure-reporter.html) அறிக்கையாளர் மூலம், உங்கள் சோதனைகளை எளிதாக பிழைத்திருத்தம் செய்யலாம் மற்றும் உங்கள் சோதனை முடிவுகளை கண்காணிக்கலாம். ஒருங்கிணைப்பு மிகவும் எளிதானது.

1. JUnit சோதனை அறிக்கையாளரை நிறுவவும்: `$ npm install @wdio/junit-reporter --save-dev`)
1. பாம்பூ கண்டறியக்கூடிய இடத்தில் உங்கள் JUnit முடிவுகளைச் சேமிக்க உங்கள் கட்டமைப்பைப் புதுப்பிக்கவும் (`junit` அறிக்கையாளரை குறிப்பிடவும்):

```js
// wdio.conf.js
module.exports = {
    // ...
    reporters: [
        'dot',
        ['junit', {
            outputDir: './testresults/'
        }]
    ],
    // ...
}
```
குறிப்பு: *சோதனை முடிவுகளை மூல கோப்புறையில் வைப்பதை விட தனி கோப்புறையில் வைப்பது எப்போதும் நல்ல நியமமாகும்.*

```js
// wdio.conf.js - இணையாக இயங்கும் சோதனைகளுக்கு
module.exports = {
    // ...
    reporters: [
        'dot',
        ['junit', {
            outputDir: './testresults/',
            outputFileFormat: function (options) {
                return `results-${options.cid}.xml`;
            }
        }]
    ],
    // ...
}
```

அறிக்கைகள் அனைத்து கட்டமைப்புகளுக்கும் ஒத்திருக்கும், நீங்கள் Mocha, Jasmine அல்லது Cucumber போன்ற எதையும் பயன்படுத்தலாம்.

இந்த நேரத்தில், நீங்கள் சோதனைகளை எழுதி ```./testresults/``` கோப்புறையில் முடிவுகள் உருவாக்கப்பட்டிருக்கும் என்றும், உங்கள் பாம்பூ இயங்கிக்கொண்டிருக்கும் என்றும் நாங்கள் நம்புகிறோம்.

## பாம்பூவில் உங்கள் சோதனைகளை ஒருங்கிணைக்கவும்

1. உங்கள் பாம்பூ திட்டத்தைத் திறக்கவும்
    > புதிய திட்டத்தை உருவாக்கவும், உங்கள் களஞ்சியத்தை இணைக்கவும் (எப்போதும் உங்கள் களஞ்சியத்தின் புதிய பதிப்பை சுட்டிக்காட்டுகிறது என்பதை உறுதிசெய்யவும்) மற்றும் உங்கள் நிலைகளை உருவாக்கவும்

    ![திட்ட விவரங்கள்](/img/bamboo/plancreation.png "திட்ட விவரங்கள்")

    நான் இயல்புநிலை கட்டம் மற்றும் வேலையுடன் செல்வேன். உங்கள் சூழலில், நீங்கள் உங்கள் சொந்த நிலைகள் மற்றும் வேலைகளை உருவாக்கலாம்

    ![இயல்புநிலை கட்டம்](/img/bamboo/defaultstage.png "இயல்புநிலை கட்டம்")
2. உங்கள் சோதனை வேலையைத் திறந்து பாம்பூவில் உங்கள் சோதனைகளை இயக்க பணிகளை உருவாக்கவும்
    >**பணி 1:** மூல குறியீடு செக்அவுட்

    >**பணி 2:** உங்கள் சோதனைகளை இயக்கவும் ```npm i && npm run test```. மேலே உள்ள கட்டளைகளை இயக்க நீங்கள் *ஸ்கிரிப்ட்* பணி மற்றும் *ஷெல் விளக்கி* பயன்படுத்தலாம் (இது சோதனை முடிவுகளை உருவாக்கி ```./testresults/``` கோப்புறையில் சேமிக்கும்)

    ![சோதனை இயக்கம்](/img/bamboo/testrun.png "சோதனை இயக்கம்")

    >**பணி: 3** உங்கள் சேமித்த சோதனை முடிவுகளை பாகுபடுத்த *jUnit Parser* பணியைச் சேர்க்கவும். இங்கே சோதனை முடிவுகள் அடைவு குறிப்பிடவும் (நீங்கள் Ant ஸ்டைல் பேட்டர்ன்களையும் பயன்படுத்தலாம்)

    ![jUnit Parser](/img/bamboo/junitparser.png "jUnit Parser")

    குறிப்பு: *உங்கள் சோதனை பணி தோல்வியடைந்தாலும் எப்போதும் செயல்படுத்தப்படுவதை உறுதிசெய்ய, முடிவுகள் பாகுபடுத்தி பணியை *இறுதி* பகுதியில் வைத்திருப்பதை உறுதிசெய்யவும்*

    >**பணி: 4** (விருப்பத்தேர்வு) உங்கள் சோதனை முடிவுகள் பழைய கோப்புகளுடன் குழப்பமடையாமல் இருப்பதை உறுதிசெய்ய, பாம்பூவுக்கு வெற்றிகரமான பகுப்பாய்வுக்குப் பிறகு ```./testresults/``` கோப்புறையை அகற்ற ஒரு பணியை உருவாக்கலாம். முடிவுகளை அகற்ற ```rm -f ./testresults/*.xml``` போன்ற ஷெல் ஸ்கிரிப்ட்டை சேர்க்கலாம் அல்லது முழு கோப்புறையை அகற்ற ```rm -r testresults``` ஐ பயன்படுத்தலாம்

மேலே உள்ள *ராக்கெட் அறிவியல்* முடிந்தவுடன், திட்டத்தை இயக்கி இயக்கவும். உங்கள் இறுதி வெளியீடு இவ்வாறு இருக்கும்:

## வெற்றிகரமான சோதனை

![வெற்றிகரமான சோதனை](/img/bamboo/successfulltest.png "வெற்றிகரமான சோதனை")

## தோல்வியடைந்த சோதனை

![தோல்வியடைந்த சோதனை](/img/bamboo/failedtest.png "தோல்வியடைந்த சோதனை")

## தோல்வியடைந்து பின் சரிசெய்யப்பட்டது

![தோல்வியடைந்து சரிசெய்யப்பட்டது](/img/bamboo/failedandfixed.png "தோல்வியடைந்து சரிசெய்யப்பட்டது")

ஆஹா!! அவ்வளவுதான். நீங்கள் வெற்றிகரமாக உங்கள் WebdriverIO சோதனைகளை பாம்பூவில் ஒருங்கிணைத்துள்ளீர்கள்.