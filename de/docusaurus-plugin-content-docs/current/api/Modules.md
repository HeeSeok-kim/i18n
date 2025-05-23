---
id: modules
title: Module
---

WebdriverIO veröffentlicht verschiedene Module auf NPM und anderen Registries, die Sie verwenden können, um Ihr eigenes Automatisierungs-Framework aufzubauen. Weitere Dokumentation zu WebdriverIO-Einrichtungstypen finden Sie [hier](/docs/setuptypes).

## `webdriver` und `devtools`

Die Protokoll-Pakete ([`webdriver`](https://www.npmjs.com/package/webdriver) und [`devtools`](https://www.npmjs.com/package/devtools)) stellen eine Klasse mit den folgenden statischen Funktionen zur Verfügung, die es Ihnen ermöglichen, Sitzungen zu initiieren:

#### `newSession(options, modifier, userPrototype, customCommandWrapper)`

Startet eine neue Sitzung mit spezifischen Capabilities. Basierend auf der Sitzungsantwort werden Befehle aus verschiedenen Protokollen bereitgestellt.

##### Paramaters

- `options`: [WebDriver Options](/docs/configuration#webdriver-options)
- `modifier`: Funktion, die es ermöglicht, die Client-Instanz zu modifizieren, bevor sie zurückgegeben wird
- `userPrototype`: Eigenschaftsobjekt, das es ermöglicht, den Instanzprototyp zu erweitern
- `customCommandWrapper`: Funktion, die es ermöglicht, Funktionalität um Funktionsaufrufe zu wickeln

##### Returns

- [Browser](/docs/api/browser) Objekt

##### Example

```js
const client = await WebDriver.newSession({
    capabilities: { browserName: 'chrome' }
})
```

#### `attachToSession(attachInstance, modifier, userPrototype, customCommandWrapper)`

Verbindet sich mit einer laufenden WebDriver- oder DevTools-Sitzung.

##### Paramaters

- `attachInstance`: Instanz, mit der eine Sitzung verbunden werden soll, oder zumindest ein Objekt mit einer Eigenschaft `sessionId` (z.B. `{ sessionId: 'xxx' }`)
- `modifier`: Funktion, die es ermöglicht, die Client-Instanz zu modifizieren, bevor sie zurückgegeben wird
- `userPrototype`: Eigenschaftsobjekt, das es ermöglicht, den Instanzprototyp zu erweitern
- `customCommandWrapper`: Funktion, die es ermöglicht, Funktionalität um Funktionsaufrufe zu wickeln

##### Returns

- [Browser](/docs/api/browser) Objekt

##### Example

```js
const client = await WebDriver.newSession({...})
const clonedClient = await WebDriver.attachToSession(client)
```

#### `reloadSession(instance)`

Lädt eine Sitzung mit der bereitgestellten Instanz neu.

##### Paramaters

- `instance`: Paketinstanz zum Neuladen

##### Example

```js
const client = await WebDriver.newSession({...})
await WebDriver.reloadSession(client)
```

## `webdriverio`

Ähnlich wie bei den Protokoll-Paketen (`webdriver` und `devtools`) können Sie auch die WebdriverIO-Paket-APIs verwenden, um Sitzungen zu verwalten. Die APIs können mit `import { remote, attach, multiremote } from 'webdriverio'` importiert werden und enthalten die folgende Funktionalität:

#### `remote(options, modifier)`

Startet eine WebdriverIO-Sitzung. Die Instanz enthält alle Befehle wie das Protokoll-Paket, aber mit zusätzlichen Funktionen höherer Ordnung, siehe [API-Dokumentation](/docs/api).

##### Paramaters

- `options`: [WebdriverIO Options](/docs/configuration#webdriverio)
- `modifier`: Funktion, die es ermöglicht, die Client-Instanz zu modifizieren, bevor sie zurückgegeben wird

##### Returns

- [Browser](/docs/api/browser) Objekt

##### Example

```js
import { remote } from 'webdriverio'

const browser = await remote({
    capabilities: { browserName: 'chrome' }
})
```

#### `attach(attachOptions)`

Verbindet sich mit einer laufenden WebdriverIO-Sitzung.

##### Paramaters

- `attachOptions`: Instanz, mit der eine Sitzung verbunden werden soll, oder zumindest ein Objekt mit einer Eigenschaft `sessionId` (z.B. `{ sessionId: 'xxx' }`)

##### Returns

- [Browser](/docs/api/browser) Objekt

##### Example

```js
import { remote, attach } from 'webdriverio'

const browser = await remote({...})
const newBrowser = await attach(browser)
```

#### `multiremote(multiremoteOptions)`

Initiiert eine Multiremote-Instanz, mit der Sie mehrere Sitzungen innerhalb einer einzigen Instanz steuern können. Schauen Sie sich unsere [Multiremote-Beispiele](https://github.com/webdriverio/webdriverio/tree/main/examples/multiremote) für konkrete Anwendungsfälle an.

##### Paramaters

- `multiremoteOptions`: ein Objekt mit Schlüsseln, die den Browsernamen und ihre [WebdriverIO Options](/docs/configuration#webdriverio) repräsentieren.

##### Returns

- [Browser](/docs/api/browser) Objekt

##### Example

```js
import { multiremote } from 'webdriverio'

const matrix = await multiremote({
    myChromeBrowser: {
        capabilities: { browserName: 'chrome' }
    },
    myFirefoxBrowser: {
        capabilities: { browserName: 'firefox' }
    }
})
await matrix.url('http://json.org')
await matrix.getInstance('browserA').url('https://google.com')

console.log(await matrix.getTitle())
// returns ['Google', 'JSON']
```

## `@wdio/cli`

Anstatt den `wdio`-Befehl aufzurufen, können Sie den Testrunner auch als Modul einbinden und in einer beliebigen Umgebung ausführen. Dafür müssen Sie das `@wdio/cli`-Paket als Modul einbinden, wie hier:

<Tabs
  defaultValue="esm"
  values={[
    {label: 'EcmaScript Modules', value: 'esm'},
    {label: 'CommonJS', value: 'cjs'}
  ]
}>
<TabItem value="esm">

```js
import Launcher from '@wdio/cli'
```

</TabItem>
<TabItem value="cjs">

```js
const Launcher = require('@wdio/cli').default
```

</TabItem>
</Tabs>

Danach erstellen Sie eine Instanz des Launchers und führen den Test aus.

#### `Launcher(configPath, opts)`

Der `Launcher`-Klassenkonstruktor erwartet die URL zur Konfigurationsdatei und ein `opts`-Objekt mit Einstellungen, die die in der Konfiguration überschreiben.

##### Paramaters

- `configPath`: Pfad zur auszuführenden `wdio.conf.js`
- `opts`: Argumente ([`<RunCommandArguments>`](https://github.com/webdriverio/webdriverio/blob/main/packages/wdio-cli/src/types.ts#L51-L77)) zum Überschreiben von Werten aus der Konfigurationsdatei

##### Example

```js
const wdio = new Launcher(
    '/path/to/my/wdio.conf.js',
    { spec: '/path/to/a/single/spec.e2e.js' }
)

wdio.run().then((exitCode) => {
    process.exit(exitCode)
}, (error) => {
    console.error('Launcher failed to start the test', error.stacktrace)
    process.exit(1)
})
```

Der `run`-Befehl gibt ein [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) zurück. Es wird aufgelöst, wenn Tests erfolgreich ausgeführt wurden oder fehlgeschlagen sind, und es wird abgelehnt, wenn der Launcher die Tests nicht starten konnte.

## `@wdio/browser-runner`

Wenn Sie Unit- oder Komponententests mit WebdriverIO's [Browser-Runner](/docs/runner#browser-runner) ausführen, können Sie Mocking-Hilfsprogramme für Ihre Tests importieren, z.B.:

```ts
import { fn, spyOn, mock, unmock } from '@wdio/browser-runner'
```

Die folgenden benannten Exports sind verfügbar:

#### `fn`

Mock-Funktion, weitere Informationen finden Sie in der offiziellen [Vitest-Dokumentation](https://vitest.dev/api/mock.html#mock-functions).

#### `spyOn`

Spy-Funktion, weitere Informationen finden Sie in der offiziellen [Vitest-Dokumentation](https://vitest.dev/api/mock.html#mock-functions).

#### `mock`

Methode zum Mocken von Dateien oder Abhängigkeitsmodulen.

##### Paramaters

- `moduleName`: entweder ein relativer Pfad zur zu mockenden Datei oder ein Modulname.
- `factory`: Funktion, die den gemockten Wert zurückgibt (optional)

##### Example

```js
mock('../src/constants.ts', () => ({
    SOME_DEFAULT: 'mocked out'
}))

mock('lodash', (origModuleFactory) => {
    const origModule = await origModuleFactory()
    return {
        ...origModule,
        pick: fn()
    }
})
```

#### `unmock`

Unmockt eine Abhängigkeit, die innerhalb des manuellen Mock-Verzeichnisses (`__mocks__`) definiert ist.

##### Paramaters

- `moduleName`: Name des Moduls, das unmocked werden soll.

##### Example

```js
unmock('lodash')
```