---
id: gettingstarted
title: शुरू करना
---

WebdriverIO दस्तावेज़ीकरण में आपका स्वागत है। यह आपको जल्दी शुरू करने में मदद करेगा। अगर आपको कोई समस्या आती है, तो आप हमारे [Discord सपोर्ट सर्वर](https://discord.webdriver.io) पर मदद और जवाब पा सकते हैं या आप मुझे [Twitter](https://twitter.com/webdriverio) पर संपर्क कर सकते हैं।

:::info
ये दस्तावेज़ नवीनतम संस्करण (__>=9.x__) के WebdriverIO के लिए हैं। अगर आप अभी भी पुराने संस्करण का उपयोग कर रहे हैं, तो कृपया [पुराने दस्तावेज़ वेबसाइट](/versions) पर जाएँ!
:::

<LiteYouTubeEmbed
    id="rA4IFNyW54c"
    title="Getting Started with WebdriverIO"
/>

:::tip आधिकारिक YouTube चैनल 🎥

आप [आधिकारिक YouTube चैनल](https://youtube.com/@webdriverio) पर WebdriverIO के बारे में अधिक वीडियो पा सकते हैं। सुनिश्चित करें कि आप सब्सक्राइब करें!

:::

## WebdriverIO सेटअप शुरू करें

किसी मौजूदा या नए प्रोजेक्ट में पूर्ण WebdriverIO सेटअप जोड़ने के लिए [WebdriverIO स्टार्टर टूलकिट](https://www.npmjs.com/package/create-wdio) का उपयोग करके, निम्न चलाएँ:

अगर आप किसी मौजूदा प्रोजेक्ट के रूट डायरेक्टरी में हैं, तो चलाएँ:

<Tabs
  defaultValue="npm"
  values={[
    {label: 'NPM', value: 'npm'},
    {label: 'Yarn', value: 'yarn'},
    {label: 'pnpm', value: 'pnpm'},
    {label: 'bun', value: 'bun'},
  ]
}>
<TabItem value="npm">

```sh
npm init wdio@latest .
```

या अगर आप एक नया प्रोजेक्ट बनाना चाहते हैं:

```sh
npm init wdio@latest ./path/to/new/project
```

</TabItem>
<TabItem value="yarn">

```sh
yarn create wdio .
```

या अगर आप एक नया प्रोजेक्ट बनाना चाहते हैं:

```sh
yarn create wdio ./path/to/new/project
```

</TabItem>
<TabItem value="pnpm">

```sh
pnpm create wdio@latest .
```

या अगर आप एक नया प्रोजेक्ट बनाना चाहते हैं:

```sh
pnpm create wdio@latest ./path/to/new/project
```

</TabItem>
<TabItem value="bun">

```sh
bun create wdio@latest .
```

या अगर आप एक नया प्रोजेक्ट बनाना चाहते हैं:

```sh
bun create wdio@latest ./path/to/new/project
```

</TabItem>
</Tabs>

यह एकल कमांड WebdriverIO CLI टूल डाउनलोड करता है और एक कॉन्फ़िगरेशन विज़ार्ड चलाता है जो आपको अपनी टेस्ट सूट को कॉन्फ़िगर करने में मदद करता है।

<CreateProjectAnimation />

विज़ार्ड कुछ सवाल पूछेगा जो आपको सेटअप के माध्यम से मार्गदर्शन करेगा। आप एक `--yes` पैरामीटर पास कर सकते हैं जो एक डिफ़ॉल्ट सेटअप चुनेगा जो [पेज ऑब्जेक्ट](https://martinfowler.com/bliki/PageObject.html) पैटर्न का उपयोग करके क्रोम के साथ मोचा का उपयोग करेगा।

<Tabs
  defaultValue="npm"
  values={[
    {label: 'NPM', value: 'npm'},
    {label: 'Yarn', value: 'yarn'},
    {label: 'pnpm', value: 'pnpm'},
    {label: 'bun', value: 'bun'},
  ]
}>
<TabItem value="npm">

```sh
npm init wdio@latest . -- --yes
```

</TabItem>
<TabItem value="yarn">

```sh
yarn create wdio . --yes
```

</TabItem>
<TabItem value="pnpm">

```sh
pnpm create wdio@latest . --yes
```

</TabItem>
<TabItem value="bun">

```sh
bun create wdio@latest . --yes
```

</TabItem>
</Tabs>

## CLI मैन्युअल रूप से इंस्टॉल करें

आप CLI पैकेज को अपने प्रोजेक्ट में मैन्युअल रूप से भी जोड़ सकते हैं:

```sh
npm i --save-dev @wdio/cli
npx wdio --version # prints e.g. `8.13.10`

# run configuration wizard
npx wdio config
```

## टेस्ट चलाएँ

आप अपनी टेस्ट सूट को `run` कमांड का उपयोग करके और अभी बनाए गए WebdriverIO कॉन्फ़िग की ओर इशारा करके शुरू कर सकते हैं:

```sh
npx wdio run ./wdio.conf.js
```

अगर आप विशिष्ट टेस्ट फ़ाइलें चलाना चाहते हैं तो आप `--spec` पैरामीटर जोड़ सकते हैं:

```sh
npx wdio run ./wdio.conf.js --spec example.e2e.js
```

या अपने कॉन्फ़िग फ़ाइल में सूट परिभाषित करें और सिर्फ उन टेस्ट फ़ाइलों को चलाएँ जो सूट में परिभाषित हैं:

```sh
npx wdio run ./wdio.conf.js --suite exampleSuiteName
```

## स्क्रिप्ट में चलाएँ

अगर आप Node.JS स्क्रिप्ट के भीतर [स्टैंडअलोन मोड](/docs/setuptypes#standalone-mode) में ऑटोमेशन इंजन के रूप में WebdriverIO का उपयोग करना चाहते हैं, तो आप सीधे WebdriverIO को इंस्टॉल कर सकते हैं और इसे एक पैकेज के रूप में उपयोग कर सकते हैं, उदाहरण के लिए, वेबसाइट का स्क्रीनशॉट बनाने के लिए:

```js reference useHTTPS
https://github.com/webdriverio/example-recipes/blob/fc362f2f8dd823d294b9bb5f92bd5991339d4591/getting-started/run-in-script.js#L2-L19
```

__नोट:__ सभी WebdriverIO कमांड्स असिंक्रोनस हैं और उन्हें [`async/await`](https://javascript.info/async-await) का उपयोग करके उचित रूप से संभाला जाना चाहिए।

## टेस्ट रिकॉर्ड करें

WebdriverIO ऐसे टूल प्रदान करता है जो आपको स्क्रीन पर अपनी टेस्ट क्रियाओं को रिकॉर्ड करके और स्वचालित रूप से WebdriverIO टेस्ट स्क्रिप्ट जनरेट करके शुरू करने में मदद करते हैं। अधिक जानकारी के लिए [Chrome DevTools रिकॉर्डर के साथ टेस्ट रिकॉर्ड करें](/docs/record) देखें।

## सिस्टम आवश्यकताएँ

आपको [Node.js](http://nodejs.org) इंस्टॉल करने की आवश्यकता होगी।

- कम से कम v18.20.0 या उससे अधिक इंस्टॉल करें क्योंकि यह सबसे पुराना सक्रिय LTS संस्करण है
- केवल वे रिलीज़ आधिकारिक तौर पर समर्थित हैं जो LTS रिलीज़ हैं या बनने वाले हैं

अगर Node वर्तमान में आपके सिस्टम पर इंस्टॉल नहीं है, तो हम [NVM](https://github.com/creationix/nvm) या [Volta](https://volta.sh/) जैसे टूल का उपयोग करने का सुझाव देते हैं जो कई सक्रिय Node.js संस्करणों को प्रबंधित करने में सहायता करता है। NVM एक लोकप्रिय विकल्प है, जबकि Volta भी एक अच्छा विकल्प है।