---
id: savePDF
title: savePDF
custom_edit_url: https://github.com/webdriverio/webdriverio/edit/main/packages/webdriverio/src/commands/browser/savePDF.ts
---

現在のブラウジングコンテキストのページをPDFファイルとしてOSに保存します。

##### Usage

```js
browser.savePDF(filepath, { orientation, scale, background, width, height, top, bottom, left, right, shrinkToFit, pageRanges })
```

##### Parameters

<table>
  <thead>
    <tr>
      <th>Name</th><th>Type</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>filepath</var></code></td>
      <td>`String`</td>
      <td>生成されるPDFへのパス（`.pdf`サフィックスが必要）実行ディレクトリからの相対パス</td>
    </tr>
    <tr>
      <td><code><var>options</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>`PDFPrintOptions`</td>
      <td>PDF印刷オプション</td>
    </tr>
    <tr>
      <td><code><var>options.orientation</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>`String`</td>
      <td>PDFページの向き</td>
    </tr>
    <tr>
      <td><code><var>options.scale</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>`number`</td>
      <td>PDFページの拡大縮小</td>
    </tr>
    <tr>
      <td><code><var>options.background</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>`boolean`</td>
      <td>PDFページの背景を含める</td>
    </tr>
    <tr>
      <td><code><var>options.width</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>`number`</td>
      <td>PDFページの幅</td>
    </tr>
    <tr>
      <td><code><var>options.height</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>`number`</td>
      <td>PDFページの高さ</td>
    </tr>
    <tr>
      <td><code><var>options.top</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>`number`</td>
      <td>PDFページの上部パディング</td>
    </tr>
    <tr>
      <td><code><var>options.bottom</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>`number`</td>
      <td>PDFページの下部パディング</td>
    </tr>
    <tr>
      <td><code><var>options.left</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>`number`</td>
      <td>PDFページの左側パディング</td>
    </tr>
    <tr>
      <td><code><var>options.right</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>`number`</td>
      <td>PDFページの右側パディング</td>
    </tr>
    <tr>
      <td><code><var>options.shrinkToFit</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>`boolean`</td>
      <td>ページに合わせて縮小する</td>
    </tr>
    <tr>
      <td><code><var>options.pageRanges</var></code></td>
      <td>`object[]`</td>
      <td>PDFに含めるページ範囲</td>
    </tr>
  </tbody>
</table>

##### Example

```js title="savePDF.js"
it('should save a PDF screenshot of the browser view', function () {
    await browser.savePDF('./some/path/screenshot.pdf');
});
```

##### Returns

- **&lt;Buffer&gt;**
            **<code><var>return</var></code>:**    screenshot buffer    