---
id: macos
title: مک‌او‌اس
---

WebdriverIO می‌تواند برنامه‌های دلخواه مک‌او‌اس را با استفاده از [Appium](https://appium.io/docs/en/2.0/) به صورت خودکار اجرا کند. تمام چیزی که نیاز دارید نصب [XCode](https://developer.apple.com/xcode/) روی سیستم شما، Appium و [Mac2 Driver](https://github.com/appium/appium-mac2-driver) به عنوان وابستگی و تنظیم قابلیت‌های صحیح است.

## شروع به کار

برای شروع یک پروژه جدید WebdriverIO، اجرا کنید:

```sh
npm create wdio@latest ./
```

یک ویزارد نصب شما را در این فرآیند راهنمایی خواهد کرد. اطمینان حاصل کنید که _"Desktop Testing - of MacOS Applications"_ را هنگامی که از شما می‌پرسد چه نوع تستی می‌خواهید انجام دهید، انتخاب کنید. پس از آن، مقادیر پیش‌فرض را حفظ کنید یا بر اساس ترجیح خود تغییر دهید.

ویزارد پیکربندی تمام بسته‌های مورد نیاز Appium را نصب می‌کند و یک `wdio.conf.js` یا `wdio.conf.ts` با پیکربندی لازم برای تست روی مک‌او‌اس ایجاد می‌کند. اگر با تولید خودکار برخی فایل‌های تست موافقت کرده‌اید، می‌توانید اولین تست خود را از طریق `npm run wdio` اجرا کنید.

<CreateMacOSProjectAnimation />

همین است 🎉

## مثال

این یک تست ساده است که برنامه ماشین حساب را باز می‌کند، یک محاسبه انجام می‌دهد و نتیجه آن را تأیید می‌کند:

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

__نکته:__ برنامه ماشین حساب در ابتدای جلسه به طور خودکار باز شد زیرا `'appium:bundleId': 'com.apple.calculator'` به عنوان گزینه قابلیت تعریف شده بود. شما می‌توانید در طول جلسه در هر زمان برنامه‌ها را تغییر دهید.

## اطلاعات بیشتر

برای کسب اطلاعات درباره جزئیات مربوط به تست روی مک‌او‌اس، توصیه می‌کنیم پروژه [Appium Mac2 Driver](https://github.com/appium/appium-mac2-driver) را بررسی کنید.