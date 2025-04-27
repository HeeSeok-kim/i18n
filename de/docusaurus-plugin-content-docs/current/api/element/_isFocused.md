---
id: isFocused
title: isFocused
custom_edit_url: https://github.com/webdriverio/webdriverio/edit/main/packages/webdriverio/src/commands/element/isFocused.ts
---

Gibt true oder false zurück, wenn das ausgewählte DOM-Element aktuell fokussiert ist. Wenn der Selektor mehrere Elemente auswählt, wird true zurückgegeben, wenn eines der Elemente fokussiert ist.

##### Usage

```js
$(selector).isFocused()
```

##### Examples

```html title="index.html"
<input name="login" autofocus="" />
```

```js title="hasFocus.js"
it('should detect the focus of an element', async () => {
    await browser.url('/');
    const loginInput = await $('[name="login"]');
    console.log(await loginInput.isFocused()); // outputs: false

    await loginInput.click();
    console.log(await loginInput.isFocused()); // outputs: true
})
```

##### Returns

- **&lt;Boolean&gt;**
            **<code><var>return</var></code>:**          true if one of the matching elements has focus