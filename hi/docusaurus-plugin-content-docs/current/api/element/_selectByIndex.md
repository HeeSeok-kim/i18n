---
id: selectByIndex
title: selectByIndex
custom_edit_url: https://github.com/webdriverio/webdriverio/edit/main/packages/webdriverio/src/commands/element/selectByIndex.ts
---

विशेष अनुक्रमणिका (इंडेक्स) वाले विकल्प का चयन करें।

##### उपयोग

```js
$(selector).selectByIndex(index)
```

##### पैरामीटर्स

<table>
  <thead>
    <tr>
      <th>नाम</th><th>प्रकार</th><th>विवरण</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>index</var></code></td>
      <td>`number`</td>
      <td>विकल्प अनुक्रमणिका</td>
    </tr>
  </tbody>
</table>

##### उदाहरण

```html title="example.html"
<select id="selectbox">
    <option value="someValue0">uno</option>
    <option value="someValue1">dos</option>
    <option value="someValue2">tres</option>
    <option value="someValue3">cuatro</option>
    <option value="someValue4">cinco</option>
    <option value="someValue5">seis</option>
</select>
```

```js title="selectByIndex.js"
it('Should demonstrate the selectByIndex command', async () => {
    const selectBox = await $('#selectbox');
    console.log(await selectBox.getValue()); // returns "someValue0"
    await selectBox.selectByIndex(4);
    console.log(await selectBox.getValue()); // returns "someValue4"
});
```