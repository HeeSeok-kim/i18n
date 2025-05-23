---
id: shadow$$
title: shadow$$
custom_edit_url: https://github.com/webdriverio/webdriverio/edit/main/packages/webdriverio/src/commands/element/shadow$$.ts
---

Åtkomst till element inuti en given elements shadowRoot. Om du arbetar
med många nästlade shadow roots är ett alternativt tillvägagångssätt till `shadow$$`
att använda [deep selector](https://webdriver.io/docs/selectors#deep-selectors).

:::info

WebdriverIO genomtränger automatiskt shadow roots när du använder `$` eller `$$` kommandon.
Detta kommando behövs endast om du automatiserar inom en miljö som inte
stöder WebDriver Bidi ännu, t.ex. mobil webbtestning med Appium.

:::

##### Användning

```js
$(selector).shadow$$(selector)
```

##### Parametrar

<table>
  <thead>
    <tr>
      <th>Namn</th><th>Typ</th><th>Detaljer</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>selector</var></code></td>
      <td>`String, Function`</td>
      <td>selector eller JS-funktion för att hämta ett visst element</td>
    </tr>
  </tbody>
</table>

##### Exempel

```js title="shadow$$.js"
it('should return elements inside a shadowRoot', async () => {
    const innerEl = await $('.input').shadow$$('#innerEl');
    console.log(await innerEl.getValue()); // outputs: 'test123'
});
```

##### Returnerar

- **&lt;WebdriverIO.ElementArray&gt;**