---
id: seleniumgrid
title: Selenium Grid
---

Puoi utilizzare WebdriverIO con la tua istanza Selenium Grid esistente. Per collegare i tuoi test a Selenium Grid, devi solo aggiornare le opzioni nelle configurazioni del test runner.

Ecco un frammento di codice da un esempio di wdio.conf.ts.

```ts title=wdio.conf.ts
export const config: WebdriverIO.Config = {
    // ...
    protocol: 'https',
    hostname: 'yourseleniumgridhost.yourdomain.com',
    port: 443,
    path: '/wd/hub',
    // ...

}
```
Devi fornire i valori appropriati per il protocollo, l'hostname, la porta e il percorso in base alla tua configurazione di Selenium Grid.
Se stai eseguendo Selenium Grid sulla stessa macchina dei tuoi script di test, ecco alcune opzioni tipiche:

```ts title=wdio.conf.ts
export const config: WebdriverIO.Config = {
    // ...
    protocol: 'http',
    hostname: 'localhost',
    port: 4444,
    path: '/wd/hub',
    // ...

}
```

### Autenticazione di base con Selenium Grid protetto

È altamente consigliato proteggere il tuo Selenium Grid. Se hai un Selenium Grid protetto che richiede l'autenticazione, puoi passare le intestazioni di autenticazione tramite le opzioni.
Fai riferimento alla sezione [headers](https://webdriver.io/docs/configuration/#headers) nella documentazione per maggiori informazioni.

### Configurazioni di timeout con Selenium Grid dinamico

Quando si utilizza un Selenium Grid dinamico in cui i pod del browser vengono creati su richiesta, la creazione della sessione potrebbe affrontare un avvio a freddo. In questi casi, è consigliabile aumentare i timeout di creazione della sessione. Il valore predefinito nelle opzioni è di 120 secondi, ma puoi aumentarlo se la tua grid impiega più tempo per creare una nuova sessione.

```ts
connectionRetryTimeout: 180000,
```

### Configurazioni avanzate

Per configurazioni avanzate, fai riferimento al [file di configurazione](https://webdriver.io/docs/configurationfile) del Testrunner.

### Operazioni sui file con Selenium Grid

Quando esegui casi di test con un Selenium Grid remoto, il browser viene eseguito su una macchina remota, e devi prestare particolare attenzione ai casi di test che coinvolgono caricamenti e download di file.

### Download di file

Per i browser basati su Chromium, puoi fare riferimento alla documentazione [Download file](https://webdriver.io/docs/api/browser/downloadFile). Se i tuoi script di test devono leggere il contenuto di un file scaricato, devi scaricarlo dal nodo Selenium remoto alla macchina del test runner. Ecco un esempio di frammento di codice dalla configurazione di esempio `wdio.conf.ts` per il browser Chrome:

```ts title=wdio.conf.ts
export const config: WebdriverIO.Config = {
    // ...
    protocol: 'https',
    hostname: 'yourseleniumgridhost.yourdomain.com',
    port: 443,
    path: '/wd/hub',
    // ...
    capabilities: [{
        browserName: 'chrome',
        'se:downloadsEnabled': true
    }],
    //...
}
```

### Caricamento di file con Selenium Grid remoto

Per caricare un file su un'applicazione web nel browser remoto, devi prima caricare il file sulla grid remota. Puoi fare riferimento alla documentazione [uploadFile](https://webdriver.io/docs/api/browser/uploadFile) per i dettagli.

### Altre operazioni su file/grid

Ci sono alcune altre operazioni che puoi eseguire con Selenium Grid. Le istruzioni per Selenium Standalone dovrebbero funzionare bene anche con Selenium Grid. Fai riferimento alla documentazione [Selenium Standalone](https://webdriver.io/docs/api/selenium/) per le opzioni disponibili.

### Documentazione ufficiale di Selenium Grid

Per ulteriori informazioni su Selenium Grid, puoi fare riferimento alla [documentazione](https://www.selenium.dev/documentation/grid/) ufficiale di Selenium Grid.

Se desideri eseguire Selenium Grid in Docker, Docker compose o Kubernetes, fai riferimento al [repository GitHub](https://github.com/SeleniumHQ/docker-selenium) di Selenium-Docker.