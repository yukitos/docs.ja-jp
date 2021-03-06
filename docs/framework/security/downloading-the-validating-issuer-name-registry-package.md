---
title: "発行者名レジストリの検証パッケージのダウンロード | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: ff8b0014-c5d4-4614-90f0-13fcc0ba777a
caps.latest.revision: 3
author: "wadepickett"
ms.author: "wpickett"
manager: "wpickett"
caps.handback.revision: 3
---
# 発行者名レジストリの検証パッケージのダウンロード
このトピックでは、Validating Issuer Name Registry \(VINR\) をダウンロードしてプロジェクトで使用する方法について説明します。  
  
## Validating Issuer Name Registry のダウンロード  
 VINR は、プロジェクトに必要なアセンブリと参照を追加する NuGet パッケージとして入手できます。  まだ NuGet がインストールされていない場合は、[nuget.org](http://nuget.org) にアクセスしてインストールしてください。  NuGet の「[Microsoft Validating Issuer Name Registry](https://nuget.org/packages/System.IdentityModel.Tokens.ValidatingIssuerNameRegistry/)」ページにアクセスすると、拡張機能のバージョン履歴を参照できます。  
  
#### パッケージ マネージャーの GUI を使用した Validating Issuer Name Registry のダウンロード  
  
1.  Visual Studio の**ソリューション エクスプローラー**でプロジェクトを右クリックし、**\[NuGet パッケージの管理\]** を選択します。  
  
2.  **\[NuGet パッケージの管理\]** ウィンドウで、検索ボックスをクリックし、「`ValidatingIssuerNameRegistry`」と入力して **Enter** キーを押します。  
  
3.  結果ペインから、最初の結果の **\[インストール\]** をクリックします。  
  
4.  パッケージのダウンロードが開始されます。  プロジェクトに追加される前に、\[License Acceptance\] ダイアログ ボックスが表示されます。  ライセンス条項に同意する場合は、**\[I Accept\]** をクリックします。  
  
5.  最新の VINR アセンブリがダウンロードされ、プロジェクトに追加されます。  
  
#### パッケージ マネージャー コンソールを使用した Validating Issuer Name Registry のダウンロード  
  
1.  Visual Studio で、**\[ツール\]**、**\[ライブラリ パッケージ マネージャー\]**、**\[パッケージ マネージャー コンソール\]** の順にクリックします。  
  
2.  **\[パッケージ マネージャー コンソール\]** が表示されます。  次のテキストを入力し、**Enter** キーを押します。  
  
    ```powershell  
    Install-Package System.IdentityModel.Tokens.ValidatingIssuerNameRegistry  
    ```  
  
3.  The latest VINR assemblies will be downloaded and added to your project.