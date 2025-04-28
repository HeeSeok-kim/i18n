---
id: vscode-extensions
title: VS Code 拡張機能のテスト
---

WebdriverIOを使用すると、[VS Code](https://code.visualstudio.com/)拡張機能をVS Codeデスクトップ IDEまたはWeb拡張機能としてシームレスにエンドツーエンドでテストできます。拡張機能へのパスを提供するだけで、フレームワークが残りの処理を行います。[`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service)によって、すべてが処理されるだけでなく、さらに多くの機能があります：

- 🏗️ VSCodeのインストール（安定版、インサイダー版、または指定したバージョン）
- ⬇️ 特定のVSCodeバージョンに対応するChromedriverのダウンロード
- 🚀 テストからVSCode APIにアクセスする機能
- 🖥️ カスタムユーザー設定でのVSCodeの起動（Ubuntu、MacOS、WindowsのVSCodeをサポート）
- 🌐 またはWeb拡張機能のテスト用にVSCodeをサーバーから提供
- 📔 VSCodeバージョンに合わせたロケーターを持つページオブジェクトのブートストラップ

## 始めるには

新しいWebdriverIOプロジェクトを開始するには、次のコマンドを実行します：

```sh
npm create wdio@latest ./
```

インストールウィザードが手順をガイドします。どのようなテストを行いたいかを尋ねられたら、必ず_「VS Code拡張機能のテスト」_を選択し、その後はデフォルトのままにするか、好みに合わせて変更してください。

## 設定例

このサービスを使用するには、サービスのリストに`vscode`を追加し、必要に応じて設定オブジェクトを続けます。これによりWebdriverIOは指定されたVSCodeバイナリと適切なChromedriverバージョンをダウンロードします：

```js
// wdio.conf.ts
export const config = {
    outputDir: 'trace',
    // ...
    capabilities: [{
        browserName: 'vscode',
        browserVersion: '1.71.0', // 最新のVSCodeバージョンには "insiders" または "stable"
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

`browserName`が`vscode`以外（例えば`chrome`）で`wdio:vscodeOptions`を定義すると、サービスは拡張機能をWeb拡張機能として提供します。Chromeでテストする場合、追加のドライバーサービスは必要ありません：

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

_注意：_ Web拡張機能をテストする場合、`browserVersion`として`stable`または`insiders`のみを選択できます。

### TypeScriptの設定

`tsconfig.json`で、`wdio-vscode-service`を型のリストに追加してください：

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

そこから適切なページオブジェクトメソッドを使用して、すべてのページオブジェクトにアクセスできます。利用可能なすべてのページオブジェクトとそのメソッドについては、[ページオブジェクトのドキュメント](https://webdriverio-community.github.io/wdio-vscode-service/)で詳細を確認してください。

### VSCode APIへのアクセス

[VSCode API](https://code.visualstudio.com/api/references/vscode-api)を通じて特定の自動化を実行したい場合は、カスタム`executeWorkbench`コマンドを使用してリモートコマンドを実行できます。このコマンドを使用すると、テストからVSCode環境内でコードをリモート実行し、VSCode APIにアクセスできるようになります。任意のパラメータを関数に渡すことができ、それらは関数内に伝播されます。`vscode`オブジェクトは常に最初の引数として渡され、その後に外部関数のパラメータが続きます。コールバックはリモートで実行されるため、関数スコープ外の変数にはアクセスできないことに注意してください。例：

```ts
const workbench = await browser.getWorkbench()
await browser.executeWorkbench((vscode, param1, param2) => {
    vscode.window.showInformationMessage(`I am an ${param1} ${param2}!`)
}, 'API', 'call')

const notifs = await workbench.getNotifications()
console.log(await notifs[0].getMessage()) // 出力: "I am an API call!"
```

ページオブジェクトの完全なドキュメントについては、[ドキュメント](https://webdriverio-community.github.io/wdio-vscode-service/modules.html)を確認してください。このプロジェクトの[テストスイート](https://github.com/webdriverio-community/wdio-vscode-service/blob/main/test/specs)にさまざまな使用例があります。

## 詳細情報

[`wdio-vscode-service`](https://www.npmjs.com/package/wdio-vscode-service)の設定方法やカスタムページオブジェクトの作成方法については、[サービスドキュメント](/docs/wdio-vscode-service)で詳細を確認できます。また、[Christian Bromann](https://twitter.com/bromann)による[_ウェブ標準の力を使った複雑なVSCode拡張機能のテスト_](https://www.youtube.com/watch?v=PhGNTioBUiU)に関する以下の講演も視聴できます：

<LiteYouTubeEmbed
    id="PhGNTioBUiU"
    title="Testing Complex VSCode Extensions With the Power of Web Standards"
/>