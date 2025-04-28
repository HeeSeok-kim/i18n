---
id: bestpractices
title: أفضل الممارسات
---

# أفضل الممارسات

يهدف هذا الدليل إلى مشاركة أفضل ممارساتنا التي تساعدك على كتابة اختبارات فعالة ومرنة.

## استخدام محددات مرنة

باستخدام محددات مرنة للتغييرات في DOM، ستحصل على اختبارات أقل عرضة للفشل أو حتى عدم فشلها عندما تتم إزالة فئة من عنصر ما على سبيل المثال.

يمكن تطبيق الفئات على عناصر متعددة ويجب تجنبها إذا أمكن ما لم تكن ترغب عمداً في جلب جميع العناصر بتلك الفئة.

```js
// 👎
await $('.button')
```

يجب أن تعيد جميع هذه المحددات عنصراً واحداً.

```js
// 👍
await $('aria/Submit')
await $('[test-id="submit-button"]')
await $('#submit-button')
```

__ملاحظة:__ لمعرفة جميع المحددات التي يدعمها WebdriverIO، تحقق من صفحة [المحددات](./Selectors.md) الخاصة بنا.

## الحد من عدد استعلامات العناصر

في كل مرة تستخدم فيها أمر [`$`](https://webdriver.io/docs/api/browser/$) أو [`$$`](https://webdriver.io/docs/api/browser/$$) (بما في ذلك ربطهما)، يحاول WebdriverIO تحديد موقع العنصر في DOM. هذه الاستعلامات مكلفة لذا يجب أن تحاول الحد منها قدر الإمكان.

استعلامات ثلاثة عناصر.

```js
// 👎
await $('table').$('tr').$('td')
```

استعلام عنصر واحد فقط.

``` js
// 👍
await $('table tr td')
```

المرة الوحيدة التي يجب عليك فيها استخدام التسلسل هي عندما تريد الجمع بين استراتيجيات محددات مختلفة [selector strategies](https://webdriver.io/docs/selectors/#custom-selector-strategies).
في المثال نستخدم [المحددات العميقة](https://webdriver.io/docs/selectors#deep-selectors)، وهي استراتيجية للدخول إلى shadow DOM للعنصر.

``` js
// 👍
await $('custom-datepicker').$('#calendar').$('aria/Select')
```

### تفضيل تحديد عنصر واحد بدلاً من أخذه من قائمة

ليس من الممكن دائمًا القيام بذلك ولكن باستخدام فئات CSS الزائفة مثل [:nth-child](https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-child) يمكنك مطابقة العناصر بناءً على فهارس العناصر في قائمة الأطفال الخاصة بآبائهم.

استعلام جميع صفوف الجدول.

```js
// 👎
await $$('table tr')[15]
```

استعلام صف جدول واحد.

```js
// 👍
await $('table tr:nth-child(15)')
```

## استخدام التأكيدات المدمجة

لا تستخدم التأكيدات اليدوية التي لا تنتظر تلقائيًا تطابق النتائج لأن هذا سيؤدي إلى اختبارات غير مستقرة.

```js
// 👎
expect(await button.isDisplayed()).toBe(true)
```

باستخدام التأكيدات المدمجة، سينتظر WebdriverIO تلقائيًا تطابق النتيجة الفعلية مع النتيجة المتوقعة، مما يؤدي إلى اختبارات مرنة.
يحقق ذلك من خلال إعادة محاولة التأكيد تلقائيًا حتى يتم اجتيازه أو انتهاء المهلة.

```js
// 👍
await expect(button).toBeDisplayed()
```

## التحميل البطيء وتسلسل الوعود

يمتلك WebdriverIO بعض الحيل عندما يتعلق الأمر بكتابة كود نظيف حيث يمكنه تحميل العنصر بشكل كسول مما يسمح لك بتسلسل وعودك وتقليل استخدام `await`. كما يسمح لك بتمرير العنصر كـ ChainablePromiseElement بدلاً من Element ولاستخدام أسهل مع كائنات الصفحة.

فمتى يجب عليك استخدام `await`؟
يجب عليك دائمًا استخدام `await` باستثناء أوامر `$` و `$$`.

```js
// 👎
const div = await $('div')
const button = await div.$('button')
await button.click()
// أو
await (await (await $('div')).$('button')).click()
```

```js
// 👍
const button = $('div').$('button')
await button.click()
// أو
await $('div').$('button').click()
```

## لا تفرط في استخدام الأوامر والتأكيدات

عند استخدام expect.toBeDisplayed فإنك تنتظر ضمنيًا أيضًا وجود العنصر. لا داعي لاستخدام أوامر waitForXXX عندما يكون لديك بالفعل تأكيد يقوم بنفس الشيء.

```js
// 👎
await button.waitForExist()
await expect(button).toBeDisplayed()

// 👎
await button.waitForDisplayed()
await expect(button).toBeDisplayed()

// 👍
await expect(button).toBeDisplayed()
```

لا حاجة للانتظار حتى يوجد عنصر أو يظهر عند التفاعل أو عند تأكيد شيء ما مثل نصه ما لم يكن العنصر يمكن أن يكون غير مرئي بشكل صريح (opacity: 0 على سبيل المثال) أو يمكن تعطيله بشكل صريح (سمة disabled على سبيل المثال) وفي هذه الحالة يكون الانتظار حتى يظهر العنصر منطقيًا.

```js
// 👎
await expect(button).toBeExisting()
await expect(button).toHaveText('Submit')

// 👎
await expect(button).toBeDisplayed()
await expect(button).toHaveText('Submit')

// 👎
await expect(button).toBeDisplayed()
await button.click()
```

```js
// 👍
await button.click()

// 👍
await expect(button).toHaveText('Submit')
```

## الاختبارات الديناميكية

استخدم متغيرات البيئة لتخزين بيانات الاختبار الديناميكية مثل بيانات الاعتماد السرية، في بيئتك بدلاً من ترميزها في الاختبار. انتقل إلى صفحة [اختبارات المعلمات](parameterize-tests) لمزيد من المعلومات حول هذا الموضوع.

## تنظيف الكود الخاص بك

استخدم eslint لتنظيف الكود الخاص بك يمكنك اكتشاف الأخطاء مبكرًا، استخدم [قواعد التنظيف](https://www.npmjs.com/package/eslint-plugin-wdio) الخاصة بنا للتأكد من تطبيق بعض أفضل الممارسات دائمًا.

## لا تستخدم الإيقاف المؤقت

قد يكون من المغري استخدام أمر الإيقاف المؤقت ولكن استخدام هذا فكرة سيئة لأنه ليس مرناً وسيؤدي فقط إلى اختبارات غير مستقرة على المدى الطويل.

```js
// 👎
await nameInput.setValue('Bob')
await browser.pause(200) // wait for submit button to enable
await submitFormButton.click()

// 👍
await nameInput.setValue('Bob')
await submitFormButton.waitForEnabled()
await submitFormButton.click()
```

## حلقات غير متزامنة

عندما يكون لديك بعض التعليمات البرمجية غير المتزامنة التي تريد تكرارها، من المهم معرفة أن ليست كل الحلقات يمكنها القيام بذلك.
على سبيل المثال، لا تسمح وظيفة forEach الخاصة بالمصفوفة بمكالمات غير متزامنة كما يمكن قراءتها على [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach).

__ملاحظة:__ لا يزال بإمكانك استخدام هذه عندما لا تحتاج إلى أن تكون العملية متزامنة كما هو موضح في هذا المثال `console.log(await $$('h1').map((h1) => h1.getText()))`.

فيما يلي بعض الأمثلة على ما يعنيه هذا.

ما يلي لن يعمل لأن المكالمات غير المتزامنة غير مدعومة.

```js
// 👎
const characters = 'this is some example text that should be put in order'
characters.forEach(async (character) => {
    await browser.keys(character)
})
```

ما يلي سيعمل.

```js
// 👍
const characters = 'this is some example text that should be put in order'
for (const character of characters) {
    await browser.keys(character)
}
```

## حافظ على البساطة

في بعض الأحيان نرى مستخدمينا يقومون بتخطيط بيانات مثل النص أو القيم. غالبًا ما يكون هذا غير ضروري وغالبًا ما يكون رائحة كود، تحقق من الأمثلة أدناه لمعرفة سبب ذلك.

```js
// 👎 معقد للغاية، تأكيد متزامن، استخدم التأكيدات المدمجة لمنع الاختبارات غير المستقرة
const headerText = ['Products', 'Prices']
const texts = await $$('th').map(e => e.getText());
expect(texts).toBe(headerText)

// 👎 معقد للغاية
const headerText = ['Products', 'Prices']
const columns = await $$('th');
await expect(columns).toBeElementsArrayOfSize(2);
for (let i = 0; i < columns.length; i++) {
    await expect(columns[i]).toHaveText(headerText[i]);
}

// 👎 يجد العناصر حسب نصها ولكن لا يأخذ في الاعتبار موضع العناصر
await expect($('th=Products')).toExist();
await expect($('th=Prices')).toExist();
```

```js
// 👍 استخدم معرفات فريدة (غالبًا ما تستخدم للعناصر المخصصة)
await expect($('[data-testid="Products"]')).toHaveText('Products');
// 👍 أسماء إمكانية الوصول (غالبًا ما تستخدم لعناصر html الأصلية)
await expect($('aria/Product Prices')).toHaveText('Prices');
```

شيء آخر نراه أحيانًا هو أن الأشياء البسيطة لها حل معقد للغاية.

```js
// 👎
class BadExample {
    public async selectOptionByValue(value: string) {
        await $('select').click();
        await $$('option')
            .map(async function (element) {
                const hasValue = (await element.getValue()) === value;
                if (hasValue) {
                    await $(element).click();
                }
                return hasValue;
            });
    }

    public async selectOptionByText(text: string) {
        await $('select').click();
        await $$('option')
            .map(async function (element) {
                const hasText = (await element.getText()) === text;
                if (hasText) {
                    await $(element).click();
                }
                return hasText;
            });
    }
}
```

```js
// 👍
class BetterExample {
    public async selectOptionByValue(value: string) {
        await $('select').click();
        await $(`option[value=${value}]`).click();
    }

    public async selectOptionByText(text: string) {
        await $('select').click();
        await $(`option=${text}]`).click();
    }
}
```

## تنفيذ الكود بالتوازي

إذا كنت لا تهتم بالترتيب الذي يتم فيه تشغيل بعض الكود، فيمكنك استخدام [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) لتسريع التنفيذ.

__ملاحظة:__ نظرًا لأن هذا يجعل الكود أصعب في القراءة، فيمكنك تجريده باستخدام كائن صفحة أو وظيفة، على الرغم من أنه يجب عليك أيضًا التساؤل عما إذا كانت الفائدة في الأداء تستحق تكلفة القراءة.

```js
// 👎
await name.setValue('Bob')
await email.setValue('bob@webdriver.io')
await age.setValue('50')
await submitFormButton.waitForEnabled()
await submitFormButton.click()

// 👍
await Promise.all([
    name.setValue('Bob'),
    email.setValue('bob@webdriver.io'),
    age.setValue('50'),
])
await submitFormButton.waitForEnabled()
await submitFormButton.click()
```

إذا تم تجريده، فقد يبدو شيئًا مثل ما يلي حيث يتم وضع المنطق في طريقة تسمى submitWithDataOf ويتم استرداد البيانات بواسطة فئة Person.

```js
// 👍
await form.submitData(new Person('bob@webdriver.io'))
```