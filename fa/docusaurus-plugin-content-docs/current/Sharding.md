---
id: sharding
title: شاردینگ
---

به طور پیش‌فرض، WebdriverIO تست‌ها را به صورت موازی اجرا می‌کند و برای استفاده بهینه از هسته‌های CPU در دستگاه شما تلاش می‌کند. برای دستیابی به موازی‌سازی بیشتر، می‌توانید اجرای تست WebdriverIO را با اجرای تست‌ها در چندین دستگاه به طور همزمان مقیاس‌پذیر کنید. ما این حالت عملیاتی را "شاردینگ" می‌نامیم.

## تقسیم تست‌ها بین چندین دستگاه

برای تقسیم مجموعه تست، گزینه `--shard=x/y` را به خط فرمان اضافه کنید. به عنوان مثال، برای تقسیم مجموعه به چهار شارد، که هر کدام یک چهارم از تست‌ها را اجرا می‌کنند:

```sh
npx wdio run wdio.conf.js --shard=1/4
npx wdio run wdio.conf.js --shard=2/4
npx wdio run wdio.conf.js --shard=3/4
npx wdio run wdio.conf.js --shard=4/4
```

حالا، اگر این شاردها را به صورت موازی در کامپیوترهای مختلف اجرا کنید، مجموعه تست شما چهار برابر سریعتر تکمیل می‌شود.

## مثال GitHub Actions

GitHub Actions از [تقسیم تست‌ها بین چندین کار](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs) با استفاده از گزینه [`jobs.<job_id>.strategy.matrix`](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix) پشتیبانی می‌کند. گزینه ماتریس یک کار جداگانه برای هر ترکیب ممکن از گزینه‌های ارائه شده اجرا می‌کند.

مثال زیر نشان می‌دهد که چگونه یک کار را برای اجرای تست‌ها روی چهار دستگاه به صورت موازی پیکربندی کنید. می‌توانید تنظیمات کامل خط لوله را در پروژه [Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate/blob/main/.github/workflows/test.yaml) مشاهده کنید.

-   ابتدا یک گزینه ماتریس به پیکربندی کار خود با گزینه شارد حاوی تعداد شاردهایی که می‌خواهیم ایجاد کنیم، اضافه می‌کنیم. `shard: [1, 2, 3, 4]` چهار شارد ایجاد می‌کند، هر کدام با شماره شارد متفاوت.
-   سپس تست‌های WebdriverIO خود را با گزینه `--shard ${{ matrix.shard }}/${{ strategy.job-total }}` اجرا می‌کنیم. این دستور تست ما برای هر شارد خواهد بود.
-   در نهایت گزارش لاگ wdio خود را به Artifacts اکشن‌های GitHub آپلود می‌کنیم. این کار لاگ‌ها را در صورت شکست شارد در دسترس قرار می‌دهد.

خط لوله تست به شرح زیر تعریف شده است:

```yaml title=.github/workflows/test.yaml
name: Test

on: [push, pull_request]

jobs:
    lint:
        # ...
    unit:
        # ...
    e2e:
        name: 🧪 Test (${{ matrix.shard }}/${{ strategy.job-total }})
        runs-on: ubuntu-latest
        needs: [lint, unit]
        strategy:
            matrix:
                shard: [1, 2, 3, 4]
        steps:
            - uses: actions/checkout@v4
            - uses: ./.github/workflows/actions/setup
            - name: E2E Test
              run: npm run test:features -- --shard ${{ matrix.shard }}/${{ strategy.job-total }}
            - uses: actions/upload-artifact@v1
              if: failure()
              with:
                  name: logs-${{ matrix.shard }}
                  path: logs
```

این تمام شاردها را به صورت موازی اجرا می‌کند، زمان اجرای تست‌ها را به میزان ۴ کاهش می‌دهد:

![مثال GitHub Actions](/img/sharding.png "مثال GitHub Actions")

کامیت [`96d444e`](https://github.com/webdriverio/cucumber-boilerplate/commit/96d444ea23919389682b9b1c9408ed91c452c7f8) از پروژه [Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate) را ببینید که شاردینگ را به خط لوله تست آن معرفی کرد و باعث کاهش زمان کلی اجرا از `2:23 دقیقه` به `1:30 دقیقه` شد، کاهش __۳۷٪__ 🎉.