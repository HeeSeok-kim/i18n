---
id: file-download
title: Завантаження файлів
---

При автоматизації завантаження файлів у веб-тестуванні важливо обробляти їх послідовно у різних браузерах для забезпечення надійного виконання тестів.

Тут ми надаємо найкращі практики для завантаження файлів і демонструємо, як налаштувати каталоги завантаження для **Google Chrome**, **Mozilla Firefox** та **Microsoft Edge**.

## Шляхи завантаження

**Хардкодинг** шляхів завантаження у тестових скриптах може призвести до проблем з обслуговуванням та портативністю. Використовуйте **відносні шляхи** для каталогів завантаження, щоб забезпечити портативність та сумісність у різних середовищах.

```javascript
// 👎
// Жорстко закодований шлях завантаження
const downloadPath = '/path/to/downloads';

// 👍
// Відносний шлях завантаження
const downloadPath = path.join(__dirname, 'downloads');
```

## Стратегії очікування

Відсутність належних стратегій очікування може призвести до умов гонки або ненадійних тестів, особливо для завершення завантаження. Реалізуйте **явні** стратегії очікування, щоб дочекатися завершення завантаження файлів, забезпечуючи синхронізацію між кроками тесту.

```javascript
// 👎
// Немає явного очікування завершення завантаження
await browser.pause(5000);

// 👍
// Очікування завершення завантаження файлу
await waitUntil(async ()=> await fs.existsSync(downloadPath), 5000);
```

## Налаштування каталогів завантаження

Щоб змінити поведінку завантаження файлів для **Google Chrome**, **Mozilla Firefox** та **Microsoft Edge**, вкажіть каталог завантаження у можливостях WebDriverIO:

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

Для прикладу реалізації зверніться до [WebdriverIO Test Download Behavior Recipe](https://github.com/webdriverio/example-recipes/tree/main/testDownloadBehavior).

## Налаштування завантажень браузера Chromium

Щоб змінити шлях завантаження для браузерів на основі __Chromium__ (таких як Chrome, Edge, Brave тощо) за допомогою методу WebDriverIO `getPuppeteer` для доступу до Chrome DevTools.

```javascript
const page = await browser.getPuppeteer();
// Ініціювати CDP сесію:
const cdpSession = await page.target().createCDPSession();
// Встановити шлях завантаження:
await cdpSession.send('Browser.setDownloadBehavior', { behavior: 'allow', downloadPath: downloadPath });
```

## Обробка кількох завантажень файлів

При роботі зі сценаріями, що включають кілька завантажень файлів, важливо реалізувати стратегії для ефективного управління та перевірки кожного завантаження. Розгляньте такі підходи:

__Послідовна обробка завантажень:__ Завантажуйте файли один за одним і перевіряйте кожне завантаження перед початком наступного, щоб забезпечити впорядковане виконання та точну перевірку.

__Паралельна обробка завантажень:__ Використовуйте асинхронні методи програмування для одночасного ініціювання кількох завантажень файлів, оптимізуючи час виконання тестів. Реалізуйте надійні механізми перевірки для перевірки всіх завантажень після завершення.

## Міркування щодо кросбраузерної сумісності

Хоча WebDriverIO надає уніфікований інтерфейс для автоматизації браузера, важливо враховувати відмінності в поведінці та можливостях браузерів. Розгляньте можливість тестування функціональності завантаження файлів у різних браузерах для забезпечення сумісності та послідовності.

__Специфічні для браузера конфігурації:__ Налаштуйте параметри шляху завантаження та стратегії очікування для врахування відмінностей у поведінці та параметрах браузера для Chrome, Firefox, Edge та інших підтримуваних браузерів.

__Сумісність версій браузера:__ Регулярно оновлюйте WebDriverIO та версії браузерів, щоб використовувати найновіші функції та вдосконалення, забезпечуючи сумісність з існуючим набором тестів.