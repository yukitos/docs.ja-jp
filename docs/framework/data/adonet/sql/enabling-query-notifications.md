---
title: "クエリ通知の有効化 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: a5333e19-8e55-4aa9-82dc-ca8745e516ed
caps.latest.revision: 6
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 6
---
# クエリ通知の有効化
クエリ通知を使用するアプリケーションには、いくつか共通する要件があります。  SQL クエリ通知をサポートするには、データ ソースが正しく設定され、ユーザーがクライアント側およびサーバー側の正しい権限を所有している必要があります。  
  
 クエリ通知を使用するためには、次のことが必要です。  
  
-   データベースのクエリ通知を有効にする  
  
-   データベースへの接続に使用するユーザー ID に対して、必要なアクセス許可を設定する  
  
-   <xref:System.Data.SqlClient.SqlCommand> オブジェクトを使用して、関連する通知オブジェクト \(<xref:System.Data.SqlClient.SqlDependency> または <xref:System.Data.Sql.SqlNotificationRequest> のいずれか\) を持つ有効な SELECT ステートメントを実行する  
  
-   監視対象のデータが変更された場合に通知を処理するコードを設定する  
  
## クエリ通知の要件  
 クエリ通知は、特定の要件を満たす SELECT ステートメントでのみサポートされます。  次の表に、SQL Server オンライン ブックの Service Broker とクエリ通知に関するドキュメントへのリンクを示します。  
  
 **SQL Server オンライン ブック**  
  
-   [クエリ通知の作成](http://msdn.microsoft.com/library/ms181122.aspx)  
  
-   [Service Broker のセキュリティに関する注意点](http://msdn.microsoft.com/library/ms166059.aspx)  
  
-   [セキュリティと保護 \(Service Broker\)](http://msdn.microsoft.com/library/bb522911.aspx)  
  
-   [Notification Services のセキュリティに関する注意点](http://msdn.microsoft.com/library/ms172604.aspx)  
  
-   [クエリ通知の権限](http://msdn.microsoft.com/library/ms188311.aspx)  
  
-   [Service Broker の国際化に関する注意点](http://msdn.microsoft.com/library/ms166028.aspx)  
  
-   [ソリューション設計に関する考慮事項 \(Service Broker\)](http://msdn.microsoft.com/library/bb522899.aspx)  
  
-   [Service Broker 開発者向けの情報](http://msdn.microsoft.com/library/ms166100.aspx)  
  
-   [開発者ガイド \(Service Broker\)](http://msdn.microsoft.com/library/bb522908.aspx)  
  
## サンプル コードを実行するためのクエリ通知の有効化  
 **AdventureWorks** データベースで Service Broker を有効にするには、SQL Server Management Studio を通じて、次の Transact\-SQL ステートメントを実行します。  
  
 `ALTER DATABASE AdventureWorks SET ENABLE_BROKER;`  
  
 クエリ通知のサンプルを正しく実行するには、次の Transact\-SQL ステートメントをデータベース サーバー上で実行する必要があります。  
  
```  
CREATE QUEUE ContactChangeMessages;  
  
CREATE SERVICE ContactChangeNotifications  
  ON QUEUE ContactChangeMessages  
([http://schemas.microsoft.com/SQL/Notifications/PostQueryNotification]);  
```  
  
## クエリ通知のアクセス許可  
 通知を要求するコマンドを実行するユーザーは、特定のサーバーに対する SUBSCRIBE QUERY NOTIFICATIONS データベース アクセス許可を必要とします。  
  
 部分信頼の状態で実行されるクライアント側のコードには、<xref:System.Data.SqlClient.SqlClientPermission> が必要です。  
  
 次のコードでは、<xref:System.Data.SqlClient.SqlClientPermission> オブジェクトを作成して、<xref:System.Security.Permissions.PermissionState> を <xref:System.Security.Permissions.PermissionState> に設定します。  <xref:System.Security.CodeAccessPermission.Demand%2A> を指定すると、呼び出し履歴の上流に、権限の付与されていない呼び出し元が 1 つでも存在した場合、実行時に強制的に <xref:System.Security.SecurityException> が発生します。  
  
 [!code-csharp[DataWorks SqlNotification.Perms#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlNotification.Perms/CS/source.cs#1)]
 [!code-vb[DataWorks SqlNotification.Perms#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlNotification.Perms/VB/source.vb#1)]  
  
## 通知オブジェクトの選択  
 クエリ通知 API は、通知を処理するために <xref:System.Data.SqlClient.SqlDependency> と <xref:System.Data.Sql.SqlNotificationRequest> という 2 つのオブジェクトを提供します。  通常、ASP.NET 以外のほとんどのアプリケーションでは、<xref:System.Data.SqlClient.SqlDependency> オブジェクトを使用する必要があります。  ASP.NET アプリケーションは、高レベルの <xref:System.Web.Caching.SqlCacheDependency> を使用する必要があります。これは <xref:System.Data.SqlClient.SqlDependency> をラップし、通知オブジェクトとキャッシュ オブジェクトを管理するためのフレームワークになります。  
  
### SqlDependency の使用  
 <xref:System.Data.SqlClient.SqlDependency> を使用するには、使用する SQL Server データベースに対して Service Broker を有効にし、通知を受け取るためのアクセス許可をユーザーに与える必要があります。  通知キューなどの Service Broker オブジェクトは、あらかじめ定義されています。  
  
 さらに、<xref:System.Data.SqlClient.SqlDependency> によってワーカー スレッドが自動的に開始され、キューにポストされた通知を処理すると共に、Service Broker メッセージが解析され、情報がイベント引数データとして公開されます。  <xref:System.Data.SqlClient.SqlDependency> は `Start` メソッドを呼び出し、データベースに対する依存関係を設定して初期化する必要があります。  これは、必要となる各データベース接続に対するアプリケーション初期化中に、1 回だけ呼び出す必要がある静的メソッドです。  `Stop` メソッドは、作成された依存関係を持つ接続それぞれのアプリケーションの終了時に呼び出す必要があります。  
  
### SqlNotificationRequest の使用  
 これに対し <xref:System.Data.Sql.SqlNotificationRequest> では、待機するインフラストラクチャ全体を自分で実装する必要があります。  さらに、キュー、サービス、およびキューによってサポートされるメッセージの種類など、サポート対象となるすべての Service Broker オブジェクトを定義する必要があります。  手動によるこの方法は、使用しているアプリケーションで特殊な通知メッセージや通知動作が必要な場合、またはそのアプリケーションが Service Broker アプリケーションの一部である場合に使用すると便利です。  
  
## 参照  
 [SQL Server のクエリ通知](../../../../../docs/framework/data/adonet/sql/query-notifications-in-sql-server.md)   
 [ADO.NET Managed Providers and DataSet Developer Center \(ADO.NET マネージ プロバイダーと DataSet デベロッパー センター\)](http://go.microsoft.com/fwlink/?LinkId=217917)