---
id: sharding
title: شاردینگ
---

به طور پیش‌فرض، WebdriverIO تست‌ها را به صورت موازی اجرا می‌کند و برای استفاده بهینه از هسته‌های CPU روی دستگاه شما تلاش می‌کند. برای دستیابی به موازی‌سازی بیشتر، می‌توانید اجرای تست WebdriverIO را با اجرای همزمان تست‌ها روی چندین ماشین مقیاس‌پذیر کنید. ما این حالت عملیات را "شاردینگ" می‌نامیم.

## شاردینگ تست‌ها بین چندین ماشین

برای شارد کردن مجموعه تست، `--shard=x/y` را به خط فرمان ارسال کنید. به عنوان مثال، برای تقسیم مجموعه به چهار شارد، که هر کدام یک چهارم تست‌ها را اجرا می‌کنند:

```sh
npx wdio run wdio.conf.js --shard=1/4
npx wdio run wdio.conf.js --shard=2/4
npx wdio run wdio.conf.js --shard=3/4
npx wdio run wdio.conf.js --shard=4/4
```

حال، اگر این شاردها را به طور موازی روی کامپیوترهای مختلف اجرا کنید، مجموعه تست شما چهار برابر سریع‌تر تکمیل می‌شود.

## مثال GitHub Actions

GitHub Actions از [شارد کردن تست‌ها بین چندین کار](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs) با استفاده از گزینه [`jobs.<job_id>.strategy.matrix`](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix) پشتیبانی می‌کند. گزینه ماتریس یک کار جداگانه برای هر ترکیب ممکن از گزینه‌های ارائه شده اجرا می‌کند.

مثال زیر نحوه پیکربندی یک کار برای اجرای تست‌ها روی چهار ماشین به صورت موازی را نشان می‌دهد. می‌توانید کل تنظیمات پایپلاین را در پروژه [Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate/blob/main/.github/workflows/test.yaml) مشاهده کنید.

-   ابتدا یک گزینه ماتریس به پیکربندی کار خود با گزینه شارد که حاوی تعداد شاردهایی است که می‌خواهیم ایجاد کنیم، اضافه می‌کنیم. `shard: [1, 2, 3, 4]` چهار شارد ایجاد می‌کند، هر کدام با شماره شارد متفاوت.
-   سپس تست‌های WebdriverIO خود را با گزینه `--shard ${{ matrix.shard }}/${{ strategy.job-total }}` اجرا می‌کنیم. این دستور تست ما برای هر شارد خواهد بود.
-   در نهایت، گزارش لاگ wdio خود را به آرتیفکت‌های GitHub Actions آپلود می‌کنیم. این کار لاگ‌ها را در صورت شکست شارد در دسترس قرار می‌دهد.

پایپلاین تست به شرح زیر تعریف شده است:

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

این کار تمام شاردها را به طور موازی اجرا می‌کند و زمان اجرای تست‌ها را به میزان ۴ برابر کاهش می‌دهد:

![مثال GitHub Actions](/img/sharding.png "مثال GitHub Actions")

کامیت [`96d444e`](https://github.com/webdriverio/cucumber-boilerplate/commit/96d444ea23919389682b9b1c9408ed91c452c7f8) از پروژه [Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate) را ببینید که شاردینگ را به پایپلاین تست خود معرفی کرده است و به کاهش زمان کلی اجرا از `2:23 دقیقه` به `1:30 دقیقه`، یعنی کاهش __37%__ کمک کرده است 🎉.