---
id: file-download
title: دانلود فایل
---

هنگام خودکارسازی دانلود فایل‌ها در تست وب، ضروری است که آن‌ها را به طور یکسان در مرورگرهای مختلف مدیریت کنید تا اجرای تست قابل اعتماد باشد.

در اینجا، بهترین روش‌ها برای دانلود فایل‌ها را ارائه می‌دهیم و نشان می‌دهیم چگونه دایرکتوری‌های دانلود را برای **گوگل کروم**، **موزیلا فایرفاکس** و **مایکروسافت اج** پیکربندی کنید.

## مسیرهای دانلود

**هاردکدینگ** مسیرهای دانلود در اسکریپت‌های تست می‌تواند باعث مشکلات نگهداری و مشکلات قابلیت حمل شود. از **مسیرهای نسبی** برای دایرکتوری‌های دانلود استفاده کنید تا قابلیت حمل و سازگاری در محیط‌های مختلف تضمین شود.

```javascript
// 👎
// Hardcoded download path
const downloadPath = '/path/to/downloads';

// 👍
// Relative download path
const downloadPath = path.join(__dirname, 'downloads');
```

## استراتژی‌های انتظار

عدم پیاده‌سازی استراتژی‌های انتظار مناسب می‌تواند منجر به شرایط رقابتی یا تست‌های غیرقابل اعتماد، به ویژه برای تکمیل دانلود شود. استراتژی‌های انتظار **صریح** را پیاده‌سازی کنید تا منتظر تکمیل دانلود فایل‌ها باشید و هماهنگی بین مراحل تست را تضمین کنید.

```javascript
// 👎
// No explicit wait for download completion
await browser.pause(5000);

// 👍
// Wait for file download completion
await waitUntil(async ()=> await fs.existsSync(downloadPath), 5000);
```

## پیکربندی دایرکتوری‌های دانلود

برای تغییر رفتار دانلود فایل برای **گوگل کروم**، **موزیلا فایرفاکس** و **مایکروسافت اج**، دایرکتوری دانلود را در قابلیت‌های WebDriverIO ارائه دهید:

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

برای یک نمونه پیاده‌سازی، به [دستور العمل تست رفتار دانلود WebdriverIO](https://github.com/webdriverio/example-recipes/tree/main/testDownloadBehavior) مراجعه کنید.

## پیکربندی دانلود‌های مرورگر کرومیوم

برای تغییر مسیر دانلود برای مرورگرهای __مبتنی بر کرومیوم__ (مانند کروم، اج، بریو و غیره) با استفاده از متد `getPuppeteer` WebDriverIO برای دسترسی به ابزارهای توسعه کروم.

```javascript
const page = await browser.getPuppeteer();
// Initiate a CDP Session:
const cdpSession = await page.target().createCDPSession();
// Set the Download Path:
await cdpSession.send('Browser.setDownloadBehavior', { behavior: 'allow', downloadPath: downloadPath });
```

## مدیریت دانلود چندین فایل

هنگام برخورد با سناریوهای شامل دانلود چندین فایل، ضروری است استراتژی‌هایی برای مدیریت و اعتبارسنجی موثر هر دانلود پیاده‌سازی کنید. روش‌های زیر را در نظر بگیرید:

__مدیریت دانلود ترتیبی:__ فایل‌ها را یکی پس از دیگری دانلود کنید و قبل از شروع دانلود بعدی، هر دانلود را تأیید کنید تا اجرای منظم و اعتبارسنجی دقیق تضمین شود.

__مدیریت دانلود موازی:__ از تکنیک‌های برنامه‌نویسی ناهمگام برای شروع همزمان دانلود چندین فایل استفاده کنید تا زمان اجرای تست بهینه شود. مکانیزم‌های اعتبارسنجی قوی را برای تأیید تمام دانلودها پس از تکمیل پیاده‌سازی کنید.

## ملاحظات سازگاری بین مرورگرها

در حالی که WebDriverIO یک رابط یکپارچه برای خودکارسازی مرورگر ارائه می‌دهد، در نظر گرفتن تغییرات در رفتار و قابلیت‌های مرورگر ضروری است. عملکرد دانلود فایل خود را در مرورگرهای مختلف آزمایش کنید تا از سازگاری و یکنواختی اطمینان حاصل کنید.

__پیکربندی‌های خاص مرورگر:__ تنظیمات مسیر دانلود و استراتژی‌های انتظار را برای تطبیق با تفاوت‌های رفتار و ترجیحات مرورگر در کروم، فایرفاکس، اج و سایر مرورگرهای پشتیبانی شده تنظیم کنید.

__سازگاری نسخه مرورگر:__ WebDriverIO و نسخه‌های مرورگر خود را به طور منظم به‌روزرسانی کنید تا از آخرین ویژگی‌ها و بهبودها بهره‌مند شوید در حالی که سازگاری با مجموعه تست موجود خود را تضمین می‌کنید.