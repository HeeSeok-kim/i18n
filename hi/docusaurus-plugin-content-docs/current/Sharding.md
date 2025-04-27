---
id: sharding
title: शार्डिंग
---

डिफ़ॉल्ट रूप से, WebdriverIO परीक्षणों को समानांतर में चलाता है और आपकी मशीन पर CPU कोर के इष्टतम उपयोग के लिए प्रयास करता है। और भी अधिक समानांतरीकरण प्राप्त करने के लिए, आप एक साथ कई मशीनों पर परीक्षण चलाकर WebdriverIO परीक्षण निष्पादन को और अधिक बढ़ा सकते हैं। हम इस संचालन के मोड को "शार्डिंग" कहते हैं।

## कई मशीनों के बीच परीक्षणों की शार्डिंग

टेस्ट सूट को शार्ड करने के लिए, कमांड लाइन में `--shard=x/y` पास करें। उदाहरण के लिए, सूट को चार शार्ड्स में विभाजित करने के लिए, प्रत्येक एक चौथाई परीक्षण चलाएगा:

```sh
npx wdio run wdio.conf.js --shard=1/4
npx wdio run wdio.conf.js --shard=2/4
npx wdio run wdio.conf.js --shard=3/4
npx wdio run wdio.conf.js --shard=4/4
```

अब, यदि आप इन शार्ड्स को विभिन्न कंप्यूटरों पर समानांतर में चलाते हैं, तो आपका परीक्षण सूट चार गुना तेजी से पूरा होता है।

## GitHub Actions उदाहरण

GitHub Actions [`jobs.<job_id>.strategy.matrix`](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix) विकल्प का उपयोग करके [कई जॉब्स के बीच शार्डिंग परीक्षणों का समर्थन](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs) करता है। मैट्रिक्स विकल्प प्रदान किए गए विकल्पों के हर संभव संयोजन के लिए एक अलग जॉब चलाएगा।

निम्नलिखित उदाहरण आपको दिखाता है कि चार मशीनों पर समानांतर में अपने परीक्षण चलाने के लिए जॉब को कैसे कॉन्फ़िगर करें। आप पूरा पाइपलाइन सेटअप [Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate/blob/main/.github/workflows/test.yaml) प्रोजेक्ट में पा सकते हैं।

-   सबसे पहले हम अपने जॉब कॉन्फ़िगरेशन में मैट्रिक्स विकल्प जोड़ते हैं, जिसमें शार्ड विकल्प शामिल है जो हमें बनाने वाले शार्ड्स की संख्या बताता है। `shard: [1, 2, 3, 4]` चार शार्ड्स बनाएगा, प्रत्येक के पास एक अलग शार्ड नंबर होगा।
-   फिर हम अपने WebdriverIO परीक्षणों को `--shard ${{ matrix.shard }}/${{ strategy.job-total }}` विकल्प के साथ चलाते हैं। यह प्रत्येक शार्ड के लिए हमारा परीक्षण कमांड होगा।
-   अंत में हम अपनी wdio लॉग रिपोर्ट को GitHub Actions Artifacts में अपलोड करते हैं। यह लॉग्स को उपलब्ध कराएगा यदि शार्ड विफल होता है।

परीक्षण पाइपलाइन इस प्रकार परिभाषित है:

```yaml title=.github/workflows/test.yaml
name: Test

on: [push, pull_request]

jobs:
    lint:
        # ...
    unit:
        # ...
    e2e:
        name: 🧪 Test (${{ matrix.shard }}/${{ strategy.job-total }})
        runs-on: ubuntu-latest
        needs: [lint, unit]
        strategy:
            matrix:
                shard: [1, 2, 3, 4]
        steps:
            - uses: actions/checkout@v4
            - uses: ./.github/workflows/actions/setup
            - name: E2E Test
              run: npm run test:features -- --shard ${{ matrix.shard }}/${{ strategy.job-total }}
            - uses: actions/upload-artifact@v1
              if: failure()
              with:
                  name: logs-${{ matrix.shard }}
                  path: logs
```

यह सभी शार्ड्स को समानांतर में चलाएगा, जिससे परीक्षणों के लिए निष्पादन समय 4 से कम हो जाएगा:

![GitHub Actions उदाहरण](/img/sharding.png "GitHub Actions उदाहरण")

[Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate) प्रोजेक्ट से कमिट [`96d444e`](https://github.com/webdriverio/cucumber-boilerplate/commit/96d444ea23919389682b9b1c9408ed91c452c7f8) देखें जिसने अपने टेस्ट पाइपलाइन में शार्डिंग को पेश किया, जिससे कुल निष्पादन समय `2:23 मिनट` से घटकर `1:30 मिनट` तक हो गया, यानी __37%__ की कमी 🎉।