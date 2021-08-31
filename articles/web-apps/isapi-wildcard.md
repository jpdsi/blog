---
title: ASP.NET で 拡張子のない URL にてアクセスすると、追加したカスタム ISAPI 拡張ハンドラーが動作しない現象について
date: 2021-08-30
tags: 
  - Internet Information Services
  - ASP.NET
  - ハンドラーマッピング
---

# ASP.NET で 拡張子のない URL にてアクセスすると、追加したカスタム ISAPI 拡張ハンドラーが動作しない現象について <!-- omit in toc -->

こんにちは。IIS サポート チームです！  

Windows Server 2012 以降の OS バージョンの IIS (IIS 8.0 以降) にて、カスタムの ISAPI 拡張 (ハンドラー) をワイルドカード マッピングを利用して動作させたい場合に、ASP.NET の機能との関連で動作しない現象について、お知らせします。

   [ISAPI 拡張について](https://docs.microsoft.com/en-us/previous-versions/iis/6.0-sdk/ms525172(v=vs.90))


## 現象

 IIS 10.0 , ASP.NET の環境下で、 要求パス に "*" を指定した ISAPI 拡張をハンドラーに追加した場合、 拡張子のない URL でアクセスすると、追加した ISAPI 拡張ハンドラーが動作しない。

 コマンド例
```
appcmd set config "Default Web Site" /section:handlers /+"[name='ISAPI_Extension', path='*', verb='*', modules='IsapiModule', scriptProcessor='C:\ISAPI\ISAPI_Extension.dll', resourceType='Unspecified', requireAccess='None',preCondition='bitness64']" /commit:APPHOST
```



## 原因

ASP.NET がインストールされていると既定で 要求パス "*." に対して Extensionless URL Handler がマップされ、拡張子のない URL は Extensionless URL Handler に処理されるようになります。

この動作により 要求パスに "*" を指定した場合に、 Extensionless URL Handler が処理する動作となり、追加した ISAPI 拡張機能は機能しません。

 ASP.NET の場合、 MVC で処理される拡張子のない URL へのリクエストを処理できるようにするために意図的に実装した動作になります。



## 対処方法

拡張子のない URL からアクセスするためには、追加する ISAPI 拡張ハンドラーの要求パスに "*." を指定します。

なお、拡張子の有無に関わらず 全ての URL に対して 動作するようにするように ハンドラー を追加するためには、 要求パス に "\*" を指定した ハンドラー と "*." を指定したハンドラー の 2 つを追加し、順序を上にすることで 拡張子の有無に関わらず、全ての URL に対して 動作することができます。

![ハンドラー マッピングの設定](./isapi-wildcard/isapi-wildcard_2021-08-30-17-08-10.png)
