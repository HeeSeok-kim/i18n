---
id: getPuppeteer
title: getPuppeteer
custom_edit_url: https://github.com/webdriverio/webdriverio/edit/main/packages/webdriverio/src/commands/browser/getPuppeteer.ts
---

[Puppeteer Browserインスタンス](https://pptr.dev/#?product=Puppeteer&version=v5.1.0&show=api-class-browser)を取得して、
Puppeteerでコマンドを実行します。すべてのPuppeteerコマンドはデフォルトで
非同期であるため、同期と非同期実行を切り替えるには、例のように
Puppeteerの呼び出しを`browser.call`コマンド内にラップしてください。

:::info

Puppeteerの使用にはChrome DevToolsプロトコルのサポートが必要であり、
クラウドで自動化テストを実行する場合などには使用できません。Chrome DevToolsプロトコルはデフォルトではインストールされていないため、
`npm install puppeteer-core`を使用してインストールしてください。
詳細は[自動化プロトコル](/docs/automationProtocols)セクションを参照してください。

:::

:::info

注意: Puppeteerは現在、[コンポーネントテスト](/docs/component-testing)実行時には__サポートされていません__。

:::

##### 使用方法

```js
browser.getPuppeteer()
```

##### 例

```js title="getPuppeteer.test.js"
it('should allow me to use Puppeteer', async () => {
    // WebDriver command
    await browser.url('https://webdriver.io')

    const puppeteerBrowser = await browser.getPuppeteer()
    // switch to Puppeteer
    const metrics = await browser.call(async () => {
        const pages = await puppeteerBrowser.pages()
        pages[0].setGeolocation({ latitude: 59.95, longitude: 30.31667 })
        return pages[0].metrics()
    })

    console.log(metrics.LayoutCount) // returns LayoutCount value
})
```

##### 戻り値

- **&lt;PuppeteerBrowser&gt;**
            **<code><var>return</var></code>:**   ブラウザに接続された初期化されたpuppeteerインスタンス