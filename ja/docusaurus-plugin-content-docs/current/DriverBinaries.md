---
id: driverbinaries
title: ドライバーバイナリ
---

WebDriverプロトコルに基づく自動化を実行するには、自動化コマンドを変換してブラウザで実行できるブラウザドライバーが必要です。

## 自動セットアップ

WebdriverIO `v8.14`以降では、WebdriverIOがブラウザドライバーを自動的に処理するため、手動でダウンロードしてセットアップする必要はありません。テストしたいブラウザを指定するだけで、残りはWebdriverIOが行います。

### 自動化レベルのカスタマイズ

WebdriverIOには3つの自動化レベルがあります：

**1. [@puppeteer/browsers](https://www.npmjs.com/package/@puppeteer/browsers)を使用してブラウザをダウンロードしインストールする。**

[capabilities](configuration#capabilities-1)設定で`browserName`/`browserVersion`の組み合わせを指定すると、マシンに既存のインストールがあるかどうかに関係なく、WebdriverIOはリクエストされた組み合わせをダウンロードしてインストールします。`browserVersion`を省略すると、WebdriverIOは最初に[locate-app](https://www.npmjs.com/package/locate-app)を使用して既存のインストールを見つけて使用しようとし、見つからない場合は現在の安定版ブラウザリリースをダウンロードしてインストールします。`browserVersion`の詳細については、[こちら](capabilities#automate-different-browser-channels)を参照してください。

:::caution

自動ブラウザセットアップはMicrosoft Edgeをサポートしていません。現在、Chrome、Chromium、Firefoxのみがサポートされています。

:::

WebdriverIOによって自動検出できない場所にブラウザがインストールされている場合は、ブラウザバイナリを指定することができます。これにより自動ダウンロードとインストールが無効になります。

```ts
{
    capabilities: [
        {
            browserName: 'chrome', // または 'firefox' や 'chromium'
            'goog:chromeOptions': { // または 'moz:firefoxOptions' や 'wdio:chromedriverOptions'
                binary: '/path/to/chrome'
            },
        }
    ]
}
```

**2. [Chromedriver](https://www.npmjs.com/package/chromedriver)、[Edgedriver](https://www.npmjs.com/package/edgedriver)、[Geckodriver](https://www.npmjs.com/package/geckodriver)を使用してドライバーをダウンロードしインストールする。**

設定でドライバーの[binary](capabilities#binary)が指定されていない限り、WebdriverIOは常にこれを行います：

```ts
{
    capabilities: [
        {
            browserName: 'chrome', // または 'firefox'、'msedge'、'safari'、'chromium'
            'wdio:chromedriverOptions': { // または 'wdio:geckodriverOptions'、'wdio:edgedriverOptions'
                binary: '/path/to/chromedriver' // または 'geckodriver'、'msedgedriver'
            }
        }
    ]
}
```

:::info

WebdriverIOはSafariドライバーを自動的にダウンロードしません。それはすでにmacOSにインストールされているためです。

:::

:::caution

ブラウザの`binary`を指定して対応するドライバーの`binary`を省略したり、その逆をしたりすることは避けてください。`binary`値の片方だけが指定されている場合、WebdriverIOはそれと互換性のあるブラウザ/ドライバーを使用またはダウンロードしようとします。ただし、一部のシナリオでは互換性のない組み合わせになる可能性があります。したがって、バージョンの非互換性による問題を避けるため、常に両方を指定することをお勧めします。

:::

**3. ドライバーの起動/停止。**

デフォルトでは、WebdriverIOは未使用のポートを使用して自動的にドライバーを起動および停止します。以下の設定のいずれかを指定すると、この機能は無効になり、手動でドライバーを起動および停止する必要があります：

- [port](configuration#port)に任意の値を指定。
- [protocol](configuration#protocol)、[hostname](configuration#hostname)、[path](configuration#path)にデフォルトと異なる値を指定。
- [user](configuration#user)と[key](configuration#key)の両方に値を指定。

## 手動セットアップ

以下では、各ドライバーを個別にセットアップする方法について説明します。すべてのドライバーのリストは[`awesome-selenium`](https://github.com/christian-bromann/awesome-selenium#driver) READMEで確認できます。

:::tip

モバイルやその他のUIプラットフォームをセットアップする場合は、[Appium Setup](appium)ガイドをご覧ください。

:::

### Chromedriver

Chromeを自動化するには、[プロジェクトウェブサイト](http://chromedriver.chromium.org/downloads)から直接Chromedriverをダウンロードするか、NPMパッケージを通じてダウンロードできます：

```bash npm2yarn
npm install -g chromedriver
```

その後、次のように起動できます：

```sh
chromedriver --port=4444 --verbose
```

### Geckodriver

Firefoxを自動化するには、お使いの環境に合わせた最新バージョンの`geckodriver`をダウンロードし、プロジェクトディレクトリに展開します：

<Tabs
  defaultValue="npm"
  values={[
    {label: 'NPM', value: 'npm'},
    {label: 'Curl', value: 'curl'},
    {label: 'Brew', value: 'brew'},
    {label: 'Windows (64 bit / Chocolatey)', value: 'chocolatey'},
    {label: 'Windows (64 bit / Powershell) DevTools', value: 'powershell'},
  ]
}>
<TabItem value="npm">

```bash npm2yarn
npm install geckodriver
```

</TabItem>
<TabItem value="curl">

Linux:

```sh
curl -L https://github.com/mozilla/geckodriver/releases/download/v0.24.0/geckodriver-v0.24.0-linux64.tar.gz | tar xz
```

MacOS (64 bit):

```sh
curl -L https://github.com/mozilla/geckodriver/releases/download/v0.24.0/geckodriver-v0.24.0-macos.tar.gz | tar xz
```

</TabItem>
<TabItem value="brew">

```sh
brew install geckodriver
```

</TabItem>
<TabItem value="chocolatey">

```sh
choco install selenium-gecko-driver
```

</TabItem>
<TabItem value="powershell">

```sh
# Run as privileged session. Right-click and set 'Run as Administrator'
# Use geckodriver-v0.24.0-win32.zip for 32 bit Windows
$url = "https://github.com/mozilla/geckodriver/releases/download/v0.24.0/geckodriver-v0.24.0-win64.zip"
$output = "geckodriver.zip" # will drop into current directory unless defined otherwise
$unzipped_file = "geckodriver" # will unzip to this folder name

# By default, Powershell uses TLS 1.0 the site security requires TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12

# Downloads Geckodriver
Invoke-WebRequest -Uri $url -OutFile $output

# Unzip Geckodriver
Expand-Archive $output -DestinationPath $unzipped_file
cd $unzipped_file

# Globally Set Geckodriver to PATH
[System.Environment]::SetEnvironmentVariable("PATH", "$Env:Path;$pwd\geckodriver.exe", [System.EnvironmentVariableTarget]::Machine)
```

</TabItem>
</Tabs>

**注意：** 他の`geckodriver`リリースは[こちら](https://github.com/mozilla/geckodriver/releases)で入手できます。ダウンロード後、次のようにドライバーを起動できます：

```sh
/path/to/binary/geckodriver --port 4444
```

### Edgedriver

Microsoft Edge用のドライバーは[プロジェクトウェブサイト](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/)からダウンロードするか、NPMパッケージとして次のようにインストールできます：

```sh
npm install -g edgedriver
edgedriver --version # 出力: Microsoft Edge WebDriver 115.0.1901.203 (a5a2b1779bcfe71f081bc9104cca968d420a89ac)
```

### Safaridriver

SafaridriverはMacOSにプリインストールされており、次のように直接起動できます：

```sh
safaridriver -p 4444
```