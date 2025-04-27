---
id: sharding
title: ஷார்டிங்
---

இயல்பாக, WebdriverIO சோதனைகளை இணையாக இயக்குகிறது மற்றும் உங்கள் கணினியில் உள்ள CPU கோர்களின் சிறந்த பயன்பாட்டிற்காக முயற்சி செய்கிறது. மேலும் அதிகமான இணையாக்கத்தை அடைய, பல இயந்திரங்களில் ஒரே நேரத்தில் சோதனைகளை இயக்குவதன் மூலம் WebdriverIO சோதனை செயல்பாட்டை மேலும் அளவிடலாம். இந்த செயல்பாட்டு முறையை நாங்கள் "ஷார்டிங்" என்று அழைக்கிறோம்.

## பல இயந்திரங்களுக்கு இடையில் சோதனைகளை ஷார்டிங் செய்தல்

சோதனை தொகுப்பை ஷார்ட் செய்ய, கட்டளை வரியில் `--shard=x/y` ஐ பாஸ் செய்யவும். எடுத்துக்காட்டாக, தொகுப்பை நான்கு ஷார்டுகளாகப் பிரிக்க, ஒவ்வொன்றும் சோதனைகளில் ஒரு நான்காவது பங்கை இயக்குகிறது:

```sh
npx wdio run wdio.conf.js --shard=1/4
npx wdio run wdio.conf.js --shard=2/4
npx wdio run wdio.conf.js --shard=3/4
npx wdio run wdio.conf.js --shard=4/4
```

இப்போது, நீங்கள் இந்த ஷார்டுகளை வெவ்வேறு கணினிகளில் இணையாக இயக்கினால், உங்கள் சோதனை தொகுப்பு நான்கு மடங்கு வேகமாக முடிக்கப்படும்.

## GitHub Actions உதாரணம்

GitHub Actions [பல வேலைகளுக்கு இடையே சோதனைகளை ஷார்டிங் செய்வதை](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs) [`jobs.<job_id>.strategy.matrix`](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix) விருப்பத்தைப் பயன்படுத்தி ஆதரிக்கிறது. மேட்ரிக்ஸ் விருப்பம் வழங்கப்பட்ட விருப்பங்களின் ஒவ்வொரு சாத்தியமான கலவைக்கும் ஒரு தனி வேலையை இயக்கும்.

பின்வரும் உதாரணம் நான்கு இயந்திரங்களில் உங்கள் சோதனைகளை இணையாக இயக்க ஒரு வேலையை எவ்வாறு கட்டமைப்பது என்பதைக் காட்டுகிறது. முழு பைப்லைன் அமைப்பையும் [Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate/blob/main/.github/workflows/test.yaml) திட்டத்தில் காணலாம்.

-   முதலில், நாம் உருவாக்க விரும்பும் ஷார்டுகளின் எண்ணிக்கையைக் கொண்ட ஷார்ட் விருப்பத்துடன் நமது வேலை கட்டமைப்பிற்கு ஒரு மேட்ரிக்ஸ் விருப்பத்தைச் சேர்க்கிறோம். `shard: [1, 2, 3, 4]` நான்கு ஷார்டுகளை உருவாக்கும், ஒவ்வொன்றும் வெவ்வேறு ஷார்ட் எண்ணைக் கொண்டிருக்கும்.
-   பின்னர் நாம் `--shard ${{ matrix.shard }}/${{ strategy.job-total }}` விருப்பத்துடன் எங்கள் WebdriverIO சோதனைகளை இயக்குகிறோம். இது ஒவ்வொரு ஷார்டுக்கும் எங்கள் சோதனை கட்டளையாக இருக்கும்.
-   இறுதியாக நாங்கள் எங்கள் wdio பதிவு அறிக்கையை GitHub Actions Artifacts-க்கு பதிவேற்றுகிறோம். ஷார்ட் தோல்வியடைந்தால் இது பதிவுகளைக் கிடைக்கச் செய்யும்.

சோதனை பைப்லைன் பின்வருமாறு வரையறுக்கப்பட்டுள்ளது:

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

இது அனைத்து ஷார்டுகளையும் இணையாக இயக்கும், சோதனைகளின் செயல்படுத்தும் நேரத்தை 4 மடங்கு குறைக்கும்:

![GitHub Actions உதாரணம்](/img/sharding.png "GitHub Actions உதாரணம்")

[Cucumber Boilerplate](https://github.com/webdriverio/cucumber-boilerplate) திட்டத்தில் இருந்து [`96d444e`](https://github.com/webdriverio/cucumber-boilerplate/commit/96d444ea23919389682b9b1c9408ed91c452c7f8) கமிட்டைப் பார்க்கவும், இது அதன் சோதனை பைப்லைனில் ஷார்டிங்கை அறிமுகப்படுத்தியது, இது ஒட்டுமொத்த செயல்படுத்தும் நேரத்தை `2:23 நிமிடங்களில்` இருந்து `1:30 நிமிடங்கள்` வரை குறைக்க உதவியது, __37%__ குறைப்பு 🎉.