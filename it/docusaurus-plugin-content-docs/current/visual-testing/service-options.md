---
id: service-options
title: Opzioni del Servizio
---

Le opzioni del servizio sono le opzioni che possono essere impostate quando il servizio viene istanziato e saranno utilizzate per ogni chiamata di metodo.

```js
// wdio.conf.(js|ts)
export const config = {
    // ...
    // =====
    // Setup
    // =====
    services: [
        [
            "visual",
            {
                // The options
            },
        ],
    ],
    // ...
};
```

## Opzioni Predefinite

### `addressBarShadowPadding`

-   **Tipo:** `number`
-   **Obbligatorio:** No
-   **Predefinito:** `6`
-   **Supportato:** Web

Il padding che deve essere aggiunto alla barra degli indirizzi su iOS e Android per effettuare un ritaglio adeguato del viewport.

### `autoElementScroll`

-   **Tipo:** `boolean`
-   **Obbligatorio:** No
-   **Predefinito:** `true`
-   **Supportato:** Web, App Ibrida (Webview)

Questa opzione ti permette di disabilitare lo scorrimento automatico dell'elemento nella visualizzazione quando viene creato uno screenshot di un elemento.

### `addIOSBezelCorners`

-   **Tipo:** `boolean`
-   **Obbligatorio:** No
-   **Predefinito:** `false`
-   **Supportato:** Web, App Ibrida (Webview), App Nativa

Aggiunge angoli della cornice e notch/dynamic island allo screenshot per i dispositivi iOS.

:::info NOTA
Questo può essere fatto solo quando il nome del dispositivo **PUÒ** essere determinato automaticamente e corrisponde al seguente elenco di nomi di dispositivi normalizzati. La normalizzazione sarà eseguita da questo modulo.
**iPhone:**

-   iPhone X: `iphonex`
-   iPhone XS: `iphonexs`
-   iPhone XS Max: `iphonexsmax`
-   iPhone XR: `iphonexr`
-   iPhone 11: `iphone11`
-   iPhone 11 Pro: `iphone11pro`
-   iPhone 11 Pro Max: `iphone11promax`
-   iPhone 12: `iphone12`
-   iPhone 12 Mini: `iphone12mini`
-   iPhone 12 Pro: `iphone12pro`
-   iPhone 12 Pro Max: `iphone12promax`
-   iPhone 13: `iphone13`
-   iPhone 13 Mini: `iphone13mini`
-   iPhone 13 Pro: `iphone13pro`
-   iPhone 13 Pro Max: `iphone13promax`
-   iPhone 14: `iphone14`
-   iPhone 14 Plus: `iphone14plus`
-   iPhone 14 Pro: `iphone14pro`
-   iPhone 14 Pro Max: `iphone14promax`
    **iPads:**
-   iPad Mini 6th Generation: `ipadmini`
-   iPad Air 4th Generation: `ipadair`
-   iPad Air 5th Generation: `ipadair`
-   iPad Pro (11-inch) 1st Generation: `ipadpro11`
-   iPad Pro (11-inch) 2nd Generation: `ipadpro11`
-   iPad Pro (11-inch) 3rd Generation: `ipadpro11`
-   iPad Pro (12.9-inch) 3rd Generation: `ipadpro129`
-   iPad Pro (12.9-inch) 4th Generation: `ipadpro129`
-   iPad Pro (12.9-inch) 5th Generation: `ipadpro129`

:::

### `autoSaveBaseline`

-   **Tipo:** `boolean`
-   **Obbligatorio:** No
-   **Predefinito:** `true`
-   **Supportato:** Web, App Ibrida (Webview), App Nativa

Se durante il confronto non viene trovata un'immagine di base, l'immagine viene automaticamente copiata nella cartella di base.

### `baselineFolder`

-   **Tipo:** `string|()=> string`
-   **Obbligatorio:** No
-   **Predefinito:** `.path/to/testfile/__snapshots__/`
-   **Supportato:** Web, App Ibrida (Webview), App Nativa

La directory che conterrà tutte le immagini di base utilizzate durante il confronto. Se non impostato, verrà utilizzato il valore predefinito, che memorizzerà i file in una cartella `__snapshots__/` accanto alla spec che esegue i test visivi. Può essere utilizzata anche una funzione che restituisce una `string` per impostare il valore di `baselineFolder`:

```js
{
    baselineFolder: path.join(process.cwd(), 'foo', 'bar', 'baseline')
},
// OPPURE
{
    baselineFolder: () => {
        // Fai un po' di magia qui
        return path.join(process.cwd(), 'foo', 'bar', 'baseline');
    }
}
```

### `clearRuntimeFolder`

-   **Tipo:** `boolean`
-   **Obbligatorio:** No
-   **Predefinito:** `false`
-   **Supportato:** Web, App Ibrida (Webview), App Nativa

Elimina la cartella di runtime (`actual` & `diff`) all'inizializzazione

:::info NOTA
Questo funzionerà solo quando lo [`screenshotPath`](#screenshotpath) è impostato attraverso le opzioni del plugin, e **NON FUNZIONERÀ** quando imposti le cartelle nei metodi
:::

### `createJsonReportFiles` **(NUOVO)**

-   **Tipo:** `boolean`
-   **Obbligatorio:** No
-   **Predefinito:** `false`

Ora hai la possibilità di esportare i risultati del confronto in un file di report JSON. Fornendo l'opzione `createJsonReportFiles: true`, ogni immagine confrontata creerà un report memorizzato nella cartella `actual`, accanto a ciascun risultato dell'immagine `actual`. L'output sarà simile a questo:

```json
{
    "parent": "check methods",
    "test": "should fail comparing with a baseline",
    "tag": "examplePageFail",
    "instanceData": {
        "browser": {
            "name": "chrome-headless-shell",
            "version": "126.0.6478.183"
        },
        "platform": {
            "name": "mac",
            "version": "not-known"
        }
    },
    "commandName": "checkScreen",
    "boundingBoxes": {
        "diffBoundingBoxes": [
            {
                "left": 1088,
                "top": 717,
                "right": 1186,
                "bottom": 730
            }
            //....
        ],
        "ignoredBoxes": [
            {
                "left": 159,
                "top": 652,
                "right": 356,
                "bottom": 703
            }
            //...
        ]
    },
    "fileData": {
        "actualFilePath": "/Users/wdio/visual-testing/.tmp/actual/desktop_chrome-headless-shellexamplePageFail-local-chrome-latest-1366x768.png",
        "baselineFilePath": "/Users/wdio/visual-testing/localBaseline/desktop_chrome-headless-shellexamplePageFail-local-chrome-latest-1366x768.png",
        "diffFilePath": "/Users/wdio/visual-testing/.tmp/diff/desktop_chrome-headless-shell/examplePageFail-local-chrome-latest-1366x768png",
        "fileName": "examplePageFail-local-chrome-latest-1366x768.png",
        "size": {
            "actual": {
                "height": 768,
                "width": 1366
            },
            "baseline": {
                "height": 768,
                "width": 1366
            },
            "diff": {
                "height": 768,
                "width": 1366
            }
        }
    },
    "misMatchPercentage": "12.90",
    "rawMisMatchPercentage": 12.900729014153246
}
```

Quando tutti i test sono eseguiti, verrà generato un nuovo file JSON con la raccolta dei confronti che si troverà nella root della tua cartella `actual`. I dati sono raggruppati per:

-   `describe` per Jasmine/Mocha o `Feature` per CucumberJS
-   `it` per Jasmine/Mocha o `Scenario` per CucumberJS
    e poi ordinati per:
-   `commandName`, che sono i nomi dei metodi di confronto utilizzati per confrontare le immagini
-   `instanceData`, prima browser, poi dispositivo, quindi piattaforma
    avrà questo aspetto

```json
[
    {
        "description": "check methods",
        "data": [
            {
                "test": "should fail comparing with a baseline",
                "data": [
                    {
                        "tag": "examplePageFail",
                        "instanceData": {},
                        "commandName": "checkScreen",
                        "framework": "mocha",
                        "boundingBoxes": {
                            "diffBoundingBoxes": [],
                            "ignoredBoxes": []
                        },
                        "fileData": {},
                        "misMatchPercentage": "14.34",
                        "rawMisMatchPercentage": 14.335403703025868
                    },
                    {
                        "tag": "exampleElementFail",
                        "instanceData": {},
                        "commandName": "checkElement",
                        "framework": "mocha",
                        "boundingBoxes": {
                            "diffBoundingBoxes": [],
                            "ignoredBoxes": []
                        },
                        "fileData": {},
                        "misMatchPercentage": "1.34",
                        "rawMisMatchPercentage": 1.335403703025868
                    }
                ]
            }
        ]
    }
]
```

I dati del report ti daranno l'opportunità di costruire il tuo report visivo senza dover fare tutta la magia e la raccolta dati da solo.

:::info NOTA
Devi usare la versione `@wdio/visual-testing` 5.2.0 o superiore
:::

### `disableBlinkingCursor`

-   **Tipo:** `boolean`
-   **Obbligatorio:** No
-   **Predefinito:** `false`
-   **Supportato:** Web, App Ibrida (Webview)

Abilita/disabilita il "lampeggiamento" del cursore in tutti gli elementi `input`, `textarea`, `[contenteditable]` nell'applicazione. Se impostato su `true`, il cursore verrà impostato su `transparent` prima di scattare uno screenshot
e ripristinato al termine

### `disableCSSAnimation`

-   **Tipo:** `boolean`
-   **Obbligatorio:** No
-   **Predefinito:** `false`
-   **Supportato:** Web, App Ibrida (Webview)

Abilita/disabilita tutte le animazioni CSS nell'applicazione. Se impostato su `true`, tutte le animazioni verranno disabilitate prima di scattare uno screenshot
e ripristinate al termine

### `enableLayoutTesting`

-   **Tipo:** `boolean`
-   **Obbligatorio:** No
-   **Predefinito:** `false`
-   **Supportato:** Web

Questo nasconderà tutto il testo in una pagina in modo che solo il layout venga utilizzato per il confronto. La nascita avverrà aggiungendo lo stile `'color': 'transparent !important'` a **ciascun** elemento.

Per l'output vedere [Test Output](/docs/visual-testing/test-output#enablelayouttesting)

:::info
Utilizzando questo flag, ogni elemento che contiene testo (quindi non solo `p, h1, h2, h3, h4, h5, h6, span, a, li`, ma anche `div|button|..`) riceverà questa proprietà. Non esiste **nessuna** opzione per personalizzarlo.
:::

### `formatImageName`

-   **Tipo:** `string`
-   **Obbligatorio:** No
-   **Predefinito:** `{tag}-{browserName}-{width}x{height}-dpr-{dpr}`
-   **Supportato:** Web, App Ibrida (Webview), App Nativa

Il nome delle immagini salvate può essere personalizzato passando il parametro `formatImageName` con una stringa di formato come:

```sh
{tag}-{browserName}-{width}x{height}-dpr-{dpr}
```

Le seguenti variabili possono essere passate per formattare la stringa e verranno automaticamente lette dalle capacità dell'istanza.
Se non possono essere determinate, verranno utilizzati i valori predefiniti.

-   `browserName`: Il nome del browser nelle capacità fornite
-   `browserVersion`: La versione del browser fornita nelle capacità
-   `deviceName`: Il nome del dispositivo dalle capacità
-   `dpr`: La densità di pixel del dispositivo
-   `height`: L'altezza dello schermo
-   `logName`: Il logName dalle capacità
-   `mobile`: Questo aggiungerà `_app`, o il nome del browser dopo il `deviceName` per distinguere gli screenshot dell'app dagli screenshot del browser
-   `platformName`: Il nome della piattaforma nelle capacità fornite
-   `platformVersion`: La versione della piattaforma fornita nelle capacità
-   `tag`: Il tag fornito nei metodi che viene chiamato
-   `width`: La larghezza dello schermo

:::info

Non puoi fornire percorsi/cartelle personalizzati nel `formatImageName`. Se desideri modificare il percorso, controlla la modifica delle seguenti opzioni:

- [`baselineFolder`](/docs/visual-testing/service-options#baselinefolder)
- [`screenshotPath`](/docs/visual-testing/service-options#screenshotpath)
- [`folderOptions`](/docs/visual-testing/method-options#folder-options) per metodo

:::

### `fullPageScrollTimeout`

-   **Tipo:** `number`
-   **Obbligatorio:** No
-   **Predefinito:** `1500`
-   **Supportato:** Web

Il timeout in millisecondi da attendere dopo uno scorrimento. Questo potrebbe aiutare a identificare pagine con caricamento lazy.

:::info

Questo funzionerà solo quando l'opzione del servizio/metodo `userBasedFullPageScreenshot` è impostata su `true`, vedi anche [`userBasedFullPageScreenshot`](/docs/visual-testing/service-options#userbasedbullpagescreenshot)

:::

### `hideScrollBars`

-   **Tipo:** `boolean`
-   **Obbligatorio:** No
-   **Predefinito:** `true`
-   **Supportato:** Web, App Ibrida (Webview)

Nasconde le barre di scorrimento nell'applicazione. Se impostato su true, tutte le barre di scorrimento verranno disabilitate prima di scattare uno screenshot. Questo è impostato su `true` per prevenire problemi extra.

### `logLevel`

-   **Tipo:** `string`
-   **Obbligatorio:** No
-   **Predefinito:** `info`
-   **Supportato:** Web, App Ibrida (Webview), App Nativa

Aggiunge log extra, le opzioni sono `debug | info | warn | silent`

Gli errori vengono sempre registrati nella console.

### `savePerInstance`

-   **Tipo:** `boolean`
-   **Predefinito:** `false`
-   **Obbligatorio:** no
-   **Supportato:** Web, App Ibrida (Webview), App Nativa

Salva le immagini per istanza in una cartella separata, quindi ad esempio tutti gli screenshot di Chrome verranno salvati in una cartella Chrome come `desktop_chrome`.

### `screenshotPath`

-   **Tipo:** `string | () => string`
-   **Predefinito:** `.tmp/`
-   **Obbligatorio:** no
-   **Supportato:** Web, App Ibrida (Webview), App Nativa

La directory che conterrà tutti gli screenshot attuali/differenti. Se non impostato, verrà utilizzato il valore predefinito. Una funzione che
restituisce una stringa può anche essere utilizzata per impostare il valore screenshotPath:

```js
{
    screenshotPath: path.join(process.cwd(), 'foo', 'bar', 'screenshotPath')
},
// OPPURE
{
    screenshotPath: () => {
        // Fai un po' di magia qui
        return path.join(process.cwd(), 'foo', 'bar', 'screenshotPath');
    }
}
```

### `toolBarShadowPadding`

-   **Tipo:** `number`
-   **Obbligatorio:** No
-   **Predefinito:** `6` per Android e `15` per iOS (`6` di default e `9` verranno aggiunti automaticamente per la possibile barra home su iPhone con notch o iPad che hanno una barra home)
-   **Supportato:** Web

Il padding che deve essere aggiunto alla barra degli strumenti su iOS e Android per effettuare un ritaglio adeguato del viewport.

### `userBasedFullPageScreenshot`

-   **Tipo:** `boolean`
-   **Obbligatorio:** No
-   **Predefinito:** `false`
-   **Supportato:** Web, App Ibrida (Webview) **Introdotto in visual-service@7.0.0**

Per impostazione predefinita, gli screenshot a pagina intera sul web desktop vengono acquisiti utilizzando il protocollo WebDriver BiDi, che consente screenshot veloci, stabili e coerenti senza scorrimento.
Quando userBasedFullPageScreenshot è impostato su true, il processo di screenshot simula un utente reale: scorrendo la pagina, acquisendo screenshot di dimensioni del viewport e unendoli insieme. Questo metodo è utile per pagine con contenuto a caricamento lazy o rendering dinamico che dipende dalla posizione di scorrimento.

Usa questa opzione se la tua pagina si basa sul caricamento del contenuto durante lo scorrimento o se vuoi preservare il comportamento dei metodi di screenshot più vecchi.

### `waitForFontsLoaded`

-   **Tipo:** `boolean`
-   **Obbligatorio:** No
-   **Predefinito:** `true`
-   **Supportato:** Web, App Ibrida (Webview)

I font, inclusi i font di terze parti, possono essere caricati in modo sincrono o asincrono. Il caricamento asincrono significa che i font potrebbero caricarsi dopo che WebdriverIO ha determinato che una pagina è stata completamente caricata. Per prevenire problemi di rendering dei font, questo modulo, per impostazione predefinita, attenderà che tutti i font siano caricati prima di scattare uno screenshot.

## Opzioni Tabbable

:::info NOTA

Questo modulo supporta anche il disegno del modo in cui un utente userebbe la sua tastiera per _spostarsi con il tab_ attraverso il sito web disegnando linee e punti da un elemento tabbable all'altro.<br/>
Il lavoro è ispirato al post del blog di [Viv Richards](https://github.com/vivrichards600) su ["AUTOMATING PAGE TABABILITY (IS THAT A WORD?) WITH VISUAL TESTING"](https://vivrichards.co.uk/accessibility/automating-page-tab-flows-using-visual-testing-and-javascript).<br/>
Il modo in cui vengono selezionati gli elementi tabbable si basa sul modulo [tabbable](https://github.com/davidtheclark/tabbable). Se ci sono problemi riguardanti il tabbing, controlla il [README.md](https://github.com/davidtheclark/tabbable/blob/master/README.md) e in particolare la sezione [Maggiori dettagli](https://github.com/davidtheclark/tabbable/blob/master/README.md#more-details).

:::

### `tabbableOptions`

-   **Tipo:** `object`
-   **Obbligatorio:** No
-   **Predefinito:** Vedi [qui](https://github.com/webdriverio/visual-testing/blob/b7d66afadc88f03f09646c28806a687d2ae46000/packages/webdriver-image-comparison/src/helpers/options.ts#L6-L68) per tutti i valori predefiniti
-   **Supportato:** Web

Le opzioni che possono essere modificate per le linee e i punti se usi i metodi `{save|check}Tabbable`. Le opzioni sono spiegate di seguito.

#### `tabbableOptions.circle`

-   **Tipo:** `object`
-   **Obbligatorio:** No
-   **Predefinito:** Vedi [qui](https://github.com/webdriverio/visual-testing/blob/b7d66afadc88f03f09646c28806a687d2ae46000/packages/webdriver-image-comparison/src/helpers/options.ts#L6-L68) per tutti i valori predefiniti
-   **Supportato:** Web

Le opzioni per cambiare il cerchio.

##### `tabbableOptions.circle.backgroundColor`

-   **Tipo:** `string`
-   **Obbligatorio:** No
-   **Predefinito:** Vedi [qui](https://github.com/webdriverio/visual-testing/blob/b7d66afadc88f03f09646c28806a687d2ae46000/packages/webdriver-image-comparison/src/helpers/options.ts#L6-L68) per tutti i valori predefiniti
-   **Supportato:** Web

Il colore di sfondo del cerchio.

##### `tabbableOptions.circle.borderColor`

-   **Tipo:** `string`
-   **Obbligatorio:** No
-   **Predefinito:** Vedi [qui](https://github.com/webdriverio/visual-testing/blob/b7d66afadc88f03f09646c28806a687d2ae46000/packages/webdriver-image-comparison/src/helpers/options.ts#L6-L68) per tutti i valori predefiniti
-   **Supportato:** Web

Il colore del bordo del cerchio.

##### `tabbableOptions.circle.borderWidth`

-   **Tipo:** `number`
-   **Obbligatorio:** No
-   **Predefinito:** Vedi [qui](https://github.com/webdriverio/visual-testing/blob/b7d66afadc88f03f09646c28806a687d2ae46000/packages/webdriver-image-comparison/src/helpers/options.ts#L6-L68) per tutti i valori predefiniti
-   **Supportato:** Web

La larghezza del bordo del cerchio.

##### `tabbableOptions.circle.fontColor`

-   **Tipo:** `string`
-   **Obbligatorio:** No
-   **Predefinito:** Vedi [qui](https://github.com/webdriverio/visual-testing/blob/b7d66afadc88f03f09646c28806a687d2ae46000/packages/webdriver-image-comparison/src/helpers/options.ts#L6-L68) per tutti i valori predefiniti
-   **Supportato:** Web

Il colore del font del testo nel cerchio. Questo verrà mostrato solo se [`showNumber`](./#tabbableoptionscircleshownumber) è impostato su `true`.

##### `tabbableOptions.circle.fontFamily`

-   **Tipo:** `string`
-   **Obbligatorio:** No
-   **Predefinito:** Vedi [qui](https://github.com/webdriverio/visual-testing/blob/b7d66afadc88f03f09646c28806a687d2ae46000/packages/webdriver-image-comparison/src/helpers/options.ts#L6-L68) per tutti i valori predefiniti
-   **Supportato:** Web

La famiglia del font del testo nel cerchio. Questo verrà mostrato solo se [`showNumber`](./#tabbableoptionscircleshownumber) è impostato su `true`.

Assicurati di impostare font supportati dai browser.

##### `tabbableOptions.circle.fontSize`

-   **Tipo:** `number`
-   **Obbligatorio:** No
-   **Predefinito:** Vedi [qui](https://github.com/webdriverio/visual-testing/blob/b7d66afadc88f03f09646c28806a687d2ae46000/packages/webdriver-image-comparison/src/helpers/options.ts#L6-L68) per tutti i valori predefiniti
-   **Supportato:** Web

La dimensione del font del testo nel cerchio. Questo verrà mostrato solo se [`showNumber`](./#tabbableoptionscircleshownumber) è impostato su `true`.

##### `tabbableOptions.circle.size`

-   **Tipo:** `number`
-   **Obbligatorio:** No
-   **Predefinito:** Vedi [qui](https://github.com/webdriverio/visual-testing/blob/b7d66afadc88f03f09646c28806a687d2ae46000/packages/webdriver-image-comparison/src/helpers/options.ts#L6-L68) per tutti i valori predefiniti
-   **Supportato:** Web

La dimensione del cerchio.

##### `tabbableOptions.circle.showNumber`

-   **Tipo:** `showNumber`
-   **Obbligatorio:** No
-   **Predefinito:** Vedi [qui](https://github.com/webdriverio/visual-testing/blob/b7d66afadc88f03f09646c28806a687d2ae46000/packages/webdriver-image-comparison/src/helpers/options.ts#L6-L68) per tutti i valori predefiniti
-   **Supportato:** Web

Mostra il numero della sequenza di tab nel cerchio.

#### `tabbableOptions.line`

-   **Tipo:** `object`
-   **Obbligatorio:** No
-   **Predefinito:** Vedi [qui](https://github.com/webdriverio/visual-testing/blob/b7d66afadc88f03f09646c28806a687d2ae46000/packages/webdriver-image-comparison/src/helpers/options.ts#L6-L68) per tutti i valori predefiniti
-   **Supportato:** Web

Le opzioni per cambiare la linea.

##### `tabbableOptions.line.color`

-   **Tipo:** `string`
-   **Obbligatorio:** No
-   **Predefinito:** Vedi [qui](https://github.com/webdriverio/visual-testing/blob/b7d66afadc88f03f09646c28806a687d2ae46000/packages/webdriver-image-comparison/src/helpers/options.ts#L6-L68) per tutti i valori predefiniti
-   **Supportato:** Web

Il colore della linea.

##### `tabbableOptions.line.width`

-   **Tipo:** `number`
-   **Obbligatorio:** No
-   **Predefinito:** Vedi [qui](https://github.com/webdriverio/visual-testing/blob/b7d66afadc88f03f09646c28806a687d2ae46000/packages/webdriver-image-comparison/src/helpers/options.ts#L6-L68) per tutti i valori predefiniti
-   **Supportato:** Web

La larghezza della linea.

## Opzioni di confronto

### `compareOptions`

-   **Tipo:** `object`
-   **Obbligatorio:** No
-   **Predefinito:** Vedi [qui](https://github.com/webdriverio/visual-testing/blob/6a988808c9adc58f58c5a66cd74296ae5c1ad6dc/packages/webdriver-image-comparison/src/helpers/options.ts#L46-L60) per tutti i valori predefiniti
-   **Supportato:** Web, App Ibrida (Webview), App Nativa (Vedi [Opzioni di confronto del metodo](./method-options#compare-check-options) per maggiori informazioni)

Le opzioni di confronto possono anche essere impostate come opzioni di servizio, sono descritte nelle [Opzioni di confronto del metodo](/docs/visual-testing/method-options#compare-check-options)