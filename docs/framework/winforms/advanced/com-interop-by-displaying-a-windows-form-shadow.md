---
title: "方法 : ShowDialog メソッドで Windows フォームを表示して COM 相互運用機能をサポートする | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "COM [Windows フォーム]"
  - "Windows フォーム, アンマネージ"
  - "COM interop, 呼び出しメソッド"
  - "ActiveX コントロール [Windows フォーム], COM 相互運用機能"
  - "ShowDialog メソッド"
  - "Windows フォーム, 相互運用"
ms.assetid: 87aac8ad-3c04-43b3-9b0c-d0b00df9ee74
caps.latest.revision: 6
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 6
---
# 方法 : ShowDialog メソッドで Windows フォームを表示して COM 相互運用機能をサポートする
[!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] メッセージ ループで Windows フォームを表示して、コンポーネント オブジェクト モデル \(COM\) 相互運用性の問題を解決できます。これは、<xref:System.Windows.Forms.Application.Run%2A?displayProperty=fullName> メソッドを使用して作成されます。  
  
 フォームが COM クライアント アプリケーションから正しく動作するには、Windows フォームのメッセージ ループ上で実行する必要があります。 そのためには、次の方法のいずれかを使用します。  
  
-   <xref:System.Windows.Forms.Form.ShowDialog%2A?displayProperty=fullName> メソッドを使用して、Windows フォームを表示します。  
  
-   各 Windows フォームを別のスレッドで表示します。 詳細については、「[方法 : 独自のスレッドで各 Windows フォームを表示して COM 相互運用機能をサポートする](../../../../docs/framework/winforms/advanced/how-to-support-com-interop-by-displaying-each-windows-form-on-its-own-thread.md)」を参照してください。  
  
## プロシージャ  
 [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] メッセージ ループにフォームを表示する方法としては、すべての方法の中で実装する必要があるコードが最も少ない <xref:System.Windows.Forms.Form.ShowDialog%2A?displayProperty=fullName> メソッドを使用する方法が最も簡単です。  
  
 <xref:System.Windows.Forms.Form.ShowDialog%2A?displayProperty=fullName> メソッドは、管理されていないアプリケーションのメッセージ ループを一時停止し、フォームをダイアログ ボックスとして表示します。 ホスト アプリケーションのメッセージ ループが一時停止されているので、<xref:System.Windows.Forms.Form.ShowDialog%2A?displayProperty=fullName> メソッドは、新しい [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] メッセージ ループを作成してフォームのメッセージを処理します。  
  
 <xref:System.Windows.Forms.Form.ShowDialog%2A?displayProperty=fullName> メソッドを使用する場合の欠点は、フォームがモーダル ダイアログ ボックスとして開かれることです。 この動作により、Windows フォームが開かれている間は呼び出し元のアプリケーションですべてのユーザー インターフェイス \(UI\) がブロックされます。 ユーザーがフォームを終了すると、[!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] メッセージ ループが終了し、以前のアプリケーションのメッセージ ループの実行が再び開始されます。  
  
 フォームを表示するためのメソッドが含まれるクラス ライブラリを Windows フォームで作成し、その後で COM 相互運用機能のクラス ライブラリをビルドすることができます。 Visual Basic 6.0 または Microsoft Foundation Classes \(MFC\) からこの DLL ファイルを使用し、これらのいずれかの環境から <xref:System.Windows.Forms.Form.ShowDialog%2A?displayProperty=fullName> メソッドを呼び出してフォームを表示することができます。  
  
#### ShowDialog メソッドで Windows フォームを表示して COM 相互運用機能をサポートする方法  
  
-   [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] コンポーネントで、<xref:System.Windows.Forms.Form.Show%2A?displayProperty=fullName> メソッドのすべての呼び出しを <xref:System.Windows.Forms.Form.ShowDialog%2A?displayProperty=fullName> メソッドの呼び出しに置き換えます。  
  
## 参照  
 [COM への .NET Framework コンポーネントの公開](../../../../docs/framework/interop/exposing-dotnet-components-to-com.md)   
 [方法 : 独自のスレッドで各 Windows フォームを表示して COM 相互運用機能をサポートする](../../../../docs/framework/winforms/advanced/how-to-support-com-interop-by-displaying-each-windows-form-on-its-own-thread.md)   
 [Windows フォームとアンマネージ アプリケーション](../../../../docs/framework/winforms/advanced/windows-forms-and-unmanaged-applications.md)