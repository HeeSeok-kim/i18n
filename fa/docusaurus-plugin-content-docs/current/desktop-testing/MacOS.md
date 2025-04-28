---
id: macos
title: مک‌او‌اس
---

WebdriverIO می‌تواند برنامه‌های دلخواه مک‌او‌اس را با استفاده از [Appium](https://appium.io/docs/en/2.0/) به صورت خودکار آزمایش کند. تمام چیزی که نیاز دارید، نصب [XCode](https://developer.apple.com/xcode/) روی سیستم شما، نصب Appium و [Mac2 Driver](https://github.com/appium/appium-mac2-driver) به عنوان وابستگی و تنظیم قابلیت‌های صحیح است.

## شروع کار

برای ایجاد یک پروژه جدید WebdriverIO، این دستور را اجرا کنید:

```sh
npm create wdio@latest ./
```

یک ویزارد نصب شما را در این فرآیند راهنمایی می‌کند. اطمینان حاصل کنید که _"Desktop Testing - of MacOS Applications"_ را هنگامی که از شما می‌پرسد چه نوع آزمایشی می‌خواهید انجام دهید، انتخاب کنید. پس از آن، تنظیمات پیش‌فرض را نگه دارید یا بر اساس ترجیح خود تغییر دهید.

ویزارد پیکربندی تمام بسته‌های Appium مورد نیاز را نصب می‌کند و یک `wdio.conf.js` یا `wdio.conf.ts` با پیکربندی لازم برای آزمایش در مک‌او‌اس ایجاد می‌کند. اگر با تولید خودکار برخی فایل‌های آزمایشی موافقت کردید، می‌توانید اولین آزمایش خود را از طریق `npm run wdio` اجرا کنید.

<CreateMacOSProjectAnimation />

تمام شد 🎉

## مثال

این نمونه‌ای از یک آزمایش ساده است که برنامه ماشین‌حساب را باز می‌کند، یک محاسبه انجام می‌دهد و نتیجه آن را تأیید می‌کند:

```js
describe('My Login application', () => {
    it('should set a text to a text view', async function () {
        await $('//XCUIElementTypeButton[@label="seven"]').click()
        await $('//XCUIElementTypeButton[@label="multiply"]').click()
        await $('//XCUIElementTypeButton[@label="six"]').click()
        await $('//XCUIElementTypeButton[@title="="]').click()
        await expect($('//XCUIElementTypeStaticText[@label="main display"]')).toHaveText('42')
    });
})
```

__نکته:__ برنامه ماشین‌حساب در ابتدای جلسه به طور خودکار باز شد زیرا `'appium:bundleId': 'com.apple.calculator'` به عنوان گزینه قابلیت تعریف شده بود. شما می‌توانید در هر زمان در طول جلسه بین برنامه‌ها جابجا شوید.

## اطلاعات بیشتر

برای کسب اطلاعات بیشتر در مورد جزئیات آزمایش در مک‌او‌اس، توصیه می‌کنیم پروژه [Appium Mac2 Driver](https://github.com/appium/appium-mac2-driver) را بررسی کنید.