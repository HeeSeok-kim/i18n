---
id: getComputedRole
title: getComputedRole
custom_edit_url: https://github.com/webdriverio/webdriverio/edit/main/packages/webdriverio/src/commands/element/getComputedRole.ts
---

Obter o rótulo WAI-ARIA computado de um elemento.

##### Uso

```js
$(selector).getComputedRole()
```

##### Exemplo

```js title="getComputedRole.js"
it('should demonstrate the getComputedRole command', async () => {
    await browser.url('https://www.google.com/ncr')
    const elem = await $('*[name="q"]');
    console.log(await elem.getComputedRole()); // outputs: "combobox"
})
```

##### Retorna

- **&lt;String&gt;**
            **<code><var>return</var></code>:**  o rótulo WAI-ARIA computado