---
title: 新しい Microsoft Edge での file プロトコルの制限について
date: 2022-04-15
tags: 
  - Microsoft Edge
  - Chromium
---

更新履歴:
2021/10/06 更新
2021/11/18 更新
2022/04/15 更新

---

※ 後述の追記の内容をご覧ください

こんにちは！

今回は、新しい Microsoft Edge での file: プロトコルの制限についてお知らせします。  
Chromium ベースの新しい Microsoft Edge では、セキュリティ上の制限により file: プロトコルのリンクは機能しません。  
この制限は Chromium における制限となりますが、新しい Microsoft Edge 側でこの制限を解除可能なオプションを提供する予定は現状ありません。

> Restrictions on File Urls
https://textslashplain.com/2019/10/09/navigating-to-file-urls/

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

**2021/11/18 追記:** [2021/10/21 にリリースされたバージョン 95](https://docs.microsoft.com/en-us/deployedge/microsoft-edge-relnote-stable-channel#version-950102030-october-21) にて、"ローカル イントラネット" ゾーン、かつ HTTPS のサイトに限って、file:// のリンク押下した場合に、リンクの指定対象ファイル (もしくは対象フォルダ) の一階層上のフォルダ (親フォルダ) をエクスプローラーで開くことを可能とする [IntranetFileLinksEnabled ポリシー](https://docs.microsoft.com/ja-jp/deployedge/microsoft-edge-policies#intranetfilelinksenabled) をリリースしました。セキュリティ上の観点から、ファイルを直接開くわけではなく一階層上のフォルダを開く動作となっています。予めご了承ください。

**2022/04/15 追記:** [2022/04/01 にリリースされたバージョン 100](https://docs.microsoft.com/en-us/deployedge/microsoft-edge-relnote-stable-channel#version-1000118529-april-1) から、上記のバージョン 95 と同じ条件でフォルダを開くことができますが、開く対象がフォルダであればそのものがエクスプローラーで開きます。開く対象がファイルであれば、そのファイルが存在するフォルダが開き、対象のファイルが選択された状態になります。

短いですが、今回は以上です。  
それでは、また次回！
