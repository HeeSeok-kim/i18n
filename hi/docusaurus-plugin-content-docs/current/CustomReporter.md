---
id: customreporter
title: कस्टम रिपोर्टर
---

आप WDIO टेस्ट रनर के लिए अपना खुद का कस्टम रिपोर्टर लिख सकते हैं जो आपकी जरूरतों के अनुरूप हो। और यह आसान है!

आपको बस एक नोड मॉड्यूल बनाना होगा जो `@wdio/reporter` पैकेज से विरासत में मिला हो, ताकि यह टेस्ट से संदेश प्राप्त कर सके।

बेसिक सेटअप कुछ इस तरह का होना चाहिए:

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

इस रिपोर्टर का उपयोग करने के लिए, आपको केवल इसे अपने कॉन्फिगरेशन में `reporter` प्रॉपर्टी को असाइन करने की आवश्यकता है।


आपकी `wdio.conf.js` फ़ाइल इस तरह दिखनी चाहिए:

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

आप रिपोर्टर को NPM पर भी प्रकाशित कर सकते हैं ताकि हर कोई इसका उपयोग कर सके। पैकेज का नाम अन्य रिपोर्टर की तरह `wdio-<reportername>-reporter` रखें, और इसे `wdio` या `wdio-reporter` जैसे कीवर्ड के साथ टैग करें।

## इवेंट हैंडलर

आप कई इवेंट्स के लिए इवेंट हैंडलर रजिस्टर कर सकते हैं जो टेस्टिंग के दौरान ट्रिगर होते हैं। निम्नलिखित सभी हैंडलर वर्तमान स्थिति और प्रगति के बारे में उपयोगी जानकारी के साथ पेलोड प्राप्त करेंगे।

इन पेलोड ऑब्जेक्ट्स की संरचना इवेंट पर निर्भर करती है, और फ्रेमवर्क (Mocha, Jasmine, और Cucumber) में एकरूप होती है। एक बार जब आप एक कस्टम रिपोर्टर लागू करते हैं, तो यह सभी फ्रेमवर्क के लिए काम करना चाहिए।

निम्नलिखित सूची में सभी संभावित विधियाँ शामिल हैं जिन्हें आप अपनी रिपोर्टर क्लास में जोड़ सकते हैं:

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

मेथड के नाम काफी स्पष्ट हैं।

किसी निश्चित इवेंट पर कुछ प्रिंट करने के लिए, `this.write(...)` मेथड का उपयोग करें, जो पैरेंट `WDIOReporter` क्लास द्वारा प्रदान किया गया है। यह या तो सामग्री को `stdout` पर स्ट्रीम करता है, या लॉग फाइल में (रिपोर्टर के विकल्पों के आधार पर)।

```js
import WDIOReporter from '@wdio/reporter'

export default class CustomReporter extends WDIOReporter {
    onTestPass(test) {
        this.write(`Congratulations! Your test "${test.title}" passed 👏`)
    }
}
```

ध्यान दें कि आप किसी भी तरह से टेस्ट एक्जिक्यूशन को स्थगित नहीं कर सकते।

सभी इवेंट हैंडलर को सिंक्रोनस रूटीन को निष्पादित करना चाहिए (या आप रेस कंडीशन में पड़ जाएंगे)।

[उदाहरण अनुभाग](https://github.com/webdriverio/webdriverio/tree/main/examples/wdio) को जरूर देखें जहां आप एक उदाहरण कस्टम रिपोर्टर पा सकते हैं जो प्रत्येक इवेंट के लिए इवेंट नाम प्रिंट करता है।

यदि आपने एक कस्टम रिपोर्टर लागू किया है जो समुदाय के लिए उपयोगी हो सकता है, तो पुल रिक्वेस्ट बनाने में संकोच न करें ताकि हम रिपोर्टर को जनता के लिए उपलब्ध करा सकें!

इसके अलावा, यदि आप WDIO टेस्टरनर को `Launcher` इंटरफ़ेस के माध्यम से चलाते हैं, तो आप निम्नानुसार फ़ंक्शन के रूप में एक कस्टम रिपोर्टर लागू नहीं कर सकते:

```js
import Launcher from '@wdio/cli'

import CustomReporter from './reporter/my.custom.reporter'

const launcher = new Launcher('/path/to/config.file.js', {
    // this will NOT work, because CustomReporter is not serializable
    reporters: ['dot', CustomReporter]
})
```

## `isSynchronised` होने तक प्रतीक्षा करें

अगर आपके रिपोर्टर को डेटा की रिपोर्ट करने के लिए एसिंक ऑपरेशन निष्पादित करने होते हैं (जैसे लॉग फाइलों या अन्य एसेट्स का अपलोड) तो आप अपने कस्टम रिपोर्टर में `isSynchronised` मेथड को ओवरराइट कर सकते हैं ताकि WebdriverIO रनर तब तक प्रतीक्षा करे जब तक आप सबकुछ कंप्यूट न कर लें। इसका एक उदाहरण [`@wdio/sumologic-reporter`](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-sumologic-reporter/src/index.ts) में देखा जा सकता है:

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

इस तरह से रनर तब तक प्रतीक्षा करेगा जब तक सभी लॉग जानकारी अपलोड नहीं हो जाती है।

## NPM पर रिपोर्टर प्रकाशित करें

रिपोर्टर को WebdriverIO समुदाय द्वारा आसानी से उपयोग और खोजने योग्य बनाने के लिए, कृपया इन सिफारिशों का पालन करें:

* सर्विसेज को इस नामकरण कनवेंशन का उपयोग करना चाहिए: `wdio-*-reporter`
* NPM कीवर्ड्स का उपयोग करें: `wdio-plugin`, `wdio-reporter`
* `main` एंट्री को रिपोर्टर का इंस्टेंस `export` करना चाहिए
* उदाहरण रिपोर्टर: [`@wdio/dot-service`](https://github.com/webdriverio/webdriverio/tree/main/packages/wdio-dot-reporter)

अनुशंसित नामकरण पैटर्न का पालन करने से सेवाओं को नाम से जोड़ा जा सकता है:

```js
// Add wdio-custom-reporter
export const config = {
    // ...
    reporter: ['custom'],
    // ...
}
```

### WDIO CLI और डॉक्स में प्रकाशित सेवा जोड़ें

हम वास्तव में हर नए प्लगइन की सराहना करते हैं जो दूसरे लोगों को बेहतर परीक्षण चलाने में मदद कर सकते हैं! यदि आपने ऐसा प्लगइन बनाया है, तो कृपया इसे हमारे CLI और डॉक्स में जोड़ने पर विचार करें ताकि इसे आसानी से खोजा जा सके।

कृपया निम्नलिखित परिवर्तनों के साथ एक पुल रिक्वेस्ट बनाएँ:

- अपनी सेवा को CLI मॉड्यूल में [समर्थित रिपोर्टर्स](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-cli/src/constants.ts#L74-L91)) की सूची में जोड़ें
- आधिकारिक Webdriver.io पेज पर अपने डॉक्स को जोड़ने के लिए [रिपोर्टर सूची](https://github.com/webdriverio/webdriverio/blob/main/scripts/docs-generation/3rd-party/reporters.json) को बढ़ाएं