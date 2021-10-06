---
title: 新しい Microsoft Edge での file プロトコルの制限について
date: 2021-10-06
tags: 
  - Microsoft Edge
  - Chromium
---
こんにちわ！

今回は、新しい Microsoft Edge での file: プロトコルの制限についてお知らせします。  
Chromium ベースの新しい Microsoft Edge では、セキュリティ上の制限により file: プロトコルのリンクは機能しません。  
この制限は Chromium における制限となりますが、新しい Microsoft Edge 側でこの制限を解除可能なオプションを提供する予定は現状ありません。

> File system search result link does not open on Firefox or Chrome
https://support.google.com/gsa/answer/2664790?hl=en

新しい Microsoft Edge においても file プロトコルが使用できるようにするには、サードパーティー製の拡張機能ですが [Enable local file links](https://chrome.google.com/webstore/detail/enable-local-file-links/nikfmfgobenbhmocjaaboihbeocackld) などのご利用や、[IE モード](https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode) のご利用をご検討ください。

以下に動作の具体例をあげます。

---

A : アドレスバーに file:// の URL を直接入力して開く場合
外部からのアクセスではなく、ファイルを直接開く動作となります。
そのため、ファイルを開くことが可能です。

B : ローカル マシンに保存している HTML ファイルを Microsoft Edge (Chromium) で開いている場合
外部ではなくローカル ファイルからのアクセスのため、file:// のリンク先を開くことが可能です。

C : [IE モード](https://docs.microsoft.com/ja-jp/deployedge/edge-ie-mode) を利用した場合
端末にインストールされている Internet Explorer を利用してサイトを開きます。
Internet Explorer の単体利用時と同様に file:// のリンク先を開くことが可能です。

---

※ IE モードについて、以下の記事もご参照いただければと思いますが、ご利用に関してお困りなことがありましたら私たちサポートまでお問い合わせください！

新しいバージョンの Microsoft Edge の "IE モード" について
https://jpdsi.github.io/blog/internet-explorer-microsoft-edge/IEMode/

**2021/10/06 追記:** なお、[こちらのロードマップにて情報公開されました](https://www.microsoft.com/ja-jp/microsoft-365/roadmap?filters=Microsoft%20Edge%2CRolling%20out%2CIn%20development&searchterms=file%2Clinks)通り、早くて [2021/10/21 の週にリリース予定のバージョン 95](https://docs.microsoft.com/en-us/deployedge/microsoft-edge-release-schedule) にて file:// のリンクを有効にできる新しいポリシーを追加する見込みです。今後の予定についてはロードマップのドキュメントをご覧ください。

短いですが、今回は以上です。  
それでは、また次回！
