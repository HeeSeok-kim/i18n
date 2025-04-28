---
id: visual-reporter
title: Visuell Rapportör
---

Visuell Rapportör är en ny funktion som introduceras i `@wdio/visual-service`, från och med version [v5.2.0](https://github.com/webdriverio/visual-testing/releases/tag/%40wdio%2Fvisual-service%405.2.0). Denna rapportör låter användare visualisera JSON-diff-rapporter som genereras av Visual Testing-tjänsten och omvandla dem till ett lättläst format. Den hjälper team att bättre analysera och hantera resultaten från visuell testning genom att tillhandahålla ett grafiskt gränssnitt för att granska resultaten.

För att använda denna funktion, se till att du har den nödvändiga konfigurationen för att generera den nödvändiga `output.json`-filen. Detta dokument kommer att guida dig genom att konfigurera, köra och förstå Visuell Rapportör.

# Förutsättningar

Innan du använder Visuell Rapportör, se till att du har konfigurerat Visual Testing-tjänsten för att generera JSON-rapportfiler:

```ts
export const config = {
    // ...
    services: [
        [
            "visual",
            {
                createJsonReportFiles: true, // Genererar output.json-filen
            },
        ],
    ],
};
```

För mer detaljerade installationsinstruktioner, se WebdriverIO [Visual Testing-dokumentation](./) eller [`createJsonReportFiles`](./service-options.md#createjsonreportfiles-new)

# Installation

För att installera Visuell Rapportör, lägg till den som en utvecklingsberoende i ditt projekt med npm:

```bash
npm install @wdio/visual-reporter --save-dev
```

Detta säkerställer att de nödvändiga filerna är tillgängliga för att generera rapporter från dina visuella tester.

# Användning

## Bygga den visuella rapporten

När du har kört dina visuella tester och de har genererat `output.json`-filen kan du bygga den visuella rapporten med antingen CLI eller interaktiva uppmaningar.

### CLI-användning

Du kan använda CLI-kommandot för att generera rapporten genom att köra:

```bash
npx wdio-visual-reporter --jsonOutput=<path-to-output.json> --reportFolder=<path-to-store-report> --logLevel=debug
```

#### Obligatoriska alternativ:

-   `--jsonOutput`: Den relativa sökvägen till `output.json`-filen som genereras av Visual Testing-tjänsten. Denna sökväg är relativ till katalogen från vilken du kör kommandot.
-   `--reportFolder`: Den relativa katalogen där den genererade rapporten kommer att lagras. Denna sökväg är också relativ till katalogen från vilken du kör kommandot.

#### Valfria alternativ:

-   `--logLevel`: Ställ in till `debug` för att få detaljerad loggning, särskilt användbart för felsökning.

#### Exempel

```bash
npx wdio-visual-reporter --jsonOutput=/path/to/output.json --reportFolder=/path/to/report --logLevel=debug
```

Detta kommer att generera rapporten i den angivna mappen och ge feedback i konsolen. Till exempel:

```bash
✔ Build output copied successfully to "/path/to/report".
⠋ Prepare report assets...
✔ Successfully generated the report assets.
```

#### Visa rapporten

:::warning
Att öppna `path/to/report/index.html` direkt i en webbläsare **utan att serva den från en lokal server** kommer **INTE** att fungera.
:::

För att visa rapporten behöver du använda en enkel server som [sirv-cli](https://www.npmjs.com/package/sirv-cli). Du kan starta servern med följande kommando:

```bash
npx sirv-cli /path/to/report --single
```

Detta kommer att producera loggar som liknar exemplet nedan. Observera att portnumret kan variera:

```logs
  Your application is ready~! 🚀

  - Local:      http://localhost:8080
  - Network:    Add `--host` to expose

────────────────── LOGS ──────────────────
```

Du kan nu visa rapporten genom att öppna den angivna URL:en i din webbläsare.

### Använda interaktiva uppmaningar

Alternativt kan du köra följande kommando och svara på uppmaningarna för att generera rapporten:

```bash
npx @wdio/visual-reporter
```

Uppmaningarna guidar dig genom att tillhandahålla de nödvändiga sökvägarna och alternativen. I slutet kommer den interaktiva uppmaningen också att fråga om du vill starta en server för att visa rapporten. Om du väljer att starta servern kommer verktyget att starta en enkel server och visa en URL i loggarna. Du kan öppna denna URL i din webbläsare för att visa rapporten.

![Visual Reporter CLI](/img/visual/cli-screen-recording.gif)

![Visual Reporter](/img/visual/visual-reporter.gif)

#### Visa rapporten

:::warning
Att öppna `path/to/report/index.html` direkt i en webbläsare **utan att serva den från en lokal server** kommer **INTE** att fungera.
:::

Om du valde att **inte** starta servern via den interaktiva uppmaningen kan du fortfarande visa rapporten genom att köra följande kommando manuellt:

```bash
npx sirv-cli /path/to/report --single
```

Detta kommer att producera loggar som liknar exemplet nedan. Observera att portnumret kan variera:

```logs
  Your application is ready~! 🚀

  - Local:      http://localhost:8080
  - Network:    Add `--host` to expose

────────────────── LOGS ──────────────────
```

Du kan nu visa rapporten genom att öppna den angivna URL:en i din webbläsare.

# Rapportdemo

För att se ett exempel på hur rapporten ser ut, besök vår [GitHub Pages-demo](https://webdriverio.github.io/visual-testing/).

# Förstå den visuella rapporten

Visuell Rapportör ger en organiserad vy av dina visuella testresultat. För varje testkörning kommer du att kunna:

-   Enkelt navigera mellan testfall och se aggregerade resultat.
-   Granska metadata såsom testnamn, använda webbläsare och jämförelseresultat.
-   Visa diff-bilder som visar var visuella skillnader upptäcktes.

Denna visuella representation förenklar analysen av dina testresultat, vilket gör det lättare att identifiera och åtgärda visuella regressioner.

# CI-integrationer

Vi arbetar på att stödja olika CI-verktyg som Jenkins, GitHub Actions och så vidare. Om du vill hjälpa oss, kontakta oss på [Discord - Visual Testing](https://discord.com/channels/1097401827202445382/1186908940286574642).