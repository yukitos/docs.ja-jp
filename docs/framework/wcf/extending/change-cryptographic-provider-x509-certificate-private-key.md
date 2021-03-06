---
title: "方法 : X.509 証明書の秘密キーの暗号化プロバイダーを変更する | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "暗号化プロバイダー [WCF]"
  - "暗号化プロバイダー [WCF], 変更"
ms.assetid: b4254406-272e-4774-bd61-27e39bbb6c12
caps.latest.revision: 7
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 7
---
# 方法 : X.509 証明書の秘密キーの暗号化プロバイダーを変更する
ここでは、X.509 証明書の秘密キーを提供するために使用する暗号化プロバイダーを変更する方法、およびそのプロバイダーを [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] のセキュリティ フレームワークに統合する方法を説明します。  証明書の使用に関する詳細については、「[証明書の使用](../../../../docs/framework/wcf/feature-details/working-with-certificates.md)」を参照してください。  
  
 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] のセキュリティ フレームワークには、「[方法 : カスタム トークンを作成する](../../../../docs/framework/wcf/extending/how-to-create-a-custom-token.md)」で説明されているように、新しい種類のセキュリティ トークンを導入する方法が用意されています。  また、カスタム トークンを使用して既存のシステム指定のトークンを置き換えることも可能です。  
  
 このトピックでは、システム指定の X.509 セキュリティ トークンが、証明書の秘密キーに別の実装を提供するカスタムの X.509 トークンによって置き換えられます。  これは、実際の秘密キーが、既定の Windows 暗号化プロバイダーとは異なる暗号化プロバイダーから提供されるシナリオで役立ちます。  別の暗号化プロバイダーの例として、ハードウェア セキュリティ モジュールがあります。このモジュールでは、すべての秘密キー関連の暗号操作を実行し、メモリに秘密キーを格納しないので、システムのセキュリティが向上します。  
  
 次の例は、デモンストレーションのためだけに作成されています。  この例では、既定の Windows 暗号化プロバイダーを置き換えていませんが、Windows 暗号化プロバイダーをどこで統合できるかを示します。  
  
## 手順  
 セキュリティ キーが関連付けられているセキュリティ トークンごとに、セキュリティ トークンのインスタンスからキーのコレクションを返す <xref:System.IdentityModel.Tokens.SecurityToken.SecurityKeys%2A> プロパティを実装する必要があります。  トークンが X.509 セキュリティ トークンの場合、コレクションには、証明書に関連付けられた公開キーと秘密キーの両方を表す <xref:System.IdentityModel.Tokens.X509AsymmetricSecurityKey> クラスのインスタンスが 1 つ追加されます。  証明書のキーの提供に使用する既定の暗号化プロバイダーを置き換えるには、このクラスの新しい実装を作成します。  
  
#### カスタムの X.509 非対称キーを作成するには  
  
1.  <xref:System.IdentityModel.Tokens.X509AsymmetricSecurityKey> クラスから派生する新しいクラスを定義します。  
  
2.  <xref:System.IdentityModel.Tokens.SecurityKey.KeySize%2A> 読み取り専用プロパティをオーバーライドします。  このプロパティは、証明書の公開キーと秘密キーのペアの実際のキー サイズを返します。  
  
3.  <xref:System.IdentityModel.Tokens.SecurityKey.DecryptKey%2A> メソッドをオーバーライドします。  このメソッドは、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] のセキュリティ フレームワークによって呼び出され、対称キーを証明書の秘密キーで復号化します  \(このキーは、以前に証明書の公開キーで暗号化されています\)。  
  
4.  <xref:System.IdentityModel.Tokens.AsymmetricSecurityKey.GetAsymmetricAlgorithm%2A> メソッドをオーバーライドします。  このメソッドは、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] のセキュリティ フレームワークによって呼び出され、このメソッドに渡されるパラメーターに応じて、証明書の秘密キーまたは公開キーの暗号化プロバイダーを表す <xref:System.Security.Cryptography.AsymmetricAlgorithm> クラスのインスタンスを取得します。  
  
5.  省略可能です。  <xref:System.IdentityModel.Tokens.AsymmetricSecurityKey.GetHashAlgorithmForSignature%2A> メソッドをオーバーライドします。  <xref:System.Security.Cryptography.HashAlgorithm> クラスの別の実装が必要な場合は、このメソッドをオーバーライドします。  
  
6.  <xref:System.IdentityModel.Tokens.AsymmetricSecurityKey.GetSignatureFormatter%2A> メソッドをオーバーライドします。  このメソッドは、証明書の秘密キーに関連付けられた <xref:System.Security.Cryptography.AsymmetricSignatureFormatter> クラスのインスタンスを返します。  
  
7.  <xref:System.IdentityModel.Tokens.SecurityKey.IsSupportedAlgorithm%2A> メソッドをオーバーライドします。  このメソッドを使用して、特定の暗号アルゴリズムがセキュリティ キー実装によってサポートされているかどうかを示します。  
  
     [!code-csharp[c_CustomX509Token#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#1)]
     [!code-vb[c_CustomX509Token#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#1)]  
  
 次の手順は、システム指定の X.509 セキュリティ トークンを置き換えるために、前の手順で作成したカスタムの X.509 非対称セキュリティ キーの実装を [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] のセキュリティ フレームワークに統合する方法を示しています。  
  
#### システム指定の X.509 セキュリティ トークンをカスタムの X.509 非対称セキュリティ キー トークンで置き換えるには  
  
1.  次の例に示すように、システム指定のセキュリティ キーではなく、カスタムの X.509 非対称セキュリティ キーを返すカスタムの X.509 非対称セキュリティ トークンを作成します。  カスタム セキュリティ トークンの詳細については、「[方法 : カスタム トークンを作成する](../../../../docs/framework/wcf/extending/how-to-create-a-custom-token.md)」を参照してください。  
  
     [!code-csharp[c_CustomX509Token#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#2)]
     [!code-vb[c_CustomX509Token#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#2)]  
  
2.  次の例に示すように、カスタムの X.509 セキュリティ トークンを返すカスタムのセキュリティ トークン プロバイダーを作成します。  カスタム セキュリティ トークン プロバイダーの詳細については、「[方法 : カスタム セキュリティ トークン プロバイダーを作成する](../../../../docs/framework/wcf/extending/how-to-create-a-custom-security-token-provider.md)」を参照してください。  
  
     [!code-csharp[c_CustomX509Token#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#3)]
     [!code-vb[c_CustomX509Token#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#3)]  
  
3.  イニシエーター側でカスタム セキュリティ キーを使用する必要がある場合、次の例に示すように、カスタムのクライアント セキュリティ トークン マネージャーとカスタムのクライアント資格情報クラスを作成します。  カスタムのクライアント資格情報とクライアント セキュリティ トークン マネージャーの詳細については、「[チュートリアル: カスタム クライアントおよびサービスの資格情報を作成する](../../../../docs/framework/wcf/extending/walkthrough-creating-custom-client-and-service-credentials.md)」を参照してください。  
  
     [!code-csharp[c_CustomX509Token#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#4)]
     [!code-vb[c_CustomX509Token#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#4)]  
  
     [!code-csharp[c_CustomX509Token#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#6)]
     [!code-vb[c_CustomX509Token#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#6)]  
  
4.  受信側でカスタム セキュリティ キーを使用する必要がある場合、次の例に示すように、カスタムのサービス セキュリティ トークン マネージャーとカスタムのサービス資格情報クラスを作成します。  カスタムのサービス資格情報とサービス セキュリティ トークン マネージャーの詳細については、「[チュートリアル: カスタム クライアントおよびサービスの資格情報を作成する](../../../../docs/framework/wcf/extending/walkthrough-creating-custom-client-and-service-credentials.md)」を参照してください。  
  
     [!code-csharp[c_CustomX509Token#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#5)]
     [!code-vb[c_CustomX509Token#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#5)]  
  
     [!code-csharp[c_CustomX509Token#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_customx509token/cs/source.cs#7)]
     [!code-vb[c_CustomX509Token#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_customx509token/vb/source.vb#7)]  
  
## 参照  
 <xref:System.IdentityModel.Tokens.X509AsymmetricSecurityKey>   
 <xref:System.IdentityModel.Tokens.AsymmetricSecurityKey>   
 <xref:System.IdentityModel.Tokens.SecurityKey>   
 <xref:System.Security.Cryptography.AsymmetricAlgorithm>   
 <xref:System.Security.Cryptography.HashAlgorithm>   
 <xref:System.Security.Cryptography.AsymmetricSignatureFormatter>   
 [チュートリアル: カスタム クライアントおよびサービスの資格情報を作成する](../../../../docs/framework/wcf/extending/walkthrough-creating-custom-client-and-service-credentials.md)   
 [方法 : カスタム セキュリティ トークン認証システムを作成する](../../../../docs/framework/wcf/extending/how-to-create-a-custom-security-token-authenticator.md)   
 [方法 : カスタム セキュリティ トークン プロバイダーを作成する](../../../../docs/framework/wcf/extending/how-to-create-a-custom-security-token-provider.md)   
 [方法 : カスタム トークンを作成する](../../../../docs/framework/wcf/extending/how-to-create-a-custom-token.md)   
 [Security Architecture](http://msdn.microsoft.com/ja-jp/16593476-d36a-408d-808c-ae6fd483e28f)