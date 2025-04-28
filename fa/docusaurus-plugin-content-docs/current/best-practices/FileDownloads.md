---
id: file-download
title: دانلود فایل
---

هنگام خودکارسازی دانلود فایل در تست‌های وب، ضروری است که آن‌ها را به طور یکنواخت در مرورگرهای مختلف مدیریت کنید تا اجرای تست قابل اعتماد باشد.

در اینجا، بهترین روش‌ها برای دانلود فایل‌ها را ارائه می‌دهیم و نحوه پیکربندی مسیرهای دانلود برای **گوگل کروم**، **موزیلا فایرفاکس** و **مایکروسافت اج** را نشان می‌دهیم.

## مسیرهای دانلود

**کدگذاری ثابت** مسیرهای دانلود در اسکریپت‌های تست می‌تواند باعث مشکلات نگهداری و مشکلات قابلیت حمل شود. از **مسیرهای نسبی** برای دایرکتوری‌های دانلود استفاده کنید تا قابلیت حمل و سازگاری در محیط‌های مختلف تضمین شود.

```javascript
// 👎
// Hardcoded download path
const downloadPath = '/path/to/downloads';

// 👍
// Relative download path
const downloadPath = path.join(__dirname, 'downloads');
```

## استراتژی‌های انتظار

عدم پیاده‌سازی استراتژی‌های انتظار مناسب می‌تواند منجر به شرایط رقابتی یا تست‌های غیرقابل اعتماد شود، به خصوص برای تکمیل دانلود. استراتژی‌های انتظار **صریح** را پیاده‌سازی کنید تا منتظر تکمیل دانلود فایل‌ها بمانید و هماهنگی بین مراحل تست را تضمین کنید.

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

برای تغییر مسیر دانلود برای مرورگرهای __مبتنی بر کرومیوم__ (مانند کروم، اج، بریو و غیره) با استفاده از روش `getPuppeteer` وب‌درایور آی‌او برای دسترسی به Chrome DevTools.

```javascript
const page = await browser.getPuppeteer();
// Initiate a CDP Session:
const cdpSession = await page.target().createCDPSession();
// Set the Download Path:
await cdpSession.send('Browser.setDownloadBehavior', { behavior: 'allow', downloadPath: downloadPath });
```

## مدیریت دانلود چند فایل

هنگام مواجهه با سناریوهای شامل دانلود چند فایل، ضروری است استراتژی‌هایی برای مدیریت و اعتبارسنجی موثر هر دانلود پیاده‌سازی کنید. رویکردهای زیر را در نظر بگیرید:

__مدیریت دانلود متوالی:__ فایل‌ها را یکی پس از دیگری دانلود کنید و قبل از شروع دانلود بعدی، هر دانلود را تأیید کنید تا اجرای منظم و اعتبارسنجی دقیق تضمین شود.

__مدیریت دانلود موازی:__ از تکنیک‌های برنامه‌نویسی ناهمگام استفاده کنید تا چند دانلود فایل را همزمان آغاز کنید و زمان اجرای تست را بهینه کنید. مکانیزم‌های اعتبارسنجی قوی را برای تأیید همه دانلودها پس از تکمیل پیاده‌سازی کنید.

## ملاحظات سازگاری بین مرورگرها

در حالی که WebDriverIO یک رابط یکپارچه برای خودکارسازی مرورگر فراهم می‌کند، ضروری است که تفاوت در رفتار و قابلیت‌های مرورگر را در نظر بگیرید. قابلیت دانلود فایل خود را در مرورگرهای مختلف آزمایش کنید تا از سازگاری و یکنواختی اطمینان حاصل کنید.

__پیکربندی‌های خاص مرورگر:__ تنظیمات مسیر دانلود و استراتژی‌های انتظار را برای سازگاری با تفاوت‌های رفتار و ترجیحات مرورگر در کروم، فایرفاکس، اج و سایر مرورگرهای پشتیبانی شده تنظیم کنید.

__سازگاری نسخه مرورگر:__ به طور منظم WebDriverIO و نسخه‌های مرورگر خود را به‌روزرسانی کنید تا از جدیدترین ویژگی‌ها و بهبودها استفاده کنید و در عین حال از سازگاری با مجموعه تست موجود خود اطمینان حاصل کنید.