---
id: pageobjects
title: Page Object-mönstret
---

Version 5 av WebdriverIO designades med stöd för Page Object-mönstret i åtanke. Genom att introducera principen "element som förstaklassobjekt", är det nu möjligt att bygga stora testsviter med detta mönster.

Inga ytterligare paket krävs för att skapa sidoobjekt. Det visar sig att rena, moderna klasser ger alla nödvändiga funktioner vi behöver:

- arv mellan sidoobjekt
- lat laddning av element
- inkapsling av metoder och åtgärder

Målet med att använda sidoobjekt är att abstrahera all sidinformation från de faktiska testerna. Idealiskt bör du lagra alla selektorer eller specifika instruktioner som är unika för en viss sida i ett sidoobjekt, så att du fortfarande kan köra ditt test efter att du har helt omdesignat din sida.

## Skapa ett sidoobjekt

Först behöver vi ett huvud-sidoobjekt som vi kallar `Page.js`. Det kommer att innehålla allmänna selektorer eller metoder som alla sidoobjekt kommer att ärva från.

```js
// Page.js
export default class Page {
    constructor() {
        this.title = 'My Page'
    }

    async open (path) {
        await browser.url(path)
    }
}
```

Vi kommer alltid att `export`-era en instans av ett sidoobjekt, och aldrig skapa den instansen i testet. Eftersom vi skriver end-to-end-tester, betraktar vi alltid sidan som en tillståndslös konstruktion&mdash;precis som varje HTTP-förfrågan är en tillståndslös konstruktion.

Visst kan webbläsaren bära sessionsinformation och därför visa olika sidor baserat på olika sessioner, men detta bör inte återspeglas inom ett sidoobjekt. Dessa typer av tillståndsändringar bör finnas i dina faktiska tester.

Låt oss börja testa den första sidan. För demonstrationsändamål använder vi webbplatsen [The Internet](http://the-internet.herokuapp.com) av [Elemental Selenium](http://elementalselenium.com) som försökskanin. Låt oss försöka bygga ett exempel på sidoobjekt för [inloggningssidan](http://the-internet.herokuapp.com/login).

## `Get` -ting av dina selektorer

Det första steget är att skriva alla viktiga selektorer som krävs i vårt `login.page`-objekt som getter-funktioner:

```js
// login.page.js
import Page from './page'

class LoginPage extends Page {

    get username () { return $('#username') }
    get password () { return $('#password') }
    get submitBtn () { return $('form button[type="submit"]') }
    get flash () { return $('#flash') }
    get headerLinks () { return $$('#header a') }

    async open () {
        await super.open('login')
    }

    async submit () {
        await this.submitBtn.click()
    }

}

export default new LoginPage()
```

Att definiera selektorer i getter-funktioner kan se lite konstigt ut, men det är verkligen användbart. Dessa funktioner utvärderas _när du använder egenskapen_, inte när du genererar objektet. Med det begär du alltid elementet innan du utför en åtgärd på det.

## Kedja kommandon

WebdriverIO kommer ihåg det senaste resultatet av ett kommando internt. Om du kedjar ett elementkommando med ett åtgärdskommando, hittar det elementet från föregående kommando och använder resultatet för att utföra åtgärden. Med det kan du ta bort selektorn (första parametern) och kommandot ser så enkelt ut som:

```js
await LoginPage.username.setValue('Max Mustermann')
```

Vilket i princip är samma sak som:

```js
let elem = await $('#username')
await elem.setValue('Max Mustermann')
```

eller

```js
await $('#username').setValue('Max Mustermann')
```

## Använda sidoobjekt i dina tester

Efter att du har definierat de nödvändiga elementen och metoderna för sidan, kan du börja skriva test för den. Allt du behöver göra för att använda sidoobjektet är att `import`-era (eller `require`) det. Det är allt!

Eftersom du exporterade en redan skapad instans av sidoobjektet, kan du börja använda det direkt efter import.

Om du använder ett påståenderamverk kan dina tester vara ännu mer uttrycksfulla:

```js
// login.spec.js
import LoginPage from '../pageobjects/login.page'

describe('login form', () => {
    it('should deny access with wrong creds', async () => {
        await LoginPage.open()
        await LoginPage.username.setValue('foo')
        await LoginPage.password.setValue('bar')
        await LoginPage.submit()

        await expect(LoginPage.flash).toHaveText('Your username is invalid!')
    })

    it('should allow access with correct creds', async () => {
        await LoginPage.open()
        await LoginPage.username.setValue('tomsmith')
        await LoginPage.password.setValue('SuperSecretPassword!')
        await LoginPage.submit()

        await expect(LoginPage.flash).toHaveText('You logged into a secure area!')
    })
})
```

Från den strukturella sidan är det vettigt att separera specifikationsfiler och sidoobjekt i olika kataloger. Dessutom kan du ge varje sidoobjekt ändelsen: `.page.js`. Detta gör det tydligare att du importerar ett sidoobjekt.

## Gå vidare

Detta är den grundläggande principen för hur man skriver sidoobjekt med WebdriverIO. Men du kan bygga upp mycket mer komplexa sidoobjektstrukturer än detta! Till exempel kan du ha specifika sidoobjekt för modaler, eller dela upp ett stort sidoobjekt i olika klasser (var och en representerar en annan del av den övergripande webbsidan) som ärver från huvudsidoobjektet. Mönstret ger verkligen många möjligheter att separera sidinformation från dina tester, vilket är viktigt för att hålla din testsvit strukturerad och tydlig i tider då projektet och antalet tester växer.

Du kan hitta detta exempel (och ännu fler sidoobjekt-exempel) i [`example`-mappen](https://github.com/webdriverio/webdriverio/tree/main/examples/pageobject) på GitHub.