---
title: "WCF の委任と偽装 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "偽装 [WCF]"
  - "委任 [WCF]"
ms.assetid: 110e60f7-5b03-4b69-b667-31721b8e3152
caps.latest.revision: 40
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 40
---
# WCF の委任と偽装
*偽装*は、サービス ドメインのリソースへのクライアント アクセスを制限するためにサービスが使用する一般的な手法です。 サービス ドメインのリソースは、ローカル ファイルなどのコンピューター リソースの場合もあれば \(偽装\)、ファイル共有などの別のコンピューター上のリソースの場合もあります \(委任\)。 サンプル アプリケーションについては、「[クライアントの偽装](../../../../docs/framework/wcf/samples/impersonating-the-client.md)」を参照してください。 偽装の使用方法の例については、「[方法 : サービスでクライアントに偽装する](../../../../docs/framework/wcf/how-to-impersonate-a-client-on-a-service.md)」を参照してください。  
  
> [!IMPORTANT]
>  サービスでクライアントを偽装すると、サービスはそのクライアントの資格情報を使用して実行されます。この資格情報の特権がサーバー プロセスより高い場合があることに注意してください。  
  
## 概要  
 通常、クライアントは、クライアントに代わって何らかのアクションを実行してもらうためにサービスを呼び出します。 偽装により、サービスはアクションの実行時にクライアントとして機能できます。 委任を使用すると、フロントエンド サービスは、バックエンド サービスもクライアントを偽装できるような方法で、バックエンド サービスにクライアントの要求を転送できます。 偽装は、クライアントが特定のアクションを実行するために承認されているかどうかを確認する方法として、最も一般的に使用されます。委任は、偽装機能をクライアントの ID と共にバックエンド サービスにフローする方法です。 委任は、Kerberos ベースの認証を実行するときに使用できる Windows ドメインの機能です。 委任は ID フローとは異なります。委任では、クライアントのパスワードを保持せずにクライアントを偽装する機能を転送するため、ID フローよりもかなり高い特権が与えられた操作です。  
  
 偽装と委任はいずれも、クライアントが Windows ID を保持していることが必要となります。 クライアントが Windows ID を保持していない場合、クライアントの ID を 2 番目のサービスにフローする以外に選択肢はありません。  
  
## 偽装の基本  
 [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] では、さまざまなクライアント資格情報の偽装をサポートしています。 このトピックでは、サービス メソッドの実装時に呼び出し元を偽装するためのサービス モデルのサポートについて説明します。 また、偽装と SOAP セキュリティを必要とする一般的な展開シナリオ、およびこれらのシナリオでの [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] のオプションについても説明します。  
  
 ここでは、SOAP セキュリティを使用する場合の [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] での偽装と委任について集中的に説明します。 「[トランスポート セキュリティでの偽装の使用](../../../../docs/framework/wcf/feature-details/using-impersonation-with-transport-security.md)」に記載されているように、トランスポート セキュリティを使用する場合にも、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] で偽装と委任を使用できます。  
  
## 2 つの方法  
 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] の SOAP セキュリティには、偽装を実行するための 2 とおりの方法があります。 使用する方法は、バインディングによって異なります。 1 つは、セキュリティ サポート プロバイダー インターフェイス \(SSPI\) または Kerberos 認証から取得し、サーバーにキャッシュされた Windows トークンの偽装です。 もう 1 つは、*Service\-for\-User \(S4U\)* と総称される Kerberos 拡張から取得した Windows トークンの偽装です。  
  
### キャッシュされたトークンの偽装  
 キャッシュされたトークンの偽装は、以下で実行できます。  
  
-   Windows クライアント資格情報を使用する <xref:System.ServiceModel.WSHttpBinding>、<xref:System.ServiceModel.WSDualHttpBinding>、および <xref:System.ServiceModel.NetTcpBinding>。  
  
-   <xref:System.ServiceModel.BasicHttpBinding> が <xref:System.ServiceModel.BasicHttpSecurityMode> 資格情報に設定された <xref:System.ServiceModel.BasicHttpSecurityMode>。または、サービスが有効な Windows アカウントにマップできるユーザー名資格情報をクライアントが提示する、その他の標準バインディング。  
  
-   <xref:System.ServiceModel.Channels.CustomBinding> が `requireCancellation` に設定された Windows クライアント資格情報を使用する `true` \(このプロパティは、<xref:System.ServiceModel.Security.Tokens.SecureConversationSecurityTokenParameters>、<xref:System.ServiceModel.Security.Tokens.SslSecurityTokenParameters>、および <xref:System.ServiceModel.Security.Tokens.SspiSecurityTokenParameters> の各クラスで使用できます\)。 セキュリティで保護されたメッセージ交換をバインディングで使用する場合は、`requireCancellation` プロパティを `true` に設定することも必要です。  
  
-   クライアントがユーザー名資格情報を提示する <xref:System.ServiceModel.Channels.CustomBinding>。 セキュリティで保護されたメッセージ交換をバインディングで使用する場合は、`requireCancellation` プロパティを `true` に設定することも必要です。  
  
### S4U ベースの偽装  
 S4U ベースの偽装は、以下で実行できます。  
  
-   サービスが有効な Windows アカウントにマップできる証明書クライアント資格情報を使用する <xref:System.ServiceModel.WSHttpBinding>、<xref:System.ServiceModel.WSDualHttpBinding>、および <xref:System.ServiceModel.NetTcpBinding>。  
  
-   <xref:System.ServiceModel.Channels.CustomBinding> プロパティが `requireCancellation` に設定された Windows クライアント資格情報を使用する `false`。  
  
-   <xref:System.ServiceModel.Channels.CustomBinding> プロパティが `requireCancellation` に設定されたユーザー名資格情報または Windows クライアント資格情報とセキュリティで保護されたメッセージ交換を使用する `false`。  
  
 サービスがクライアントを偽装できるエクステントは、偽装を試みるときにサービス アカウントが保持している特権、使用する偽装の種類、およびクライアントが許可すると考えられる偽装のエクステントによって異なります。  
  
> [!NOTE]
>  クライアントとサービスが同じコンピューター上で実行されており、クライアントがシステム アカウント \(`Local System` や `Network Service` など\) で実行されているときに、ステートレスなセキュリティ コンテキスト トークンを使用してセキュリティで保護されたセッションを確立した場合は、クライアントを偽装できません。 通常、Windows フォームまたはコンソール アプリケーションは、現在ログインしているアカウントで実行されるため、既定でそのアカウントを偽装できます。 ただし、クライアントが [!INCLUDE[vstecasp](../../../../includes/vstecasp-md.md)] ページであり、そのページが [!INCLUDE[iis601](../../../../includes/iis601-md.md)] または [!INCLUDE[iisver](../../../../includes/iisver-md.md)] でホストされている場合、既定では、クライアントは `Network Service` アカウントで実行されます。 セキュリティで保護されたセッションをサポートするシステム提供のすべてのバインディングは、ステートフルなセキュリティ コンテキスト トークン \(SCT: Security Context Token\) を既定で使用します。 ただし、クライアントが [!INCLUDE[vstecasp](../../../../includes/vstecasp-md.md)] ページであり、ステートフルな SCT を使用する、セキュリティで保護されたセッションを使用している場合は、クライアントを偽装できません。 セキュリティで保護されたセッションでステートフルな SCT を使用する方法の[!INCLUDE[crabout](../../../../includes/crabout-md.md)]については、「[方法 : セキュリティで保護されたセッションに対しセキュリティ コンテキスト トークンを作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-security-context-token-for-a-secure-session.md)」を参照してください。  
  
## サービス メソッドでの偽装 : 宣言モデル  
 ほとんどの偽装シナリオでは、呼び出し元のコンテキストでサービス メソッドを実行する必要があります。[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] には、ユーザーが <xref:System.ServiceModel.OperationBehaviorAttribute> 属性で偽装要件を指定できるようにすることで、これを容易に実行できるようにした偽装機能が用意されています。 たとえば、次のコードでは、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] インフラストラクチャは `Hello` メソッドを実行する前に呼び出し元を偽装します。`Hello` メソッド内でネイティブ リソースへのアクセス試行が成功するのは、そのリソースのアクセス制御リスト \(ACL\) で呼び出し元のアクセス特権が許可されている場合だけです。 偽装を有効にするには、次の例に示すように、<xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A> プロパティを <xref:System.ServiceModel.ImpersonationOption> 列挙値のいずれか \(<xref:System.ServiceModel.ImpersonationOption?displayProperty=fullName> または <xref:System.ServiceModel.ImpersonationOption?displayProperty=fullName>\) に設定します。  
  
> [!NOTE]
>  サービスの資格情報がリモート クライアントよりも高い場合は、<xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A> プロパティが <xref:System.ServiceModel.ImpersonationOption> に設定されていても、サービスの資格情報が使用されます。 つまり、低い特権を持つユーザーがその資格情報を提示した場合、そのユーザーよりも高い特権を持つサービスは、サービスの資格情報を使用してメソッドを実行し、特権の低いユーザーが本来は使用できないリソースを使用できることになります。  
  
 [!code-csharp[c_ImpersonationAndDelegation#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_impersonationanddelegation/cs/source.cs#1)]
 [!code-vb[c_ImpersonationAndDelegation#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_impersonationanddelegation/vb/source.vb#1)]  
  
 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] インフラストラクチャが呼び出し元を偽装できるのは、呼び出し元が Windows ユーザー アカウントにマップできる資格情報を使用して認証された場合だけです。 サービスが Windows アカウントにマップできない資格情報を使用して認証を行うように構成されている場合には、サービス メソッドは実行されません。  
  
> [!NOTE]
>  [!INCLUDE[wxp](../../../../includes/wxp-md.md)] では、ステートフルな SCT が作成されると偽装が失敗し、<xref:System.InvalidOperationException> になります。[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [サポートされていないシナリオ](../../../../docs/framework/wcf/feature-details/unsupported-scenarios.md)。  
  
## サービス メソッドでの偽装 : 強制モデル  
 呼び出し元がサービス メソッドの全体ではなく、一部を偽装するだけで、その機能が実行される場合があります。 この場合、サービス メソッド内で呼び出し元の Windows ID を取得し、偽装を強制的に実行します。 これを行うには、<xref:System.ServiceModel.ServiceSecurityContext.WindowsIdentity%2A> の <xref:System.ServiceModel.ServiceSecurityContext> プロパティを使用して <xref:System.Security.Principal.WindowsIdentity> クラスのインスタンスを返し、このインスタンスを使用する前に <xref:System.Security.Principal.WindowsIdentity.Impersonate%2A> メソッドを呼び出します。  
  
> [!NOTE]
>  [!INCLUDE[vbprvb](../../../../includes/vbprvb-md.md)] の `Using` ステートメント、または C\# の `using` ステートメントを使用して、偽装操作を自動的に元に戻すようにしてください。 これらのステートメントを使用しない場合、または [!INCLUDE[vbprvb](../../../../includes/vbprvb-md.md)] や C\# 以外のプログラミング言語を使用する場合は、偽装レベルを必ず元に戻してください。 この作業を怠ると、サービス拒否攻撃や権限の昇格攻撃のもとになるおそれがあります。  
  
 [!code-csharp[c_ImpersonationAndDelegation#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_impersonationanddelegation/cs/source.cs#2)]
 [!code-vb[c_ImpersonationAndDelegation#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_impersonationanddelegation/vb/source.vb#2)]  
  
## すべてのサービス メソッドの偽装  
 サービスのすべてのメソッドを呼び出し元のコンテキストで実行することが必要になる場合があります。 メソッドごとにこの機能を明示的に有効にするのではなく、<xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> を使用します。 次のコードに示すように、<xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.ImpersonateCallerForAllOperations%2A> プロパティを `true` に設定します。<xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> は、<xref:System.ServiceModel.ServiceHost> クラスの動作コレクションから取得されます。 また、各メソッドに適用する `Impersonation` の <xref:System.ServiceModel.OperationBehaviorAttribute> プロパティを <xref:System.ServiceModel.ImpersonationOption> または <xref:System.ServiceModel.ImpersonationOption> に設定することも必要です。  
  
 [!code-csharp[c_ImpersonationAndDelegation#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_impersonationanddelegation/cs/source.cs#3)]
 [!code-vb[c_ImpersonationAndDelegation#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_impersonationanddelegation/vb/source.vb#3)]  
  
 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] と `ImpersonationOption` の可能なすべての組み合わせに対する `ImpersonateCallerForAllServiceOperations` の動作を次の表に示します。  
  
|`ImpersonationOption`|`ImpersonateCallerForAllServiceOperations`|動作|  
|---------------------------|------------------------------------------------|--------|  
|必須|適用なし|[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] は、呼び出し元を偽装します。|  
|Allowed|false|[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] は、呼び出し元を偽装しません。|  
|Allowed|TRUE|[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] は、呼び出し元を偽装します。|  
|NotAllowed|false|[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] は、呼び出し元を偽装しません。|  
|NotAllowed|TRUE|使用できません  \(<xref:System.InvalidOperationException> がスローされます\)。|  
  
## Windows 資格情報とキャッシュされたトークンの偽装から取得する偽装レベル  
 Windows クライアント資格情報の使用時にサービスが実行する偽装のレベルを、クライアントが部分的に制御するシナリオがあります。 クライアントが匿名偽装レベルを指定した場合に発生するシナリオもあれば、 キャッシュされたトークンを使用して偽装を実行した場合に発生するシナリオもあります。 これを行うには、<xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> クラスの <xref:System.ServiceModel.Security.WindowsClientCredential> プロパティを設定します。このクラスには、ジェネリック クラス <xref:System.ServiceModel.ChannelFactory%601> のプロパティとしてアクセスします。  
  
> [!NOTE]
>  匿名偽装レベルを指定すると、クライアントはサービスに匿名でログオンします。 したがって、偽装を実行するかどうかに関係なく、サービスは匿名ログオンを許可する必要があります。  
  
 クライアントは、偽装レベルとして <xref:System.Security.Principal.TokenImpersonationLevel>、<xref:System.Security.Principal.TokenImpersonationLevel>、<xref:System.Security.Principal.TokenImpersonationLevel>、または <xref:System.Security.Principal.TokenImpersonationLevel> を指定できます。 次のコードに示すように、指定したレベルのトークンだけが作成されます。  
  
 [!code-csharp[c_ImpersonationAndDelegation#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_impersonationanddelegation/cs/source.cs#4)]
 [!code-vb[c_ImpersonationAndDelegation#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_impersonationanddelegation/vb/source.vb#4)]  
  
 キャッシュされたトークンを使用して偽装するときに、サービスが取得する偽装レベルを次の表に示します。  
  
|`AllowedImpersonationLevel` の値|サービスに `SeImpersonatePrivilege` がある|サービスとクライアントに処理を代行する機能がある|キャッシュされたトークンの `ImpersonationLevel`|  
|------------------------------------|----------------------------------------|------------------------------|----------------------------------------|  
|Anonymous|はい|適用なし|偽装|  
|Anonymous|いいえ|適用なし|識別|  
|識別|適用なし|適用なし|識別|  
|偽装|はい|適用なし|偽装|  
|偽装|いいえ|適用なし|識別|  
|処理の代行|はい|はい|処理の代行|  
|処理の代行|はい|いいえ|偽装|  
|処理の代行|いいえ|適用なし|識別|  
  
## ユーザー名資格情報とキャッシュされたトークンの偽装から取得する偽装レベル  
 クライアントがユーザー名とパスワードをサービスに渡すことで、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] はそのユーザーとしてログオンできるようになります。これは、`AllowedImpersonationLevel` プロパティを <xref:System.Security.Principal.TokenImpersonationLevel> に設定することに相当します  \(`AllowedImpersonationLevel` は、<xref:System.ServiceModel.Security.WindowsClientCredential> クラスと <xref:System.ServiceModel.Security.HttpDigestClientCredential> クラスで使用できます\)。 サービスがユーザー名資格情報を受け取るときに取得する偽装レベルを次の表に示します。  
  
|`AllowedImpersonationLevel`|サービスに `SeImpersonatePrivilege` がある|サービスとクライアントに処理を代行する機能がある|キャッシュされたトークンの `ImpersonationLevel`|  
|---------------------------------|----------------------------------------|------------------------------|----------------------------------------|  
|適用なし|はい|はい|処理の代行|  
|適用なし|はい|いいえ|偽装|  
|適用なし|いいえ|適用なし|識別|  
  
## S4U ベースの偽装から取得する偽装レベル  
  
|サービスに `SeTcbPrivilege` がある|サービスに `SeImpersonatePrivilege` がある|サービスとクライアントに処理を代行する機能がある|キャッシュされたトークンの `ImpersonationLevel`|  
|--------------------------------|----------------------------------------|------------------------------|----------------------------------------|  
|はい|はい|適用なし|偽装|  
|はい|いいえ|適用なし|識別|  
|いいえ|適用なし|適用なし|識別|  
  
## クライアント証明書から Windows アカウントへのマッピング  
 クライアントは、証明書を使用してサービスに対してクライアント自身を認証し、サービスが Active Directory を使用してクライアントを既存のアカウントにマップするように操作できます。 次の XML は、証明書をマップするためにサービスを構成する方法を示しています。  
  
```xml  
  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="MapToWindowsAccount">  
      <serviceCredentials>  
        <clientCertificate>  
          <authentication mapClientCertificateToWindowsAccount="true" />  
        </clientCertificate>  
      </serviceCredentials>  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
  
```  
  
 次のコードはサービスを構成する方法を示しています。  
  
```  
  
// Create a binding that sets a certificate as the client credential type.  
WSHttpBinding b = new WSHttpBinding();  
b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;  
  
// Create a service host that maps the certificate to a Windows account.  
Uri httpUri = new Uri("http://localhost/Calculator");  
ServiceHost sh = new ServiceHost(typeof(HelloService), httpUri);  
sh.Credentials.ClientCertificate.Authentication.MapClientCertificateToWindowsAccount = true;  
  
```  
  
## 処理の代行  
 バックエンド サービスに処理を代行させるには、サービスが Kerberos マルチレッグ \(NTLM にフォールバックしない SSPI\) を実行するか、クライアントの Windows ID を使用して、バックエンド サービスに対する Kerberos 直接認証を実行する必要があります。 バックエンド サービスに処理を代行させる場合、<xref:System.ServiceModel.ChannelFactory%601> とチャネルを作成し、クライアントを偽装している間、このチャネル経由で通信を行います。 この形式の委任では、バックエンド サービスとフロントエンド サービス間の距離は、フロントエンド サービスで実現される偽装レベルによって決まります。 偽装レベルが <xref:System.Security.Principal.TokenImpersonationLevel> の場合、フロントエンド サービスとバックエンド サービスは、同じコンピューター上で実行されている必要があります。 偽装レベルが <xref:System.Security.Principal.TokenImpersonationLevel> の場合、フロントエンド サービスとバックエンド サービスは、別々のコンピューター上にあっても、同じコンピューター上にあってもかまいません。 Delegation レベルの偽装を有効にする場合、委任を許可するように Windows ドメイン ポリシーを構成する必要があります。 委任をサポートできるように Active Directory を構成する方法の詳細については、「[Enabling Delegated Authentication \(委任認証の有効化\)](http://go.microsoft.com/fwlink/?LinkId=99690)」を参照してください。  
  
> [!NOTE]
>  バックエンド サービスで Windows アカウントに対応するユーザー名とパスワードを使用して、フロントエンド サービスに対するクライアント認証を行うと、フロントエンド サービスはこのユーザー名とパスワードを再利用して、バックエンド サービスに対する認証を行うことができます。 ユーザー名とパスワードをバックエンド サービスに渡すことで、バックエンド サービスが偽装を実行できるため、これは ID フローの特に強力な形式と言えますが、Kerberos を使用しないため、委任は構成されません。 Active Directory による委任の制御は、ユーザー名\/パスワード認証には適用されません。  
  
### 偽装レベルの 1 つの機能としての委任機能  
  
|偽装レベル|サービスがプロセス間の委任を実行できる|サービスがコンピューター間の委任を実行できる|  
|-----------|-------------------------|----------------------------|  
|<xref:System.Security.Principal.TokenImpersonationLevel>|いいえ|いいえ|  
|<xref:System.Security.Principal.TokenImpersonationLevel>|○|いいえ|  
|<xref:System.Security.Principal.TokenImpersonationLevel>|はい|はい|  
  
 委任の使用方法を次のコード例に示します。  
  
 [!code-csharp[c_delegation#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_delegation/cs/source.cs#1)]
 [!code-vb[c_delegation#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_delegation/vb/source.vb#1)]  
  
### 制約された委任を使用するようにアプリケーションを構成する方法  
 制約された委任を使用するには、送信側、受信側、およびドメイン コントローラーを制約された委任を使用するように構成する必要があります。 制約された委任を有効にする手順を以下に示します。 委任と制約された委任の違いの詳細については、「[Windows Server 2003 Kerberos Extensions \(Windows Server 2003 Kerberos 拡張機能\)](http://go.microsoft.com/fwlink/?LinkId=100194)」の制約された委任に関する部分を参照してください。  
  
1.  ドメイン コントローラーで、クライアント アプリケーションを実行しているアカウントの **\[アカウントは重要なので委任できない\]** チェック ボックスをオフにします。  
  
2.  ドメイン コントローラーで、クライアント アプリケーションを実行しているアカウントの **\[アカウントは委任に対して信頼されている\]** チェック ボックスをオンにします。  
  
3.  ドメイン コントローラーで、**\[コンピューターを委任に対して信頼する\]** をクリックして、委任に対して信頼されるように中間層コンピューターを構成します。  
  
4.  ドメイン コントローラーで、**\[指定されたサービスへの委任でのみこのコンピューターを信頼する\]** をクリックして、制約された委任を使用するように中間層コンピューターを構成します。  
  
 制約された委任を構成する手順の詳細については、MSDN の次のトピックを参照してください。  
  
-   [Kerberos 委任のトラブルシューティング](http://go.microsoft.com/fwlink/?LinkId=36724)  
  
-   [Kerberos プロトコルの遷移および制約委任](http://go.microsoft.com/fwlink/?LinkId=36725)  
  
## 参照  
 <xref:System.ServiceModel.OperationBehaviorAttribute>   
 <xref:System.ServiceModel.OperationBehaviorAttribute.Impersonation%2A>   
 <xref:System.ServiceModel.ImpersonationOption>   
 <xref:System.ServiceModel.ServiceSecurityContext.WindowsIdentity%2A>   
 <xref:System.ServiceModel.ServiceSecurityContext>   
 <xref:System.Security.Principal.WindowsIdentity>   
 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>   
 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.ImpersonateCallerForAllOperations%2A>   
 <xref:System.ServiceModel.ServiceHost>   
 <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A>   
 <xref:System.ServiceModel.Security.WindowsClientCredential>   
 <xref:System.ServiceModel.ChannelFactory%601>   
 <xref:System.Security.Principal.TokenImpersonationLevel>   
 [トランスポート セキュリティでの偽装の使用](../../../../docs/framework/wcf/feature-details/using-impersonation-with-transport-security.md)   
 [クライアントの偽装](../../../../docs/framework/wcf/samples/impersonating-the-client.md)   
 [方法 : サービスでクライアントに偽装する](../../../../docs/framework/wcf/how-to-impersonate-a-client-on-a-service.md)   
 [ServiceModel メタデータ ユーティリティ ツール \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md)