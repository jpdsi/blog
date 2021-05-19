---
title: IE モードのよくあるご質問
date: 2021-5-14
tags: 
  - Microsoft Edge
  - IE モード
---

更新履歴:
2021/05/14 新規
2021/05/19 更新: 追記しました

---

みなさんこんにちは。日本マイクロソフトの IE/Edge サポートチームです。

これまでに「IE モード」について以下の２つのブログ記事を公開していますが、今回はよくあるご質問についてまとめたいと思います。

新しいバージョンの Microsoft Edge の "IE モード" について
https://jpdsi.github.io/blog/internet-explorer-microsoft-edge/IEMode/

まだデフォルトIE？ 新しい Microsoft Edge を使いませんか？
https://jpdsi.github.io/blog/internet-explorer-microsoft-edge/how-about-using-new-edge/

なお、IE モードに関する公式ドキュメントは以下にまとまっていますので、基本的な内容についてはこちらをご覧ください。

Internet Explorer (IE) モードとは
https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode

IE モードのトラブルシューティングと FAQ
https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode-faq

---

## Web コンテンツのデバッグ方法
Microsoft Edge の IE11 モードでは、IE11 のブラウザー エンジンで表示する必要のある Web サイトを Microsoft Edge 上のタブ内で表示しますが、IE モードで表示されている Web コンテンツに対して、Microsoft Edge の F12 開発者ツールを使用することはできません。
IE モードで表示されている Web コンテンツをデバッグするには、IEChooser (Microsoft Edge 開発者ツール) を使用します。具体的には、以下の手順にてデバッグ作業を行います。

※ このトピックは、Windows 10 および Windows Server 2016、Windows Server 2019 以上の環境を前提としています。Windows 8.1 および Windows Server 2012 R2 以前の Windows プラットフォームについては、恐れ入りますが、後述の方法で IE モードで表示されている Web コンテンツをデバッグすることはできません。

1. Microsoft Edge 上で、IE モードの Web コンテンツを表示します。
2. %WINDIR%\System32\F12 フォルダー内にある、IEChooser.exe (Windows 10 のバージョン 1709 以前、および Windows Server 2016 では F12Chooser.exe) を起動します。
3. 以下のようなウィンドウが表示されますので、IE モードで表示されている Web コンテンツをクリックします。
なお、以下のウィンドウ内には、IE11 および IE モードで表示されている Web コンテンツのファイル名またはタイトルが表示されますので、その情報を基に対象の Web コンテンツを選択します。

![ターゲットの選択](./ie-mode-faq/IEChooser1.png)

4. 開発者ツールが起動しますので、Web コンテンツをデバッグします。

![開発者ツール画面](./ie-mode-faq/IEChooser2.png)

---

## window.open() による子ウィンドウの扱い
Microsoft Edge の IE11 モードでは、IE11 のブラウザー エンジンで表示する必要のある Web サイトを Microsoft Edge 上のタブ内で表示しますが、IE モードで表示されている Web コンテンツ内で window.open() メソッドを実行して表示される子ウィンドウは新しい Microsoft Edge のウィンドウとなります。

IE モードでは、Web ページ コンテンツの表示 (レンダリング) やスクリプトは IE11 のブラウザー エンジンによって処理されますが、ブラウザー ウィンドウとしての外枠 (フレーム) 部分は Microsoft Edge によって制御されます。具体的には、以下の図の緑色の枠線内は IE モードによって処理されますが、枠の外側は Microsoft Edge によって処理されます。

![ウィンドウの構造](./ie-mode-faq/windowopen1.png)

上記動作に伴い、window.open(url, windowName, windowFeatures) の引数 windowFeatures で指定する以下のオプションは反映されません。
なお、この動作は Microsoft Edge における想定された動作となります。

- location
引数 url で指定されるサイトがローカル イントラネットや信頼済みサイト ゾーンで、かつ、引数 windowFeatures において location=no と指定していても、以下のようにアドレス バーは表示されます (ただし、アドレス バーでの操作はできません)。
![画面例](./ie-mode-faq/windowopen2.png)

- resizable
resizable オプション指定は常に無視され、ユーザーはウィンドウ サイズを変更することができます。

- fullscreen
fullscreen オプション指定は常に無視され、全画面表示とはなりません。

(参考情報)
window.open - Web API | MDN
https://developer.mozilla.org/ja/docs/Web/API/Window/open

---

## Cookie の共有
以下のドキュメントに詳細がまとまっていますが、<span style="color: #ff0000">セッション Cookie</span> に関して、Edge から IE モードにのみ共有することができます。
<span style="color: #ff0000">セッション Cookie</span>を逆方向へ (IE モードから Edge へ) 共有することはできません。

Microsoft Edge から Internet Explorer への Cookie の共有
https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode-add-guidance-cookieshare

<span style="color: #ff0000;font-weight:bold;">※ 共有できる Cookie の種類は、有効期限のある永続 Cookie ではなく、有効期間のないセッション Cookie のみですのでご注意ください。</span>

---

## ページ遷移時のブラウザー エンジンの切り替え
以下のポリシーの設定状態によって、IE モードで表示しているページからの遷移時の動作が変わります。

InternetExplorerIntegrationSiteRedirect
https://docs.microsoft.com/ja-jp/deployedge/microsoft-edge-policies#internetexplorerintegrationsiteredirect

実際の動きをみてみましょう。(ポリシー変更後はいったん Edge を再起動させてください)

サイトリストには以下のふたつの URL を登録した状態にします。

```
<site url="docs.microsoft.com/ja-jp/deployedge/edge-ie-mode">
<open-in>IE11</open-in>
</site>
<site url="aka.ms">
<open-in>IE11</open-in>
</site>
```

このサイトリストが反映された状態で以下の A, B の操作をすると、ポリシーの設定状態によって下記のとおりになります。

A) https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode を (IE モードで) 開いたあと、ページ左側の目次から任意の別のページを開く
B) https://aka.ms/windows/releaseinfo を開く

- 未構成もしくは既定
A, B) 遷移先のページが Edge モードで表示される。

- 自動ナビゲーションのみを Internet Explorer モードで維持する
A) 遷移先のページが Edge モードで表示される。
B) 遷移先のページが IE モードで表示される。

- すべてのページ内ナビゲーション を Internet Explorer モードで維持する
A, B) 遷移先のページが IE モードで表示される。

---

## ブラウザー エンジンの切り替え時の通信で起こること
ブラウザー エンジンの切り替え時 (Edge から IE モード、およびその逆) の通信は以下のようになります。

- POST メソッドが GET メソッドになる
- HTTP リクエスト ヘッダーに Referrer が含まれない

ページ遷移時に POST リクエストではなく GET リクエストが Web サーバーに送信されている場合には、こちらの制限に該当していると言えます。
POST リクエストが GET リクエストとなる動作は、異なるプロセス間での POST データのやり取りに対するセキュリティ的な懸念の観点と、技術的な制約からなる想定された動作となり、この動作を変更することはできません。

元のページから POST リクエストで通信を行った場合は GET リクエストに変わるため、POST リクエストの body に含まれる内容は消失します。
対処策としては、データを引き渡す必要がないように、関連する一連のページをすべて IE モードで表示するか、Edge で開けるように統一するかのどちらかとなります。

---

## IE モードでの表示をテストしたい

IE モードのテストのために、毎回サイトリストを書き替えたり、ポリシーを設定しなおすことは大変な手間がかかります。
この手間を解消するために、以下の A, B どちらかの方法でメニューから IE モードもしくは Edge モードを切り替え、コンテンツを表示することが可能です。

<span style="color: #ff0000">これらの機能はテストを目的に用意されており、意図しないサイトで IE モードを利用することは、想定しない ActiveX の実行など、セキュリティのリスクがありますので十分ご注意ください。</span>

A) InternetExplorerIntegrationTestingAllowed ポリシーを有効に設定する
https://docs.microsoft.com/ja-jp/deployedge/microsoft-edge-policies#internetexplorerintegrationtestingallowed

B) "--ie-mode-test" オプションを付加して msedge.exe を実行する
実行例)
"C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" --ie-mode-test

これらの方法を利用することで、ポリシーの説明にもあるように [その他のツール] 以下に、[サイトを Internet Explorer モードで開く]、[サイトを Edge モードで開く] メニューが表示されます。
![選択したモードで開く操作](./ie-mode-faq/iemodetest.png)

なお、テストモードの利用は IE モードが構成されていることが前提となります。以下のドキュメントでもご案内している手順にて、IE モードを有効に設定してください。

グループ ポリシーを使用して Internet Explorer 統合を有効にする
https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode-policies#enable-internet-explorer-integration-using-group-policy

---

## サイトリストで指定しているサイトが IE モードで表示されない

まずは edge://compat を開いて、Edge が読み込んでいるサイトリストの内容を確認します。

![Edge が読み込んでいるサイトリストの内容](./ie-mode-faq/compat_enterprise.png)

確認ポイント
- URL 一覧の中に、IE モードで表示したい項目が存在するか。
- 対象のサイトについて "エンジン" が "IE11" となっているか。
- "エンジン"が "ニュートラル" となっている場合、遷移元のページが IE モードで表示されているか。

もし想定していない状態であれば、なぜ意図したサイトリストが利用されていないのかを掘り下げていきます。

確認ポイント
- サイトリストのバージョンを更新したか。
- [エンタープライズ モード スキーマ v.2 ガイダンス](https://docs.microsoft.com/ja-jp/internet-explorer/ie11-deploy-guide/enterprise-mode-schema-version-2-guidance) に沿っているか。
- シンプルに１つだけ登録した状態であれば、Edge に反映されるか。

サイトリストの内容には問題がない場合、edge://compat/iediagnostic を開いて、ポリシーの設定状態を確認します。

![診断情報の画面](./ie-mode-faq/compat_iediagnostic.png)
![診断情報の画面](./ie-mode-faq/compat_iediagnostic2.png)

確認ポイント
- "Internet Explorer モードの設定" で上記のようになっているかどうか。
- "グループ ポリシーの設定" が想定したとおりになっているかどうか。

設定について [IE モード ポリシーの構成](https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode-policies) のドキュメントをみながら構成を確認します。

---

## ドキュメント モードを確認したい

アドレス バーの横にある IE モードのアイコンをクリックすると確認できます。

![ドキュメント モードなどの確認](./ie-mode-faq/docmode.png)

---

## ニュートラル サイト

ニュートラルの機能は、<font color="red">その URL に遷移してきた際の直前のブラウザーを維持することです。</font>
つまり、Edge で遷移してきた場合には Edge を維持し、IE モードで遷移してきた場合には IE モードを維持するにようにできます。

以下のような場面で利用します。

### 認証がうまくいかないケース
IE モードを利用するとき『認証がうまくいかない』というお問い合せをよくいただきますので、ここで紹介します。

認証がうまくいかないという話では [保護モードをまたいだ Cookie について](../cross-protected-mode-cookie/) という記事で、Azure AD (AAD) 認証を必要とする Web アプリケーションを例としました。
対象の Web アプリケーションのドメインと認証時にアクセスする login.microsoftonline.com が異なったセキュリティ ゾーンに属していた場合、認証済みを示す Cookie がリクエストに含まれないことで、ユーザーがログオンしていないと判断され、再度認証を求められてしまうという現象を解説しました。

IE モードを利用するとき『どの URL を IE モードで表示するか』を設定しますが、対象の Web アプリケーションの URL に加えて、**<font color="red">認証サーバー / シングル サインオン サーバーに対してのアクセスも IE モードで動作するように明示的に構成する</font>** 必要があります。
認証サーバーも含めておかないと、以下のように IE モードと Edge とで行き来するときに認証に必要な Cookie が共有されずに認証ができません。

1. 対象の Web アプリケーションの URL にアクセスします。(IE モードで動作します)
2. 認証のために認証サーバーにアクセスします。(Edge で通信します)
3. Edge 側で認証済みを示す Cookie を受け取ります。
4. 対象の Web アプリケーションにリダイレクトしますが、ここで IE モードに切り替わります。
5. IE モードとしては認証済みを示す Cookie を持っていないので、Cookie なしの状態でリクエストすることになります。
6. 認証済みを示す Cookie がリクエストに含まれないことで、ユーザーがログオンしていないと判断され、再度認証を求められます。

ただ、認証時に通信するサーバーをすべて IE モードで動作するように明示的に指定してしまうと、逆に Edge でアクセスする必要があるパターンでうまくいきません。

具体例として、Azure AD (AAD) 認証を必要とする Web アプリケーションとして Office.com を使って試してみます。
(本来は IE モードを使わずに Edge でご覧いただくことをおすすめします)

まずは以下のように対象サイト (office.com) だけを登録してみます。
```
<site url="office.com">
    <open-in>IE11</open-in>
</site>
```

このサイト リストを読み込んで、office.com にアクセスすると IE モードで開きます。
サインインの操作をしてみるとわかりますが、サインインが完了しません。(場合によっては、認証画面の表示がループしてしまうこともあります)
これは、上述のとおり、認証済みを示す Cookie がリクエストに含まれないことで、ユーザーがログオンしていないと判断されたためです。

そこで以下のように、今回の認証時に経由する可能性がある、login.microsoftonline.com, login.windows.net, login.live.com に対しても IE モードになるように \<open-in\>IE11\</open-in\> を設定してみます。

```
<site url="office.com">
    <open-in>IE11</open-in>
</site>
<site url="login.microsoftonline.com">
    <open-in>IE11</open-in>
</site>
<site url="login.windows.net">
    <open-in>IE11</open-in>
</site>
<site url="login.live.com">
    <open-in>IE11</open-in>
</site>
```

これにより、認証時に通信するサーバーすべてが IE モードで動作し、認証完了まで進みます。

それでは、この状態で [Outlook on the web (OWA)](https://outlook.live.com/owa/) を開いてみましょう。
OWA は IE モードで開くように登録をしていないので、通常の Edge で開きます。
サインインをしようとすると、login.live.com が IE モードとして登録されているので、サインイン画面が IE モードで開きます。
アカウント情報を入力してみると...

エラーになってしまいました :(

![サインインのエラー画面](./ie-mode-faq/owa.png)


認証サーバー (login.microsoftonline.com, login.windows.net, login.live.com) を IE モードの対象から外せば、OWA は開きます。
でもそうすると、先ほどの Office.com がまたダメになってしまいます。

どちらもうまくいくように構成するにはどうしたらよいか...

**<font color="red">Edge と IE のどちらからも経由され得るサイトを『ニュートラル』に登録することで解決できます！</font>**

冒頭に記述したように、ニュートラルの機能は、<font color="red">その URL に遷移してきた際の直前のブラウザーを維持することです。</font>
つまり、Edge で遷移してきた場合には Edge を維持し、IE モードで遷移してきた場合には IE モードを維持するにようにできます。

それでは、先ほどのサイト リストを以下のように認証サーバー (login.microsoftonline.com, login.windows.net, login.live.com) に対してそれぞれ \<open-in\>None\</open-in\> に変えます。

```
<site url="office.com">
    <open-in>IE11</open-in>
</site>
<site url="login.microsoftonline.com">
    <open-in>None</open-in>
</site>
<site url="login.windows.net">
    <open-in>None</open-in>
</site>
<site url="login.live.com">
    <open-in>None</open-in>
</site>
```

こうすると、IE モードで開く Office.com から認証サーバーにアクセスするときは IE モードが維持され、Edge で開く OWA から認証サーバーにアクセスするときは Edge が維持されますので、認証処理の途中でモードが変わってしまうことを回避できます。

なお、下記ドキュメントの「ニュートラル サイトを構成する」の部分にも記載がありますが、上記の例のように Edge と IE のどちらからも経由され得るサイトはすべてニュートラルに登録する必要があります。

エンタープライズ モード サイト リストでのサイトの構成
https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode-sitelist

一時的な対応策としては [Internet Explorer モード ページから起動したときに未構成サイトへの「ページ内」ナビゲーションの動作を指定する] というポリシーが利用できますので、状況に合わせた対応をご検討いただければと思います。

Internet Explorer モードでページ内ナビゲーションを保持する
https://docs.microsoft.com/ja-jp/deployedge/edge-learnmore-inpage-nav

---