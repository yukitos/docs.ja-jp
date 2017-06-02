---
title: ".NET Framework を使用したカスタム Windows フォーム コントロールの開発 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "Component クラス"
  - "Control クラス, Windows フォーム"
  - "カスタム コントロール [Windows フォーム], 開発 (コードを使用して)"
ms.assetid: 236cebc0-bd71-4f18-9fd6-5d0e592375df
caps.latest.revision: 22
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 22
---
# .NET Framework を使用したカスタム Windows フォーム コントロールの開発
Windows フォーム コントロールは、ユーザー インターフェイスの機能をカプセル化して、クライアント側の Windows ベースのアプリケーションで使用される再利用可能なコンポーネントです。  Windows フォームは、すぐに使用できる多数のコントロールを提供するだけでなく、独自のコントロールを開発するためのインフラストラクチャも提供します。  既存のコントロールの結合、既存のコントロールの拡張、または独自のカスタム コントロールの記述ができます。  このセクションでは、Windows フォーム コントロールの開発に役立つ背景情報とサンプルを提供します。  
  
## このセクションの内容  
 [Windows フォームでのコントロールの使用方法の概要](../../../../docs/framework/winforms/controls/overview-of-using-controls-in-windows-forms.md)  
 Windows フォーム アプリケーションでのコントロールの使用の重要な要素を示しています。  
  
 [さまざまなカスタム コントロール](../../../../docs/framework/winforms/controls/varieties-of-custom-controls.md)  
 <xref:System.Windows.Forms?displayProperty=fullName> 名前空間を使用して作成できる様々な種類のカスタム コントロールについて説明します。  
  
 [Windows フォーム コントロール開発の基本概念](../../../../docs/framework/winforms/controls/windows-forms-control-development-basics.md)  
 Windows フォーム コントロールの開発の最初の手順について説明します。  
  
 [Windows フォーム コントロールのプロパティ](../../../../docs/framework/winforms/controls/properties-in-windows-forms-controls.md)  
 Windows フォーム コントロールのプロパティを追加する方法を示します。  
  
 [Windows フォーム コントロールのイベント](../../../../docs/framework/winforms/controls/events-in-windows-forms-controls.md)  
 Windows フォーム コントロールのイベントを処理して定義する方法を示します。  
  
 [Windows フォーム コントロールの属性](../../../../docs/framework/winforms/controls/attributes-in-windows-forms-controls.md)  
 カスタム コントロールとコンポーネントのプロパティや他のメンバーに適用できる属性について説明します。  
  
 [コントロールのカスタム描画およびレンダリング](../../../../docs/framework/winforms/controls/custom-control-painting-and-rendering.md)  
 コントロールの外観をカスタマイズする方法を示します。  
  
 [Windows フォーム コントロールのレイアウト](../../../../docs/framework/winforms/controls/layout-in-windows-forms-controls.md)  
 コントロールとフォームの高度なレイアウトを作成する方法を示します。  
  
 [Windows フォーム コントロールのマルチスレッド処理](../../../../docs/framework/winforms/controls/multithreading-in-windows-forms-controls.md)  
 マルチスレッド コントロールを実装する方法を示します。  
  
## 関連項目  
 <xref:System.Windows.Forms.Control?displayProperty=fullName>  
 このクラスについて説明し、すべてのメンバーへのリンクの一覧を示します。  
  
 <xref:System.Windows.Forms.UserControl?displayProperty=fullName>  
 このクラスについて説明し、すべてのメンバーへのリンクの一覧を示します。  
  
## 関連項目  
 [コンポーネントのデザイン時属性](../Topic/Design-Time%20Attributes%20for%20Components.md)  
 ビジュアル デザイナーでデザインするときに正しく表示されるようにコンポーネントとコントロールに適用するメタデータ属性の一覧を表示します。  
  
 [Extending Design\-Time Support](../Topic/Extending%20Design-Time%20Support.md)  
 デザイン時サポートを提供するエディターやデザイナーなどのクラスを実装する方法について説明します。  
  
 [方法 : コンポーネントおよびコントロールのライセンス処理を行う](../Topic/How%20to:%20License%20Components%20and%20Controls.md)  
 コントロールやコンポーネントのライセンスを実装する方法について説明します。  
  
 「[デザイン時の Windows フォーム コントロールの開発](http://msdn.microsoft.com/library/w29y3h59\(v=vs.110\))」も参照してください。