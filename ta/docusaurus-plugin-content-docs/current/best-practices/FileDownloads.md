---
id: file-download
title: கோப்பு பதிவிறக்கம்
---

வலை சோதனையில் கோப்பு பதிவிறக்கத்தை தானியக்கமாக்கும்போது, நம்பகமான சோதனை செயலாக்கத்தை உறுதி செய்ய வெவ்வேறு உலாவிகளில் அவற்றை ஒருமித்த முறையில் கையாள்வது அவசியமாகும்.

இங்கே, கோப்பு பதிவிறக்கங்களுக்கான சிறந்த நடைமுறைகளை வழங்குகிறோம், மேலும் **Google Chrome**, **Mozilla Firefox** மற்றும் **Microsoft Edge** ஆகியவற்றிற்கான பதிவிறக்க கோப்பகங்களை எவ்வாறு உள்ளமைப்பது என்பதை நிரூபிக்கிறோம்.

## பதிவிறக்க பாதைகள்

சோதனை ஸ்கிரிப்ட்களில் பதிவிறக்க பாதைகளை **Hardcoding** செய்வது பராமரிப்பு சிக்கல்கள் மற்றும் போர்ட்டபிலிட்டி பிரச்சனைகளை ஏற்படுத்தும். வெவ்வேறு சூழல்களில் போர்ட்டபிலிட்டி மற்றும் இணக்கத்தன்மையை உறுதி செய்ய பதிவிறக்க கோப்பகங்களுக்கு **சார்புடைய பாதைகளை** பயன்படுத்தவும்.

```javascript
// 👎
// Hardcoded download path
const downloadPath = '/path/to/downloads';

// 👍
// Relative download path
const downloadPath = path.join(__dirname, 'downloads');
```

## காத்திருக்கும் உத்திகள்

சரியான காத்திருக்கும் உத்திகளை செயல்படுத்தத் தவறுவது, குறிப்பாக பதிவிறக்க முடிவுக்காக, ரேஸ் நிலைமைகள் அல்லது நம்பகமற்ற சோதனைகளுக்கு வழிவகுக்கும். கோப்பு பதிவிறக்கங்கள் முடிவடைய காத்திருக்க **தெளிவான** காத்திருப்பு உத்திகளைச் செயல்படுத்தவும், சோதனை படிகளுக்கு இடையேயான ஒத்திசைவை உறுதி செய்யவும்.

```javascript
// 👎
// No explicit wait for download completion
await browser.pause(5000);

// 👍
// Wait for file download completion
await waitUntil(async ()=> await fs.existsSync(downloadPath), 5000);
```

## பதிவிறக்க கோப்பகங்களை உள்ளமைத்தல்

**Google Chrome**, **Mozilla Firefox** மற்றும் **Microsoft Edge** ஆகியவற்றிற்கான கோப்பு பதிவிறக்க நடத்தையை மேற்றிடுவதற்கு, WebDriverIO திறன்களில் பதிவிறக்க கோப்பகத்தை வழங்கவும்:

<Tabs
defaultValue="chrome"
values={[
{label: 'Chrome', value: 'chrome'},
{label: 'Firefox', value: 'firefox'},
{label: 'Microsoft Edge', value: 'edge'},
]
}>

<TabItem value='chrome'>

```javascript reference title="wdio.conf.js"

https://github.com/webdriverio/example-recipes/blob/84dda93011234d0b2a34ee0cfb3cdfa2a06136a5/testDownloadBehavior/wdio.conf.js#L8-L16

```

</TabItem>

<TabItem value='firefox'>

```javascript reference title="wdio.conf.js"

https://github.com/webdriverio/example-recipes/blob/84dda93011234d0b2a34ee0cfb3cdfa2a06136a5/testDownloadBehavior/wdio.conf.js#L20-L32

```

</TabItem>

<TabItem value='edge'>

```javascript reference title="wdio.conf.js"

https://github.com/webdriverio/example-recipes/blob/84dda93011234d0b2a34ee0cfb3cdfa2a06136a5/testDownloadBehavior/wdio.conf.js#L36-L44

```

</TabItem>

</Tabs>

ஒரு எடுத்துக்காட்டு செயல்படுத்தலுக்கு, [WebdriverIO Test Download Behavior Recipe](https://github.com/webdriverio/example-recipes/tree/main/testDownloadBehavior) ஐ பார்க்கவும்.

## Chromium உலாவி பதிவிறக்கங்களை உள்ளமைத்தல்

Chrome DevTools அணுகுவதற்கான WebDriverIO இன் `getPuppeteer` முறையைப் பயன்படுத்தி __Chromium அடிப்படையிலான__ உலாவிகளுக்கான (Chrome, Edge, Brave போன்றவை) பதிவிறக்க பாதையை மாற்ற.

```javascript
const page = await browser.getPuppeteer();
// Initiate a CDP Session:
const cdpSession = await page.target().createCDPSession();
// Set the Download Path:
await cdpSession.send('Browser.setDownloadBehavior', { behavior: 'allow', downloadPath: downloadPath });
```

## பல கோப்பு பதிவிறக்கங்களை கையாளுதல்

பல கோப்பு பதிவிறக்கங்களை உள்ளடக்கிய சூழ்நிலைகளைக் கையாளும் போது, ஒவ்வொரு பதிவிறக்கத்தையும் திறம்பட நிர்வகிக்கவும் சரிபார்க்கவும் உத்திகளை செயல்படுத்துவது அவசியம். பின்வரும் அணுகுமுறைகளைக் கருத்தில் கொள்ளுங்கள்:

__தொடர்ச்சியான பதிவிறக்க கையாளுதல்:__ கோப்புகளை ஒன்றன்பின் ஒன்றாகப் பதிவிறக்கி, அடுத்ததைத் தொடங்குவதற்கு முன் ஒவ்வொரு பதிவிறக்கத்தையும் சரிபார்க்கவும், இது ஒழுங்கான செயலாக்கம் மற்றும் துல்லியமான சரிபார்ப்பை உறுதி செய்கிறது.

__இணையான பதிவிறக்க கையாளுதல்:__ பல கோப்பு பதிவிறக்கங்களை ஒரே நேரத்தில் தொடங்க ஒத்திசைவற்ற நிரலாக்க நுட்பங்களைப் பயன்படுத்தி, சோதனை செயலாக்க நேரத்தை மேம்படுத்தவும். முடிவில் அனைத்து பதிவிறக்கங்களையும் சரிபார்க்க வலுவான சரிபார்ப்பு வழிமுறைகளை செயல்படுத்தவும்.

## குறுக்கு-உலாவி இணக்கத்தன்மை கருத்துகள்

WebDriverIO உலாவி ஆட்டோமேஷனுக்கான ஒருங்கிணைந்த இடைமுகத்தை வழங்கினாலும், உலாவி நடத்தை மற்றும் திறன்களில் உள்ள மாறுபாடுகளைக் கணக்கில் கொள்வது அவசியம். இணக்கத்தன்மை மற்றும் ஒத்திசைவை உறுதி செய்ய வெவ்வேறு உலாவிகளில் உங்கள் கோப்பு பதிவிறக்க செயல்பாட்டை சோதிக்க கருத்தில் கொள்ளுங்கள்.

__உலாவி-குறிப்பிட்ட உள்ளமைவுகள்:__ Chrome, Firefox, Edge மற்றும் பிற ஆதரிக்கப்படும் உலாவிகளில் உலாவி நடத்தை மற்றும் விருப்பங்களில் உள்ள வேறுபாடுகளை சரிசெய்ய பதிவிறக்க பாதை அமைப்புகள் மற்றும் காத்திருப்பு உத்திகளைச் சரிசெய்யவும்.

__உலாவி பதிப்பு இணக்கத்தன்மை:__ சமீபத்திய அம்சங்கள் மற்றும் மேம்பாடுகளைப் பயன்படுத்த உங்கள் WebDriverIO மற்றும் உலாவி பதிப்புகளை வழக்கமாக புதுப்பிக்கவும், அதேசமயம் உங்கள் தற்போதைய சோதனை தொகுப்புடன் இணக்கத்தை உறுதி செய்யவும்.