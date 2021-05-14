---
title: IE モードのよくあるご質問
date: 2021-5-14
tags: 
  - Microsoft Edge
  - IE モード
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

![IEChooser1](./ie-mode-faq/IEChooser1.png)

4. 開発者ツールが起動しますので、Web コンテンツをデバッグします。

![IEChooser2](./ie-mode-faq/IEChooser2.png)

---

## window.open() による子ウィンドウの扱い
Microsoft Edge の IE11 モードでは、IE11 のブラウザー エンジンで表示する必要のある Web サイトを Microsoft Edge 上のタブ内で表示しますが、IE モードで表示されている Web コンテンツ内で window.open() メソッドを実行して表示される子ウィンドウは新しい Microsoft Edge のウィンドウとなります。

IE モードでは、Web ページ コンテンツの表示 (レンダリング) やスクリプトは IE11 のブラウザー エンジンによって処理されますが、ブラウザー ウィンドウとしての外枠 (フレーム) 部分は Microsoft Edge によって制御されます。具体的には、以下の図の緑色の枠線内は IE モードによって処理されますが、枠の外側は Microsoft Edge によって処理されます。

![WindowOpen1](./ie-mode-faq/windowopen1.png)

上記動作に伴い、window.open(url, windowName, windowFeatures) の引数 windowFeatures で指定する以下のオプションは反映されません。
なお、この動作は Microsoft Edge における想定された動作となります。

- location
引数 url で指定されるサイトがローカル イントラネットや信頼済みサイト ゾーンで、かつ、引数 windowFeatures において location=no と指定していても、以下のようにアドレス バーは表示されます (ただし、アドレス バーでの操作はできません)。
![WindowOpen2](./ie-mode-faq/windowopen2.png)

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
![IEModeTest](./ie-mode-faq/iemodetest.png)

なお、テストモードの利用は IE モードが構成されていることが前提となります。以下のドキュメントでもご案内している手順にて、IE モードを有効に設定してください。

グループ ポリシーを使用して Internet Explorer 統合を有効にする
https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode-policies#enable-internet-explorer-integration-using-group-policy

