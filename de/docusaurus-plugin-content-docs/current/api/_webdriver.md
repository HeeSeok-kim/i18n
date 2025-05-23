---
id: webdriver
title: WebDriver Protokoll
custom_edit_url: https://github.com/webdriverio/webdriverio/edit/main/packages/wdio-protocols/src/protocols/webdriver.ts
---
## newSession
Der New Session Befehl erzeugt eine neue WebDriver-Sitzung mit dem Endpunktknoten. Wenn die Erstellung fehlschlägt, wird ein "session not created"-Fehler zurückgegeben.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-new-sessions).



##### Verwendung

```js
browser.newSession(capabilities)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>capabilities</var></code></td>
      <td>object</td>
      <td>ein JSON-Objekt, das Set von Capabilities, das letztendlich im Capability-Verarbeitungsalgorithmus zusammengeführt und abgeglichen wurde</td>
    </tr>
  </tbody>
</table>


##### Gibt zurück

- **&lt;Object&gt;**
            **<code><var>session</var></code>:** Objekt, das sessionId und Capabilities der erstellten WebDriver-Sitzung enthält.    


---
## deleteSession
Der Delete Session Befehl schließt alle übergeordneten Browsing-Kontexte, die mit der aktuellen Sitzung verknüpft sind, beendet die Verbindung und schließt schließlich die aktuelle Sitzung.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-delete-session).



##### Verwendung

```js
browser.deleteSession(deleteSessionOpts)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>deleteSessionOpts</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>object</td>
      <td>Objekt, das Optionen für den deleteSession-Befehl enthält, z.B. `{ shutdownDriver: boolean }`</td>
    </tr>
  </tbody>
</table>





---
## status
Der Status-Befehl gibt Informationen darüber zurück, ob ein Remote-Ende in einem Zustand ist, in dem es neue Sitzungen erstellen kann, und kann zusätzlich beliebige Meta-Informationen enthalten, die für die Implementierung spezifisch sind.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-status).



##### Verwendung

```js
browser.status()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L5-L16
```



##### Gibt zurück

- **&lt;Object&gt;**
            **<code><var>status</var></code>:** Objekt, das den Status des Treibers enthält.    


---
## getTimeouts
Der Get Timeouts Befehl ruft Timeout-Dauern ab, die mit der aktuellen Sitzung verknüpft sind.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-timeouts).



##### Verwendung

```js
browser.getTimeouts()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L20-L24
```



##### Gibt zurück

- **&lt;Object&gt;**
            **<code><var>timeouts</var></code>:** Objekt, das Timeout-Dauern für `script`, `pageLoad` und `implicit` Timeouts enthält.    


---
## setTimeouts
Der Set Timeouts Befehl legt Timeout-Dauern fest, die mit der aktuellen Sitzung verknüpft sind. Die Timeouts, die gesteuert werden können, sind in der Tabelle der Sitzungs-Timeouts unten aufgeführt.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-set-timeouts).



##### Verwendung

```js
browser.setTimeouts(implicit, pageLoad, script)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>implicit</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>number</td>
      <td>Ganzzahl in ms für impliziten Sitzungs-Timeout</td>
    </tr>
    <tr>
      <td><code><var>pageLoad</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>number</td>
      <td>Ganzzahl in ms für Sitzungs-Seiten-Lade-Timeout</td>
    </tr>
    <tr>
      <td><code><var>script</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>number</td>
      <td>Ganzzahl in ms für Sitzungs-Skript-Timeout</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L28-L33
```






---
## getUrl
Der Get Current URL Befehl gibt die URL des aktuellen übergeordneten Browsing-Kontexts zurück.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-current-url).



##### Verwendung

```js
browser.getUrl()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L39-L43
```



##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>url</var></code>:** aktuelle URL des aktiven Dokuments des übergeordneten Browsing-Kontexts    


---
## navigateTo
Der navigateTo (go) Befehl wird verwendet, um den User-Agent dazu zu bringen, den aktuellen übergeordneten Browsing-Kontext zu einer neuen Adresse zu navigieren.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-navigate-to).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [url](/docs/api/browser/url). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.navigateTo(url)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>url</var></code></td>
      <td>string</td>
      <td>eine Zeichenfolge, die eine absolute URL darstellt (beginnend mit http(s)), möglicherweise einschließlich eines Fragments (#...), könnte auch ein lokales Schema sein (about: usw.)</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L47-L51
```






---
## back
Der Back-Befehl veranlasst den Browser, einen Schritt rückwärts im gemeinsamen Sitzungsverlauf des aktuellen übergeordneten Browsing-Kontexts zu gehen. Dies entspricht dem Drücken der Zurück-Schaltfläche in der Browser-Chrome oder dem Aufruf von `window.history.back`.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-back).



##### Verwendung

```js
browser.back()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L55-L59
```






---
## forward
Der Forward-Befehl veranlasst den Browser, einen Schritt vorwärts im gemeinsamen Sitzungsverlauf des aktuellen übergeordneten Browsing-Kontexts zu gehen.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-forward).



##### Verwendung

```js
browser.forward()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L63-L69
```






---
## refresh
Der Refresh-Befehl veranlasst den Browser, die Seite im aktuellen übergeordneten Browsing-Kontext neu zu laden.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-refresh).



##### Verwendung

```js
browser.refresh()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L73-L78
```






---
## getTitle
Der Get Title Befehl gibt den Dokumenttitel des aktuellen übergeordneten Browsing-Kontexts zurück, was dem Aufruf von `document.title` entspricht.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-title).



##### Verwendung

```js
browser.getTitle()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L82-L86
```



##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>title</var></code>:** Gibt eine Zeichenfolge zurück, die identisch mit `document.title` des aktuellen übergeordneten Browsing-Kontexts ist.    


---
## getWindowHandle
Der Get Window Handle Befehl gibt den Fensterhandle für den aktuellen übergeordneten Browsing-Kontext zurück. Er kann als Argument für Switch To Window verwendet werden.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-window-handle).



##### Verwendung

```js
browser.getWindowHandle()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L90-L93
```



##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>handle</var></code>:** Gibt eine Zeichenfolge zurück, die der Fensterhandle für den aktuellen übergeordneten Browsing-Kontext ist.    


---
## closeWindow
Der Close Window Befehl schließt den aktuellen übergeordneten Browsing-Kontext. Sobald dies erledigt ist, wenn keine weiteren übergeordneten Browsing-Kontexte geöffnet sind, wird die WebDriver-Sitzung selbst geschlossen.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-close-window).



##### Verwendung

```js
browser.closeWindow()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L97-L117
```






---
## switchToWindow
Der Switch To Window Befehl wird verwendet, um den aktuellen übergeordneten Browsing-Kontext für die aktuelle Sitzung auszuwählen, d.h. denjenigen, der für die Verarbeitung von Befehlen verwendet wird.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-switch-to-window).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [switchWindow](/docs/api/browser/switchWindow). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.switchToWindow(handle)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>handle</var></code></td>
      <td>string</td>
      <td>eine Zeichenkette, die einen Fensterhandle darstellt, sollte eine der Zeichenketten sein, die in einem Aufruf von getWindowHandles zurückgegeben wurde</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L121-L130
```






---
## createWindow
Erstellt einen neuen übergeordneten Browsing-Kontext.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#new-window).



##### Verwendung

```js
browser.createWindow(type)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>type</var></code></td>
      <td>string</td>
      <td>Auf 'tab' gesetzt, wenn das neu erstellte Fenster ein OS-Level-Fenster mit dem aktuellen Browsing-Kontext teilt, oder 'window' andernfalls.</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L134-L136
```



##### Gibt zurück

- **&lt;Object&gt;**
            **<code><var>window</var></code>:** Neues Fensterobjekt, das 'handle' mit dem Wert des Handles und 'type' mit dem Wert des erstellten Fenstertyps enthält    


---
## getWindowHandles
Der Get Window Handles Befehl gibt eine Liste von Fensterhandles für jeden offenen übergeordneten Browsing-Kontext zurück. Die Reihenfolge, in der die Fensterhandles zurückgegeben werden, ist beliebig.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-window-handles).



##### Verwendung

```js
browser.getWindowHandles()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L140-L143
```



##### Gibt zurück

- **&lt;String[]&gt;**
            **<code><var>handles</var></code>:** Ein Array, das eine Liste von Fensterhandles ist.    


---
## printPage
Der Print Page Befehl rendert das Dokument zu einem paginierten PDF-Dokument. __Hinweis:__ Chrome unterstützt dies derzeit nur im [Headless-Modus](https://webdriver.io/docs/capabilities/#run-browser-headless), siehe [`crbug753118`](https://bugs.chromium.org/p/chromium/issues/detail?id=753118)).<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#print-page).



##### Verwendung

```js
browser.printPage(orientation, scale, background, width, height, top, bottom, left, right, shrinkToFit, pageRanges)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>orientation</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>string</td>
      <td>Seitenausrichtung. Standard: `portrait`</td>
    </tr>
    <tr>
      <td><code><var>scale</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>number</td>
      <td>Seitenskalierung. Standard: `1`</td>
    </tr>
    <tr>
      <td><code><var>background</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>boolean</td>
      <td>Seitenhintergrund. Standard: `false`</td>
    </tr>
    <tr>
      <td><code><var>width</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>number</td>
      <td>Seitenbreite in cm. Standard: `21.59` von der Seite</td>
    </tr>
    <tr>
      <td><code><var>height</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>number</td>
      <td>Seitenhöhe in cm. Standard: `27.94` von der Seite</td>
    </tr>
    <tr>
      <td><code><var>top</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>number</td>
      <td>Seitenrand in cm vom oberen Rand. Standard: `1`</td>
    </tr>
    <tr>
      <td><code><var>bottom</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>number</td>
      <td>Seitenrand in cm vom unteren Rand. Standard: `1`</td>
    </tr>
    <tr>
      <td><code><var>left</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>number</td>
      <td>Seitenrand in cm vom linken Rand. Standard: `1`</td>
    </tr>
    <tr>
      <td><code><var>right</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>number</td>
      <td>Seitenrand in cm vom rechten Rand. Standard: `1`</td>
    </tr>
    <tr>
      <td><code><var>shrinkToFit</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>boolean</td>
      <td>PDF an Seite anpassen. Standard: `true`</td>
    </tr>
    <tr>
      <td><code><var>pageRanges</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>object[]</td>
      <td>Seitenbereiche. Standard `[]`</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L150-L151
```



##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>pdf</var></code>:** Die Base64-kodierte PDF-Darstellung des paginierten Dokuments.    


---
## switchToFrame
Der Switch To Frame Befehl wird verwendet, um den aktuellen übergeordneten Browsing-Kontext oder einen untergeordneten Browsing-Kontext des aktuellen Browsing-Kontexts als aktuellen Browsing-Kontext für nachfolgende Befehle auszuwählen.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-switch-to-frame).
:::caution

Dieser Protokollbefehl ist veraltet<br />Dieser Befehl ist veraltet und wir empfehlen jedem, stattdessen `switchFrame` für das Wechseln in Frames zu verwenden. Lesen Sie mehr über diesen Befehl unter https://webdriver.io/docs/api/browser/switchFrame.
:::



##### Verwendung

```js
browser.switchToFrame(id)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>id</var></code></td>
      <td>number, object, null</td>
      <td>eine von drei möglichen Typen: null: dies repräsentiert den übergeordneten Browsing-Kontext (d.h. nicht einen iframe), eine Zahl, die den Index des Fensterobjekts darstellt, das einem Frame entspricht, ein Element-Objekt, das über `findElement` empfangen wurde.</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L155-L168
```






---
## switchToParentFrame
Der Switch to Parent Frame Befehl setzt den aktuellen Browsing-Kontext für zukünftige Befehle auf den übergeordneten Kontext des aktuellen Browsing-Kontexts.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-switch-to-parent-frame).



##### Verwendung

```js
browser.switchToParentFrame()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L172-L189
```






---
## getWindowRect
Der Get Window Rect Befehl gibt die Größe und Position auf dem Bildschirm des Betriebssystemfensters zurück, das dem aktuellen übergeordneten Browsing-Kontext entspricht.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-window-rect).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [getWindowSize](/docs/api/browser/getWindowSize). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.getWindowRect()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L193-L196
```



##### Gibt zurück

- **&lt;Object&gt;**
            **<code><var>windowRect</var></code>:** Eine JSON-Darstellung eines "window rect"-Objekts. Dies hat 4 Eigenschaften: `x`, `y`, `width` und `height`.    


---
## setWindowRect
Der Set Window Rect Befehl ändert die Größe und Position des Betriebssystemfensters, das dem aktuellen übergeordneten Browsing-Kontext entspricht.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-set-window-rect).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [setWindowSize](/docs/api/browser/setWindowSize). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.setWindowRect(x, y, width, height)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>x</var></code></td>
      <td>number, null</td>
      <td>das screenX-Attribut des Fensterobjekts</td>
    </tr>
    <tr>
      <td><code><var>y</var></code></td>
      <td>number, null</td>
      <td>das screenY-Attribut des Fensterobjekts</td>
    </tr>
    <tr>
      <td><code><var>width</var></code></td>
      <td>number, null</td>
      <td>die Breite der äußeren Abmessungen des übergeordneten Browsing-Kontexts, einschließlich Browser-Chrome usw...</td>
    </tr>
    <tr>
      <td><code><var>height</var></code></td>
      <td>number, null</td>
      <td>die Höhe der äußeren Abmessungen des übergeordneten Browsing-Kontexts, einschließlich Browser-Chrome usw...</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L200-L204
```



##### Gibt zurück

- **&lt;Object&gt;**
            **<code><var>windowRect</var></code>:** Eine JSON-Darstellung eines "window rect"-Objekts basierend auf dem neuen Fensterzustand.    


---
## maximizeWindow
Der Maximize Window Befehl ruft die fenstermanager-spezifische "maximize"-Operation auf, falls vorhanden, für das Fenster, das den aktuellen übergeordneten Browsing-Kontext enthält. Dies vergrößert das Fenster typischerweise auf die maximal verfügbare Größe, ohne in den Vollbildmodus zu gehen.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-maximize-window).



##### Verwendung

```js
browser.maximizeWindow()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L208-L212
```



##### Gibt zurück

- **&lt;Object&gt;**
            **<code><var>windowRect</var></code>:** Eine JSON-Darstellung eines "window rect"-Objekts basierend auf dem neuen Fensterzustand.    


---
## minimizeWindow
Der Minimize Window Befehl ruft die fenstermanager-spezifische "minimize"-Operation auf, falls vorhanden, für das Fenster, das den aktuellen übergeordneten Browsing-Kontext enthält. Dies versteckt das Fenster typischerweise in der System-Taskleiste.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-minimize-window).



##### Verwendung

```js
browser.minimizeWindow()
```




##### Gibt zurück

- **&lt;Object&gt;**
            **<code><var>windowRect</var></code>:** Eine JSON-Darstellung eines "window rect"-Objekts des (neuen) aktuellen übergeordneten Browsing-Kontexts.    


---
## fullscreenWindow
Der Fullscreen Window Befehl ruft die fenstermanager-spezifische "full screen"-Operation auf, falls vorhanden, für das Fenster, das den aktuellen übergeordneten Browsing-Kontext enthält. Dies vergrößert das Fenster typischerweise auf die Größe des physischen Displays und kann Browser-Chrome-Elemente wie Symbolleisten ausblenden.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-fullscreen-window).



##### Verwendung

```js
browser.fullscreenWindow()
```




##### Gibt zurück

- **&lt;Object&gt;**
            **<code><var>windowRect</var></code>:** Eine JSON-Darstellung eines "window rect"-Objekts des (neuen) aktuellen übergeordneten Browsing-Kontexts.    


---
## findElement
Der Find Element Befehl wird verwendet, um ein Element im aktuellen Browsing-Kontext zu finden, das für zukünftige Befehle verwendet werden kann. Dieser Befehl gibt eine JSON-Darstellung des Elements zurück, die an den $ Befehl übergeben werden kann, um die Referenz in ein erweitertes WebdriverIO-Element zu transformieren.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-find-element).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [$](/docs/api/browser/$). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.findElement(using, value)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>using</var></code></td>
      <td>string</td>
      <td>eine gültige Element-Lokalisierungsstrategie</td>
    </tr>
    <tr>
      <td><code><var>value</var></code></td>
      <td>string</td>
      <td>der tatsächliche Selektor, der verwendet wird, um ein Element zu finden</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L226-L232
```



##### Gibt zurück

- **&lt;object&gt;**
            **<code><var>element</var></code>:** Eine JSON-Darstellung eines Element-Objekts, z.B. `{ 'element-6066-11e4-a52e-4f735466cecf': 'ELEMENT_1' }`.    


---
## findElementFromShadowRoot
Der Find Element From Shadow Root Befehl wird verwendet, um ein Element innerhalb des Shadow-Root eines Elements zu finden, das für zukünftige Befehle verwendet werden kann. Dieser Befehl gibt eine JSON-Darstellung des Elements zurück, die an den $ Befehl übergeben werden kann, um die Referenz in ein erweitertes WebdriverIO-Element zu transformieren.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#find-element-from-shadow-root).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [shadow$](/docs/api/element/shadow$). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.findElementFromShadowRoot(shadowId, using, value)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>shadowId</var></code></td>
      <td>String</td>
      <td>Element-ID eines Shadow-Root-Elements</td>
    </tr>
    <tr>
      <td><code><var>using</var></code></td>
      <td>string</td>
      <td>eine gültige Element-Lokalisierungsstrategie</td>
    </tr>
    <tr>
      <td><code><var>value</var></code></td>
      <td>string</td>
      <td>der tatsächliche Selektor, der verwendet wird, um ein Element zu finden</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L236-L248
```



##### Gibt zurück

- **&lt;object&gt;**
            **<code><var>element</var></code>:** Eine JSON-Darstellung eines Element-Shadow-Objekts, z.B. `{ 'element-6066-11e4-a52e-4f735466cecf': 'ELEMENT_1' }`.    


---
## findElements
Der Find Elements Befehl wird verwendet, um Elemente im aktuellen Browsing-Kontext zu finden, die für zukünftige Befehle verwendet werden können. Dieser Befehl gibt ein Array von JSON-Darstellungen der Elemente zurück, die an den $ Befehl übergeben werden können, um die Referenz in ein erweitertes WebdriverIO-Element zu transformieren (siehe findElement).<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-find-elements).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [$$](/docs/api/browser/$$). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.findElements(using, value)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>using</var></code></td>
      <td>string</td>
      <td>eine gültige Element-Lokalisierungsstrategie</td>
    </tr>
    <tr>
      <td><code><var>value</var></code></td>
      <td>string</td>
      <td>der tatsächliche Selektor, der verwendet wird, um ein Element zu finden</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L252-L254
```



##### Gibt zurück

- **&lt;object[]&gt;**
            **<code><var>elements</var></code>:** Eine (möglicherweise leere) JSON-Liste von Darstellungen eines Element-Objekts, z.B. `[{ 'element-6066-11e4-a52e-4f735466cecf': 'ELEMENT_1' }]`.    


---
## findElementsFromShadowRoot
Der Find Elements Befehl wird verwendet, um Elemente innerhalb des Shadow-Root eines Elements zu finden, die für zukünftige Befehle verwendet werden können. Dieser Befehl gibt ein Array von JSON-Darstellungen der Elemente zurück, die an den $ Befehl übergeben werden können, um die Referenz in ein erweitertes WebdriverIO-Element zu transformieren (siehe findElement).<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#find-elements-from-shadow-root).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [shadow$$](/docs/api/element/shadow$$). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.findElementsFromShadowRoot(shadowId, using, value)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>shadowId</var></code></td>
      <td>String</td>
      <td>Element-ID eines Shadow-Root-Elements</td>
    </tr>
    <tr>
      <td><code><var>using</var></code></td>
      <td>string</td>
      <td>eine gültige Element-Lokalisierungsstrategie</td>
    </tr>
    <tr>
      <td><code><var>value</var></code></td>
      <td>string</td>
      <td>der tatsächliche Selektor, der verwendet wird, um ein Element zu finden</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L258-L268
```



##### Gibt zurück

- **&lt;object[]&gt;**
            **<code><var>elements</var></code>:** Eine (möglicherweise leere) JSON-Liste von Darstellungen eines Element-Objekts, z.B. `{ 'element-6066-11e4-a52e-4f735466cecf': 'ELEMENT_1' }`.    


---
## findElementFromElement
Der Find Element From Element Befehl wird verwendet, um ein Element von einem Web-Element im aktuellen Browsing-Kontext zu finden, das für zukünftige Befehle verwendet werden kann. Dieser Befehl gibt eine JSON-Darstellung des Elements zurück, die an den $ Befehl übergeben werden kann, um die Referenz in ein erweitertes WebdriverIO-Element zu transformieren (siehe findElement).<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-find-element-from-element).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [$](/docs/api/element/$). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.findElementFromElement(elementId, using, value)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
    <tr>
      <td><code><var>using</var></code></td>
      <td>string</td>
      <td>eine gültige Element-Lokalisierungsstrategie</td>
    </tr>
    <tr>
      <td><code><var>value</var></code></td>
      <td>string</td>
      <td>der tatsächliche Selektor, der verwendet wird, um ein Element zu finden</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L272-L279
```



##### Gibt zurück

- **&lt;object&gt;**
            **<code><var>element</var></code>:** Eine JSON-Darstellung eines Element-Objekts, z.B. `{ 'element-6066-11e4-a52e-4f735466cecf': 'ELEMENT_1' }`.    


---
## findElementsFromElement
Der Find Elements From Element Befehl wird verwendet, um Elemente von einem Web-Element im aktuellen Browsing-Kontext zu finden, die für zukünftige Befehle verwendet werden können. Dieser Befehl gibt ein Array von JSON-Darstellungen der Elemente zurück, die an den $ Befehl übergeben werden können, um die Referenz in ein erweitertes WebdriverIO-Element zu transformieren (siehe findElement).<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-find-elements-from-element).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [$$](/docs/api/element/$$). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.findElementsFromElement(elementId, using, value)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
    <tr>
      <td><code><var>using</var></code></td>
      <td>string</td>
      <td>eine gültige Element-Lokalisierungsstrategie</td>
    </tr>
    <tr>
      <td><code><var>value</var></code></td>
      <td>string</td>
      <td>der tatsächliche Selektor, der verwendet wird, um ein Element zu finden</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L283-L290
```



##### Gibt zurück

- **&lt;object[]&gt;**
            **<code><var>elements</var></code>:** Eine (möglicherweise leere) JSON-Liste von Darstellungen eines Element-Objekts, z.B. `[{ 'element-6066-11e4-a52e-4f735466cecf': 'ELEMENT_1' }]`.    


---
## getElementShadowRoot
Ruft das Shadow-Root-Objekt eines Elements ab. Das Ergebnisobjekt kann verwendet werden, um Elemente innerhalb dieses Shadow-Roots zu finden, z.B. mit findElementFromShadowRoots oder findElementsFromShadowRoots.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-active-element).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [shadow$](/docs/api/element/shadow$). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.getElementShadowRoot(elementId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L294-L305
```



##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>shadowRoot</var></code>:** Eine JSON-Darstellung eines Element-Shadow-Roots, z.B. `{ 'shadow-6066-11e4-a52e-4f735466cecf': 'ELEMENT_1' }`.    


---
## getActiveElement
Get Active Element gibt das aktive Element des Dokumentelements des aktuellen Browsing-Kontexts zurück. Dieser Befehl gibt eine JSON-Darstellung des Elements zurück, die an den $ Befehl übergeben werden kann, um die Referenz in ein erweitertes WebdriverIO-Element zu transformieren (siehe findElement).<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-active-element).



##### Verwendung

```js
browser.getActiveElement()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L309-L316
```



##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>element</var></code>:** Eine JSON-Darstellung eines Element-Objekts, z.B. `{ 'element-6066-11e4-a52e-4f735466cecf': 'ELEMENT_1' }`.    


---
## isElementSelected
Is Element Selected bestimmt, ob das referenzierte Element ausgewählt ist oder nicht. Diese Operation ist nur sinnvoll für Eingabeelemente der Zustände Checkbox und Radio Button oder für Option-Elemente.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-is-element-selected).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [isSelected](/docs/api/element/isSelected). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.isElementSelected(elementId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L322-L325
```



##### Gibt zurück

- **&lt;Boolean&gt;**
            **<code><var>isSelected</var></code>:** `true` oder `false` basierend auf dem ausgewählten Zustand.    


---
## isElementDisplayed
Is Element Displayed bestimmt die Sichtbarkeit eines Elements, die von dem geleitet wird, was für das menschliche Auge wahrnehmbar sichtbar ist. In diesem Kontext steht die Anzeigbarkeit eines Elements nicht im Zusammenhang mit den CSS-Eigenschaften `visibility` oder `display`.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#element-displayedness).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [isDisplayed](/docs/api/element/isDisplayed). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.isElementDisplayed(elementId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L331-L333
```



##### Gibt zurück

- **&lt;Boolean&gt;**
            **<code><var>isDisplayed</var></code>:** `true` oder `false` basierend auf dem sichtbaren Zustand.    


---
## getElementAttribute
Der Get Element Attribute Befehl gibt das Attribut eines Web-Elements zurück.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-element-attribute).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [getAttribute](/docs/api/element/getAttribute). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.getElementAttribute(elementId, name)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
    <tr>
      <td><code><var>name</var></code></td>
      <td>String</td>
      <td>Name des abzurufenden Attributwerts</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L339-L341
```



##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>attribute</var></code>:** Das benannte Attribut des Elements.    


---
## getElementProperty
Der Get Element Property Befehl gibt das Ergebnis des Abrufs einer Eigenschaft eines Elements zurück.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-element-property).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [getProperty](/docs/api/element/getProperty). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.getElementProperty(elementId, name)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
    <tr>
      <td><code><var>name</var></code></td>
      <td>String</td>
      <td>Name der abzurufenden Attributeigenschaft</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L347-L349
```



##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>property</var></code>:** Die benannte Eigenschaft des Elements, auf die durch Aufruf von GetOwnProperty auf dem Element-Objekt zugegriffen wird.    


---
## getElementCSSValue
Der Get Element CSS Value Befehl ruft den berechneten Wert der angegebenen CSS-Eigenschaft des gegebenen Web-Elements ab.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-element-css-value).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [getCSSProperty](/docs/api/element/getCSSProperty). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.getElementCSSValue(elementId, propertyName)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
    <tr>
      <td><code><var>propertyName</var></code></td>
      <td>String</td>
      <td>Name der abzurufenden CSS-Eigenschaft</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L355-L357
```



##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>cssValue</var></code>:** Der berechnete Wert des Parameters, der der Eigenschaft name aus den Stildeklarationen des Elements entspricht (es sei denn, der Dokumenttyp ist xml, in diesem Fall ist der Rückgabewert einfach die leere Zeichenfolge).    


---
## getElementText
Der Get Element Text Befehl soll den Text eines Elements "wie gerendert" zurückgeben. Der gerenderte Text eines Elements wird auch zum Lokalisieren von Elementen nach ihrem Linktext und teilweisem Linktext verwendet.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-element-text).



##### Verwendung

```js
browser.getElementText(elementId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L363-L365
```



##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>text</var></code>:** Der sichtbare Text des Elements (einschließlich untergeordneter Elemente), gemäß dem im Selenium Atoms für [`bot.dom.getVisibleText`](https://github.com/SeleniumHQ/selenium/blob/e09e28f016c9f53196cf68d6f71991c5af4a35d4/javascript/atoms/dom.js#L981) definierten Algorithmus.    


---
## getElementTagName
Der Get Element Tag Name Befehl gibt den qualifizierten Elementnamen des gegebenen Web-Elements zurück.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-element-tag-name).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [getTagName](/docs/api/element/getTagName). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.getElementTagName(elementId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L371-L373
```



##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>text</var></code>:** Das tagName-Attribut des Elements.    


---
## getElementRect
Der Get Element Rect Befehl gibt die Abmessungen und Koordinaten des gegebenen Web-Elements zurück.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-element-rect).

:::info

Dieser Protokollbefehl ist in den folgenden praktischen Methoden eingebettet: [getSize](/docs/api/element/getSize), [getLocation](/docs/api/element/getLocation). Es wird empfohlen, diese Befehle stattdessen zu verwenden.

:::


##### Verwendung

```js
browser.getElementRect(elementId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L379-L381
```



##### Gibt zurück

- **&lt;Object&gt;**
            **<code><var>elementRect</var></code>:** Ein JSON-Objekt, das die Position und den Begrenzungsrahmen des Elements darstellt.    


---
## isElementEnabled
Is Element Enabled bestimmt, ob das referenzierte Element aktiviert ist oder nicht. Diese Operation ist nur für Formularsteuerelemente sinnvoll.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-is-element-enabled).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [isEnabled](/docs/api/element/isEnabled). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.isElementEnabled(elementId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L387-L390
```



##### Gibt zurück

- **&lt;Boolean&gt;**
            **<code><var>isEnabled</var></code>:** Wenn das Element in einem XML-Dokument ist oder ein deaktiviertes Formularelement ist: `false`, andernfalls `true`.    


---
## elementClick
Der Element Click Befehl scrollt das Element in die Ansicht, wenn es noch nicht Zeiger-interaktionsfähig ist, und klickt auf seinen sichtbaren Mittelpunkt. Wenn der Mittelpunkt des Elements durch ein anderes Element verdeckt wird, wird ein Fehler "element click intercepted" zurückgegeben. Wenn das Element außerhalb des Viewports liegt, wird ein Fehler "element not interactable" zurückgegeben.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-element-click).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [click](/docs/api/element/click). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.elementClick(elementId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L396-L398
```






---
## elementClear
Der Element Clear Befehl scrollt ein bearbeitbares oder zurücksetzbares Element in die Ansicht und versucht dann, seine ausgewählten Dateien oder Textinhalte zu löschen.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-element-clear).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [clearValue](/docs/api/element/clearValue). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.elementClear(elementId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L404-L407
```






---
## elementSendKeys
Der Element Send Keys Befehl scrollt das Formularsteuerelement in die Ansicht und sendet dann die bereitgestellten Tasten an das Element. Falls das Element nicht tastatur-interaktionsfähig ist, wird ein Fehler "element not interactable" zurückgegeben.<br /><br />Der für die Eingabe verwendete Tasteneingabezustand kann mittendurch beim "Tippen" gelöscht werden, indem die Null-Taste gesendet wird, die U+E000 (NULL) ist.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-element-send-keys).

:::info

Dieser Protokollbefehl ist in den folgenden praktischen Methoden eingebettet: [addValue](/docs/api/element/addValue), [setValue](/docs/api/element/setValue). Es wird empfohlen, diese Befehle stattdessen zu verwenden.

:::


##### Verwendung

```js
browser.elementSendKeys(elementId, text)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
    <tr>
      <td><code><var>text</var></code></td>
      <td>string</td>
      <td>Zeichenfolge, die als Tastatureingaben an das Element gesendet werden soll</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L413-L416
```






---
## getPageSource
Der Get Page Source Befehl gibt eine Zeichenkettenserialisierung des DOM des aktuellen aktiven Dokuments des Browsing-Kontexts zurück.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-page-source).



##### Verwendung

```js
browser.getPageSource()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L420-L421
```



##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>pageSource</var></code>:** das DOM des aktuellen aktiven Dokuments des Browsing-Kontexts    


---
## executeScript
Der Execute Script Befehl führt eine JavaScript-Funktion im Kontext des aktuellen Browsing-Kontexts aus und gibt den Rückgabewert der Funktion zurück.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-execute-script).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [execute](/docs/api/browser/execute). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.executeScript(script, args)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>script</var></code></td>
      <td>string</td>
      <td>eine Zeichenfolge, der JavaScript-Funktionskörper, den Sie ausführen möchten</td>
    </tr>
    <tr>
      <td><code><var>args</var></code></td>
      <td>string, object, number, boolean, null, undefined[]</td>
      <td>ein Array von JSON-Werten, die deserialisiert und als Argumente an Ihre Funktion übergeben werden</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L425-L426
```



##### Gibt zurück

- **&lt;*&gt;**
            **<code><var>result</var></code>:** Entweder der Rückgabewert Ihres Skripts, die Erfüllung des von Ihrem Skript zurückgegebenen Versprechens oder der Fehler, der der Grund für die Ablehnung des von Ihrem Skript zurückgegebenen Versprechens war.    


---
## executeAsyncScript
Der Execute Async Script Befehl führt JavaScript als anonyme Funktion aus. Im Gegensatz zum Execute Script Befehl wird das Ergebnis der Funktion ignoriert. Stattdessen wird ein zusätzliches Argument als letztes Argument für die Funktion bereitgestellt. Dies ist eine Funktion, die bei Aufruf ihr erstes Argument als Antwort zurückgibt.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-execute-async-script).

:::info

Dieser Protokollbefehl ist in der folgenden praktischen Methode eingebettet: [executeAsync](/docs/api/browser/executeAsync). Es wird empfohlen, stattdessen diesen Befehl zu verwenden.

:::


##### Verwendung

```js
browser.executeAsyncScript(script, args)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>script</var></code></td>
      <td>string</td>
      <td>eine Zeichenfolge, der JavaScript-Funktionskörper, den Sie ausführen möchten</td>
    </tr>
    <tr>
      <td><code><var>args</var></code></td>
      <td>string, object, number, boolean, null, undefined[]</td>
      <td>ein Array von JSON-Werten, die deserialisiert und als Argumente an Ihre Funktion übergeben werden</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L430-L434
```



##### Gibt zurück

- **&lt;*&gt;**
            **<code><var>result</var></code>:** Entweder der Rückgabewert Ihres Skripts, die Erfüllung des von Ihrem Skript zurückgegebenen Versprechens oder der Fehler, der der Grund für die Ablehnung des von Ihrem Skript zurückgegebenen Versprechens war.    


---
## getAllCookies
Der Get All Cookies Befehl gibt alle Cookies zurück, die mit der Adresse des aktiven Dokuments des aktuellen Browsing-Kontexts verknüpft sind.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-all-cookies).



##### Verwendung

```js
browser.getAllCookies()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L438-L455
```



##### Gibt zurück

- **&lt;Object[]&gt;**
            **<code><var>cookies</var></code>:** Eine Liste serialisierter Cookies. Jedes serialisierte Cookie hat eine Reihe optionaler Felder, die zusätzlich zu `name` und `value` zurückgegeben werden können oder auch nicht.    


---
## addCookie
Der Add Cookie Befehl fügt ein einzelnes Cookie zum Cookie-Speicher hinzu, der mit der Adresse des aktiven Dokuments verknüpft ist.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-adding-a-cookie).



##### Verwendung

```js
browser.addCookie(cookie)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>cookie</var></code></td>
      <td>object</td>
      <td>Ein JSON-Objekt, das ein Cookie darstellt. Es muss mindestens die Felder name und value haben und kann weitere haben, einschließlich Ablaufzeit usw.</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L459-L477
```






---
## deleteAllCookies
Der Delete All Cookies Befehl ermöglicht das Löschen aller Cookies, die mit der Adresse des aktiven Dokuments verknüpft sind.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-delete-all-cookies).



##### Verwendung

```js
browser.deleteAllCookies()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L481-L485
```






---
## getNamedCookie
Der Get Named Cookie Befehl gibt das Cookie mit dem angeforderten Namen aus den zugehörigen Cookies im Cookie-Speicher des aktiven Dokuments des aktuellen Browsing-Kontexts zurück. Wenn kein Cookie gefunden wird, wird ein Fehler "no such cookie" zurückgegeben.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-named-cookie).



##### Verwendung

```js
browser.getNamedCookie(name)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>name</var></code></td>
      <td>String</td>
      <td>Name des abzurufenden Cookies</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L489-L503
```



##### Gibt zurück

- **&lt;Object&gt;**
            **<code><var>cookie</var></code>:** Ein serialisiertes Cookie mit name und value Feldern. Es gibt eine Reihe optionaler Felder wie `path`, `domain` und `expiry-time`, die ebenfalls vorhanden sein können.    


---
## deleteCookie
Der Delete Cookie Befehl ermöglicht es Ihnen, entweder ein einzelnes Cookie nach Parametername zu löschen oder alle Cookies, die mit der Adresse des aktiven Dokuments verknüpft sind, wenn name undefiniert ist.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-delete-cookie).



##### Verwendung

```js
browser.deleteCookie(name)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>name</var></code></td>
      <td>String</td>
      <td>Name des zu löschenden Cookies</td>
    </tr>
  </tbody>
</table>

##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L507-L512
```






---
## performActions
Der Perform Actions Befehl wird verwendet, um komplexe Benutzeraktionen auszuführen. Siehe [Spezifikation](https://github.com/jlipps/simple-wd-spec#perform-actions) für weitere Details.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-perform-actions).



##### Verwendung

```js
browser.performActions(actions)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>actions</var></code></td>
      <td>object[]</td>
      <td>eine Liste von Objekten, von denen jedes eine Eingabequelle und die zugehörigen Aktionen darstellt</td>
    </tr>
  </tbody>
</table>





---
## releaseActions
Der Release Actions Befehl wird verwendet, um alle Tasten und Zeigerknöpfe freizugeben, die derzeit gedrückt sind. Dies führt dazu, dass Ereignisse ausgelöst werden, als ob der Zustand durch eine explizite Reihe von Aktionen freigegeben wurde. Es löscht auch den gesamten internen Zustand der virtuellen Geräte.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-release-actions).



##### Verwendung

```js
browser.releaseActions()
```







---
## dismissAlert
Der Dismiss Alert Befehl schließt einen einfachen Dialog, falls vorhanden, andernfalls gibt es einen Fehler. Eine Anfrage zum Schließen einer Warnmeldung für den Benutzer, die möglicherweise nicht unbedingt einen Schließen-Button hat, hat den gleichen Effekt wie das Akzeptieren.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-dismiss-alert).



##### Verwendung

```js
browser.dismissAlert()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L516-L517
```






---
## acceptAlert
Der Accept Alert Befehl akzeptiert einen einfachen Dialog, falls vorhanden, andernfalls gibt es einen Fehler.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-accept-alert).



##### Verwendung

```js
browser.acceptAlert()
```







---
## getAlertText
Der Get Alert Text Befehl gibt die Nachricht der aktuellen Benutzeraufforderung zurück. Wenn es keine aktuelle Benutzeraufforderung gibt, gibt er einen Fehler zurück.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-get-alert-text).



##### Verwendung

```js
browser.getAlertText()
```



##### Beispiel

```js reference title="examples.js" useHTTPS
https://github.com/webdriverio/example-recipes/blob/e8b147e88e7a38351b0918b4f7efbd9ae292201d/api/webdriver/examples.js#L521-L522
```



##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>alertText</var></code>:** Die Nachricht der Benutzeraufforderung.    


---
## sendAlertText
Der Send Alert Text Befehl setzt das Textfeld einer window.prompt-Benutzeraufforderung auf den angegebenen Wert.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-send-alert-text).



##### Verwendung

```js
browser.sendAlertText(text)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>text</var></code></td>
      <td>string</td>
      <td>Zeichenkette, auf die die Aufforderung gesetzt werden soll</td>
    </tr>
  </tbody>
</table>





---
## takeScreenshot
Der Take Screenshot Befehl macht einen Screenshot vom Viewport des übergeordneten Browsing-Kontexts.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-take-screenshot).



##### Verwendung

```js
browser.takeScreenshot()
```




##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>screenshot</var></code>:** Die Base64-kodierten PNG-Bilddaten, die den Screenshot des anfänglichen Viewports umfassen.    


---
## takeElementScreenshot
Der Take Element Screenshot Befehl macht einen Screenshot der sichtbaren Region, die durch das Begrenzungsrechteck eines Elements umfasst wird.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#dfn-take-element-screenshot).



##### Verwendung

```js
browser.takeElementScreenshot(elementId, scroll)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
    <tr>
      <td><code><var>scroll</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>boolean</td>
      <td>scrolle das Element in die Ansicht. Standard: true</td>
    </tr>
  </tbody>
</table>


##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>screenshot</var></code>:** Die Base64-kodierten PNG-Bilddaten, die den Screenshot der sichtbaren Region des Begrenzungsrechtecks eines Elements umfassen, nachdem es in die Ansicht gescrollt wurde.    


---
## getElementComputedRole
Ruft die berechnete WAI-ARIA-Rolle eines Elements ab.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#get-computed-role).



##### Verwendung

```js
browser.getElementComputedRole(elementId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
  </tbody>
</table>


##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>role</var></code>:** Das Ergebnis der Berechnung der WAI-ARIA-Rolle des Elements.    


---
## getElementComputedLabel
Ruft den barrierefreien Namen des Elements ab.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/webdriver/#get-computed-label).



##### Verwendung

```js
browser.getElementComputedLabel(elementId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>elementId</var></code></td>
      <td>String</td>
      <td>die ID eines Elements, das in einem früheren Aufruf von Find Element(s) zurückgegeben wurde</td>
    </tr>
  </tbody>
</table>


##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>label</var></code>:** Das Ergebnis einer Berechnung des barrierefreien Namens und der Beschreibung für den barrierefreien Namen des Elements.    


---
## setPermissions
Simuliert die Benutzermodifikation des Berechtigungszustands eines PermissionDescriptors. __Hinweis:__ Diese Funktion ist noch nicht in allen Browsern verfügbar.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/permissions/#set-permission-command).



##### Verwendung

```js
browser.setPermissions(descriptor, state, oneRealm)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>descriptor</var></code></td>
      <td>object</td>
      <td>Jede leistungsstarke Funktion hat einen oder mehrere Aspekte, für die Websites um Berechtigungen zum Zugriff bitten können. Um diese Aspekte zu beschreiben, definiert jede Funktion einen Untertyp von PermissionDescriptor als ihren Berechtigungsdeskriptortyp. __Hinweis:__ Diese Funktion ist noch nicht in allen Browsern verfügbar.</td>
    </tr>
    <tr>
      <td><code><var>state</var></code></td>
      <td>string</td>
      <td>Bestimmt, ob die Erlaubnis erteilt, verweigert oder abgefragt wird.</td>
    </tr>
    <tr>
      <td><code><var>oneRealm</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>boolean</td>
      <td>Ob Berechtigungen auf alle Ausführungskontexte angewendet werden sollen oder nicht.</td>
    </tr>
  </tbody>
</table>

##### Beispiele


```js
// MIDI-Berechtigungen festlegen
browser.setPermissions(
  { name: 'midi', sysex: true },
  'granted' // kann auch "denied" oder "prompt" sein
);
```


```js
// Zwischenablage-Berechtigungen festlegen
browser.setPermissions({ name: 'clipboard-read' }, 'granted');
// jetzt können Sie die Zwischenablage lesen via, z.B.
const clipboardText = await browser.execute(() => navigator.clipboard.readText());
```





---
## generateTestReport
Erzeugt einen Bericht für Tests. Erweiterung für [Reporting API](https://developers.google.com/web/updates/2018/09/reportingapi). __Hinweis:__ Diese Funktion ist noch nicht in allen Browsern verfügbar.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/reporting/#automation).



##### Verwendung

```js
browser.generateTestReport(message, group)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>message</var></code></td>
      <td>string</td>
      <td>Nachricht, die im Bericht angezeigt werden soll.</td>
    </tr>
    <tr>
      <td><code><var>group</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>string</td>
      <td>Gibt die Endpunktgruppe an, an die der Bericht geliefert werden soll.</td>
    </tr>
  </tbody>
</table>





---
## createMockSensor
Erstellt einen Mock-Sensor, um Sensoren wie den Umgebungslichtsensor zu emulieren. __Hinweis:__ Diese Funktion ist noch nicht in allen Browsern verfügbar.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/sensors/#create-mock-sensor-command).



##### Verwendung

```js
browser.createMockSensor(mockSensorType, maxSamplingFrequency, minSamplingFrequency)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>mockSensorType</var></code></td>
      <td>string</td>
      <td>Typ der zu emulierenden Sensor-API, z.B. 'ambient-light'</td>
    </tr>
    <tr>
      <td><code><var>maxSamplingFrequency</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>number</td>
      <td>Eine Gleitkommazahl, die die Frequenz in Hz darstellt und verwendet wird, um die maximal unterstützte Abtastfrequenz für den zugehörigen Mock-Sensor festzulegen.</td>
    </tr>
    <tr>
      <td><code><var>minSamplingFrequency</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>number</td>
      <td>Eine Gleitkommazahl, die die Frequenz in Hz darstellt und verwendet wird, um die minimal unterstützte Abtastfrequenz für den zugehörigen Mock-Sensor festzulegen.</td>
    </tr>
  </tbody>
</table>





---
## getMockSensor
Ruft Informationen über einen bestimmten Typ von Mock-Sensor ab. __Hinweis:__ Diese Funktion ist noch nicht in allen Browsern verfügbar.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/sensors/#get-mock-sensor-command).



##### Verwendung

```js
browser.getMockSensor(type)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>type</var></code></td>
      <td>String</td>
      <td>Mock-Sensor-Typ, von dem Informationen abgerufen werden sollen.</td>
    </tr>
  </tbody>
</table>


##### Gibt zurück

- **&lt;object&gt;**
            **<code><var>sensorReading</var></code>:** Werte der Mock-Sensor-Lesung.    


---
## updateMockSensor
Aktualisiert den Mock-Sensor-Typ. __Hinweis:__ Diese Funktion ist noch nicht in allen Browsern verfügbar.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/sensors/#update-mock-sensor-reading-command).



##### Verwendung

```js
browser.updateMockSensor(type, mockSensorType, maxSamplingFrequency, minSamplingFrequency)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>type</var></code></td>
      <td>String</td>
      <td>Mock-Sensor-Typ, für den Informationen aktualisiert werden sollen.</td>
    </tr>
    <tr>
      <td><code><var>mockSensorType</var></code></td>
      <td>string</td>
      <td>Typ der zu emulierenden Sensor-API, z.B. 'ambient-light'</td>
    </tr>
    <tr>
      <td><code><var>maxSamplingFrequency</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>number</td>
      <td>Eine Gleitkommazahl, die die Frequenz in Hz darstellt und verwendet wird, um die maximal unterstützte Abtastfrequenz für den zugehörigen Mock-Sensor festzulegen.</td>
    </tr>
    <tr>
      <td><code><var>minSamplingFrequency</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>number</td>
      <td>Eine Gleitkommazahl, die die Frequenz in Hz darstellt und verwendet wird, um die minimal unterstützte Abtastfrequenz für den zugehörigen Mock-Sensor festzulegen.</td>
    </tr>
  </tbody>
</table>





---
## deleteMockSensor
Der Delete Session Befehl schließt alle übergeordneten Browsing-Kontexte, die mit der aktuellen Sitzung verknüpft sind, beendet die Verbindung und schließt schließlich die aktuelle Sitzung. __Hinweis:__ Diese Funktion ist noch nicht in allen Browsern verfügbar.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/sensors/#delete-mock-sensor-command).



##### Verwendung

```js
browser.deleteMockSensor(type)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>type</var></code></td>
      <td>String</td>
      <td>Zu löschender Mock-Sensor-Typ.</td>
    </tr>
  </tbody>
</table>





---
## setTimeZone
Simuliert das Ändern einer Zeitzone für Testzwecke. __Hinweis:__ Diese Funktion ist noch nicht in allen Browsern verfügbar.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://w3c.github.io/sensors/#create-mock-sensor-command).



##### Verwendung

```js
browser.setTimeZone(time_zone)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>time_zone</var></code></td>
      <td>string</td>
      <td>Name der Zeitzone, z.B. Asia/Tokyo</td>
    </tr>
  </tbody>
</table>





---
## addVirtualAuthenticator
Erstellt einen Software-[Virtual Authenticator](https://www.w3.org/TR/webauthn-2/#virtual-authenticators).<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://www.w3.org/TR/webauthn-2/#sctn-automation-add-virtual-authenticator).



##### Verwendung

```js
browser.addVirtualAuthenticator(protocol, transport, hasResidentKey, hasUserVerification, isUserConsenting, isUserVerified, extensions, uvm)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>protocol</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>string</td>
      <td>Gültige Werte: 'ctap1/u2f', 'ctap2', 'ctap2_1'.</td>
    </tr>
    <tr>
      <td><code><var>transport</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>string</td>
      <td>Gültige Werte: 'usb', 'nfc', 'ble' oder 'internal'.</td>
    </tr>
    <tr>
      <td><code><var>hasResidentKey</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>boolean</td>
      <td>Gültige Werte: true, false.</td>
    </tr>
    <tr>
      <td><code><var>hasUserVerification</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>boolean</td>
      <td>Gültige Werte: true, false.</td>
    </tr>
    <tr>
      <td><code><var>isUserConsenting</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>boolean</td>
      <td>Gültige Werte: true, false.</td>
    </tr>
    <tr>
      <td><code><var>isUserVerified</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>boolean</td>
      <td>Gültige Werte: Ein Array, das Erweiterungs-Identifikatoren enthält.</td>
    </tr>
    <tr>
      <td><code><var>extensions</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>string[]</td>
      <td>Gültige Werte: Bis zu 3 User Verification Method-Einträge.</td>
    </tr>
    <tr>
      <td><code><var>uvm</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>object[]</td>
      <td></td>
    </tr>
  </tbody>
</table>


##### Gibt zurück

- **&lt;string&gt;**
            **<code><var>authenticatorId</var></code>:** Gibt die String-ID des Authentifikators zurück.    


---
## removeVirtualAuthenticator
Entfernt einen zuvor erstellten Virtual Authenticator.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://www.w3.org/TR/webauthn-2/#sctn-automation-remove-virtual-authenticator).



##### Verwendung

```js
browser.removeVirtualAuthenticator(authenticatorId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>authenticatorId</var></code></td>
      <td>String</td>
      <td>ID des Authentifikators</td>
    </tr>
  </tbody>
</table>





---
## addCredential
Fügt eine Public Key Credential Source in einen bestehenden Virtual Authenticator ein.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://www.w3.org/TR/webauthn-2/#sctn-automation-add-credential).



##### Verwendung

```js
browser.addCredential(authenticatorId, credentialId, isResidentCredential, rpId, privateKey, userHandle, signCount, largeBlob)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>authenticatorId</var></code></td>
      <td>String</td>
      <td>ID des Authentifikators</td>
    </tr>
    <tr>
      <td><code><var>credentialId</var></code></td>
      <td>string</td>
      <td>Die Credential-ID, kodiert mit Base64url-Kodierung.</td>
    </tr>
    <tr>
      <td><code><var>isResidentCredential</var></code></td>
      <td>boolean</td>
      <td>Wenn auf true gesetzt, wird eine clientseitig auffindbare Anmeldeinformation erstellt. Wenn auf false gesetzt, wird stattdessen eine serverseitige Anmeldeinformation erstellt.</td>
    </tr>
    <tr>
      <td><code><var>rpId</var></code></td>
      <td>string</td>
      <td>Die Relying Party ID, auf die die Anmeldeinformation beschränkt ist.</td>
    </tr>
    <tr>
      <td><code><var>privateKey</var></code></td>
      <td>string</td>
      <td>Ein asymmetrisches Schlüsselpaket, das einen einzelnen privaten Schlüssel gemäß [RFC5958] enthält, kodiert mit Base64url-Kodierung.</td>
    </tr>
    <tr>
      <td><code><var>userHandle</var></code></td>
      <td>string</td>
      <td>Der mit der Anmeldeinformation verknüpfte userHandle, kodiert mit Base64url-Kodierung. Diese Eigenschaft ist möglicherweise nicht definiert.</td>
    </tr>
    <tr>
      <td><code><var>signCount</var></code></td>
      <td>number</td>
      <td>Der Anfangswert für einen Signaturzähler, der der Public Key Credential Source zugeordnet ist.</td>
    </tr>
    <tr>
      <td><code><var>largeBlob</var></code><br /><span className="label labelWarning">optional</span></td>
      <td>string</td>
      <td>Der große, anmeldeinformationsspezifische Blob, der der Public Key Credential Source zugeordnet ist, kodiert mit Base64url-Kodierung. Diese Eigenschaft ist möglicherweise nicht definiert.</td>
    </tr>
  </tbody>
</table>





---
## getCredentials
Gibt ein Credential Parameters-Objekt für jede Public Key Credential Source zurück, die in einem Virtual Authenticator gespeichert ist, unabhängig davon, ob sie mit Add Credential oder `navigator.credentials.create()` gespeichert wurden.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://www.w3.org/TR/webauthn-2/#sctn-automation-get-credentials).



##### Verwendung

```js
browser.getCredentials(authenticatorId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>authenticatorId</var></code></td>
      <td>String</td>
      <td>ID des Authentifikators</td>
    </tr>
  </tbody>
</table>


##### Gibt zurück

- **&lt;object[]&gt;**
            **<code><var>credentials</var></code>:** Gibt ein Array von Anmeldeinformationen zurück.    


---
## removeAllCredentials
Entfernt alle Public Key Credential Sources, die auf einem Virtual Authenticator gespeichert sind.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://www.w3.org/TR/webauthn-2/#sctn-automation-remove-all-credentials).



##### Verwendung

```js
browser.removeAllCredentials(authenticatorId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>authenticatorId</var></code></td>
      <td>String</td>
      <td>ID des Authentifikators</td>
    </tr>
  </tbody>
</table>





---
## removeCredential
Entfernt eine Public Key Credential Source, die auf einem Virtual Authenticator gespeichert ist.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://www.w3.org/TR/webauthn-2/#sctn-automation-remove-credential).



##### Verwendung

```js
browser.removeCredential(authenticatorId, credentialId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>authenticatorId</var></code></td>
      <td>String</td>
      <td>ID des Authentifikators</td>
    </tr>
    <tr>
      <td><code><var>credentialId</var></code></td>
      <td>String</td>
      <td>ID der Anmeldeinformation</td>
    </tr>
  </tbody>
</table>





---
## setUserVerified
Der Set User Verified Erweiterungsbefehl setzt die isUserVerified-Eigenschaft auf dem Virtual Authenticator.<br /><br />WebDriver Protocol-Befehl. Weitere Details finden Sie in der [offiziellen Protokolldokumentation](https://www.w3.org/TR/webauthn-2/#sctn-automation-set-user-verified).



##### Verwendung

```js
browser.setUserVerified(authenticatorId)
```


##### Parameter

<table>
  <thead>
    <tr>
      <th>Name</th><th>Typ</th><th>Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code><var>authenticatorId</var></code></td>
      <td>String</td>
      <td>ID des Authentifikators</td>
    </tr>
  </tbody>
</table>




