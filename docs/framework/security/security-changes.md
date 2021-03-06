---
title: ".NET Framework におけるセキュリティの変更点 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - "Allow Partially Trusted Callers Attribute"
  - ".NET framework 4、セキュリティの変更"
  - ".NET Framework のセキュリティ"
  - "セキュリティが透過的なコード"
  - "セキュリティ クリティカルなコード"
  - "コード アクセス セキュリティ、変更"
ms.assetid: 5e87881c-9c13-4b52-8ad1-e34bb46e8aaa
caps.latest.revision: 52
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 52
---
# .NET Framework におけるセキュリティの変更点
[!INCLUDE[net_v45](../../../includes/net-v45-md.md)] のセキュリティにおける最も重要な変更は、厳密な名前付けの変更です。 これらの変更の詳細については、「[拡張された厳密な名前付け](../../../docs/framework/app-domains/enhanced-strong-naming.md)」を参照してください。  
  
 .NET Framework は、マネージ アプリケーションに 2 階層のセキュリティ モデルを提供します。[!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] アプリはリソースへのアクセスを制限する Windows セキュリティ コンテナーで実行されます。 そのコンテナー内では、マネージ アプリケーションは完全に信頼された状態で実行します。 コード アクセス セキュリティの \(CAS\) の観点からは、権限を昇格するために開発者が実行できる操作はありません。 Windows で与えられる権限の詳細については、Windows デベロッパー センターの「[App capability declarations \(Windows Store apps\)](http://go.microsoft.com/fwlink/?LinkId=230436)」 \(アプリの機能宣言 \(Windows ストア アプリ\)\) を参照してください。[!INCLUDE[win8_appname_long](../../../includes/win8-appname-long-md.md)] アプリの作成については、「[C\# または Visual Basic を使った初めての Windows ストア アプリの作成](http://go.microsoft.com/fwlink/?LinkId=230461)」を参照してください。