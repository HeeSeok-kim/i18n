---
id: bestpractices
title: أفضل الممارسات
---

# أفضل الممارسات

يهدف هذا الدليل إلى مشاركة أفضل ممارساتنا التي تساعدك على كتابة اختبارات فعالة ومرنة.

## استخدام محددات مرنة

باستخدام محددات مرنة للتغييرات في DOM، ستحصل على اختبارات أقل فشلاً أو حتى بدون فشل عندما تتم إزالة صنف من عنصر ما.

يمكن تطبيق الصفوف على عناصر متعددة ويجب تجنبها إن أمكن ما لم تكن ترغب في جلب جميع العناصر بهذا الصنف.

```js
// 👎
await $('.button')
```

يجب أن تعيد جميع هذه المحددات عنصرًا واحدًا.

```js
// 👍
await $('aria/Submit')
await $('[test-id="submit-button"]')
await $('#submit-button')
```

__ملاحظة:__ لمعرفة جميع المحددات التي يدعمها WebdriverIO، راجع صفحة [المحددات](./Selectors.md) الخاصة بنا.

## الحد من كمية استعلامات العناصر

في كل مرة تستخدم فيها أمر [`$`](https://webdriver.io/docs/api/browser/$) أو [`$$`](https://webdriver.io/docs/api/browser/$$) (وهذا يشمل ربطهما معًا)، يحاول WebdriverIO تحديد موقع العنصر في DOM. هذه الاستعلامات مكلفة لذا يجب عليك محاولة الحد منها قدر الإمكان.

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

الوقت الوحيد الذي يجب أن تستخدم فيه التسلسل هو عندما تريد الجمع بين استراتيجيات [محددات مختلفة](https://webdriver.io/docs/selectors/#custom-selector-strategies).
في المثال نستخدم [المحددات العميقة](https://webdriver.io/docs/selectors#deep-selectors)، وهي استراتيجية للدخول إلى shadow DOM لعنصر ما.

``` js
// 👍
await $('custom-datepicker').$('#calendar').$('aria/Select')
```

### يفضل تحديد موقع عنصر واحد بدلاً من أخذ عنصر من قائمة

ليس من الممكن دائمًا القيام بذلك ولكن باستخدام أشباه فئات CSS مثل [:nth-child](https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-child) يمكنك مطابقة العناصر بناءً على فهارس العناصر في قائمة الأطفال التابعة لآبائهم.

استعلامات جميع صفوف الجدول.

```js
// 👎
await $$('table tr')[15]
```

استعلام صف جدول واحد.

```js
// 👍
await $('table tr:nth-child(15)')
```

## استخدم التأكيدات المدمجة

لا تستخدم تأكيدات يدوية لا تنتظر تلقائيًا حتى تتطابق النتائج لأن هذا سيؤدي إلى اختبارات غير مستقرة.

```js
// 👎
expect(await button.isDisplayed()).toBe(true)
```

باستخدام التأكيدات المدمجة، سينتظر WebdriverIO تلقائيًا حتى تتطابق النتيجة الفعلية مع النتيجة المتوقعة، مما يؤدي إلى اختبارات مرنة.
يحقق ذلك عن طريق إعادة محاولة التأكيد تلقائيًا حتى ينجح أو ينفد الوقت.

```js
// 👍
await expect(button).toBeDisplayed()
```

## التحميل البطيء وسلسلة الوعود

يمتلك WebdriverIO بعض الحيل عندما يتعلق الأمر بكتابة شفرة نظيفة حيث يمكنه تحميل العنصر بشكل كسول مما يسمح لك بربط وعودك وتقليل كمية `await`. هذا يسمح لك أيضًا بتمرير العنصر كـ ChainablePromiseElement بدلاً من Element ولاستخدام أسهل مع كائنات الصفحة.

إذن متى يجب عليك استخدام `await`؟
يجب عليك دائمًا استخدام `await` مع استثناء أمر `$` و `$$`.

```js
// 👎
const div = await $('div')
const button = await div.$('button')
await button.click()
// or
await (await (await $('div')).$('button')).click()
```

```js
// 👍
const button = $('div').$('button')
await button.click()
// or
await $('div').$('button').click()
```

## لا تفرط في استخدام الأوامر والتأكيدات

عند استخدام expect.toBeDisplayed فإنك تنتظر ضمنياً أيضاً وجود العنصر. ليست هناك حاجة لاستخدام أوامر waitForXXX عندما يكون لديك بالفعل تأكيد يقوم بنفس الشيء.

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

لا حاجة للانتظار حتى يوجد عنصر أو يتم عرضه عند التفاعل أو عند تأكيد شيء ما مثل نصه إلا إذا كان العنصر يمكن أن يكون غير مرئي صراحة (على سبيل المثال opacity: 0) أو يمكن تعطيله صراحة (على سبيل المثال سمة disabled) وفي هذه الحالة يكون الانتظار حتى يتم عرض العنصر منطقياً.

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

استخدم متغيرات البيئة لتخزين بيانات الاختبار الديناميكية مثل بيانات الاعتماد السرية، في بيئتك بدلاً من ترميزها في الاختبار. انتقل إلى صفحة [معلمة الاختبارات](parameterize-tests) لمزيد من المعلومات حول هذا الموضوع.

## تدقيق الكود الخاص بك

باستخدام eslint لتدقيق الكود الخاص بك، يمكنك اكتشاف الأخطاء مبكراً، استخدم [قواعد التدقيق](https://www.npmjs.com/package/eslint-plugin-wdio) الخاصة بنا للتأكد من تطبيق بعض أفضل الممارسات دائمًا.

## لا توقف التنفيذ

قد يكون من المغري استخدام أمر الإيقاف المؤقت ولكن استخدام هذا فكرة سيئة لأنه ليس مرناً وسيؤدي فقط إلى اختبارات غير مستقرة على المدى الطويل.

```js
// 👎
await nameInput.setValue('Bob')
await browser.pause(200) // انتظار حتى يتم تمكين زر الإرسال
await submitFormButton.click()

// 👍
await nameInput.setValue('Bob')
await submitFormButton.waitForEnabled()
await submitFormButton.click()
```

## حلقات غير متزامنة

عندما يكون لديك بعض الكود غير المتزامن الذي تريد تكراره، من المهم أن تعرف أنه ليس كل الحلقات يمكنها القيام بذلك.
على سبيل المثال، لا تسمح دالة forEach الخاصة بالمصفوفة بالاستدعاءات غير المتزامنة كما يمكن قراءتها على [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach).

__ملاحظة:__ لا يزال بإمكانك استخدام هذه عندما لا تحتاج إلى أن تكون العملية متزامنة كما هو موضح في هذا المثال `console.log(await $$('h1').map((h1) => h1.getText()))`.

فيما يلي بعض الأمثلة على ما يعنيه هذا.

ما يلي لن يعمل لأن الاستدعاءات غير المتزامنة غير مدعومة.

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

في بعض الأحيان نرى أن مستخدمينا يقومون بتعيين بيانات مثل النص أو القيم. غالباً ما لا يكون هذا ضرورياً وغالباً ما تكون رائحة الكود، تحقق من الأمثلة أدناه لمعرفة سبب ذلك.

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

// 👎 يجد العناصر من خلال النص الخاص بها ولكن لا يأخذ في الاعتبار موضع العناصر
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

إذا كنت لا تهتم بالترتيب الذي يتم فيه تشغيل بعض الكود، يمكنك الاستفادة من [`Promise.all`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) لتسريع التنفيذ.

__ملاحظة:__ نظرًا لأن هذا يجعل الكود أصعب في القراءة، يمكنك تجريد هذا باستخدام كائن صفحة أو دالة، على الرغم من أنه يجب عليك أيضًا أن تسأل ما إذا كانت الفائدة في الأداء تستحق تكلفة سهولة القراءة.

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

إذا تم تجريده، فقد يبدو شيئًا مثل ما هو موضح أدناه حيث يتم وضع المنطق في طريقة تسمى submitWithDataOf ويتم استرداد البيانات بواسطة فئة Person.

```js
// 👍
await form.submitData(new Person('bob@webdriver.io'))
```