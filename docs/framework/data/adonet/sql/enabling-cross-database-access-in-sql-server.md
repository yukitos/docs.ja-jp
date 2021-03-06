---
title: "SQL Server での複数データベースにまたがるアクセスの有効化 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 10663fb6-434c-4c81-8178-ec894b9cf895
caps.latest.revision: 10
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 10
---
# SQL Server での複数データベースにまたがるアクセスの有効化
複数データベースの組み合わせ所有権は、あるデータベースのプロシージャが、別のデータベースのオブジェクトに依存している場合に作用します。  複数データベースの組み合わせ所有権は、単一データベースの組み合わせ所有権とほぼ同じように機能しますが、所有権の連鎖性を保つために、すべてのオブジェクトの所有者が同じログイン アカウントにマップされていることが必要です。  ソース データベース内のソース オブジェクトおよびターゲット データベース内のターゲット オブジェクトが同じログイン アカウントによって所有されている場合、ターゲット オブジェクトに対する権限は SQL Server によってチェックされません。  
  
## 既定で無効の機能  
 複数データベースにまたがる組み合わせ所有権は、既定では無効になっています。  次に示したように、複数データベースの組み合わせ所有権はセキュリティ上のリスクを伴うため無効にすることをお勧めします。  
  
-   `db_ddladmin` データベース ロールまたは `db_owners` データベース ロールのデータベース所有者およびメンバーは、他のユーザーによって所有されたオブジェクトを作成できます。  これらのオブジェクトが、他のデータベースのオブジェクトに依存している可能性もあります。  これは、複数データベースの組み合わせ所有権を有効にした場合、すべてのデータベースのデータについて、これらのユーザーを完全に信頼する必要があることを意味します。  
  
-   CREATE DATABASE 権限を持つユーザーは、新しいデータベースを作成したり、既存のデータベースをアタッチしたりできます。  複数データベースの組み合わせ所有権を有効にした場合、これらのユーザーが、新たに作成したデータベースから \(または、アタッチしたデータベースから\)、権限を持たない他のデータベース内のオブジェクトにアクセスできます。  
  
## 複数データベースの組み合わせ所有権の有効化  
 高い権限が与えられたユーザーを完全に信頼できる環境であれば、複数データベースの組み合わせ所有権を有効にすることができます。  複数データベースの組み合わせ所有権は、セットアップ時にすべてのデータベースを対象に構成できるほか、[!INCLUDE[tsql](../../../../../includes/tsql-md.md)] コマンドの `sp_configure` および `ALTER DATABASE` を使用して特定のデータベースを対象に構成することもできます。  
  
 複数データベースの組み合わせ所有権を個別に構成するには、`sp_configure` を使用し、サーバーに対してこの機能を無効にします。  次に、ALTER DATABASE コマンドで SET DB\_CHAINING ON を指定し、必要なデータベースに対してのみ、複数データベースの組み合わせ所有権を構成します。  
  
 次のサンプルでは、すべてのデータベースに対して複数データベースの組み合わせ所有権を有効にします。  
  
```  
EXECUTE sp_configure 'show advanced', 1;  
RECONFIGURE;  
EXECUTE sp_configure 'cross db ownership chaining', 1;  
RECONFIGURE;  
```  
  
 次のサンプルでは、特定のデータベースに対して複数データベースの組み合わせ所有権を有効にします。  
  
```  
ALTER DATABASE Database1 SET DB_CHAINING ON;  
ALTER DATABASE Database2 SET DB_CHAINING ON;  
  
```  
  
### 動的 SQL  
 動的に生成された SQL ステートメントの実行では、同じユーザーが両方のデータベースに存在しない限り、複数データベースの組み合わせ所有権は機能しません。  [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)] では、別のデータベースのデータにアクセスするストアド プロシージャを作成し、両方のデータベースに存在する証明書でそのプロシージャに署名することによって、これを回避できます。  これにより、ユーザーは、データベースへのアクセス許可が付与されていなくても、そのプロシージャによって使用されるデータベース リソースにアクセスできるようになります。  
  
## 外部リソース  
 詳細については、次のリソースを参照してください。  
  
|リソース|説明|  
|----------|--------|  
|「[EXECUTE AS の使用によるデータベースの権限借用の拡張](http://msdn.microsoft.com/library/ms188304\(SQL.105\).aspx)」および「[cross db ownership chaining サーバー構成オプション](http://msdn.microsoft.com/library/ms188694.aspx) [!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)]」オンライン ブック。|[!INCLUDE[ssNoVersion](../../../../../includes/ssnoversion-md.md)] のインスタンスに対して複数データベースの組み合わせ所有権を構成する方法を説明します。|  
  
## 参照  
 [ADO.NET アプリケーションのセキュリティ保護](../../../../../docs/framework/data/adonet/securing-ado-net-applications.md)   
 [SQL Server セキュリティの概要](../../../../../docs/framework/data/adonet/sql/overview-of-sql-server-security.md)   
 [SQL Server でのストアド プロシージャを使用したアクセス許可の管理](../../../../../docs/framework/data/adonet/sql/managing-permissions-with-stored-procedures-in-sql-server.md)   
 [SQL Server での安全な動的 SQL の作成](../../../../../docs/framework/data/adonet/sql/writing-secure-dynamic-sql-in-sql-server.md)   
 [SQL Server でのストアド プロシージャの署名](../../../../../docs/framework/data/adonet/sql/signing-stored-procedures-in-sql-server.md)   
 [ADO.NET マネージ プロバイダーと DataSet デベロッパー センター](http://go.microsoft.com/fwlink/?LinkId=217917)