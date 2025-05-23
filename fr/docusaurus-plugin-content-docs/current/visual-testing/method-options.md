---
id: method-options
title: Options des méthodes
---

Les options des méthodes sont les options qui peuvent être définies par [méthode](./methods). Si l'option a la même clé qu'une option définie lors de l'instanciation du plugin, cette option de méthode remplacera la valeur de l'option du plugin.

## Options de sauvegarde

### `disableBlinkingCursor`

-   **Type:** `boolean`
-   **Obligatoire:** Non
-   **Par défaut:** `false`
-   **Pris en charge:** Web, Application hybride (Webview)

Activer/Désactiver le clignotement du curseur pour tous les éléments `input`, `textarea`, `[contenteditable]` dans l'application. Si défini à `true`, le curseur sera défini comme `transparent` avant de prendre une capture d'écran
et réinitialisé une fois terminé

### `disableCSSAnimation`

-   **Type:** `boolean`
-   **Obligatoire:** Non
-   **Par défaut:** `false`
-   **Pris en charge:** Web, Application hybride (Webview)

Activer/Désactiver toutes les animations CSS dans l'application. Si défini à `true`, toutes les animations seront désactivées avant de prendre une capture d'écran
et réinitialisées une fois terminé

### `enableLegacyScreenshotMethod`

-   **Type:** `boolean`
-   **Obligatoire:** Non
-   **Par défaut:** `false`
-   **Pris en charge:** Web, Application hybride (Webview)

Utilisez cette option pour revenir à l'ancienne méthode de capture d'écran basée sur le protocole W3C-WebDriver. Cela peut être utile si vos tests dépendent d'images de référence existantes ou si vous travaillez dans des environnements qui ne prennent pas entièrement en charge les captures d'écran basées sur BiDi.
Notez que l'activation de cette option peut produire des captures d'écran avec une résolution ou une qualité légèrement différente.

### `enableLayoutTesting`

-   **Type:** `boolean`
-   **Obligatoire:** Non
-   **Par défaut:** `false`
-   **Utilisé avec:** Toutes les [méthodes](./methods)
-   **Pris en charge:** Web

Cette option masquera tout le texte sur une page afin que seule la mise en page soit utilisée pour la comparaison. Le masquage sera effectué en ajoutant le style `'color': 'transparent !important'` à __chaque__ élément.

Pour la sortie, voir [Sortie de test](./test-output#enablelayouttesting)

:::info
En utilisant ce drapeau, chaque élément contenant du texte (donc pas seulement `p, h1, h2, h3, h4, h5, h6, span, a, li`, mais aussi `div|button|..`) recevra cette propriété. Il n'y a __pas__ d'option pour personnaliser cela.
:::

### `hideScrollBars`

-   **Type:** `boolean`
-   **Obligatoire:** Non
-   **Par défaut:** `true`
-   **Utilisé avec:** Toutes les [méthodes](./methods)
-   **Pris en charge:** Web, Application hybride (Webview)

Masquer les barres de défilement dans l'application. Si défini à true, toutes les barres de défilement seront désactivées avant de prendre une capture d'écran. La valeur par défaut est `true` pour éviter des problèmes supplémentaires.

### `hideElements`

-   **Type:** `array`
-   **Obligatoire:** non
-   **Utilisé avec:** Toutes les [méthodes](./methods)
-   **Pris en charge:** Web, Application hybride (Webview), Application native

Cette méthode peut masquer un ou plusieurs éléments en ajoutant la propriété `visibility: hidden` en fournissant un tableau d'éléments.

### `removeElements`

-   **Type:** `array`
-   **Obligatoire:** non
-   **Utilisé avec:** Toutes les [méthodes](./methods)
-   **Pris en charge:** Web, Application hybride (Webview), Application native

Cette méthode peut _supprimer_ un ou plusieurs éléments en ajoutant la propriété `display: none` en fournissant un tableau d'éléments.

### `resizeDimensions`

-   **Type:** `object`
-   **Obligatoire:** non
-   **Par défaut:** `{ top: 0, right: 0, bottom: 0, left: 0}`
-   **Utilisé avec:** Uniquement pour [`saveElement`](./methods#saveelement) ou [`checkElement`](./methods#checkelement)
-   **Pris en charge:** Web, Application hybride (Webview), Application native

Un objet qui doit contenir un nombre de pixels `top`, `right`, `bottom` et `left` qui doivent agrandir la découpe de l'élément.

### `userBasedFullPageScreenshot`

* **Type:** `boolean`
* **Obligatoire:** Non
* **Par défaut:** `false`
* **Pris en charge:** Web, Application hybride (Webview)

Lorsqu'il est défini à `true`, cette option active la **stratégie de défilement et d'assemblage** pour capturer des captures d'écran de pages entières.
Au lieu d'utiliser les capacités natives de capture d'écran du navigateur, elle fait défiler manuellement la page et assemble plusieurs captures d'écran.
Cette méthode est particulièrement utile pour les pages avec du **contenu chargé paresseusement** ou des mises en page complexes qui nécessitent un défilement pour un rendu complet.

### `fullPageScrollTimeout`

-   **Type:** `number`
-   **Obligatoire:** Non
-   **Par défaut:** `1500`
-   **Utilisé avec:** Uniquement pour [`saveFullPageScreen`](./methods#savefullpagescreen) ou [`saveTabbablePage`](./methods#savetabbablepage)
-   **Pris en charge:** Web

Le délai d'attente en millisecondes après un défilement. Cela peut aider à identifier les pages avec chargement paresseux.

> **REMARQUE:** Cela ne fonctionne que lorsque `userBasedFullPageScreenshot` est défini à `true`

### `hideAfterFirstScroll`

-   **Type:** `array`
-   **Obligatoire:** non
-   **Utilisé avec:** Uniquement pour [`saveFullPageScreen`](./methods#savefullpagescreen) ou [`saveTabbablePage`](./methods#savetabbablepage)
-   **Pris en charge:** Web

Cette méthode masquera un ou plusieurs éléments en ajoutant la propriété `visibility: hidden` en fournissant un tableau d'éléments.
Cela sera utile lorsqu'une page contient par exemple des éléments fixes qui défileront avec la page si celle-ci est déroulée mais qui donneront un effet gênant lors de la prise d'une capture d'écran de la page entière.

> **REMARQUE:** Cela ne fonctionne que lorsque `userBasedFullPageScreenshot` est défini à `true`

### `waitForFontsLoaded`

-   **Type:** `boolean`
-   **Obligatoire:** Non
-   **Par défaut:** `true`
-   **Utilisé avec:** Toutes les [méthodes](./methods)
-   **Pris en charge:** Web, Application hybride (Webview)

Les polices, y compris les polices tierces, peuvent être chargées de manière synchrone ou asynchrone. Le chargement asynchrone signifie que les polices peuvent se charger après que WebdriverIO a déterminé qu'une page est complètement chargée. Pour éviter les problèmes de rendu des polices, ce module attendra par défaut que toutes les polices soient chargées avant de prendre une capture d'écran.

## Options de comparaison (Check)

Les options de comparaison sont des options qui influencent la façon dont la comparaison, par [ResembleJS](https://github.com/Huddle/Resemble.js), est exécutée.

:::info REMARQUE

-   Toutes les options des [Options de sauvegarde](#options-de-sauvegarde) peuvent être utilisées pour les méthodes de comparaison
-   Toutes les options de comparaison peuvent être utilisées pendant l'instantiation du service __ou__ pour chaque méthode de vérification individuelle. Si une option de méthode a la même clé qu'une option définie lors de l'instantiation du service, alors l'option de comparaison de la méthode remplacera la valeur de l'option de comparaison du service.
- Toutes les options peuvent être utilisées pour :
    - Web
    - Application hybride
    - Application native

:::

### `ignoreAlpha`

-   **Type:** `boolean`
-   **Par défaut:** `false`
-   **Obligatoire:** non

Comparer les images et ignorer l'alpha.

### `blockOutSideBar`

-   **Type:** `boolean`
-   **Par défaut:** `true`
-   **Obligatoire:** non
-   **Remarque:** _Peut être utilisé uniquement pour `checkScreen()`. Ceci est **uniquement pour iPad**_

Bloquer automatiquement la barre latérale pour les iPads en mode paysage pendant les comparaisons. Cela évite les échecs sur le composant natif onglet/privé/favoris.

### `blockOutStatusBar`

-   **Type:** `boolean`
-   **Par défaut:** `true`
-   **Obligatoire:** non
-   **Remarque:** _Ceci est **uniquement pour Mobile**_

Bloquer automatiquement la barre d'état et la barre d'adresse pendant les comparaisons. Cela évite les échecs liés à l'heure, au Wi-Fi ou à l'état de la batterie.

### `blockOutToolBar`

-   **Type:** `boolean`
-   **Par défaut:** `true`
-   **Obligatoire:** non
-   **Remarque:** _Ceci est **uniquement pour Mobile**_

Bloquer automatiquement la barre d'outils.

### `ignoreAntialiasing`

-   **Type:** `boolean`
-   **Par défaut:** `false`
-   **Obligatoire:** non

Comparer les images et ignorer l'anti-aliasing.

### `ignoreColors`

-   **Type:** `boolean`
-   **Par défaut:** `false`
-   **Obligatoire:** non

Même si les images sont en couleur, la comparaison comparera 2 images en noir/blanc

### `ignoreLess`

-   **Type:** `boolean`
-   **Par défaut:** `false`
-   **Obligatoire:** non

Comparer les images avec `red = 16, green = 16, blue = 16, alpha = 16, minBrightness=16, maxBrightness=240`

### `ignoreNothing`

-   **Type:** `boolean`
-   **Par défaut:** `false`
-   **Obligatoire:** non

Comparer les images avec `red = 0, green = 0, blue = 0, alpha = 0, minBrightness=0, maxBrightness=255`

### `rawMisMatchPercentage`

-   **Type:** `boolean`
-   **Par défaut:** `false`
-   **Obligatoire:** non

Si vrai, le pourcentage de retour sera comme `0.12345678`, par défaut c'est `0.12`

### `returnAllCompareData`

-   **Type:** `boolean`
-   **Par défaut:** `false`
-   **Obligatoire:** non

Cela retournera toutes les données de comparaison, pas seulement le pourcentage de non-correspondance

### `saveAboveTolerance`

-   **Type:** `number`
-   **Par défaut:** `0`
-   **Obligatoire:** non

Valeur admissible de `misMatchPercentage` qui empêche la sauvegarde des images avec des différences

### `largeImageThreshold`

-   **Type:** `number`
-   **Par défaut:** `0`
-   **Obligatoire:** non

La comparaison de grandes images peut entraîner des problèmes de performance.
Lorsqu'on fournit un nombre pour le nombre de pixels ici (supérieur à 0), l'algorithme de comparaison ignore les pixels lorsque la largeur ou la hauteur de l'image est supérieure à `largeImageThreshold` pixels.

### `scaleImagesToSameSize`

-   **Type:** `boolean`
-   **Par défaut:** `false`
-   **Obligatoire:** non

Redimensionne 2 images à la même taille avant l'exécution de la comparaison. Il est fortement recommandé d'activer `ignoreAntialiasing` et `ignoreAlpha`

## Options de dossier

Le dossier de référence et les dossiers de captures d'écran (réel, différence) sont des options qui peuvent être définies lors de l'instanciation du plugin ou de la méthode. Pour définir les options de dossier sur une méthode particulière, passez les options de dossier à l'objet d'options de méthodes. Cela peut être utilisé pour :

- Web
- Application hybride
- Application native

```ts
import path from 'node:path'

const methodOptions = {
    actualFolder: path.join(process.cwd(), 'customActual'),
    baselineFolder: path.join(process.cwd(), 'customBaseline'),
    diffFolder: path.join(process.cwd(), 'customDiff'),
}

// Vous pouvez utiliser ceci pour toutes les méthodes
await expect(
    await browser.checkFullPageScreen("checkFullPage", methodOptions)
).toEqual(0)
```

### `actualFolder`

-   **Type:** `string`
-   **Obligatoire:** non

Dossier pour la capture qui a été prise dans le test.

### `baselineFolder`

-   **Type:** `string`
-   **Obligatoire:** non

Dossier pour l'image de référence qui est utilisée pour la comparaison.

### `diffFolder`

-   **Type:** `string`
-   **Obligatoire:** non

Dossier pour la différence d'image rendue par ResembleJS.