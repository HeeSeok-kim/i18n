---
id: bestpractices
title: Bästa praxis
---

# Bästa praxis

Denna guide syftar till att dela våra bästa metoder som hjälper dig att skriva prestandaoptimerade och robusta tester.

## Använd robusta selektorer

Genom att använda selektorer som är motståndskraftiga mot förändringar i DOM:en kommer du att ha färre eller inga tester som misslyckas när till exempel en klass tas bort från ett element.

Klasser kan tillämpas på flera element och bör undvikas om möjligt, såvida du inte medvetet vill hämta alla element med den klassen.

```js
// 👎
await $('.button')
```

Alla dessa selektorer bör returnera ett enda element.

```js
// 👍
await $('aria/Submit')
await $('[test-id="submit-button"]')
await $('#submit-button')
```

__Obs:__ För att ta reda på alla möjliga selektorer som WebdriverIO stöder, kolla in vår [Selectors](./Selectors.md) sida.

## Begränsa mängden elementfrågor

Varje gång du använder kommandot [`$`](https://webdriver.io/docs/api/browser/$) eller [`$$`](https://webdriver.io/docs/api/browser/$$) (detta inkluderar kedjning av dem), försöker WebdriverIO lokalisera elementet i DOM:en. Dessa frågor är kostsamma så du bör försöka begränsa dem så mycket som möjligt.

Frågar efter tre element.

```js
// 👎
await $('table').$('tr').$('td')
```

Frågar endast efter ett element.

``` js
// 👍
await $('table tr td')
```

Den enda gången du bör använda kedjning är när du vill kombinera olika [selektorstrategier](https://webdriver.io/docs/selectors/#custom-selector-strategies).
I exemplet använder vi [Deep Selectors](https://webdriver.io/docs/selectors#deep-selectors), vilket är en strategi för att gå in i shadow DOM för ett element.

``` js
// 👍
await $('custom-datepicker').$('#calendar').$('aria/Select')
```

### Föredra att lokalisera ett enda element istället för att ta ett från en lista

Det är inte alltid möjligt att göra detta, men med hjälp av CSS-pseudoklasser som [:nth-child](https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-child) kan du matcha element baserat på indexet för elementen i barnlistan hos deras föräldrar.

Frågar efter alla tabellrader.

```js
// 👎
await $$('table tr')[15]
```

Frågar efter en enda tabellrad.

```js
// 👍
await $('table tr:nth-child(15)')
```

## Använd de inbyggda bekräftelserna

Använd inte manuella bekräftelser som inte automatiskt väntar på att resultaten ska matcha, eftersom detta kommer att orsaka instabila tester.

```js
// 👎
expect(await button.isDisplayed()).toBe(true)
```

Genom att använda de inbyggda bekräftelserna kommer WebdriverIO automatiskt att vänta på att det faktiska resultatet ska matcha det förväntade resultatet, vilket leder till robusta tester.
Det uppnår detta genom att automatiskt försöka bekräftelsen igen tills den går igenom eller når tidsgränsen.

```js
// 👍
await expect(button).toBeDisplayed()
```

## Lat laddning och löfteskedjning

WebdriverIO har några tricks när det gäller att skriva ren kod eftersom det kan ladda elementet på ett "lat" sätt, vilket gör att du kan kedja dina löften och minskar mängden `await`. Detta gör det också möjligt att skicka elementet som ett ChainablePromiseElement istället för ett Element och för enklare användning med sidobjekt.

Så när måste du använda `await`?
Du bör alltid använda `await` med undantag för kommandona `$` och `$$`.

```js
// 👎
const div = await $('div')
const button = await div.$('button')
await button.click()
// eller
await (await (await $('div')).$('button')).click()
```

```js
// 👍
const button = $('div').$('button')
await button.click()
// eller
await $('div').$('button').click()
```

## Överanvänd inte kommandon och bekräftelser

När du använder expect.toBeDisplayed väntar du implicit även på att elementet ska existera. Det finns inget behov av att använda waitForXXX-kommandon när du redan har en bekräftelse som gör samma sak.

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

Inget behov av att vänta på att ett element ska existera eller visas vid interaktion eller när man bekräftar något som dess text, såvida elementet inte uttryckligen kan vara osynligt (opacity: 0 till exempel) eller uttryckligen kan vara inaktiverat (disabled-attribut till exempel), i vilket fall det är rimligt att vänta på att elementet ska visas.

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

## Dynamiska tester

Använd miljövariabler för att lagra dynamiska testdata, t.ex. hemliga uppgifter, i din miljö istället för att hårdkoda dem i testet. Gå över till sidan [Parameterize Tests](parameterize-tests) för mer information om detta ämne.

## Linta din kod

Genom att använda eslint för att linta din kod kan du potentiellt upptäcka fel tidigt. Använd våra [lintningsregler](https://www.npmjs.com/package/eslint-plugin-wdio) för att säkerställa att några av de bästa metoderna alltid tillämpas.

## Använd inte pause

Det kan vara frestande att använda pause-kommandot, men att använda detta är en dålig idé eftersom det inte är robust och endast kommer att orsaka instabila tester i det långa loppet.

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

## Asynkrona loopar

När du har asynkron kod som du vill upprepa är det viktigt att veta att inte alla loopar kan göra detta.
Till exempel tillåter inte Array's forEach-funktion asynkrona callbacks, vilket kan läsas på [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach).

__Obs:__ Du kan fortfarande använda dessa när du inte behöver att operationen ska vara synkron, som visas i detta exempel `console.log(await $$('h1').map((h1) => h1.getText()))`.

Nedan finns några exempel på vad detta innebär.

Följande kommer inte att fungera eftersom asynkrona callbacks inte stöds.

```js
// 👎
const characters = 'this is some example text that should be put in order'
characters.forEach(async (character) => {
    await browser.keys(character)
})
```

Följande kommer att fungera.

```js
// 👍
const characters = 'this is some example text that should be put in order'
for (const character of characters) {
    await browser.keys(character)
}
```

## Håll det enkelt

Ibland ser vi att våra användare mappar data som text eller värden. Detta behövs ofta inte och är ofta ett tecken på dålig kod, kolla exemplen nedan varför det är fallet.

```js
// 👎 för komplext, synkron bekräftelse, använd de inbyggda bekräftelserna för att förhindra instabila tester
const headerText = ['Products', 'Prices']
const texts = await $$('th').map(e => e.getText());
expect(texts).toBe(headerText)

// 👎 för komplext
const headerText = ['Products', 'Prices']
const columns = await $$('th');
await expect(columns).toBeElementsArrayOfSize(2);
for (let i = 0; i < columns.length; i++) {
    await expect(columns[i]).toHaveText(headerText[i]);
}

// 👎 hittar element genom deras text men tar inte hänsyn till elementens position
await expect($('th=Products')).toExist();
await expect($('th=Prices')).toExist();
```

```js
// 👍 använd unika identifierare (används ofta för anpassade element)
await expect($('[data-testid="Products"]')).toHaveText('Products');
// 👍 tillgänglighetsnamn (används ofta för nativa html-element)
await expect($('aria/Product Prices')).toHaveText('Prices');
```

En annan sak vi ibland ser är att enkla saker har en överkomplicerad lösning.

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

## Exekvera kod parallellt

Om du inte bryr dig om i vilken ordning viss kod körs kan du använda [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) för att snabba upp exekveringen.

__Obs:__ Eftersom detta gör koden svårare att läsa kan du abstrahera bort detta med hjälp av ett sidobjekt eller en funktion, även om du också bör fråga dig om fördelen med prestanda är värd kostnaden för läsbarhet.

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

Om det är abstraherat kan det se ut ungefär som nedan där logiken läggs i en metod som kallas submitWithDataOf och data hämtas av Person-klassen.

```js
// 👍
await form.submitData(new Person('bob@webdriver.io'))
```