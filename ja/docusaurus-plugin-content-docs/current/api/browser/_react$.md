---
id: react$
title: react$
custom_edit_url: https://github.com/webdriverio/webdriverio/edit/main/packages/webdriverio/src/commands/browser/react$.ts
---

`react$`コマンドは、実際の名前でReactコンポーネントを照会し、propsとstateでフィルタリングするための便利なコマンドです。

:::info

このコマンドはReact v16.xを使用しているアプリケーションでのみ機能します。Reactセレクターの詳細については、[セレクター](/docs/selectors#react-selectors)ガイドをお読みください。

:::

##### 使用法

```js
browser.react$(selector, { props, state })
```

##### パラメータ

<table>
  <thead>
    <tr>
      <th>名前</th><th>型</th><th>詳細</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>selector</var></code></td>
      <td>`string`</td>
      <td>Reactコンポーネントの</td>
    </tr>
    <tr>
      <td><code><var>options</var></code><br /><span className="label labelWarning">オプション</span></td>
      <td>`ReactSelectorOptions`</td>
      <td>Reactセレクターオプション</td>
    </tr>
    <tr>
      <td><code><var>options.props</var></code><br /><span className="label labelWarning">オプション</span></td>
      <td>`Object`</td>
      <td>要素が含むべきReact props</td>
    </tr>
    <tr>
      <td><code><var>options.state</var></code><br /><span className="label labelWarning">オプション</span></td>
      <td>`Array<any>, number, string, object, boolean`</td>
      <td>要素があるべきReact state</td>
    </tr>
  </tbody>
</table>

##### 例

```js title="pause.js"
it('should calculate 7 * 6', async () => {
    await browser.url('https://ahfarmer.github.io/calculator/');
    const appWrapper = await $('div#root')

    await browser.react$('t', {
        props: { name: '7' }
    }).click()
    await browser.react$('t', {
        props: { name: 'x' }
    }).click()
    await browser.react$('t', {
        props: { name: '6' }
    }).click()
    await browser.react$('t', {
        props: { name: '=' }
    }).click()

    console.log(await $('.component-display').getText()); // prints "42"
});
```

##### 戻り値

- **&lt;WebdriverIO.Element&gt;**