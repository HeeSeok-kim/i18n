---
id: lit
title: Lit
---

Litは、高速で軽量なウェブコンポーネントを構築するためのシンプルなライブラリです。WebdriverIOの[シャドウDOMセレクタ](/docs/selectors#deep-selectors)のおかげで、Litウェブコンポーネントのテストは非常に簡単です。シャドウルート内のネストされた要素を1つのコマンドだけで照会できます。

## セットアップ

LitプロジェクトでWebdriverIOをセットアップするには、コンポーネントテストドキュメントの[手順](/docs/component-testing#set-up)に従ってください。Litの場合、ウェブコンポーネントはコンパイラを通す必要がなく、純粋なウェブコンポーネントの拡張であるため、プリセットは必要ありません。

セットアップが完了したら、次のコマンドを実行してテストを開始できます：

```sh
npx wdio run ./wdio.conf.js
```

## テストの作成

次のようなLitコンポーネントがあるとします：

```ts title="./components/Component.ts"
import { LitElement, css, html } from 'lit'
import { customElement, property } from 'lit/decorators.js'

@customElement('simple-greeting')
export class SimpleGreeting extends LitElement {
    @property()
    name?: string = 'World'

    // Render the UI as a function of component state
    render() {
        return html`<p>Hello, ${this.name}!</p>`
    }
}
```

コンポーネントをテストするには、テスト開始前にテストページにコンポーネントをレンダリングし、その後確実にクリーンアップする必要があります：

```ts title="lit.test.js"
import expect from 'expect'
import { waitFor } from '@testing-library/dom'

// import Lit component
import './components/Component.ts'

describe('Lit Component testing', () => {
    let elem: HTMLElement

    beforeEach(() => {
        elem = document.createElement('simple-greeting')
    })

    it('should render component', async () => {
        elem.setAttribute('name', 'WebdriverIO')
        document.body.appendChild(elem)

        await waitFor(() => {
            expect(elem.shadowRoot.textContent).toBe('Hello, WebdriverIO!')
        })
    })

    afterEach(() => {
        elem.remove()
    })
})
```

WebdriverIOコンポーネントテストスイートのLit向け完全な例は、[サンプルリポジトリ](https://github.com/webdriverio/component-testing-examples/tree/main/lit-typescript-vite)で見つけることができます。