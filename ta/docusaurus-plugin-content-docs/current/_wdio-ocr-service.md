---
id: wdio-ocr-service
title: OCR சோதனை சேவை
custom_edit_url: https://github.com/webdriverio/visual-testing/edit/main/README.md
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

> @wdio/ocr-service என்பது ஒரு மூன்றாம் தரப்பு தொகுப்பு, மேலும் தகவலுக்கு தயவுசெய்து [GitHub](https://github.com/webdriverio/visual-testing) | [npm](https://www.npmjs.com/package/@wdio/ocr-service) ஐப் பார்க்கவும்

WebdriverIO உடன் காட்சி சோதனை பற்றிய ஆவணங்களுக்கு, தயவுசெய்து [docs](https://webdriver.io/docs/visual-testing) ஐப் பார்க்கவும். இந்த திட்டம் WebdriverIO உடன் காட்சி சோதனைகளை இயக்குவதற்கான அனைத்து தொடர்புடைய தொகுதிகளையும் கொண்டுள்ளது. `./packages` அடைவில் நீங்கள் பின்வருவனவற்றைக் காணலாம்:

-   `@wdio/visual-testing`: காட்சி சோதனையை ஒருங்கிணைப்பதற்கான WebdriverIO சேவை
-   `webdriver-image-comparison`: WebDriver நெறிமுறையை ஆதரிக்கும் வெவ்வேறு NodeJS சோதனை தானியக்க கட்டமைப்புகளுக்குப் பயன்படுத்தக்கூடிய படம் ஒப்பிடும் தொகுதி

## Storybook ரன்னர் (BETA)

<details>
  <summary>Storybook ரன்னர் BETA பற்றிய மேலும் ஆவணங்களைக் காண கிளிக் செய்யவும்</summary>

> Storybook ரன்னர் இன்னும் BETA நிலையில் உள்ளது, ஆவணங்கள் பின்னர் [WebdriverIO](https://webdriver.io/docs/visual-testing) ஆவணப்பக்கங்களுக்கு மாற்றப்படும்.

இந்த தொகுதி இப்போது ஒரு புதிய விஷுவல் ரன்னருடன் Storybook ஐ ஆதரிக்கிறது. இந்த ரன்னர் உள்ளூர்/தொலைநிலை storybook நிகழ்நிலையை தானாகவே ஸ்கேன் செய்து, ஒவ்வொரு கூறுகளின் உறுப்பு ஸ்கிரீன்ஷாட்களை உருவாக்கும். இதை

```ts
export const config: WebdriverIO.Config = {
    // ...
    services: ["visual"],
    // ....
};
```

உங்கள் `services` இல் சேர்ப்பதன் மூலமும், `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook` கட்டளை வரியில் இயக்குவதன் மூலமும் செய்யலாம்.
இது இயல்புநிலை உலாவியாக headless முறையில் Chrome ஐப் பயன்படுத்தும்.

> [!NOTE]
>
> -   விஷுவல் டெஸ்டிங் விருப்பங்களில் பெரும்பாலானவை Storybook ரன்னருக்கும் செயல்படும், [WebdriverIO](https://webdriver.io/docs/visual-testing) ஆவணத்தைப் பார்க்கவும்.
> -   Storybook ரன்னர் உங்கள் அனைத்து திறன்களையும் மேலெழுதும், மேலும் இது ஆதரிக்கும் உலாவிகளில் மட்டுமே இயங்கும், [`--browsers`](#browsers) ஐப் பார்க்கவும்.
> -   Storybook ரன்னர் Multiremote திறன்களைப் பயன்படுத்தும் இருக்கும் உள்ளமைவை ஆதரிக்காது மற்றும் பிழையை வீசும்.
> -   Storybook ரன்னர் மொபைல் வெப் அல்ல, டெஸ்க்டாப் வெப் மட்டுமே ஆதரிக்கிறது.

### Storybook ரன்னர் சேவை விருப்பங்கள்

சேவை விருப்பங்களை இவ்வாறு வழங்கலாம்

```ts
export const config: WebdriverIO.Config  = {
    // ...
    services: [
      [
        'visual',
        {
            // சில இயல்புநிலை விருப்பங்கள்
            baselineFolder: join(process.cwd(), './__snapshots__/'),
            debug: true,
            // storybook விருப்பங்கள், விளக்கத்திற்கு cli விருப்பங்களைப் பார்க்கவும்
            storybook: {
                additionalSearchParams: new URLSearchParams({foo: 'bar', abc: 'def'}),
                clip: false,
                clipSelector: ''#some-id,
                numShards: 4,
                // `skipStories` ஒரு சரம் ('example-button--secondary'),
                // ஒரு வரிசை (['example-button--secondary', 'example-button--small'])
                // அல்லது ஒரு regex ஆக இருக்கலாம், இது ஒரு சரமாக வழங்கப்பட வேண்டும் ("/.*button.*/gm")
                skipStories: ['example-button--secondary', 'example-button--small'],
                url: 'https://www.bbc.co.uk/iplayer/storybook/',
                version: 6,
                // விருப்பம் - அடிப்படை பாதைகளை மேலெழுத அனுமதிக்கிறது. இயல்பாக இது வகை மற்றும் கூறுபடி அடிப்படை பாதையை குழுவாக்கும் (எ.கா. forms/input/baseline.png)
                getStoriesBaselinePath: (category, component) => `path__${category}__${component}`,
            },
        },
      ],
    ],
    // ....
}
```

### Storybook ரன்னர் CLI விருப்பங்கள்

#### `--additionalSearchParams`

-   **வகை:** `string`
-   **கட்டாயம்:** இல்லை
-   **இயல்புநிலை:** ''
-   **எடுத்துக்காட்டு:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --additionalSearchParams="foo=bar&abc=def"`

இது Storybook URL க்கு கூடுதல் தேடல் அளவுருக்களைச் சேர்க்கும்.
மேலும் தகவலுக்கு [URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) ஆவணத்தைப் பார்க்கவும். சரம் செல்லுபடியாகும் URLSearchParams சரமாக இருக்க வேண்டும்.

> [!NOTE]
> `&` ஐ கட்டளை பிரிப்பானாக விளக்கப்படுவதைத் தடுக்க இரட்டை மேற்கோள்கள் தேவை.
> எடுத்துக்காட்டாக, `--additionalSearchParams="foo=bar&abc=def"` உடன் அது பின்வரும் Storybook URL ஐ கதைகள் சோதனைக்கு உருவாக்கும்: `http://storybook.url/iframe.html?id=story-id&foo=bar&abc=def`.

#### `--browsers`

-   **வகை:** `string`
-   **கட்டாயம்:** இல்லை
-   **இயல்புநிலை:** `chrome`, நீங்கள் `chrome|firefox|edge|safari` இலிருந்து தேர்ந்தெடுக்கலாம்
-   **எடுத்துக்காட்டு:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --browsers=chrome,firefox,edge,safari`
-   **குறிப்பு:** CLI மூலம் மட்டுமே கிடைக்கும்

கூறு ஸ்கிரீன்ஷாட்களை எடுக்க வழங்கப்பட்ட உலாவிகளைப் பயன்படுத்தும்

> [!NOTE]
> நீங்கள் இயக்க விரும்பும் உலாவிகளை உங்கள் உள்ளூர் இயந்திரத்தில் நிறுவியுள்ளீர்கள் என்பதை உறுதிப்படுத்திக் கொள்ளுங்கள்

#### `--clip`

-   **வகை:** `boolean`
-   **கட்டாயம்:** இல்லை
-   **இயல்புநிலை:** `true`
-   **எடுத்துக்காட்டு:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --clip=false`

முடக்கப்பட்டிருக்கும்போது இது ஒரு viewport ஸ்கிரீன்ஷாட்டை உருவாக்கும். இயக்கப்பட்டிருக்கும்போது, இது [`--clipSelector`](#clipselector) அடிப்படையில் உறுப்பு ஸ்கிரீன்ஷாட்களை உருவாக்கும், இது கூறு ஸ்கிரீன்ஷாட்டைச் சுற்றியுள்ள வெள்ளை இடைவெளியின் அளவைக் குறைத்து, ஸ்கிரீன்ஷாட் அளவைக் குறைக்கும்.

#### `--clipSelector`

-   **வகை:** `string`
-   **கட்டாயம்:** இல்லை
-   **இயல்புநிலை:** Storybook V7 க்கு `#storybook-root > :first-child` மற்றும் Storybook V6 க்கு `#root > :first-child:not(script):not(style)`, [`--version`](#version) ஐயும் பார்க்கவும்
-   **எடுத்துக்காட்டு:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --clipSelector="#some-id"`

இது பின்வருவனவற்றிற்குப் பயன்படுத்தப்படும் தேர்வியாகும்:

-   ஸ்கிரீன்ஷாட் எடுக்க வேண்டிய உறுப்பைத் தேர்ந்தெடுக்க
-   ஸ்கிரீன்ஷாட் எடுக்கப்படுவதற்கு முன் தெரியக்கூடிய உறுப்புக்காக காத்திருக்க

#### `--devices`

-   **வகை:** `string`
-   **கட்டாயம்:** இல்லை
-   **இயல்புநிலை:** நீங்கள் [`deviceDescriptors.ts`](https://github.com/webdriverio/visual-testing/blob/main/./packages/service/src/storybook/deviceDescriptors.ts) இலிருந்து தேர்ந்தெடுக்கலாம்
-   **எடுத்துக்காட்டு:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --devices="iPhone 14 Pro Max","Pixel 3 XL"`
-   **குறிப்பு:** CLI மூலம் மட்டுமே கிடைக்கும்

கூறு ஸ்கிரீன்ஷாட்களை எடுக்க [`deviceDescriptors.ts`](https://github.com/webdriverio/visual-testing/blob/main/./packages/service/src/storybook/deviceDescriptors.ts) உடன் பொருந்தும் வழங்கப்பட்ட சாதனங்களைப் பயன்படுத்தும்

> [!NOTE]
>
> -   ஒரு சாதன கட்டமைப்பை நீங்கள் தவறவிட்டால், [அம்ச கோரிக்கை](https://github.com/webdriverio/visual-testing/issues/new?assignees=&labels=&projects=&template=--feature-request.md) ஐச் சமர்ப்பிக்கத் தயங்காதீர்கள்
> -   இது Chrome இல் மட்டுமே வேலை செய்யும்:
>     -   நீங்கள் `--devices` வழங்கினால், அனைத்து Chrome நிகழ்வுகளும் **மொபைல் எமுலேஷன்** முறையில் இயங்கும்
>     -   நீங்கள் Chrome தவிர மற்ற உலாவிகளையும் வழங்கினால், எ.கா. `--devices --browsers=firefox,safari,edge` இது தானாகவே Chrome ஐ மொபைல் எமுலேஷன் முறையில் சேர்க்கும்
> -   Storybook ரன்னர் இயல்பாக உறுப்பு ஸ்னாப்ஷாட்களை உருவாக்கும், முழு மொபைல் எமுலேட்டட் ஸ்கிரீன்ஷாட்டைக் காண விரும்பினால், கட்டளை வரி மூலம் `--clip=false` ஐ வழங்கவும்
> -   கோப்பு பெயர் எடுத்துக்காட்டாக `__snapshots__/example/button/desktop_chrome/example-button--large-local-chrome-iPhone-14-Pro-Max-430x932-dpr-3.png` போல் இருக்கும்
> -   **[SRC:](https://chromedriver.chromium.org/mobile-emulation#h.p_ID_167)** மொபைல் எமுலேஷனைப் பயன்படுத்தி டெஸ்க்டாப்பில் மொபைல் இணையதளத்தை சோதிப்பது பயனுள்ளதாக இருக்கலாம், ஆனால் சோதனையாளர்கள் பின்வரும் பல நுட்பமான வேறுபாடுகள் இருப்பதை அறிந்திருக்க வேண்டும்:
>     -   முற்றிலும் வித்தியாசமான GPU, இது பெரிய செயல்திறன் மாற்றங்களுக்கு வழிவகுக்கும்;
>     -   மொபைல் UI முன்னிலைப்படுத்தப்படவில்லை (குறிப்பாக, url பட்டியை மறைப்பது பக்க உயரத்தை பாதிக்கிறது);
>     -   தெளிவுபடுத்தும் பாப்அப் (சில தொடு இலக்குகளில் ஒன்றைத் தேர்ந்தெடுக்கும் இடம்) ஆதரிக்கப்படவில்லை;
>     -   பல ஹார்டுவேர் API கள் (எடுத்துக்காட்டாக, orientationchange நிகழ்வு) கிடைக்கவில்லை.

#### `--headless`

-   **வகை:** `boolean`
-   **கட்டாயம்:** இல்லை
-   **இயல்புநிலை:** `true`
-   **எடுத்துக்காட்டு:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --headless=false`
-   **குறிப்பு:** CLI மூலம் மட்டுமே கிடைக்கும்

இயல்பாக இது சோதனைகளை (உலாவி ஆதரிக்கும்போது) headless முறையில் இயக்கும் அல்லது முடக்கப்படலாம்

#### `--numShards`

-   **வகை:** `number`
-   **கட்டாயம்:** இல்லை
-   **இயல்புநிலை:** `true`
-   **எடுத்துக்காட்டு:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --numShards=10`

கதைகளை இயக்க பயன்படுத்தப்படும் இணை நிகழ்வுகளின் எண்ணிக்கை இதுவாகும். இது உங்கள் `wdio.conf`-கோப்பில் உள்ள `maxInstances` ஆல் வரம்பிடப்படும்.

> [!IMPORTANT]
> `headless`-முறையில் இயக்கும்போது, வள கட்டுப்பாடுகள் காரணமாக நிலைத்தன்மையைத் தடுக்க எண்ணிக்கையை 20 க்கு மேல் அதிகரிக்க வேண்டாம்

#### `--skipStories`

-   **வகை:** `string|regex`
-   **கட்டாயம்:** இல்லை
-   **இயல்புநிலை:** null
-   **எடுத்துக்காட்டு:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --skipStories="/.*button.*/gm"`

இது இருக்கலாம்:

-   ஒரு சரம் (`example-button--secondary,example-button--small`)
-   அல்லது ஒரு regex (`"/.*button.*/gm"`)

சில கதைகளைத் தவிர்க்க. கதையின் URL இல் காணப்படும் `id` ஐப் பயன்படுத்தவும். எடுத்துக்காட்டாக, இந்த URL இல் `http://localhost:6006/?path=/story/example-page--logged-out` உள்ள `id` என்பது `example-page--logged-out` ஆகும்

#### `--url`

-   **வகை:** `string`
-   **கட்டாயம்:** இல்லை
-   **இயல்புநிலை:** `http://127.0.0.1:6006`
-   **எடுத்துக்காட்டு:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --url="https://example.com"`

உங்கள் Storybook நிகழ்வு ஹோஸ்ட் செய்யப்பட்டுள்ள URL.

#### `--version`

-   **வகை:** `number`
-   **கட்டாயம்:** இல்லை
-   **இயல்புநிலை:** 7
-   **எடுத்துக்காட்டு:** `npx wdio tests/configs/wdio.local.desktop.storybook.conf.ts --storybook --version=6`

இது Storybook இன் பதிப்பாகும், இயல்பாக `7` ஆகும். V6 [`clipSelector`](#clipselector) பயன்படுத்த வேண்டுமா என்பதை அறிய இது தேவை.

### Storybook இடைவினை சோதனை

Storybook இடைவினை சோதனை உங்கள் கூறுடன் தொடர்புகொள்ள WDIO கட்டளைகளுடன் தனிப்பயன் ஸ்கிரிப்ட்களை உருவாக்குவதன் மூலம் ஒரு குறிப்பிட்ட நிலையில் ஒரு கூறை அமைக்க அனுமதிக்கிறது. எடுத்துக்காட்டாக, கீழே உள்ள குறியீட்டு துணுக்கைப் பார்க்கவும்:

```ts
import { browser, expect } from "@wdio/globals";

describe("Storybook Interaction", () => {
    it("should create screenshots for the logged in state when it logs out", async () => {
        const componentId = "example-page--logged-in";
        await browser.waitForStorybookComponentToBeLoaded({ id: componentId });

        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-in-state`
        );
        await $("button=Log out").click();
        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-out-state`
        );
    });

    it("should create screenshots for the logged out state when it logs in", async () => {
        const componentId = "example-page--logged-out";
        await browser.waitForStorybookComponentToBeLoaded({ id: componentId });

        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-out-state`
        );
        await $("button=Log in").click();
        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-in-state`
        );
    });
});
```

இரண்டு வெவ்வேறு கூறுகளில் இரண்டு சோதனைகள் செயல்படுத்தப்படுகின்றன. ஒவ்வொரு சோதனையும் முதலில் ஒரு நிலையை அமைத்து பின்னர் ஸ்கிரீன்ஷாட் எடுக்கிறது. ஒரு புதிய தனிப்பயன் கட்டளை அறிமுகப்படுத்தப்பட்டுள்ளதையும் நீங்கள் கவனிப்பீர்கள், அதை [இங்கு](#new-custom-command) காணலாம்.

மேலே உள்ள விவரக்குறிப்பு கோப்பை ஒரு கோப்புறையில் சேமித்து பின்வரும் கட்டளையுடன் கட்டளை வரிக்குச் சேர்க்கலாம்:

```sh
pnpm run test.local.desktop.storybook.localhost -- --spec='tests/specs/storybook-interaction/*.ts'
```

Storybook ரன்னர் முதலில் தானாகவே உங்கள் Storybook நிகழ்வை ஸ்கேன் செய்து, பின்னர் ஒப்பிட வேண்டிய கதைகளுக்கு உங்கள் சோதனைகளைச் சேர்க்கும். இடைவினை சோதனைக்கு நீங்கள் பயன்படுத்தும் கூறுகள் இருமுறை ஒப்பிடப்படுவதை விரும்பவில்லை என்றால், ஸ்கேனில் இருந்து "இயல்புநிலை" கதைகளை அகற்ற [`--skipStories`](#--skipstories) வடிப்பானை வழங்குவதன் மூலம் வடிகட்டியை சேர்க்கலாம். இது இவ்வாறு இருக்கும்:

```sh
pnpm run test.local.desktop.storybook.localhost -- --skipStories="/example-page.*/gm" --spec='tests/specs/storybook-interaction/*.ts'
```

### புதிய தனிப்பயன் கட்டளை

`browser.waitForStorybookComponentToBeLoaded({ id: 'componentId' })` என்ற புதிய தனிப்பயன் கட்டளை `browser/driver`-பொருளுக்குச் சேர்க்கப்படும், இது தானாகவே கூறுகளை ஏற்றி, அது முடிவடையும் வரை காத்திருக்கும், எனவே நீங்கள் `browser.url('url.com')` முறையைப் பயன்படுத்த வேண்டியதில்லை. இதை இவ்வாறு பயன்படுத்தலாம்

```ts
import { browser, expect } from "@wdio/globals";

describe("Storybook Interaction", () => {
    it("should create screenshots for the logged in state when it logs out", async () => {
        const componentId = "example-page--logged-in";
        await browser.waitForStorybookComponentToBeLoaded({ id: componentId });

        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-in-state`
        );
        await $("button=Log out").click();
        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-out-state`
        );
    });

    it("should create screenshots for the logged out state when it logs in", async () => {
        const componentId = "example-page--logged-out";
        await browser.waitForStorybookComponentToBeLoaded({ id: componentId });

        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-out-state`
        );
        await $("button=Log in").click();
        await expect($("header")).toMatchElementSnapshot(
            `${componentId}-logged-in-state`
        );
    });
});
```

விருப்பங்கள்:

#### `additionalSearchParams`

-   **வகை:** [`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)
-   **கட்டாயம்:** இல்லை
-   **இயல்புநிலை:** `new URLSearchParams()`
-   **எடுத்துக்காட்டு:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    additionalSearchParams: new URLSearchParams({ foo: "bar", abc: "def" }),
    id: "componentId",
});
```

இது Storybook URL க்கு கூடுதல் தேடல் அளவுருக்களைச் சேர்க்கும், மேலே உள்ள எடுத்துக்காட்டில் URL `http://storybook.url/iframe.html?id=story-id&foo=bar&abc=def` ஆக இருக்கும்.
மேலும் தகவலுக்கு [URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) ஆவணத்தைப் பார்க்கவும்.

#### `clipSelector`

-   **வகை:** `string`
-   **கட்டாயம்:** இல்லை
-   **இயல்புநிலை:** Storybook V7 க்கு `#storybook-root > :first-child` மற்றும் Storybook V6 க்கு `#root > :first-child:not(script):not(style)`
-   **எடுத்துக்காட்டு:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    clipSelector: "#your-selector",
    id: "componentId",
});
```

இது பின்வருவனவற்றிற்குப் பயன்படுத்தப்படும் தேர்வியாகும்:

-   ஸ்கிரீன்ஷாட் எடுக்க வேண்டிய உறுப்பைத் தேர்ந்தெடுக்க
-   ஸ்கிரீன்ஷாட் எடுக்கப்படுவதற்கு முன் தெரியக்கூடிய உறுப்புக்காக காத்திருக்க

#### `id`

-   **வகை:** `string`
-   **கட்டாயம்:** ஆம்
-   **எடுத்துக்காட்டு:**

```ts
await browser.waitForStorybookComponentToBeLoaded({ '#your-selector', id: 'componentId' })
```

கதையின் URL இல் காணப்படும் `id` ஐப் பயன்படுத்தவும். எடுத்துக்காட்டாக, இந்த URL இல் `http://localhost:6006/?path=/story/example-page--logged-out` உள்ள `id` என்பது `example-page--logged-out` ஆகும்

#### `timeout`

-   **வகை:** `number`
-   **கட்டாயம்:** இல்லை
-   **இயல்புநிலை:** 1100 மில்லி வினாடிகள்
-   **எடுத்துக்காட்டு:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    id: "componentId",
    timeout: 20000,
});
```

பக்கத்தில் ஏற்றிய பிறகு ஒரு கூறு தெரியக்கூடியதாக இருக்க நாங்கள் காத்திருக்க விரும்பும் அதிகபட்ச நேரம்

#### `url`

-   **வகை:** `string`
-   **கட்டாயம்:** இல்லை
-   **இயல்புநிலை:** `http://127.0.0.1:6006`
-   **எடுத்துக்காட்டு:**

```ts
await browser.waitForStorybookComponentToBeLoaded({
    id: "componentId",
    url: "https://your.url",
});
```

உங்கள் Storybook நிகழ்வு ஹோஸ்ட் செய்யப்பட்டுள்ள URL.

</details>

## பங்களிப்பு

### தொகுப்புகளை புதுப்பித்தல்

நீங்கள் எளிய CLI கருவியுடன் தொகுப்புகளைப் புதுப்பிக்கலாம். அனைத்து சார்புகளையும் நிறுவியுள்ளீர்கள் என்பதை உறுதிப்படுத்திக் கொள்ளுங்கள், பின்னர் இயக்கலாம்

```sh
pnpm update.packages
```

இது பின்வரும் கேள்விகளை உங்களிடம் கேட்கும் ஒரு CLI ஐத் தூண்டும்

```logs
==========================
🤖 Package update Wizard 🧙
==========================

? Which version target would you like to update to? (Minor|Latest)
? Do you want to update the package.json files? (Y/n)
? Do you want to remove all "node_modules" and reinstall dependencies? (Y/n)
? Would you like reinstall the dependencies? (Y/n)
```

இது பின்வரும் பதிவுகளை உருவாக்கும்

<details>
    <summary>பதிவுகளின் எடுத்துக்காட்டைக் காண திறக்கவும்</summary>
    
```logs
==========================
🤖 Package update Wizard 🧙
==========================

? Which version target would you like to update to? Minor
? Do you want to update the package.json files? yes
Updating root 'package.json' for minor updates...
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/package.json
[====================] 38/38 100%

@typescript-eslint/eslint-plugin ^8.7.0 → ^8.8.0
@typescript-eslint/parser ^8.7.0 → ^8.8.0
@typescript-eslint/utils ^8.7.0 → ^8.8.0
@vitest/coverage-v8 ^2.1.1 → ^2.1.2
vitest ^2.1.1 → ^2.1.2

Run pnpm install to install new versions.
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing/packages/ocr-service...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/packages/ocr-service/package.json
[====================] 11/11 100%

All dependencies match the minor package versions :)
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing/packages/visual-reporter...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/packages/visual-reporter/package.json
[====================] 11/11 100%

eslint-config-next 14.2.13 → 14.2.14
next 14.2.13 → 14.2.14

Run pnpm install to install new versions.
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing/packages/visual-service...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/packages/visual-service/package.json
[====================] 5/5 100%

All dependencies match the minor package versions :)
Updating packages for minor updates in /Users/wswebcreation/Git/wdio/visual-testing/packages/webdriver-image-comparison...
Using pnpm
Upgrading /Users/wswebcreation/Git/wdio/visual-testing/packages/webdriver-image-comparison/package.json
[====================] 8/8 100%

All dependencies match the minor package versions :)
? Do you want to remove all "node_modules" and reinstall dependencies? yes
Removing root dependencies in /Users/wswebcreation/Git/wdio/visual-testing...
Removing dependencies in ocr-service...
Removing dependencies in visual-reporter...
Removing dependencies in visual-service...
Removing dependencies in webdriver-image-comparison...
? Would you like reinstall the dependencies? yes
Installing dependencies in /Users/wswebcreation/Git/wdio/visual-testing...

> @wdio/visual-testing-monorepo@ pnpm.install.workaround /Users/wswebcreation/Git/wdio/visual-testing
> pnpm install --shamefully-hoist

Scope: all 5 workspace projects
Lockfile is up to date, resolution step is skipped
Packages: +1274
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Progress: resolved 1274, reused 1265, downloaded 0, added 1274, done

dependencies:

-   @wdio/ocr-service 2.0.0 <- packages/ocr-service
-   @wdio/visual-service 6.0.0 <- packages/visual-service

devDependencies:

-   @changesets/cli 2.27.8
-   @inquirer/prompts 5.5.0
-   @tsconfig/node20 20.1.4
-   @types/eslint 9.6.1
-   @types/jsdom 21.1.7
-   @types/node 20.16.4
-   @types/react 18.3.5
-   @types/react-dom 18.3.0
-   @types/xml2js 0.4.14
-   @typescript-eslint/eslint-plugin 8.8.0
-   @typescript-eslint/parser 8.8.0
-   @typescript-eslint/utils 8.8.0
-   @vitest/coverage-v8 2.1.2
-   @wdio/appium-service 9.1.2
-   @wdio/cli 9.1.2
-   @wdio/globals 9.1.2
-   @wdio/local-runner 9.1.2
-   @wdio/mocha-framework 9.1.2
-   @wdio/sauce-service 9.1.2
-   @wdio/shared-store-service 9.1.2
-   @wdio/spec-reporter 9.1.2
-   @wdio/types 9.1.2
-   eslint 9.11.1
-   eslint-plugin-import 2.30.0
-   eslint-plugin-unicorn 55.0.0
-   eslint-plugin-wdio 9.0.8
-   husky 9.1.6
-   jsdom 25.0.1
-   pnpm-run-all2 6.2.3
-   release-it 17.6.0
-   rimraf 6.0.1
-   saucelabs 8.0.0
-   ts-node 10.9.2
-   typescript 5.6.2
-   vitest 2.1.2
-   webdriverio 9.1.2

. prepare$ husky
└─ Done in 204ms
Done in 9.5s
All packages updated!

````

</details>

### கேள்விகள்

இந்த திட்டத்திற்குப் பங்களிப்பதில் உங்களுக்கு ஏதேனும் கேள்விகள் அல்லது சிக்கல்கள் இருந்தால், எங்கள் [Discord](https://discord.webdriver.io) சேவையகத்தில் சேரவும். `🙏-contributing` சேனலில் பங்களிப்பாளர்களைப் பிடிக்கவும்.

### சிக்கல்கள்

உங்களுக்கு கேள்விகள், பிழைகள் அல்லது அம்ச கோரிக்கைகள் இருந்தால், தயவுசெய்து ஒரு சிக்கலைத் தாக்கல் செய்யவும். ஒரு சிக்கலைச் சமர்ப்பிப்பதற்கு முன், நகல்களைக் குறைக்க உதவ சிக்கல் காப்பகத்தைத் தேடவும், மற்றும் [FAQ](https://webdriver.io/docs/visual-testing/faq/) ஐப் படிக்கவும்.

அங்கு கண்டுபிடிக்க முடியவில்லை என்றால், நீங்கள் சமர்ப்பிக்கக்கூடிய ஒரு சிக்கலை சமர்ப்பிக்கலாம்:

-   🐛**பிழை அறிக்கை**: நாங்கள் மேம்படுத்த உதவ ஒரு அறிக்கையை உருவாக்கவும்
-   📖**ஆவணங்கள்**: முன்னேற்றங்களை பரிந்துரைக்கவும் அல்லது காணாமல் போன/தெளிவற்ற ஆவணங்களை புகாரளிக்கவும்.
-   💡**அம்ச கோரிக்கை**: இந்த தொகுதிக்கான யோசனையை பரிந்துரைக்கவும்.
-   💬**கேள்வி**: கேள்விகளைக் கேளுங்கள்.

### மேம்பாட்டு வழிமுறை

இந்த திட்டத்திற்கான PR ஐ உருவாக்கி பங்களிக்கத் தொடங்க இந்த படிப்படியான வழிகாட்டியைப் பின்பற்றவும்:

-   திட்டத்தை ஃபோர்க் செய்யவும்.
-   திட்டத்தை உங்கள் கணினியில் எங்காவது குளோன் செய்யவும்

    ```sh
    $ git clone https://github.com/webdriverio/visual-testing.git
    ```

-   அடைவுக்குச் சென்று திட்டத்தை அமைக்கவும்

    ```sh
    $ cd visual-testing
    $ corepack enable
    $ pnpm pnpm.install.workaround
    ```

-   குறியீட்டைத் தானாகவே transpile செய்யும் வாட்ச் முறையை இயக்கவும்

    ```sh
    $ pnpm watch
    ```

    திட்டத்தை உருவாக்க, இயக்கவும்:

    ```sh
    $ pnpm build
    ```

-   உங்கள் மாற்றங்கள் எந்தச் சோதனைகளையும் உடைக்காது என்பதை உறுதிப்படுத்தவும், இயக்கவும்:

    ```sh
    $ pnpm test
    ```

இந்த திட்டம் changelog களை மற்றும் வெளியீடுகளை தானாகவே உருவாக்க [changesets](https://github.com/changesets/changesets) ஐப் பயன்படுத்துகிறது.

### சோதனை

தொகுதியைச் சோதிக்க பல சோதனைகளை செயல்படுத்த வேண்டும். PR சேர்க்கும்போது அனைத்து சோதனைகளும் குறைந்தபட்சம் உள்ளூர் சோதனைகளைத் தேர்ச்சி பெற வேண்டும். ஒவ்வொரு PR ம் Sauce Labs க்கு எதிராக தானாகவே சோதிக்கப்படுகிறது, [எங்கள் GitHub Actions பைப்லைன்](https://github.com/webdriverio/visual-testing/actions/workflows/tests.yml) ஐப் பார்க்கவும். PR ஐ அங்கீகரிப்பதற்கு முன், முக்கிய பங்களிப்பாளர்கள் PR ஐ எமுலேட்டர்கள்/சிமுலேட்டர்கள் / உண்மையான சாதனங்களுக்கு எதிராக சோதிப்பார்கள்.

#### உள்ளூர் சோதனை

முதலில், ஒரு உள்ளூர் அடிப்படை உருவாக்கப்பட வேண்டும். இதை செய்யலாம்:

```sh
// வெப்டிரைவர் நெறிமுறையுடன்
$ pnpm run test.local.init
````

இந்த கட்டளை அனைத்து அடிப்படை படங்களையும் வைத்திருக்கும் `localBaseline` என்ற கோப்புறையை உருவாக்கும்.

பின்னர் இயக்கவும்:

```sh
// வெப்டிரைவர் நெறிமுறையுடன்
pnpm run test.local.desktop
```

இது உள்ளூர் இயந்திரத்தில் Chrome இல் அனைத்து சோதனைகளையும் இயக்கும்.

#### உள்ளூர் Storybook ரன்னர் சோதனை (பீட்டா)

முதலில், ஒரு உள்ளூர் அடிப்படை உருவாக்கப்பட வேண்டும். இதை செய்யலாம்:

```sh
pnpm run test.local.desktop.storybook
```

இது https://govuk-react.github.io/govuk-react/ இல் உள்ள டெமோ Storybook ரெப்போவிற்கு எதிராக headless முறையில் Chrome உடன் Storybook சோதனைகளை இயக்கும்.

மேலும் உலாவிகளுடன் சோதனைகளை இயக்க, நீங்கள் இயக்கலாம்

```sh
pnpm run test.local.desktop.storybook -- --browsers=chrome,firefox,edge,safari
```

> [!NOTE]
> நீங்கள் இயக்க விரும்பும் உலாவிகளை உங்கள் உள்ளூர் இயந்திரத்தில் நிறுவியுள்ளீர்கள் என்பதை உறுதிப்படுத்திக் கொள்ளுங்கள்

#### Sauce Labs உடன் CI சோதனை (PR க்கு தேவையில்லை)

கீழே உள்ள கட்டளை GitHub Actions இல் பில்டைச் சோதிக்கப் பயன்படுகிறது, இது அங்கு மட்டுமே பயன்படுத்தப்படலாம், உள்ளூர் மேம்பாட்டிற்கு அல்ல.

```
$ pnpm run test.saucelabs
```

இது [இங்கே](https://github.com/webdriverio/visual-testing/blob/main/./tests/configs/wdio.saucelabs.web.conf.ts) காணப்படும் பல உள்ளமைவுகளுக்கு எதிராக சோதிக்கும்.
அனைத்து PR களும் Sauce Labs க்கு எதிராக தானாகவே சரிபார்க்கப்படுகின்றன.

## வெளியீடு

மேலே பட்டியலிடப்பட்டுள்ள ஏதேனும் தொகுப்புகளின் பதிப்பை வெளியிட, பின்வருவனவற்றைச் செய்யவும்:

-   [வெளியீட்டு பைப்லைன்](https://github.com/webdriverio/visual-testing/actions/workflows/release.yml) ஐத் தூண்டவும்
-   ஒரு வெளியீட்டு PR உருவாக்கப்படுகிறது, இது மற்றொரு WebdriverIO உறுப்பினரால் மதிப்பாய்வு செய்யப்பட்டு அங்கீகரிக்கப்பட வேண்டும்
-   PR ஐ இணைக்கவும்
-   [வெளியீட்டு பைப்லைன்](https://github.com/webdriverio/visual-testing/actions/workflows/release.yml) ஐ மீண்டும் தூண்டவும்
-   ஒரு புதிய பதிப்பு வெளியிடப்பட வேண்டும் 🎉

## நன்றி

`@wdio/visual-testing` [LambdaTest](https://www.lambdatest.com/) மற்றும் [Sauce Labs](https://saucelabs.com/) இலிருந்து ஓப்பன் சோர்ஸ் உரிமத்தைப் பயன்படுத்துகிறது.