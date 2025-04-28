---
id: vscode-extensions
title: VS Code 拡張機能のテスト
---

WebdriverIOを使用すると、[VS Code](https://code.visualstudio.com/)拡張機能をVS CodeデスクトップIDEまたはWeb拡張機能としてシームレスにエンドツーエンドでテストできます。あなたの拡張機能へのパスを提供するだけで、フレームワークが残りの作業を行います。[`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service)によって、すべてが処理され、さらに多くの機能が提供されます：

- 🏗️ VSCodeのインストール（安定版、インサイダー版、または指定したバージョン）
- ⬇️ 特定のVSCodeバージョンに対応するChromedriverのダウンロード
- 🚀 テストからVSCode APIにアクセスできるようにする
- 🖥️ カスタムユーザー設定でVSCodeを起動（Ubuntu、MacOS、WindowsのVSCodeもサポート）
- 🌐 またはWeb拡張機能のテスト用にブラウザからアクセスできるようにVSCodeをサーバーから提供
- 📔 VSCodeバージョンに一致するロケーターを持つページオブジェクトのブートストラップ

## はじめに

新しいWebdriverIOプロジェクトを開始するには、次のコマンドを実行します：

```sh
npm create wdio@latest ./
```

インストールウィザードが処理をガイドします。どのようなテストをしたいかと聞かれたら、必ず_「VS Code Extension Testing」_を選択し、その後はデフォルトのままにするか、必要に応じて変更してください。

## 設定例

このサービスを使用するには、サービスのリストに`vscode`を追加し、必要に応じて設定オブジェクトを続けて記述します。これにより、WebdriverIOは指定されたVSCodeバイナリと適切なChromedriverバージョンをダウンロードします：

```js
// wdio.conf.ts
export const config = {
    outputDir: 'trace',
    // ...
    capabilities: [{
        browserName: 'vscode',
        browserVersion: '1.71.0', // 最新のVSCodeバージョンには "insiders" または "stable" を使用
        'wdio:vscodeOptions': {
            extensionPath: __dirname,
            userSettings: {
                "editor.fontSize": 14
            }
        }
    }],
    services: ['vscode'],
    /**
     * オプションでWebdriverIOがすべてのVSCodeとChromedriverバイナリを
     * 保存するパスを定義できます。例：
     * services: [['vscode', { cachePath: __dirname }]]
     */
    // ...
};
```

`vscode`以外の`browserName`で`wdio:vscodeOptions`を定義すると（例えば`chrome`）、サービスは拡張機能をWeb拡張機能として提供します。Chromeでテストする場合、追加のドライバーサービスは不要です：

```js
// wdio.conf.ts
export const config = {
    outputDir: 'trace',
    // ...
    capabilities: [{
        browserName: 'chrome',
        'wdio:vscodeOptions': {
            extensionPath: __dirname
        }
    }],
    services: ['vscode'],
    // ...
};
```

_注意：_ Web拡張機能をテストする場合、`browserVersion`として選択できるのは`stable`または`insiders`のみです。

### TypeScriptのセットアップ

`tsconfig.json`で`wdio-vscode-service`を型のリストに追加してください：

```json
{
    "compilerOptions": {
        "types": [
            "node",
            "webdriverio/async",
            "@wdio/mocha-framework",
            "expect-webdriverio",
            "wdio-vscode-service"
        ],
        "target": "es2020",
        "moduleResolution": "node16"
    }
}
```

## 使用方法

`getWorkbench`メソッドを使用して、希望するVSCodeバージョンに一致するロケーターのページオブジェクトにアクセスできます：

```ts
describe('WDIO VSCode Service', () => {
    it('should be able to load VSCode', async () => {
        const workbench = await browser.getWorkbench()
        expect(await workbench.getTitleBar().getTitle())
            .toBe('[Extension Development Host] - README.md - wdio-vscode-service - Visual Studio Code')
    })
})
```

そこから、適切なページオブジェクトメソッドを使用してすべてのページオブジェクトにアクセスできます。利用可能なすべてのページオブジェクトとそのメソッドについては、[ページオブジェクトのドキュメント](https://webdriverio-community.github.io/wdio-vscode-service/)で詳細を確認してください。

### VSCode APIへのアクセス

[VSCode API](https://code.visualstudio.com/api/references/vscode-api)を通じて特定の自動化を実行したい場合は、カスタム`executeWorkbench`コマンドを使用してリモートコマンドを実行できます。このコマンドを使用すると、テストからVSCode環境内でコードをリモート実行し、VSCode APIにアクセスできます。任意のパラメータを関数に渡すことができ、それらはその関数に伝播されます。`vscode`オブジェクトは常に最初の引数として渡され、その後に外部関数のパラメータが続きます。コールバックはリモートで実行されるため、関数スコープ外の変数にはアクセスできないことに注意してください。以下は例です：

```ts
const workbench = await browser.getWorkbench()
await browser.executeWorkbench((vscode, param1, param2) => {
    vscode.window.showInformationMessage(`I am an ${param1} ${param2}!`)
}, 'API', 'call')

const notifs = await workbench.getNotifications()
console.log(await notifs[0].getMessage()) // 出力: "I am an API call!"
```

ページオブジェクトの完全なドキュメントについては、[ドキュメント](https://webdriverio-community.github.io/wdio-vscode-service/modules.html)を確認してください。このプロジェクトの[テストスイート](https://github.com/webdriverio-community/wdio-vscode-service/blob/main/test/specs)でさまざまな使用例を見つけることができます。

## 詳細情報

[`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service)の設定方法やカスタムページオブジェクトの作成方法については、[サービスドキュメント](/docs/wdio-vscode-service)で詳細を確認できます。また、[Christian Bromann](https://twitter.com/bromann)による[_Web標準の力を使った複雑なVSCode拡張機能のテスト_](https://www.youtube.com/watch?v=PhGNTioBUiU)に関する次の講演を視聴することもできます：

<LiteYouTubeEmbed
    id="PhGNTioBUiU"
    title="Testing Complex VSCode Extensions With the Power of Web Standards"
/>