---
id: customreporter
title: தனிப்பயன் அறிக்கையாளர்
---

நீங்கள் உங்கள் தேவைகளுக்கு ஏற்ப WDIO சோதனை இயக்கிக்கான உங்கள் சொந்த தனிப்பயன் அறிக்கையாளரை எழுதலாம். மேலும், இது எளிதானது!

நீங்கள் செய்ய வேண்டியதெல்லாம், சோதனையிலிருந்து செய்திகளைப் பெறக்கூடிய `@wdio/reporter` பேக்கேஜிலிருந்து வாரிசுரிமை பெறும் ஒரு node மாடூலை உருவாக்குவது.

அடிப்படை அமைப்பு பின்வருமாறு இருக்க வேண்டும்:

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    constructor(options) {
        /*
         * make reporter to write to the output stream by default
         */
        options = Object.assign(options, { stdout: true })
        super(options)
    }

    onTestPass(test) {
        this.write(`Congratulations! Your test "${test.title}" passed 👏`)
    }
}
```

இந்த அறிக்கையாளரைப் பயன்படுத்த, நீங்கள் செய்ய வேண்டியதெல்லாம் உங்கள் கட்டமைப்பில் `reporter` பண்புக்கு அதை ஒதுக்குவதுதான்.


உங்கள் `wdio.conf.js` கோப்பு இவ்வாறு இருக்க வேண்டும்:

```js
import CustomReporter from './reporter/my.custom.reporter'

export const config = {
    // ...
    reporters: [
        /**
         * use imported reporter class
         */
        [CustomReporter, {
            someOption: 'foobar'
        }],
        /**
         * use absolute path to reporter
         */
        ['/path/to/reporter.js', {
            someOption: 'foobar'
        }]
    ],
    // ...
}
```

அறிக்கையாளரை NPM இல் வெளியிட்டு அனைவரும் பயன்படுத்தலாம். பிற அறிக்கையாளர்களைப் போல பேக்கேஜை `wdio-<reportername>-reporter` என்று பெயரிடவும், மேலும் அதை `wdio` அல்லது `wdio-reporter` போன்ற முக்கிய சொற்களால் குறிக்கவும்.

## நிகழ்வு கையாளுதல்

சோதனையின் போது தூண்டப்படும் பல நிகழ்வுகளுக்கு நீங்கள் ஒரு நிகழ்வு கையாளுனரைப் பதிவு செய்யலாம். பின்வரும் அனைத்து கையாளுனர்களும் தற்போதைய நிலை மற்றும் முன்னேற்றம் பற்றிய பயனுள்ள தகவல்களுடன் பேலோட்களைப் பெறும்.

இந்த பேலோட் பொருள்களின் அமைப்பு நிகழ்வைப் பொறுத்தது, மேலும் ஃபிரேம்வொர்க்குகள் (Mocha, Jasmine, மற்றும் Cucumber) முழுவதும் ஒன்றுபட்டது. நீங்கள் ஒரு தனிப்பயன் அறிக்கையாளரை செயல்படுத்தியவுடன், அது அனைத்து ஃபிரேம்வொர்க்குகளுக்கும் பணிபுரிய வேண்டும்.

பின்வரும் பட்டியலில் உங்கள் அறிக்கையாளர் வகுப்பில் சேர்க்கக்கூடிய அனைத்து சாத்தியமான முறைகளும் உள்ளன:

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    onRunnerStart() {}
    onBeforeCommand() {}
    onAfterCommand() {}
    onSuiteStart() {}
    onHookStart() {}
    onHookEnd() {}
    onTestStart() {}
    onTestPass() {}
    onTestFail() {}
    onTestSkip() {}
    onTestEnd() {}
    onSuiteEnd() {}
    onRunnerEnd() {}
}
```

முறைகளின் பெயர்கள் தன்னைத்தானே விளக்கிக் கொள்ளும் படி இருக்கின்றன.

ஒரு குறிப்பிட்ட நிகழ்வில் ஏதாவது அச்சிட, பெற்றோர் `WDIOReporter` வகுப்பால் வழங்கப்படும் `this.write(...)` முறையைப் பயன்படுத்தவும். இது உள்ளடக்கத்தை `stdout`க்கு அல்லது ஒரு பதிவு கோப்புக்கு (அறிக்கையாளரின் விருப்பங்களைப் பொறுத்து) ஸ்ட்ரீம் செய்கிறது.

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    onTestPass(test) {
        this.write(`Congratulations! Your test "${test.title}" passed 👏`)
    }
}
```

நீங்கள் எந்த வகையிலும் சோதனை செயல்பாட்டை தாமதப்படுத்த முடியாது என்பதை கவனிக்கவும்.

அனைத்து நிகழ்வு கையாளுனர்களும் ஒத்திசைவான ரூட்டீன்களை செயல்படுத்த வேண்டும் (அல்லது நீங்கள் ரேஸ் கண்டிஷன்களில் சிக்கிக்கொள்வீர்கள்).

ஒவ்வொரு நிகழ்வுக்கும் நிகழ்வுப் பெயரை அச்சிடும் உதாரண தனிப்பயன் அறிக்கையாளரைக் காணக்கூடிய [உதாரணப் பிரிவை](https://github.com/webdriverio/webdriverio/tree/main/examples/wdio) நிச்சயமாகப் பார்க்கவும்.

நீங்கள் சமூகத்திற்கு பயனுள்ளதாக இருக்கக்கூடிய ஒரு தனிப்பயன் அறிக்கையாளரை செயல்படுத்தியிருந்தால், அறிக்கையாளரை பொதுமக்களுக்குக் கிடைக்கச் செய்ய ஒரு Pull Request செய்ய தயங்க வேண்டாம்!

மேலும், நீங்கள் WDIO சோதனை இயக்கியை `Launcher` இடைமுகம் மூலம் இயக்கினால், நீங்கள் பின்வருமாறு ஒரு தனிப்பயன் அறிக்கையாளரைச் செயல்பாடாகப் பயன்படுத்த முடியாது:

```js
import Launcher from '@wdio/cli'

import CustomReporter from './reporter/my.custom.reporter'

const launcher = new Launcher('/path/to/config.file.js', {
    // this will NOT work, because CustomReporter is not serializable
    reporters: ['dot', CustomReporter]
})
```

## `isSynchronised` வரை காத்திருக்கவும்

உங்கள் அறிக்கையாளர் தரவை அறிக்கை செய்ய ஒத்திசைவற்ற செயல்பாடுகளை செயல்படுத்த வேண்டியிருந்தால் (எ.கா. பதிவு கோப்புகள் அல்லது பிற சொத்துகளின் பதிவேற்றம்) நீங்கள் அனைத்தையும் கணக்கிடும் வரை WebdriverIO இயக்கி காத்திருக்க உங்கள் தனிப்பயன் அறிக்கையாளரில் `isSynchronised` முறையை மேலெழுதலாம். இதற்கான எடுத்துக்காட்டு [`@wdio/sumologic-reporter`](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-sumologic-reporter/src/index.ts) இல் காணலாம்:

```js
export default class SumoLogicReporter extends WDIOReporter {
    constructor (options) {
        // ...
        this.unsynced = []
        this.interval = setInterval(::this.sync, this.options.syncInterval)
        // ...
    }

    /**
     * overwrite isSynchronised method
     */
    get isSynchronised () {
        return this.unsynced.length === 0
    }

    /**
     * sync log files
     */
    sync () {
        // ...
        request({
            method: 'POST',
            uri: this.options.sourceAddress,
            body: logLines
        }, (err, resp) => {
            // ...
            /**
             * remove transferred logs from log bucket
             */
            this.unsynced.splice(0, MAX_LINES)
            // ...
        }
    }
}
```

இந்த வழியில் இயக்கி அனைத்து பதிவு தகவல்களும் பதிவேற்றப்படும் வரை காத்திருக்கும்.

## அறிக்கையாளரை NPM இல் வெளியிடுக

WebdriverIO சமூகத்தால் அறிக்கையாளர்களை எளிதாக நுகரவும் கண்டுபிடிக்கவும், தயவுசெய்து இந்த பரிந்துரைகளைப் பின்பற்றவும்:

* சேவைகள் இந்த பெயரிடல் மரபைப் பயன்படுத்த வேண்டும்: `wdio-*-reporter`
* NPM குறிச்சொற்களைப் பயன்படுத்தவும்: `wdio-plugin`, `wdio-reporter`
* `main` உள்ளீடு அறிக்கையாளரின் நிகழ்வை `export` செய்ய வேண்டும்
* உதாரண அறிக்கையாளர்: [`@wdio/dot-service`](https://github.com/webdriverio/webdriverio/tree/main/packages/wdio-dot-reporter)

பரிந்துரைக்கப்பட்ட பெயரிடல் முறையைப் பின்பற்றுவது சேவைகளை பெயரால் சேர்க்க அனுமதிக்கிறது:

```js
// Add wdio-custom-reporter
export const config = {
    // ...
    reporter: ['custom'],
    // ...
}
```

### வெளியிடப்பட்ட சேவையை WDIO CLI மற்றும் ஆவணங்களில் சேர்க்கவும்

மற்றவர்கள் சிறந்த சோதனைகளை இயக்க உதவக்கூடிய ஒவ்வொரு புதிய செருகுநிரலையும் நாங்கள் மிகவும் பாராட்டுகிறோம்! நீங்கள் அத்தகைய செருகுநிரலை உருவாக்கியிருந்தால், அதை எங்கள் CLI மற்றும் ஆவணங்களில் சேர்க்க பரிசீலிக்கவும், இதனால் அதை எளிதாகக் கண்டுபிடிக்க முடியும்.

தயவுசெய்து பின்வரும் மாற்றங்களுடன் ஒரு pull request ஐ உருவாக்கவும்:

- உங்கள் சேவையை CLI மாடூலில் [ஆதரிக்கப்படும் அறிக்கையாளர்கள்](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-cli/src/constants.ts#L74-L91)) பட்டியலில் சேர்க்கவும்
- உங்கள் ஆவணங்களை அதிகாரப்பூர்வ Webdriver.io பக்கத்தில் சேர்க்க [அறிக்கையாளர் பட்டியலை](https://github.com/webdriverio/webdriverio/blob/main/scripts/docs-generation/3rd-party/reporters.json) மேம்படுத்தவும்