---
title: "WorkflowInstanceId の取得 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: bd7eea3b-1c28-4b84-9a67-003bc553aa81
caps.latest.revision: 6
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 6
---
# WorkflowInstanceId の取得
このサンプルでは、カスタム アクティビティ `GetWorkflowInstanceId` を使用して、ワークフロー インスタンス ID を返す方法を示します。  
  
## 使用例  
 カスタム アクティビティの開発、ワークフロー インスタンスにアクセスする方法。  
  
## 説明  
 実行中のワークフローのインスタンス ID を取得するには、コードを記述する必要があります。完全な宣言型のワークフローを記述する場合は、ワークフロー インスタンス ID を返すことができるアクティビティが必要です。そのアクティビティをワークフローで参照することで、完全な宣言型のワークフローを作成できるようになります。多くのシナリオで、インスタンス ID へのアクセスが必要になります。例としては、ログ記録または監査のためや、後で連携できるようにインスタンス ID をクライアントに返送する \(たとえば、SendReply アクティビティ内でこのアクティビティを使用する\) ことでアプリケーション レベルの関連付けを行うためなどが挙げられます。  
  
 `GetWorkflowInstanceId` は、<xref:System.Guid> 型の値を返す必要があり、ワークフローのインスタンス ID を取得するために <xref:System.Activities.CodeActivityContext> にアクセスできる必要があるので、<xref:System.Activities.CodeActivity%601> として実装されます。この実装は非常に基本的なものです。  
  
```  
public sealed class GetWorkflowInstanceId : CodeActivity<Guid>  
{  
protected override Guid Execute(CodeActivityContext context)  
        {  
            return context.WorkflowInstanceId;  
        }  
}  
  
```  
  
> [!IMPORTANT]
>  サンプルは、既にコンピューターにインストールされている場合があります。続行する前に、次の \(既定の\) ディレクトリを確認してください。  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  このディレクトリが存在しない場合は、「[.NET Framework 4 向けの Windows Communication Foundation \(WCF\) および Windows Workflow Foundation \(WF\) のサンプル](http://go.microsoft.com/fwlink/?LinkId=150780)」にアクセスして、[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] および [!INCLUDE[wf1](../../../../includes/wf1-md.md)] のサンプルをすべてダウンロードしてください。このサンプルは、次のディレクトリに格納されます。  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WF\Scenario\ActivityLibrary\GetWorkflowInstanceId`