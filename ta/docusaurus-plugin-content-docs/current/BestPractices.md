---
id: bestpractices
title: சிறந்த நடைமுறைகள்
---

# சிறந்த நடைமுறைகள்

இந்த வழிகாட்டி உங்களுக்கு செயல்திறன் மிக்க மற்றும் நெகிழ்வான சோதனைகளை எழுத உதவும் எங்களின் சிறந்த நடைமுறைகளைப் பகிர்வதை நோக்கமாகக் கொண்டுள்ளது.

## நெகிழ்வான தேர்வுக்குறிகளைப் பயன்படுத்துங்கள்

DOM இல் மாற்றங்களுக்கு நெகிழ்வான தேர்வுக்குறிகளைப் பயன்படுத்துவதன் மூலம், உதாரணமாக ஒரு உறுப்பிலிருந்து வகுப்பு நீக்கப்படும்போது குறைவான அல்லது தோல்வியடையும் சோதனைகளைக் கூட நீங்கள் பெறுவீர்கள்.

வகுப்புகள் பல உறுப்புகளுக்குப் பயன்படுத்தப்படலாம், அந்த வகுப்பைக் கொண்ட அனைத்து உறுப்புகளையும் பெற வேண்டும் என்று நீங்கள் வேண்டுமென்றே விரும்பினால் தவிர, முடிந்தவரை தவிர்க்க வேண்டும்.

```js
// 👎
await $('.button')
```

இந்த அனைத்து தேர்வுக்குறிகளும் ஒரு உறுப்பை மட்டுமே திருப்பி அனுப்ப வேண்டும்.

```js
// 👍
await $('aria/Submit')
await $('[test-id="submit-button"]')
await $('#submit-button')
```

__குறிப்பு:__ WebdriverIO ஆதரிக்கும் அனைத்து சாத்தியமான தேர்வுக்குறிகளையும் அறிய, எங்களது [தேர்வுக்குறிகள்](./Selectors.md) பக்கத்தைப் பார்க்கவும்.

## உறுப்பு வினவல்களின் எண்ணிக்கையை கட்டுப்படுத்தவும்

நீங்கள் [`$`](https://webdriver.io/docs/api/browser/$) அல்லது [`$$`](https://webdriver.io/docs/api/browser/$$) கட்டளையைப் பயன்படுத்தும் ஒவ்வொரு முறையும் (இதில் அவற்றை சங்கிலித்தொடராக இணைப்பதும் அடங்கும்), WebdriverIO DOM இல் உறுப்பைக் கண்டறிய முயற்சிக்கிறது. இந்த வினவல்கள் அதிக செலவு கொண்டவை, எனவே நீங்கள் அவற்றை முடிந்தவரை கட்டுப்படுத்த முயற்சிக்க வேண்டும்.

மூன்று உறுப்புகளை வினவுகிறது.

```js
// 👎
await $('table').$('tr').$('td')
```

ஒரே ஒரு உறுப்பை மட்டும் வினவுகிறது.

``` js
// 👍
await $('table tr td')
```

வெவ்வேறு [தேர்வுக்குறி உத்திகளை](https://webdriver.io/docs/selectors/#custom-selector-strategies) இணைக்க விரும்பும்போது மட்டுமே நீங்கள் சங்கிலித்தொடரை பயன்படுத்த வேண்டும்.
இந்த உதாரணத்தில் நாங்கள் [ஆழமான தேர்வுக்குறிகளை](https://webdriver.io/docs/selectors#deep-selectors) பயன்படுத்துகிறோம், இது ஒரு உறுப்பின் shadow DOM உள்ளே செல்லும் உத்தியாகும்.

``` js
// 👍
await $('custom-datepicker').$('#calendar').$('aria/Select')
```

### பட்டியலில் இருந்து ஒன்றை எடுப்பதற்குப் பதிலாக ஒரு உறுப்பைக் கண்டறிவதற்கே முன்னுரிமை கொடுக்கவும்

இதைச் செய்ய எப்போதும் சாத்தியமில்லை, ஆனால் [:nth-child](https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-child) போன்ற CSS போலி-வகுப்புகளைப் பயன்படுத்தி, பெற்றோர்களின் குழந்தைகள் பட்டியலில் உள்ள உறுப்புகளின் குறியீடுகளின் அடிப்படையில் உறுப்புகளைப் பொருத்தலாம்.

அனைத்து அட்டவணை வரிசைகளையும் வினவுகிறது.

```js
// 👎
await $$('table tr')[15]
```

ஒரு அட்டவணை வரிசையை மட்டும் வினவுகிறது.

```js
// 👍
await $('table tr:nth-child(15)')
```

## உள்ளமைக்கப்பட்ட உறுதிப்படுத்தல்களைப் பயன்படுத்தவும்

முடிவுகள் பொருந்துவதற்காக தானாகவே காத்திருக்காத கைமுறை உறுதிப்படுத்தல்களைப் பயன்படுத்த வேண்டாம், இது நிலையற்ற சோதனைகளுக்கு காரணமாகும்.

```js
// 👎
expect(await button.isDisplayed()).toBe(true)
```

உள்ளமைக்கப்பட்ட உறுதிப்படுத்தல்களைப் பயன்படுத்துவதன் மூலம் WebdriverIO உண்மையான முடிவு எதிர்பார்க்கப்பட்ட முடிவுடன் பொருந்தும் வரை தானாகவே காத்திருக்கும், இது நெகிழ்வான சோதனைகளை உருவாக்கும்.
உறுதிப்படுத்தல் வெற்றி பெறும் வரை அல்லது நேரம் முடியும் வரை தானாகவே மீண்டும் முயற்சி செய்வதன் மூலம் இதை சாதிக்கிறது.

```js
// 👍
await expect(button).toBeDisplayed()
```

## சோம்பல் ஏற்றல் மற்றும் வாக்குறுதி சங்கிலி

தூய்மையான குறியீட்டை எழுதுவதில் WebdriverIO சில தந்திரங்களைக் கொண்டுள்ளது, உறுப்பை சோம்பேறியாக ஏற்ற முடியும், இது உங்கள் வாக்குறுதிகளை சங்கிலி போல இணைக்க அனுமதிக்கிறது மற்றும் `await` அளவைக் குறைக்கிறது. இது உறுப்பை Element க்கு பதிலாக ChainablePromiseElement ஆக கடத்த அனுமதிக்கிறது மற்றும் பக்க பொருள்களுடன் எளிதாக பயன்படுத்த உதவுகிறது.

எப்போது `await` ஐப் பயன்படுத்த வேண்டும்?
`$` மற்றும் `$$` கட்டளைகளைத் தவிர நீங்கள் எப்போதும் `await` ஐப் பயன்படுத்த வேண்டும்.

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

## கட்டளைகள் மற்றும் உறுதிப்படுத்தல்களை அதிகமாகப் பயன்படுத்த வேண்டாம்

expect.toBeDisplayed ஐப் பயன்படுத்தும்போது உறுப்பு இருப்பதற்காகவும் மறைமுகமாகக் காத்திருக்கிறீர்கள். ஏற்கனவே அதே செயலைச் செய்யும் உறுதிப்படுத்தல் உள்ளபோது waitForXXX கட்டளைகளைப் பயன்படுத்த தேவையில்லை.

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

ஒரு உறுப்பு வெளிப்படையாக தெரியாமல் இருக்கும் (opacity: 0 போன்றவை) அல்லது வெளிப்படையாக முடக்கப்பட்டுள்ள (disabled attribute போன்றவை) சூழ்நிலைகளைத் தவிர, உறுப்புடன் தொடர்புகொள்ளும்போது அல்லது அதன் உரை போன்றவற்றை உறுதிப்படுத்தும்போது, அந்த உறுப்பு இருப்பதற்காக அல்லது காட்டப்படுவதற்காக காத்திருக்க வேண்டிய அவசியமில்லை.

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

## டைனமிக் சோதனைகள்

இரகசிய அடையாளச்சொற்கள் போன்ற மாறும் சோதனை தரவுகளை சேமிக்க சுற்றுச்சூழல் மாறிகளைப் பயன்படுத்தவும், சோதனையில் நேரடியாகக் குறியீடு செய்வதைத் தவிர்க்கவும். இந்த தலைப்பில் மேலும் தகவலுக்கு [அளவுருக்களை சோதனைகள்](parameterize-tests) பக்கத்திற்குச் செல்லவும்.

## உங்கள் குறியீட்டை லின்ட் செய்யுங்கள்

உங்கள் குறியீட்டை லின்ட் செய்ய eslint ஐப் பயன்படுத்துவதன் மூலம் பிழைகளை முன்கூட்டியே கண்டறியலாம், சில சிறந்த நடைமுறைகள் எப்போதும் பயன்படுத்தப்படுவதை உறுதிசெய்ய எங்கள் [லிண்டிங் விதிகளைப்](https://www.npmjs.com/package/eslint-plugin-wdio) பயன்படுத்தவும்.

## இடைநிறுத்த வேண்டாம்

pause கட்டளையைப் பயன்படுத்த ஆசைப்படலாம், ஆனால் இதைப் பயன்படுத்துவது சரியான யோசனை அல்ல, ஏனெனில் இது நெகிழ்வானது அல்ல மற்றும் நீண்ட காலத்தில் நிலையற்ற சோதனைகளை ஏற்படுத்தும்.

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

## ஒத்திசைவற்ற வளையங்கள்

நீங்கள் திரும்பச் செய்ய விரும்பும் சில ஒத்திசைவற்ற குறியீடு இருக்கும்போது, அனைத்து வளையங்களும் இதை செய்ய முடியாது என்பதை அறிவது முக்கியம்.
உதாரணமாக, Array இன் forEach செயல்பாடு [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) இல் படிக்கக்கூடியது போல ஒத்திசைவற்ற கால்பேக்களை அனுமதிக்காது.

__குறிப்பு:__ இந்த உதாரணத்தில் காட்டப்பட்டுள்ளது போல் செயல்பாடு ஒத்திசைவாக இருக்க வேண்டியதில்லை என்றால் நீங்கள் இவற்றைப் பயன்படுத்தலாம் `console.log(await $$('h1').map((h1) => h1.getText()))`.

கீழே இதன் அர்த்தம் என்ன என்பதற்கான சில உதாரணங்கள் உள்ளன.

ஒத்திசைவற்ற கால்பேக்கள் ஆதரிக்கப்படாததால் பின்வரும் வேலை செய்யாது.

```js
// 👎
const characters = 'this is some example text that should be put in order'
characters.forEach(async (character) => {
    await browser.keys(character)
})
```

பின்வருவன வேலை செய்யும்.

```js
// 👍
const characters = 'this is some example text that should be put in order'
for (const character of characters) {
    await browser.keys(character)
}
```

## எளிமையாக வைத்திருங்கள்

சில நேரங்களில் எங்கள் பயனர்கள் உரை அல்லது மதிப்புகள் போன்ற தரவுகளை மேப் செய்வதைப் பார்க்கிறோம். இது பெரும்பாலும் தேவையில்லை மற்றும் பெரும்பாலும் குறியீட்டு வாசனையாக உள்ளது, இது ஏன் இவ்வாறு உள்ளது என்பதற்கான உதாரணங்களைக் கீழே சரிபார்க்கவும்.

```js
// 👎 too complex, synchronous assertion, use the built-in assertions to prevent flaky tests
const headerText = ['Products', 'Prices']
const texts = await $$('th').map(e => e.getText());
expect(texts).toBe(headerText)

// 👎 too complex
const headerText = ['Products', 'Prices']
const columns = await $$('th');
await expect(columns).toBeElementsArrayOfSize(2);
for (let i = 0; i < columns.length; i++) {
    await expect(columns[i]).toHaveText(headerText[i]);
}

// 👎 finds elements by their text but does not take into account the position of the elements
await expect($('th=Products')).toExist();
await expect($('th=Prices')).toExist();
```

```js
// 👍 use unique identifiers (often used for custom elements)
await expect($('[data-testid="Products"]')).toHaveText('Products');
// 👍 accessibility names (often used for native html elements)
await expect($('aria/Product Prices')).toHaveText('Prices');
```

நாங்கள் சில நேரங்களில் பார்க்கும் மற்றொரு விஷயம் எளிய விஷயங்களுக்கு ஒரு சிக்கலான தீர்வு உள்ளது.

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

## குறியீட்டை இணையாக இயக்குதல்

சில குறியீடுகள் இயக்கப்படும் வரிசையைப் பற்றி நீங்கள் கவலைப்படாவிட்டால், செயல்பாட்டை துரிதப்படுத்த [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) ஐப் பயன்படுத்தலாம்.

__குறிப்பு:__ இது குறியீட்டைப் படிப்பதை கடினமாக்குவதால், பக்க பொருள் அல்லது செயல்பாடு மூலம் இதை சுருக்கலாம், இருப்பினும் செயல்திறன் சார்ந்த பலன் படிக்கக்கூடிய தன்மையின் விலைக்கு மதிப்புள்ளதா என்பதையும் நீங்கள் கேள்வி கேட்க வேண்டும்.

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

சுருக்கப்பட்டால், தர்க்கம் submitWithDataOf என்ற முறையில் வைக்கப்பட்டு, தரவு Person வகுப்பால் பெறப்படும் கீழே உள்ளது போல் தெரியலாம்.

```js
// 👍
await form.submitData(new Person('bob@webdriver.io'))
```