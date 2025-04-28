---
id: sharding
title: التجزئة (Sharding)
---

بشكل افتراضي، يقوم WebdriverIO بتشغيل الاختبارات بالتوازي ويسعى للاستفادة المثلى من نواة وحدة المعالجة المركزية على جهازك. من أجل تحقيق المزيد من التوازي، يمكنك زيادة تنفيذ اختبار WebdriverIO عن طريق تشغيل الاختبارات على أجهزة متعددة في وقت واحد. نسمي هذا النمط من العملية "التجزئة" (Sharding).

## تجزئة الاختبارات بين أجهزة متعددة

لتجزئة مجموعة الاختبار، قم بتمرير `--shard=x/y` إلى سطر الأوامر. على سبيل المثال، لتقسيم المجموعة إلى أربعة أجزاء، كل منها يشغل ربع الاختبارات:

```sh
npx wdio run wdio.conf.js --shard=1/4
npx wdio run wdio.conf.js --shard=2/4
npx wdio run wdio.conf.js --shard=3/4
npx wdio run wdio.conf.js --shard=4/4
```

الآن، إذا قمت بتشغيل هذه الأجزاء بالتوازي على أجهزة كمبيوتر مختلفة، فإن مجموعة الاختبار الخاصة بك تكتمل بسرعة أربع مرات.

## مثال GitHub Actions

يدعم GitHub Actions [تجزئة الاختبارات بين وظائف متعددة](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs) باستخدام خيار [`jobs.<job_id>.strategy.matrix`](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix). سيقوم خيار المصفوفة بتشغيل وظيفة منفصلة لكل مجموعة ممكنة من الخيارات المقدمة.

يوضح المثال التالي كيفية تكوين وظيفة لتشغيل اختباراتك على أربعة أجهزة بالتوازي. يمكنك العثور على إعداد خط الأنابيب بالكامل في مشروع [Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate/blob/main/.github/workflows/test.yaml).

-   أولاً نقوم بإضافة خيار مصفوفة إلى تكوين الوظيفة الخاصة بنا مع خيار التجزئة الذي يحتوي على عدد الأجزاء التي نريد إنشاءها. `shard: [1, 2, 3, 4]` سيقوم بإنشاء أربعة أجزاء، كل منها برقم تجزئة مختلف.
-   ثم نقوم بتشغيل اختبارات WebdriverIO مع خيار `--shard ${{ matrix.shard }}/${{ strategy.job-total }}`. هذا سيكون أمر الاختبار لكل جزء.
-   أخيرًا نقوم بتحميل تقرير سجل wdio إلى GitHub Actions Artifacts. هذا سيجعل السجلات متاحة في حالة فشل الجزء.

يتم تعريف خط أنابيب الاختبار على النحو التالي:

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

هذا سيشغل جميع الأجزاء بالتوازي، مما يقلل وقت التنفيذ للاختبارات بمقدار 4:

![مثال GitHub Actions](/img/sharding.png "مثال GitHub Actions")

انظر الالتزام [`96d444e`](https://github.com/webdriverio/cucumber-boilerplate/commit/96d444ea23919389682b9b1c9408ed91c452c7f8) من مشروع [Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate) الذي قدم التجزئة إلى خط أنابيب الاختبار الخاص به مما ساعد على تقليل وقت التنفيذ الإجمالي من `2:23 دقيقة` إلى `1:30 دقيقة`، وهو تخفيض بنسبة __37%__ 🎉.