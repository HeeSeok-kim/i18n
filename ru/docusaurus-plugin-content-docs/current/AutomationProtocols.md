---
id: automationProtocols
title: Протоколы автоматизации
---

С WebdriverIO вы можете выбирать между несколькими технологиями автоматизации при запуске ваших E2E-тестов локально или в облаке. По умолчанию WebdriverIO попытается запустить локальную сессию автоматизации, используя протокол [WebDriver Bidi](https://w3c.github.io/webdriver-bidi/).

## Протокол WebDriver Bidi

[WebDriver Bidi](https://w3c.github.io/webdriver-bidi/) - это протокол автоматизации для браузеров, использующий двунаправленную коммуникацию. Это преемник протокола [WebDriver](https://w3c.github.io/webdriver/) и предоставляет гораздо больше возможностей для интроспекции в различных сценариях тестирования.

Этот протокол в настоящее время находится в разработке, и в будущем могут быть добавлены новые примитивы. Все производители браузеров обязались реализовать этот веб-стандарт, и многие [примитивы](https://wpt.fyi/results/webdriver/tests/bidi?label=experimental&label=master&aligned) уже встроены в браузеры.

## Протокол WebDriver

> [WebDriver](https://w3c.github.io/webdriver/) - это интерфейс удаленного управления, который обеспечивает интроспекцию и контроль пользовательских агентов. Он предоставляет платформенно- и языково-нейтральный протокол, позволяющий внешним программам удаленно управлять поведением веб-браузеров.

Протокол WebDriver был разработан для автоматизации браузера с точки зрения пользователя, что означает, что всё, что может делать пользователь, вы можете делать с браузером. Он предоставляет набор команд, которые абстрагируют общие взаимодействия с приложением (например, навигацию, клики или чтение состояния элемента). Поскольку это веб-стандарт, он хорошо поддерживается всеми основными производителями браузеров и также используется в качестве базового протокола для мобильной автоматизации с помощью [Appium](http://appium.io).

Для использования этого протокола автоматизации вам нужен прокси-сервер, который переводит все команды и выполняет их в целевой среде (т.е. в браузере или мобильном приложении).

Для автоматизации браузера прокси-сервером обычно является драйвер браузера. Драйверы доступны для всех браузеров:

- Chrome – [ChromeDriver](http://chromedriver.chromium.org/downloads)
- Firefox – [Geckodriver](https://github.com/mozilla/geckodriver/releases)
- Microsoft Edge – [Edge Driver](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/)
- Internet Explorer – [InternetExplorerDriver](https://github.com/SeleniumHQ/selenium/wiki/InternetExplorerDriver)
- Safari – [SafariDriver](https://developer.apple.com/documentation/webkit/testing_with_webdriver_in_safari)

Для любого вида мобильной автоматизации вам потребуется установить и настроить [Appium](http://appium.io). Это позволит вам автоматизировать мобильные (iOS/Android) или даже настольные (macOS/Windows) приложения, используя ту же настройку WebdriverIO.

Также существует множество сервисов, которые позволяют запускать ваши автоматизированные тесты в облаке в высоком масштабе. Вместо того, чтобы настраивать все эти драйверы локально, вы можете просто общаться с этими сервисами (например, [Sauce Labs](https://saucelabs.com)) в облаке и проверять результаты на их платформе. Коммуникация между тестовым скриптом и средой автоматизации будет выглядеть следующим образом:

![WebDriver Setup](/img/webdriver.png)