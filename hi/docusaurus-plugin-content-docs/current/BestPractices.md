---
id: bestpractices
title: बेहतर प्रथाएँ
---

# बेहतर प्रथाएँ

इस गाइड का उद्देश्य हमारी बेहतर प्रथाओं को साझा करना है जो आपको बेहतर प्रदर्शन वाले और लचीले टेस्ट लिखने में मदद करेंगी।

## लचीले सिलेक्टर्स का उपयोग करें

DOM में परिवर्तनों के प्रति लचीले सिलेक्टर्स का उपयोग करके, आपके पास कम या कोई भी टेस्ट फेल नहीं होंगे जब उदाहरण के लिए किसी एलिमेंट से कोई क्लास हटा दी जाती है।

क्लासेस को कई एलिमेंट्स पर लागू किया जा सकता है और यदि संभव हो तो इनसे बचना चाहिए, जब तक कि आप जानबूझकर उस क्लास के सभी एलिमेंट्स को प्राप्त करना नहीं चाहते हैं।

```js
// 👎
await $('.button')
```

ये सभी सिलेक्टर्स एक एकल एलिमेंट लौटाने चाहिए।

```js
// 👍
await $('aria/Submit')
await $('[test-id="submit-button"]')
await $('#submit-button')
```

__नोट:__ WebdriverIO द्वारा समर्थित सभी संभावित सिलेक्टर्स के बारे में जानने के लिए, हमारे [Selectors](./Selectors.md) पेज को देखें।

## एलिमेंट क्वेरीज़ की संख्या सीमित करें

हर बार जब आप [`$`](https://webdriver.io/docs/api/browser/$) या [`$$`](https://webdriver.io/docs/api/browser/$$) कमांड का उपयोग करते हैं (इसमें उन्हें चेनिंग करना भी शामिल है), WebdriverIO DOM में एलिमेंट का पता लगाने का प्रयास करता है। ये क्वेरीज़ महंगी होती हैं, इसलिए आपको उन्हें जितना संभव हो सीमित करने का प्रयास करना चाहिए।

तीन एलिमेंट्स की क्वेरी करता है।

```js
// 👎
await $('table').$('tr').$('td')
```

केवल एक एलिमेंट की क्वेरी करता है।

``` js
// 👍
await $('table tr td')
```

चेनिंग का उपयोग केवल तभी करना चाहिए जब आप विभिन्न [सिलेक्टर रणनीतियों](https://webdriver.io/docs/selectors/#custom-selector-strategies) को जोड़ना चाहते हैं।
इस उदाहरण में हम [डीप सिलेक्टर्स](https://webdriver.io/docs/selectors#deep-selectors) का उपयोग करते हैं, जो एक एलिमेंट के शैडो DOM के अंदर जाने की रणनीति है।

``` js
// 👍
await $('custom-datepicker').$('#calendar').$('aria/Select')
```

### सूची से एक लेने के बजाय एक एलिमेंट का पता लगाना पसंद करें

ऐसा करना हमेशा संभव नहीं है, लेकिन [:nth-child](https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-child) जैसे CSS स्यूडो-क्लासेस का उपयोग करके आप उनके माता-पिता की चाइल्ड लिस्ट में एलिमेंट्स के इंडेक्स के आधार पर एलिमेंट्स से मेल कर सकते हैं।

सभी टेबल पंक्तियों को क्वेरी करता है।

```js
// 👎
await $$('table tr')[15]
```

एक एकल टेबल पंक्ति को क्वेरी करता है।

```js
// 👍
await $('table tr:nth-child(15)')
```

## अंतर्निहित एसर्शन्स का उपयोग करें

मैन्युअल एसर्शन्स का उपयोग न करें जो स्वचालित रूप से परिणामों के मेल होने की प्रतीक्षा नहीं करते हैं क्योंकि इससे अस्थिर टेस्ट होंगे।

```js
// 👎
expect(await button.isDisplayed()).toBe(true)
```

अंतर्निहित एसर्शन्स का उपयोग करके WebdriverIO स्वचालित रूप से वास्तविक परिणाम के अपेक्षित परिणाम से मेल खाने की प्रतीक्षा करेगा, जिससे लचीले टेस्ट होंगे।
यह एसर्शन को स्वचालित रूप से पास होने या टाइम आउट होने तक रीट्राई करके ऐसा करता है।

```js
// 👍
await expect(button).toBeDisplayed()
```

## लेज़ी लोडिंग और प्रॉमिस चेनिंग

WebdriverIO के पास क्लीन कोड लिखने के लिए कुछ ट्रिक्स हैं क्योंकि यह एलिमेंट को लेज़ी लोड कर सकता है जो आपको अपने प्रॉमिस को चेन करने और `await` की मात्रा को कम करने की अनुमति देता है। यह आपको एलिमेंट को एक ChainablePromiseElement के रूप में एलिमेंट के बजाय पास करने और पेज ऑब्जेक्ट्स के साथ आसानी से उपयोग करने की भी अनुमति देता है।

तो आपको `await` का उपयोग कब करना चाहिए?
आपको हमेशा `await` का उपयोग करना चाहिए, `$` और `$$` कमांड के अपवाद के साथ।

```js
// 👎
const div = await $('div')
const button = await div.$('button')
await button.click()
// or
await (await (await $('div')).$('button')).click()
```

```js
// 👍
const button = $('div').$('button')
await button.click()
// or
await $('div').$('button').click()
```

## कमांड्स और एसर्शन्स का अत्यधिक उपयोग न करें

expect.toBeDisplayed का उपयोग करते समय आप अप्रत्यक्ष रूप से एलिमेंट के मौजूद होने का भी इंतज़ार करते हैं। जब आपके पास पहले से ही वही काम करने वाला एसर्शन है तो waitForXXX कमांड्स का उपयोग करने की आवश्यकता नहीं है।

```js
// 👎
await button.waitForExist()
await expect(button).toBeDisplayed()

// 👎
await button.waitForDisplayed()
await expect(button).toBeDisplayed()

// 👍
await expect(button).toBeDisplayed()
```

इंटरैक्ट करते समय या टेक्स्ट जैसी कोई चीज़ एसर्ट करते समय एलिमेंट के मौजूद होने या प्रदर्शित होने का इंतज़ार करने की कोई आवश्यकता नहीं है, जब तक कि एलिमेंट स्पष्ट रूप से अदृश्य (उदाहरण के लिए ओपैसिटी: 0) या स्पष्ट रूप से अक्षम (उदाहरण के लिए अक्षम विशेषता) हो सकता है, जिस स्थिति में एलिमेंट के प्रदर्शित होने का इंतज़ार करना समझ में आता है।

```js
// 👎
await expect(button).toBeExisting()
await expect(button).toHaveText('Submit')

// 👎
await expect(button).toBeDisplayed()
await expect(button).toHaveText('Submit')

// 👎
await expect(button).toBeDisplayed()
await button.click()
```

```js
// 👍
await button.click()

// 👍
await expect(button).toHaveText('Submit')
```

## डायनामिक टेस्ट्स

गतिशील टेस्ट डेटा जैसे गुप्त क्रेडेंशियल्स को स्टोर करने के लिए अपने एनवायरनमेंट में एनवायरनमेंट वेरिएबल्स का उपयोग करें, बजाय उन्हें टेस्ट में हार्ड कोड करने के। इस विषय पर अधिक जानकारी के लिए [पैरामीटराइज़ टेस्ट्स](parameterize-tests) पेज पर जाएं।

## अपने कोड को लिंट करें

अपने कोड को लिंट करने के लिए eslint का उपयोग करके आप संभावित रूप से त्रुटियों को जल्दी पकड़ सकते हैं, हमारे [लिंटिंग नियमों](https://www.npmjs.com/package/eslint-plugin-wdio) का उपयोग करें यह सुनिश्चित करने के लिए कि कुछ सर्वोत्तम प्रथाओं को हमेशा लागू किया जाता है।

## पॉज़ न करें

पॉज़ कमांड का उपयोग करना आकर्षक हो सकता है, लेकिन इसका उपयोग करना एक बुरा विचार है क्योंकि यह लचीला नहीं है और लंबे समय में केवल अस्थिर टेस्ट का कारण बनेगा।

```js
// 👎
await nameInput.setValue('Bob')
await browser.pause(200) // wait for submit button to enable
await submitFormButton.click()

// 👍
await nameInput.setValue('Bob')
await submitFormButton.waitForEnabled()
await submitFormButton.click()
```

## एसिंक लूप्स

जब आपके पास कुछ एसिंक्रोनस कोड है जिसे आप दोहराना चाहते हैं, तो यह जानना महत्वपूर्ण है कि सभी लूप्स ऐसा नहीं कर सकते।
उदाहरण के लिए, ऐरे का forEach फंक्शन एसिंक्रोनस कॉलबैक्स की अनुमति नहीं देता है जैसा कि [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) पर पढ़ा जा सकता है।

__नोट:__ आप इनका उपयोग तब भी कर सकते हैं जब आपको ऑपरेशन को सिंक्रोनस होने की आवश्यकता नहीं होती, जैसा कि इस उदाहरण में दिखाया गया है `console.log(await $$('h1').map((h1) => h1.getText()))`।

नीचे कुछ उदाहरण दिए गए हैं कि इसका क्या अर्थ है।

निम्नलिखित काम नहीं करेगा क्योंकि एसिंक्रोनस कॉलबैक समर्थित नहीं हैं।

```js
// 👎
const characters = 'this is some example text that should be put in order'
characters.forEach(async (character) => {
    await browser.keys(character)
})
```

निम्नलिखित काम करेगा।

```js
// 👍
const characters = 'this is some example text that should be put in order'
for (const character of characters) {
    await browser.keys(character)
}
```

## सरल रखें

कभी-कभी हम अपने उपयोगकर्ताओं को टेक्स्ट या वैल्यूज जैसे डेटा को मैप करते हुए देखते हैं। अक्सर इसकी आवश्यकता नहीं होती है और अक्सर यह एक कोड स्मेल होता है, उदाहरणों को नीचे देखें कि ऐसा क्यों है।

```js
// 👎 बहुत जटिल, सिंक्रोनस एसर्शन, अस्थिर टेस्ट को रोकने के लिए अंतर्निहित एसर्शन्स का उपयोग करें
const headerText = ['Products', 'Prices']
const texts = await $$('th').map(e => e.getText());
expect(texts).toBe(headerText)

// 👎 बहुत जटिल
const headerText = ['Products', 'Prices']
const columns = await $$('th');
await expect(columns).toBeElementsArrayOfSize(2);
for (let i = 0; i < columns.length; i++) {
    await expect(columns[i]).toHaveText(headerText[i]);
}

// 👎 एलिमेंट्स को उनके टेक्स्ट द्वारा खोजता है लेकिन एलिमेंट्स की स्थिति को ध्यान में नहीं रखता
await expect($('th=Products')).toExist();
await expect($('th=Prices')).toExist();
```

```js
// 👍 अद्वितीय पहचानकर्ताओं का उपयोग करें (अक्सर कस्टम एलिमेंट्स के लिए उपयोग किया जाता है)
await expect($('[data-testid="Products"]')).toHaveText('Products');
// 👍 एक्सेसिबिलिटी नाम (अक्सर नेटिव एचटीएमएल एलिमेंट्स के लिए उपयोग किया जाता है)
await expect($('aria/Product Prices')).toHaveText('Prices');
```

एक और चीज जो हम कभी-कभी देखते हैं वह यह है कि सरल चीजों का एक जटिल समाधान होता है।

```js
// 👎
class BadExample {
    public async selectOptionByValue(value: string) {
        await $('select').click();
        await $$('option')
            .map(async function (element) {
                const hasValue = (await element.getValue()) === value;
                if (hasValue) {
                    await $(element).click();
                }
                return hasValue;
            });
    }

    public async selectOptionByText(text: string) {
        await $('select').click();
        await $$('option')
            .map(async function (element) {
                const hasText = (await element.getText()) === text;
                if (hasText) {
                    await $(element).click();
                }
                return hasText;
            });
    }
}
```

```js
// 👍
class BetterExample {
    public async selectOptionByValue(value: string) {
        await $('select').click();
        await $(`option[value=${value}]`).click();
    }

    public async selectOptionByText(text: string) {
        await $('select').click();
        await $(`option=${text}]`).click();
    }
}
```

## समानांतर में कोड निष्पादित करना

यदि आप इस बात की परवाह नहीं करते कि कुछ कोड किस क्रम में चलाया जाता है, तो आप निष्पादन को गति देने के लिए [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) का उपयोग कर सकते हैं।

__नोट:__ चूंकि यह कोड को पढ़ने में कठिन बनाता है, आप इसे पेज ऑब्जेक्ट या फंक्शन का उपयोग करके अमूर्त कर सकते हैं, हालांकि आपको यह भी सवाल करना चाहिए कि क्या प्रदर्शन में लाभ पठनीयता की कीमत के लायक है।

```js
// 👎
await name.setValue('Bob')
await email.setValue('bob@webdriver.io')
await age.setValue('50')
await submitFormButton.waitForEnabled()
await submitFormButton.click()

// 👍
await Promise.all([
    name.setValue('Bob'),
    email.setValue('bob@webdriver.io'),
    age.setValue('50'),
])
await submitFormButton.waitForEnabled()
await submitFormButton.click()
```

यदि अमूर्त किया गया है तो यह कुछ इस तरह दिख सकता है जहां तर्क को submitWithDataOf नामक विधि में रखा गया है और डेटा को Person क्लास द्वारा प्राप्त किया जाता है।

```js
// 👍
await form.submitData(new Person('bob@webdriver.io'))
```