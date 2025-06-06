---
id: dragAndDrop
title: dragAndDrop
custom_edit_url: https://github.com/webdriverio/webdriverio/edit/main/packages/webdriverio/src/commands/element/dragAndDrop.ts
---

Arrastar um item para um elemento de destino ou posição.

:::info

A funcionalidade deste comando depende muito da forma como o arrastar e soltar é
implementado em sua aplicação. Se você encontrar problemas, por favor, poste seu exemplo
em [#4134](https://github.com/webdriverio/webdriverio/issues/4134).

:::

##### Uso

```js
$(selector).dragAndDrop(target, { duration })
```

##### Parâmetros

<table>
  <thead>
    <tr>
      <th>Nome</th><th>Tipo</th><th>Detalhes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>target</var></code></td>
      <td>`Element, DragAndDropCoordinate`</td>
      <td>elemento de destino ou objeto com propriedades x e y</td>
    </tr>
    <tr>
      <td><code><var>options</var></code><br /><span className="label labelWarning">opcional</span></td>
      <td>`DragAndDropOptions`</td>
      <td>opções do comando dragAndDrop</td>
    </tr>
    <tr>
      <td><code><var>options.duration</var></code><br /><span className="label labelWarning">opcional</span></td>
      <td>`Number`</td>
      <td>quanto tempo o arrasto deve durar</td>
    </tr>
  </tbody>
</table>

##### Exemplo

```js title="example.test.js"
it('should demonstrate the dragAndDrop command', async () => {
    const elem = $('#someElem')
    const target = $('#someTarget')

    // drag and drop to other element
    await elem.dragAndDrop(target)

    // drag and drop relative from current position
    await elem.dragAndDrop({ x: 100, y: 200 })
})
```