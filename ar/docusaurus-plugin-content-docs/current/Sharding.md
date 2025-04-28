---
id: sharding
title: التجزئة
---

بشكل افتراضي، يقوم WebdriverIO بتشغيل الاختبارات بالتوازي ويسعى للاستفادة المثلى من نواة المعالج في جهازك. لتحقيق المزيد من التوازي، يمكنك زيادة توسيع تنفيذ اختبارات WebdriverIO عن طريق تشغيل الاختبارات على عدة أجهزة في وقت واحد. نسمي هذا الوضع من التشغيل "التجزئة".

## تجزئة الاختبارات بين أجهزة متعددة

لتجزئة مجموعة الاختبارات، قم بتمرير `--shard=x/y` إلى سطر الأوامر. على سبيل المثال، لتقسيم المجموعة إلى أربعة أجزاء، كل منها يقوم بتشغيل ربع الاختبارات:

```sh
npx wdio run wdio.conf.js --shard=1/4
npx wdio run wdio.conf.js --shard=2/4
npx wdio run wdio.conf.js --shard=3/4
npx wdio run wdio.conf.js --shard=4/4
```

الآن، إذا قمت بتشغيل هذه الأجزاء بالتوازي على أجهزة مختلفة، ستكتمل مجموعة الاختبارات الخاصة بك بسرعة أربع مرات أسرع.

## مثال GitHub Actions

يدعم GitHub Actions [تجزئة الاختبارات بين وظائف متعددة](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs) باستخدام خيار [`jobs.<job_id>.strategy.matrix`](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix). سيقوم خيار المصفوفة بتشغيل وظيفة منفصلة لكل تركيبة ممكنة من الخيارات المقدمة.

يوضح المثال التالي كيفية تكوين وظيفة لتشغيل اختباراتك على أربعة أجهزة بالتوازي. يمكنك العثور على إعداد سير العمل الكامل في مشروع [Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate/blob/main/.github/workflows/test.yaml).

-   أولاً نضيف خيار المصفوفة إلى تكوين الوظيفة الخاصة بنا مع خيار التجزئة الذي يحتوي على عدد الأجزاء التي نريد إنشاءها. `shard: [1, 2, 3, 4]` سينشئ أربعة أجزاء، كل منها برقم تجزئة مختلف.
-   ثم نقوم بتشغيل اختبارات WebdriverIO الخاصة بنا باستخدام الخيار `--shard ${{ matrix.shard }}/${{ strategy.job-total }}`. هذا سيكون أمر الاختبار لكل جزء.
-   أخيرًا، نقوم بتحميل تقرير سجل wdio إلى الملفات المستخرجة من GitHub Actions. سيجعل هذا السجلات متاحة في حالة فشل التجزئة.

يتم تعريف سير عمل الاختبار على النحو التالي:

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

سيؤدي هذا إلى تشغيل جميع الأجزاء بالتوازي، مما يقلل وقت التنفيذ للاختبارات بمقدار 4:

![مثال GitHub Actions](/img/sharding.png "مثال GitHub Actions")

انظر الالتزام [`96d444e`](https://github.com/webdriverio/cucumber-boilerplate/commit/96d444ea23919389682b9b1c9408ed91c452c7f8) من مشروع [Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate) الذي قدم التجزئة إلى سير عمل الاختبار الخاص به والذي ساعد في تقليل وقت التنفيذ الإجمالي من `2:23 دقيقة` إلى `1:30 دقيقة`، بانخفاض قدره __37%__ 🎉.