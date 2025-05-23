---
id: pause
title: pause（一時停止）
custom_edit_url: https://github.com/webdriverio/webdriverio/edit/main/packages/webdriverio/src/commands/browser/pause.ts
---

指定した時間だけ実行を一時停止します。この命令を要素の表示を待つために使用することはお勧めしません。不安定なテスト結果を避けるために、[`waitForExist`](/docs/api/element/waitForExist)や他のwaitFor*コマンドのような命令を使用する方が良いでしょう。

##### 使用法

```js
browser.pause(milliseconds)
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
      <td><code><var>milliseconds</var></code></td>
      <td>`number`</td>
      <td>ミリ秒単位の時間</td>
    </tr>
  </tbody>
</table>

##### 例

```js title="pause.js"
it('should pause the execution', async () => {
    const starttime = new Date().getTime()
    await browser.pause(3000)
    const endtime = new Date().getTime()
    console.log(endtime - starttime) // outputs: 3000
});
```