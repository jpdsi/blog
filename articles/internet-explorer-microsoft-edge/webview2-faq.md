---
title: WebView2 のよくあるご質問
date: 2023-05-23
tags: 
  - Microsoft Edge
  - Microsoft WebView2 Runtime
  - WebView2
---

みなさんこんにちは。日本マイクロソフトの IE/Edge サポートチームです。

今回はお客様からよくご質問をいただく Microsoft Edge WebView2 (以下 WebView2) のお話をお伝えしたいと思います。

最近では WebView2 Runtime を利用する WebView2 アプリケーションも増えており、その折で端末に勝手に WebView2 Runtime がインストールされているのはなぜか？といったお問い合わせをよくいただきます。 WebView2 Runtime と WebView2 アプリケーションの違いや、そもそも Microsoft Edge とはなにが違うのかなどよくいただく質問についてこちらのブログにおまとめいたしました。

WebView2 に関しては下記 URL 関連するドキュメントにおいて、WebView2 のサンプルアプリケーションや WebView2 のアーキテクチャなどを詳述しております。
本ブログにおいて不明な点がございましたら、まずはドキュメントをご確認いただけますと幸いです。

[Introduction to Microsoft Edge WebView2](https://learn.microsoft.com/en-us/microsoft-edge/webview2/)   

**※ なお全てのドキュメントは英語版のリンクを掲載しております。 /en-us/ の記述を /ja-jp/ にすることで日本語版のドキュメントを参照可能です。日本語版は自動翻訳のかつアップデートが遅れていることがあります。最新の情報を確認するためには英語版のドキュメントをご参考いただけますと幸いです。**


## 目次<!-- omit in toc -->
- [WebView2 と WebView2 Runtime と WebView2 SDK はどのような違いがありますか](#WebView2-%E3%81%A8-WebView2-Runtime-%E3%81%A8-WebView2-SDK-%E3%81%AF%E3%81%A9%E3%81%AE%E3%82%88%E3%81%86%E3%81%AA%E9%81%95%E3%81%84%E3%81%8C%E3%81%82%E3%82%8A%E3%81%BE%E3%81%99%E3%81%8B)
- [WebView2 はどのようなアプリケーションで利用されますか](#WebView2-%E3%81%AF%E3%81%A9%E3%81%AE%E3%82%88%E3%81%86%E3%81%AA%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%A7%E5%88%A9%E7%94%A8%E3%81%95%E3%82%8C%E3%81%BE%E3%81%99%E3%81%8B)
  - [WebView2 アプリケーションと WebView2 Runtime の互換性について](#WebView2-%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%A8-WebView2-Runtime-%E3%81%AE%E4%BA%92%E6%8F%9B%E6%80%A7%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)
  - [WebView2 Runtime と WebView2 SDK のバージョンの関係性について](#WebView2-Runtime-%E3%81%A8-WebView2-SDK-%E3%81%AE%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3%E3%81%AE%E9%96%A2%E4%BF%82%E6%80%A7%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)
- [WebView2 のサポート範囲について](#WebView2-%E3%81%AE%E3%82%B5%E3%83%9D%E3%83%BC%E3%83%88%E7%AF%84%E5%9B%B2%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)
- [WebView2 Runtime のインストーラーについて](#WebView2-Runtime-%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%A9%E3%83%BC%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)
  - [Evergreen 配布モードの WebView2 Runtime のダウンロードについて](#Evergreen-%E9%85%8D%E5%B8%83%E3%83%A2%E3%83%BC%E3%83%89%E3%81%AE-WebView2-Runtime-%E3%81%AE%E3%83%80%E3%82%A6%E3%83%B3%E3%83%AD%E3%83%BC%E3%83%89%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)
  - [固定バージョン配布モードの WebView2 Runtime のダウンロードについて](#%E5%9B%BA%E5%AE%9A%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3%E9%85%8D%E5%B8%83%E3%83%A2%E3%83%BC%E3%83%89%E3%81%AE-WebView2-Runtime-%E3%81%AE%E3%83%80%E3%82%A6%E3%83%B3%E3%83%AD%E3%83%BC%E3%83%89%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)
- [WebView2 Runtime の自動展開について](#WebView2-Runtime-%E3%81%AE%E8%87%AA%E5%8B%95%E5%B1%95%E9%96%8B%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)


## WebView2 と WebView2 Runtime と WebView2 SDK はどのような違いがありますか
***

**「WebView2」** はご利用いただいているデスクトップアプリケーションなどに、Edge のブラウザ機能を埋め込むためにご利用いただける機能(コントロール)になります。
WebView2 を利用することで、例えばネイティブアプリ内でブラウジングの機能を実装したり、またネイティブアプリより WebView2 のブラウザをオートメーションするなどが可能になります。

詳細は [Introduction to Microsoft Edge WebView2](https://learn.microsoft.com/en-us/microsoft-edge/webview2/) をご確認ください。


>> The Microsoft Edge WebView2 control allows you to embed web technologies (HTML, CSS, and JavaScript) in your native apps.


例えば以下は弊社が公開している [WebView2 のサンプルアプリケーション](https://github.com/MicrosoftEdge/WebView2Samples) です。画像を見るとわかるように、Bing のブラウザ画面にもかかわらずアプリは Edge などではなく、デスクトップ アプリケーションになっています。

![WebView2 Sample Application](./webview2-faq/webview2-faq_2023-04-17-09-38-15.png)

上記のようにデスクトップ アプリケーションにブラウザ画面を埋め込むことができ、なおかつよく見ると、Back や Refresh, GO やアドレスバーなどがブラウザ画面の外にあることがわかります。これは WebView2 の API を利用することで、デスクトップ アプリケーション側からブラウザコンポーネントの操作を行えることを意味します。つまり、デスクトップ アプリケーションからブラウザをオートメーションさせたり、ブラウザでの表示された内容にもとづいてアプリを処理させるなどができます。

一般に **WebView2 の機能を利用したアプリケーションを 「WebView2 アプリケーション」** と表現します。

過去の IE の WebBrowser コントロールの場合は、通常 C:\Windows\System32 配下に存在する、shdocvw.dll や mshtml.dll 等の DLL を利用して動かすため、WebBrowser コントロールを利用するアプリケーションのために端末になにかを追加でインストールすることは必要ございませんでした。
**一方で WebView2 についてはアプリケーションを実行する上で必要とするモジュールを別途インストールする必要がございます**。

その **WebView2 アプリケーションを実行するために必要になるのが「WebView2 Runtime」というランタイムになります**。

そのため WebView2 アプリケーションを利用するためには、WebView2 Runtime が端末にインストールされている必要がございます。

クライアントに WebView2 Runtime が存在すれば、WebView2 アプリケーションを問題なく実行することができます。

一方で WebView2 アプリケーションを開発の視点に立ちますと、どうやってアプリケーションに WebView2 の機能を埋め込む必要があります。
WebView2 アプリケーションを開発するために利用するのが 「WebView2 SDK」 になります。

SDK については [Release Notes for the WebView2 SDK](https://learn.microsoft.com/en-us/microsoft-edge/webview2/release-notes) のドキュメントをご確認ください。

上記をまとめますと以下になります。

- 「WebView2」 とはデスクトップアプリケーションにブラウザ機能を導入できるコントロール
- 「WebView2 Runtime」 は WebView2 アプリケーションの実行に必要なクライアントにインストールされるモジュール
- 開発者は WebView2 アプリケーションを 「WebView2 SDK」 を利用して作成しております。

※ 下記「注意」は発展的な事項のためこちらのブログを通読し理解いただいた上でご確認いただければと存じます。

注意：  
固定バージョン配布モードを利用すれば、WebView2 アプリケーション内に、WebView2 Runtime の実行可能ファイルを同梱させることも可能なため、必ずしも WebView2 アプリケーションを利用するために、端末に WebView2 Runtime が必須というわけではございません。
ただし WebView2 Runtime 自体のファイルサイズは 130~140 MB にもなりますため、アプリケーションの大きさを小さく保ちたいなどを理由として、端末に WebView2 Runtime がインストールされていることを前提として WebView2 アプリケーションを作成することが一般的です。 
なお Windows 11 においてはすでに WebView2 Runtime は自動的にインストールされており、アンインストールが行えない様になっております。

## WebView2 はどのようなアプリケーションで利用されますか
***

Microsoft 製品においても様々な場面で利用されております。

[Understanding the Office Add-ins runtime](https://devblogs.microsoft.com/microsoft365dev/understanding-office-add-ins-runtime/) において述べられておりますように 16.0.13530.20424 以降の Office においては WebView2 Runtime が存在する場合は WebView2 をアドインに利用されます。

>> Office released support for WebView2 starting with Office version 16.0.13530.20424. This means if you have Office version 16.0.13530.20424 or later, and have the WebView2 runtime installed, Office will use WebView2 as the runtime for add-ins. This is an incredibly exciting step for the Office Platform — not only because of all the benefits of WebView2, but also because it provides for uniformity of the runtime for add-ins across various versions of Windows. 

[Teams 2.0 Moves Away from Electron to Embrace Edge WebView2](https://techcommunity.microsoft.com/t5/microsoft-teams/teams-2-0-moves-away-from-electron-to-embrace-edge-webview2/m-p/2484565) に述べられておりますように Teams 2.0 は Electron ベースではなく WebView2 Runtime を利用しております。詳細は [Microsoft Teams: Advantages of the new architecture](https://techcommunity.microsoft.com/t5/microsoft-teams-blog/microsoft-teams-advantages-of-the-new-architecture/ba-p/3775704) をご確認ください。

その他にも、[Microsoft Edge WebView2 and Microsoft 365 Apps](https://learn.microsoft.com/en-us/deployoffice/webview2-install) に述べられておりますように M365 アプリにおいて、ルーム検索やミーティング インサイトなどにおいて WebView2 が利用されている旨が記載されております。

>> Microsoft 365 Apps is starting to provide new or improved features that rely on Microsoft Edge WebView2. For example, the Room Finder and the Meeting Insights features in Outlook. WebView2 uses Microsoft Edge as a rendering engine to display web-based features in a desktop application.

### WebView2 アプリケーションと WebView2 Runtime の互換性について
***

前述したように WebView2 アプリケーションは WebView2 SDK によって WebView2 の機能をアプリケーションに取り込んでおります。

そのため、例えば古い WebView2 SDK と 新しいバージョンの WebView2 SDK を比較すると、新しいバージョンにしかない API が存在することもございます。

そのため例えば以下のような組み合わせの場合については、(1) と (2) はそれぞれで問題なく動作はします。

- (1) ◯ 古い WebView2 SDK 利用アプリ <=> 古い WebView2 Runtime
- (2) ◯ 新しい WebView2 SDK 利用アプリ <=> 新しい WebView2 Runtime

しかし、新しい WebView2 SDK の機能を利用したアプリケーションを古い WebView2 Runtime で実行しようとした場合、クライアントに存在する Runtime がその API を認識できないために実行に失敗し、アプリケーションがクラッシュする可能性 (4) がございます。

- (3) ◯ 古い WebView2 SDK 利用アプリ <=> 新しい WebView2 Runtime 
- (4) ✕ 新しい WebView2 SDK 利用アプリ <=> 古い WebView2 Runtime

一方で (3) の古い WebView2 SDK のアプリケーションを新しい WebView2 Runtime で実行させる場合は特に問題は発生しません。これは WebView2 Runtime 自体が WebView2 SDK の API の前方互換性を保っているためになります。

なおこの前方互換性として記事作成時において担保されているのは WebView2 Runtime 86.0.616.0 (対応する SDK は 1.0.622.22) までになります。つまり SDK が 1.0.622.22 において作成されたアプリケーションは、(3) の例
のように、それから先の WebView2 Runtime のいずれでも動作させることが可能です。 

[Minimum version of the browser or Runtime to load WebView2](https://learn.microsoft.com/en-us/microsoft-edge/webview2/release-notes?tabs=dotnetcsharp#minimum-version-of-the-browser-or-runtime-to-load-webview2)

>> To load WebView2, the minimum version of Microsoft Edge or the WebView2 Runtime is 86.0.616.0. 
The minimum version to load WebView2 only changes when a breaking change occurs in the web platform.

WebView2 SDK に対応する WebView2 Runtime のバージョンはそれぞれの SDK のリリースノートにおいて記載がございます。

https://learn.microsoft.com/en-us/microsoft-edge/webview2/release-notes?tabs=dotnetcsharp

例えば SDK 1.0.1722.32 の機能に完全に対応するランタイムについては 112.0.1722.32 以上であることが必要になります。

>> For full API compatibility, this version of the WebView2 SDK requires WebView2 Runtime version 112.0.1722.32 or higher.

### WebView2 Runtime と WebView2 SDK のバージョンの関係性について
***

WebView2 SDK より作成したアプリケーションが、どの WebView2 Runtime 以上で完全互換性を持つかについては、バージョン情報のビルド バージョンを確認することで簡易的に判断ができます。

SDK および Runtime のバージョンは AA.BB.CC.DD で表記され、AA.BB.CC.DD のバージョンについて、AA は「メジャー バージョン」、BB は「マイナー バージョン」、CC は「ビルド バージョン」、DD は 「パッチ バージョン」となります。

以下の Chromium のドキュメントと同様の表記になります。

https://www.chromium.org/developers/version-numbers/

この内の CC の部分がビルド バージョンであり、下記のドキュメントにありますように Runtime のビルドバージョンが SDK のバージョンよりも高い場合は完全互換性を持ちます。

[Forward compatibility of APIs](https://learn.microsoft.com/en-us/microsoft-edge/webview2/concepts/versioning?source=recommendations#forward-compatibility-of-apis)

>> For example, if an API is introduced in SDK 1.0.900.0, that API would work with Runtime 94.0.900+.0, but not with Runtime 90.0.700.0.

## WebView2 のサポート範囲について
***

WebView2 は下記のドキュメントにございますように Edge とサポート範囲は同様になります。

[Microsoft Edge WebView2](https://learn.microsoft.com/en-us/lifecycle/products/microsoft-edge-webview2)

>> WebView2 follows the same Lifecycle as Microsoft Edge.

Edge のサポートバージョンは下記ブログに詳述しておりますが Edge 最新版バージョン - 2 までであり、**WebView2 Runtime も同様に 最新版バージョン - 2 までなります**。

https://jpdsi.github.io/blog/internet-explorer-microsoft-edge/how-and-why-to-update-edge/#Edge-%E3%81%AE%E7%A8%AE%E9%A1%9E%E3%81%A8-Edge-%E3%81%AE%E3%82%B5%E3%83%9D%E3%83%BC%E3%83%88%E7%AF%84%E5%9B%B2%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6

## WebView2 Runtime のインストーラーについて
***

[Distribute your app and the WebView2 Runtime](https://learn.microsoft.com/en-us/microsoft-edge/webview2/concepts/distribution) において詳細を述べております。

配布方法としては主に大きく 2 つございます。
1 つは 「Evergreen 配布モード」、もう 1 つが 「固定バージョン配布モード」 になります。

Evergreen 配布モードは、オンラインインストーラーまたはオフラインインストーラーによって WebView2 Runtime を配布する方法になります。

Evergreen 配布モードによって配布した WebView2 Runtime は通常自動更新によって最新版に更新され続けます。

なお Windows 11 においてはすでに WebView2 Runtime がプリインストールされており、Evergreen 配布モードによって配布された WebView2 Runtime と同様に自動更新によって管理されます。

弊社としては Evergreen 配布モードによる WebView2 Runtime のインストール、更新管理を推奨しております。

固定バージョン配布は、WebView2 アプリケーション自体に WebView2 Runtime を埋め込む方法になります。WebView2 アプリに WebView2 Runtime をパッケージしてアプリケーションを作成し、そのアプリをクライアントに配布します。

固定バージョンとあるように、アプリケーションに埋め込まれた WebView2 Runtime のバージョンは固定化されているため、WebView2 Runtime のサポート期間は 最新版 - 2 であることからも、数ヶ月おきに、新しい WebView2 Runtime を含めたアプリケーションに更新する必要がございます。

固定バージョン配布のメリットとして、アプリケーションに WebView2 Runtime が内蔵されているため、クライアントにおいて WebView2 Runtime を別途インストールする必要がございません。

一方、アプリケーションに WebView2 Runtime 自体を埋め込むため、WebView2 Runtime が 100 数十 MB あることから、アプリケーションのファイルサイズが肥大するなどのデメリットがございます。

固定バージョン配布の場合 WebView2 Runtime の更新のために、数ヶ月ごとにアプリケーションを配布し直す必要があるため、通常 Evergreen 配布モードをご利用いただくことが一般的です。

### Evergreen 配布モードの WebView2 Runtime のダウンロードについて
***

WebView2 Runtime の Evergreen 配布についてはいくつかの方法がございます。 詳細は [Understanding the options at the Runtime download page](https://learn.microsoft.com/en-us/microsoft-edge/webview2/concepts/distribution#understanding-the-options-at-the-runtime-download-page) をご確認ください。

[Download the WebView2 Runtime](https://developer.microsoft.com/en-us/microsoft-edge/webview2/#download-section) のダウンロードページを確認すると、Evergreen 配布モードのためのインストーラーとして 「Evergreen ブートストラッパー」および「Evergreen スタンドアローン インストーラー」を確認できます。

Evergreen ブートストラッパーは WebView2 Runtime のオンラインインストーラーになります。ブートストラッパー自体は軽量ですが、実行時に [Edge のエンドポイント](https://learn.microsoft.com/en-us/deployedge/microsoft-edge-security-endpoints) から最新の WebView2 Runtime 本体をインターネット上からダウンロードします。

そのためインターネット上のエンドポイントへの接続の許可および、100 数十 MB のファイルをダウンロードするため、そのダウンロード負荷を許容頂く必要がございます。

また Evergreen ブートストラッパーは、自身を別のアプリケーションから実行させるためのリンクなども提供しております。これは例えば WebView2 アプリケーションが、アプリケーションを実行する端末に WebView2 Runtime がインストールされていない場合に、自動的にインストールさせられるようにするなどを想定しております。

Evergreen スタンドアローン インストーラーは WebView2 Runtime のオフラインインストーラーになります。オフラインインストーラー自体に WebView2 Runtime 本体が内蔵されているため、オフラインインストーラーを起動しても、外部への通信は発生せず、インストールが可能です。

Evergreen スタンドアローン インストーラーは最新版の WebView2 Runtime しか提供はしておりません。そのため過去バージョンの WebView2 Runtime は取得できませんのでご注意ください。

### 固定バージョン配布モードの WebView2 Runtime のダウンロードについて
***

固定バージョン配布の場合、[Download the WebView2 Runtime](https://developer.microsoft.com/en-us/microsoft-edge/webview2/#download-section) のサイトの Fixed Version をアプリケーションに埋め込む必要がございます。 

## WebView2 Runtime の自動展開について
***

上述いたしましたように WebView2 Runtime はさまざまな Microsoft 製品での導入が進んでいることもあるため、WebView2 Runtime は条件によって自動的に展開されるようになっております。

[Delivering the Microsoft Edge WebView2 Runtime to Windows 10 Consumers](https://blogs.windows.com/msedgedev/2022/06/27/delivering-the-microsoft-edge-webview2-runtime-to-windows-10-consumers/) において述べられておりますように 2022 年 6 月 27 日以降においては Windows 10 の Home および Pro エディションかつ、2018 年 4 月以降の KB が適用されている Unmanaged 環境においては WebView2 が自動インストールされる場合がございます。

[Delivering Microsoft Edge WebView2 Runtime to managed Windows 10 devices](https://blogs.windows.com/msedgedev/2022/12/14/delivering-microsoft-edge-webview2-runtime-to-managed-windows-10-devices/) においてのべられておりますように 2023 年 1 月 16 日以降においてはドメイン管理下の Windows 10 においても自動インストールされる場合がございます。

※ ここにおいてのドメイン管理下とは、Intune や Active Directory, Azure Active Directory の制御下であることを指します。