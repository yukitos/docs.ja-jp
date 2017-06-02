---
title: "Windows フォームのセキュリティの概要 | Microsoft Docs"
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
  - "アクセス制御, Windows フォーム"
  - "コード アクセス セキュリティ, Windows フォーム"
  - "アクセス許可, Windows フォーム"
  - "セキュリティ [Windows フォーム], セキュリティの概要"
  - "Windows フォーム, セキュリティ"
ms.assetid: 4810dc9f-ea23-4ce1-8ea1-657f0ff1d820
caps.latest.revision: 16
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 16
---
# Windows フォームのセキュリティの概要
[!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] のリリース以前、ユーザーのコンピューターで実行されているすべてのコードは、リソースにアクセスするために、そのコンピューターのユーザーが持っていたのと同じ権限またはアクセス許可を持っていました。  たとえば、ユーザーにファイル システムへのアクセスが許可されている場合は、コードにファイル システムへのアクセスが許可され、ユーザーにデータベースへのアクセスが許可されている場合は、コードにデータベースへのアクセスが許可されていました。  これらの権限やアクセス許可は、ユーザーがローカル コンピューターに明示的にインストールした実行可能ファイルのコードに対しては受け入れることができますが、インターネットやローカル イントラネットからの悪意のある可能性があるコードに対しては受け入れることができません。  このコードは、アクセス許可がないユーザーのコンピューター リソースにアクセスすべきではありません。  
  
 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] は、アクセス許可または権限について、コードが持つものとユーザーが持つものを区別できる、コード アクセス セキュリティと呼ばれるインフラストラクチャを導入しています。  既定では、インターネットとイントラネットからのコードは、部分信頼と呼ばれるものでのみ実行できます。  部分信頼では、アプリケーションに一連の制限が適用されます。特に、アプリケーションはローカルのハード ディスクへのアクセスが制限され、アンマネージ コードを実行することができません。  [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] は、送信元、[厳密な名前付きアセンブリ](../../../docs/framework/app-domains/strong-named-assemblies.md)を持っているかどうか、証明書で署名されているかどうかなど、そのコードの身元に基づいて、コードのアクセスが許可されるリソースを制御します。  
  
 [!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] テクノロジは、Windows フォーム アプリケーションの配置に使用しますが、部分信頼で実行するアプリケーションを、完全信頼や、昇格されたアクセス許可を持つ部分信頼で簡単に開発できるようにします。  [!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] は、アクセス許可の昇格や、信頼されたアプリケーション配置などの機能を提供し、アプリケーションが完全信頼やアクセス許可の昇格をローカル ユーザーから実行可能な方法で要求できます。  
  
## .NET Framework のセキュリティについて  
 コード アクセス セキュリティにより、コードの発生元や、そのコードの身元を示すその他の基準に基づいて、コードをさまざまなレベルで信頼できます。  共通言語ランタイムがセキュリティ ポリシーを決定するために使用する証拠の詳細については、「[証拠](http://msdn.microsoft.com/ja-jp/64ceb7c8-a0b4-46c4-97dc-6c22da0539da)」を参照してください。  悪意のあるコードからコンピューター システムを保護できるだけでなく、セキュリティの意図的または偶然の侵害から信頼されているコードを保護できます。  また、コード アクセス セキュリティは、アプリケーションが実行できるアクションに対して詳細に制御できます。これは、アプリケーションが持っている必要があるアクセス許可のみを指定できるためです。  コード アクセス セキュリティは、コードが単一のコード アクセス セキュリティのアクセス許可のチェックをしない場合でも、共通言語ランタイムを対象とするすべてのマネージ コードに影響を与えます。  [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] のセキュリティの詳細については、「[セキュリティの基本概念](../../../docs/standard/security/key-security-concepts.md)」と「[コード アクセス セキュリティの基礎](../../../docs/framework/misc/code-access-security-basics.md)」を参照してください。  
  
 ユーザーが Windows フォームの実行可能ファイルを Web サーバーまたはファイル共有から直接実行する場合、アプリケーションに付与される信頼の度合いは、コードが存在する場所や開始方法によって異なります。  アプリケーションを実行すると、自動的に評価され、共通言語ランタイムから、名前付き権限セットを受け取ります。  既定では、ローカル コンピューターからのコードに完全信頼のアクセス許可セットが付与され、ローカル ネットワークからのコードには、ローカル イントラネットのアクセス許可セットが付与され、インターネットからのコードには、インターネットのアクセス許可セットが付与されます。  
  
> [!NOTE]
>  [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] バージョン 1.0 Service Pack 1 と Service Pack 2 では、インターネット ゾーンのコード グループが何もしないアクセス許可セットを受け取ります。  その他のすべてのリリースの [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] では、インターネット ゾーンのコード グループは、インターネットのアクセス許可セットを受け取ります。  
>   
>  これらのアクセス許可セットのそれぞれに付与されている既定のアクセス許可の一覧は、「[既定のセキュリティ ポリシー](http://msdn.microsoft.com/ja-jp/2c086873-0894-4f4d-8f7e-47427c1a3b55)」のトピックにあります。  アプリケーションが受け取るアクセス許可に応じて、正常に実行されるか、またはセキュリティ例外が生成されます。  
>   
>  [!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] を使用して、多数の Windows フォーム アプリケーションが配置されます。  [!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] の配置を生成するために使用されるツールは、これまでの説明とは異なる既定のセキュリティを持っています。  詳細については、次の説明を参照してください。  
  
 アプリケーションに付与される実際のアクセス許可は、セキュリティ ポリシーが変更される可能性があるため、既定値とは異なる可能性があります。つまり、アプリケーションが、あるコンピューターではアクセス許可を持ち、別のコンピューターでは持たない可能性があることを意味します。  
  
## より安全な Windows フォーム アプリケーションの開発  
 セキュリティは、アプリケーション開発のすべての手順で重要です。  まず「[安全なコーディングのガイドライン](../../../docs/standard/security/secure-coding-guidelines.md)」を確認して従います。  
  
 次に、アプリケーションを完全信頼で実行する必要があるのか、または部分信頼で実行する必要があるのかを決定します。  完全信頼でアプリケーションを実行することで、ローカル コンピューター上のリソースへのアクセスが簡単になりますが、「安全なコーディングのガイドライン」のトピックに厳密に従ってアプリケーションを設計および開発しない場合、ユーザーとアプリケーションを高いセキュリティ リスクに晒すことになります。  部分信頼でアプリケーションを実行すると、より安全なアプリケーションをより簡単に開発でき、リスクを大幅に軽減しますが、特定の機能の実装方法について、より詳細な計画が必要になります。  
  
 部分信頼 \(つまり、インターネットまたはローカル イントラネットのアクセス許可セットのいずれか\) を選択する場合は、アプリケーションがこの環境で動作する方法を決定します。  Windows フォームでは、信頼度の低い環境で、より安全に機能を実装する代わりの方法を提供します。  データ アクセスなど、アプリケーションの特定の部分が、部分信頼および完全信頼の両方の環境で異なる設計と作成がなされる場合があります。  アプリケーション設定など、一部の Windows フォームの機能は、部分信頼で動作するよう設計されています。  詳細については、「[アプリケーション設定の概要](../../../docs/framework/winforms/advanced/application-settings-overview.md)」を参照してください。  
  
 アプリケーションが部分信頼で許可するより多くのアクセス許可を必要とするが、完全信頼モードで実行したくない場合は、部分信頼で実行しながら、必要な追加のアクセス許可のみをアサートすることができます。  たとえば、部分信頼で実行するが、ユーザーのファイル システム上のディレクトリに、アプリケーションの読み取り専用のアクセス許可を付与する必要がある場合、そのディレクトリのみに対して <xref:System.Security.Permissions.FileIOPermission> を要求することができます。  適切に使用すると、このアプローチによってアプリケーションの機能が増加し、ユーザーのセキュリティ上のリスクを最小限に抑えることができます。  
  
 部分信頼で実行されるアプリケーションを開発するときに、アプリケーションが実行する必要があるアクセス許可と、アプリケーションがオプションで使用できるアクセス許可を管理してください。  すべてのアクセス許可がわかっている場合、アプリケーション レベルでアクセス許可について宣言型の要求をする必要があります。  アクセス許可を要求すると、アプリケーションが必要とするアクセス許可、および特に必要ないアクセス許可について、[!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] のランタイムに通知します。  アクセス許可の要求の詳細については、「[アクセス許可の要求](http://msdn.microsoft.com/ja-jp/0447c49d-8cba-45e4-862c-ff0b59bebdc2)」を参照してください。  
  
 オプションのアクセス許可を要求するときに、アプリケーションに付与されていないアクセス許可が必要なアクションを実行する場合に生成されるセキュリティ例外を処理する必要があります。  <xref:System.Security.SecurityException> を適切に処理することで、アプリケーションを続行できます。  アプリケーションは例外を使用して、ユーザーに対して機能を無効にする必要があるかどうかを判断できます。  たとえば、必要なファイルのアクセス許可が付与されていない場合、アプリケーションが **\[保存\]** メニュー オプションを無効にすることができます。  
  
 場合によっては、すべての適切なアクセス許可をアサートしたかどうかを確認することが難しい場合があります。  たとえば、表面的には影響のないように見えるメソッドの呼び出しが、実行中のある時点でファイル システムにアクセスすることがあります。  必要なアクセス許可をすべて使用してアプリケーションを配置しない場合は、デスクトップでデバッグしている間は問題なくテストでき、配置のときに失敗する可能性があります。  [!INCLUDE[dnprdnlong](../../../includes/dnprdnlong-md.md)] SDK と [!INCLUDE[vsprvslong](../../../includes/vsprvslong-md.md)] の両方に、それぞれ MT.exe のコマンド ライン ツールと、[!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] のアクセス許可の検出機能という、アプリケーションに必要なアクセス許可を検出するためのツールが含まれています。  
  
 次のトピックでは、Windows フォームの追加のセキュリティ機能について説明します。  
  
|トピック|説明|  
|----------|--------|  
|-   [Windows フォームにおけるファイルおよびデータへのより安全なアクセス](../../../docs/framework/winforms/more-secure-file-and-data-access-in-windows-forms.md)|部分信頼環境でファイルとデータにアクセスする方法について説明します。|  
|-   [Windows フォームでのより安全な印刷](../../../docs/framework/winforms/more-secure-printing-in-windows-forms.md)|部分信頼環境で印刷機能にアクセスする方法について説明します。|  
|-   [Windows フォームのセキュリティに関するその他の考慮事項](../../../docs/framework/winforms/additional-security-considerations-in-windows-forms.md)|部分信頼環境でのウィンドウ操作の実行、クリップボードの使用、およびアンマネージ コードへの呼び出しについて説明します。|  
  
### 適切なアクセス許可を持つアプリケーションの配置  
 クライアント コンピューターに Windows フォーム アプリケーションを配置する最も一般的な方法は、アプリケーションが実行する必要があるすべてのコンポーネントを記述する配置テクノロジの [!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] を使用することです。  [!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] は、マニフェストと呼ばれる XML ファイルを使用して、アプリケーションを構成するアセンブリとファイル、およびアプリケーションに必要なアクセス許可を記述します。  
  
 [!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] は、クライアント コンピューターでアクセス許可の昇格を要求するための 2 つのテクノロジを持っています。  どちらのテクノロジも、Authenticode 証明書の使用に依存します。  証明書は、アプリケーションが信頼できる発行元からのものであることをユーザーにある程度保証するのに役立ちます。  
  
 次の表では、それらのテクノロジについて説明します。  
  
|昇格されたアクセス許可のテクノロジ|説明|  
|-----------------------|--------|  
|アクセス許可の昇格|アプリケーションを最初に実行するときにセキュリティ ダイアログ ボックスをユーザーに表示します。  **\[アクセス許可の昇格\]** ダイアログ ボックスは、アプリケーションをだれが公開したのかについてユーザーに通知するので、ユーザーが追加の信頼を付与するかどうかについて、十分な情報を検討したうえで決定できます。|  
|信頼されたアプリケーションの配置|システム管理者がクライアント コンピューターで、発行元の Authenticode 証明書のインストールを 1 回実行する必要があります。  その時点から、その証明書で署名されたアプリケーションは信頼済みとみなされ、追加メッセージを表示せずに、ローカル コンピューター上で完全信頼で実行できます。|  
  
 どのテクノロジを選択するかは、配置環境に応じて異なります。  詳細については、「[ClickOnce 配置ストラテジの選択](../Topic/Choosing%20a%20ClickOnce%20Deployment%20Strategy.md)」を参照してください。  
  
 既定では、[!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] または [!INCLUDE[dnprdnlong](../../../includes/dnprdnlong-md.md)] SDK ツール \(Mage.exe および MageUI.exe\) のいずれかを使用して配置された [!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] アプリケーションは、完全信頼を持つクライアント コンピューターで実行するよう構成されます。  部分信頼を使用して、またはいくつか追加のアクセス許可のみを使用して、アプリケーションを配置している場合、この既定を変更する必要があります。  配置を構成するときに、[!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] または [!INCLUDE[dnprdnlong](../../../includes/dnprdnlong-md.md)] SDK ツールの MageUI.exe のいずれかを使用して構成できます。  MageUI.exe を使用する方法の詳細については、「チュートリアル : コマンドラインから ClickOnce アプリケーションを配置する」を参照してください。  「[方法 : ClickOnce アプリケーションのカスタム アクセス許可を設定する](http://msdn.microsoft.com/library/hafybdaa\(v=vs.110\))」または「[方法 : ClickOnce アプリケーションのカスタム アクセス許可を設定する](http://msdn.microsoft.com/library/hafybdaa\(v=vs.120\))」も参照してください。  
  
 [!INCLUDE[ndptecclick](../../../includes/ndptecclick-md.md)] とアクセス許可の昇格のセキュリティ面の詳細については、「[ClickOnce アプリケーションのセキュリティ](../Topic/Securing%20ClickOnce%20Applications.md)」を参照してください。  信頼されたアプリケーションの配置の詳細については、「[信頼されたアプリケーションの配置の概要](../Topic/Trusted%20Application%20Deployment%20Overview.md)」を参照してください。  
  
### アプリケーションのテスト  
 [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] を使用して Windows フォーム アプリケーションを配置した場合は、開発環境から、部分信頼または制限されたアクセス許可セットでのデバッグを有効にできます。  「[方法 : アクセス許可が制限された ClickOnce アプリケーションをデバッグする](http://msdn.microsoft.com/library/593zkfdf\(v=vs.110\))」または「[方法 : アクセス許可が制限された ClickOnce アプリケーションをデバッグする](http://msdn.microsoft.com/library/593zkfdf\(v=vs.120\))」も参照してください。  
  
## 参照  
 [Windows フォームのセキュリティ](../../../docs/framework/winforms/windows-forms-security.md)   
 [コード アクセス セキュリティの基礎](../../../docs/framework/misc/code-access-security-basics.md)   
 [ClickOnce のセキュリティと配置](../Topic/ClickOnce%20Security%20and%20Deployment.md)   
 [信頼されたアプリケーションの配置の概要](../Topic/Trusted%20Application%20Deployment%20Overview.md)   
 [Mage.exe \(マニフェストの生成および編集ツール\)](../../../docs/framework/tools/mage-exe-manifest-generation-and-editing-tool.md)   
 [MageUI.exe \(マニフェスト生成および編集ツールのグラフィカル クライアント\)](../../../docs/framework/tools/mageui-exe-manifest-generation-and-editing-tool-graphical-client.md)