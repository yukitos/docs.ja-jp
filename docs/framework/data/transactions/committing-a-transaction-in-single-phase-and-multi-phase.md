---
title: "単一フェーズおよび複数フェーズでのトランザクションのコミット  | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 694ea153-e4db-41ae-96ac-9ac66dcb69a9
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# 単一フェーズおよび複数フェーズでのトランザクションのコミット 
トランザクションで使用される各リソースは、リソース マネージャー \(RM\) によって管理され、その動作はトランザクション マネージャー \(TM\) によって調整されます。「[参加要素としてのリソースのトランザクションへの参加 ](../../../../docs/framework/data/transactions/enlisting-resources-as-participants-in-a-transaction.md)」では、単一 \(または複数\) のリソースがトランザクションに参加する方法について説明しています。ここでは、参加リソース間でトランザクションのコミットメントを調整する方法について説明します。  
  
 トランザクションの最後に、アプリケーションはトランザクションのコミットまたはロールバックを要求します。トランザクション マネージャーは、一部のリソース マネージャーがトランザクションのコミットを選択し、他のリソース マネージャーがトランザクションのロールバックを選択するというようなリスクを回避する必要があります。  
  
 トランザクションに複数のリソースが含まれる場合は、2 フェーズ コミット \(2PC\) を実行する必要があります。2 フェーズ コミット プロトコル \(準備フェーズとコミット フェーズ\) により、すべてのリソースに対する変更がトランザクションの終了時に完全にコミットされるか、または完全にロールバックされることが保証されます。その後、すべての参加要素に最終結果が通知されます。2 フェーズ コミット プロトコルの詳細については、Jim Gray 著『Transaction Processing : Concepts and Techniques \(Morgan Kaufmann Series in Data Management Systems\) ISBN:1558601902』を参照してください。  
  
 単一フェーズ コミット プロトコルに参加することで、トランザクションのパフォーマンスを最適化することもできます。詳細については、「[単一フェーズ コミットと昇格可能単一フェーズ通知を使用した最適化 ](../../../../docs/framework/data/transactions/optimization-spc-and-promotable-spn.md)」を参照してください。  
  
 トランザクション結果の通知を受信するだけで、コミットまたはロールバックの選択には参加しない場合は、<xref:System.Transactions.Transaction.TransactionCompleted> イベントに登録する必要があります。  
  
## 2 フェーズ コミット \(2PC\)  
 最初のトランザクション フェーズで、トランザクション マネージャーは、トランザクションをコミットするかロールバックするかを決定するため、各リソースに照会します。2 番目のトランザクション フェーズでは、トランザクション マネージャーは照会結果を各リソースに通知し、必要なクリーンアップを実行できるようにします。  
  
 この種のトランザクションに参加するために、リソース マネージャーは <xref:System.Transactions.IEnlistmentNotification> インターフェイスを実装する必要があります。このインターフェイスは、2PC 中に通知として TM が呼び出すメソッドを提供します。このような実装の例を次に示します。  
  
 [!code-csharp[Tx_Enlist#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/tx_enlist/cs/enlist.cs#2)]
 [!code-vb[Tx_Enlist#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/tx_enlist/vb/enlist.vb#2)]  
  
### 準備フェーズ \(フェーズ 1\)  
 アプリケーションから <xref:System.Transactions.CommittableTransaction.Commit%2A> 要求を受信すると、トランザクション マネージャーは、各リソースのトランザクションに対する選択を取得するため、各参加リソースに対して <xref:System.Transactions.IEnlistmentNotification.Prepare%2A> メソッドを呼び出し、参加しているすべての参加要素の準備フェーズを開始します。  
  
 <xref:System.Transactions.IEnlistmentNotification> インターフェイスを実装するリソース マネージャーは、次の簡単な例に示すように、最初に <xref:System.Transactions.IEnlistmentNotification.Prepare%28System.Transactions.PreparingEnlistment%29> メソッドを実装する必要があります。  
  
```  
public void Prepare(PreparingEnlistment preparingEnlistment)  
{  
     Console.WriteLine("Prepare notification received");  
     //Perform work  
  
     Console.Write("reply with prepared? [Y|N] ");  
     c = Console.ReadKey();  
     Console.WriteLine();  
  
     //If work finished correctly, reply with prepared  
     if ((c.KeyChar == 'Y') || (c.KeyChar == 'y'))  
     {  
          preparingEnlistment.Prepared();  
          break;  
     }  
  
     // otherwise, do a ForceRollback  
     else if ((c.KeyChar == 'N') || (c.KeyChar == 'n'))  
     {  
          preparingEnlistment.ForceRollback();  
          break;  
     }  
}  
  
```  
  
 永続的リソース マネージャーがこの呼び出しを受信すると、トランザクションの回復情報 \(<xref:System.Transactions.PreparingEnlistment.RecoveryInformation%2A> プロパティの取得により利用可\) と、コミット時にトランザクションを完了させるのに必要な情報をすべてログに記録する必要があります。RM はワーカー スレッドでこれを実行できるため、<xref:System.Transactions.IEnlistmentNotification.Prepare%2A> メソッド内で実行する必要はありません。  
  
 この準備操作が完了したら、RM は <xref:System.Transactions.PreparingEnlistment.Prepared%2A> メソッドまたは <xref:System.Transactions.PreparingEnlistment.ForceRollback%2A> メソッドを呼び出して、コミットまたはロールバックを選択する必要があります。<xref:System.Transactions.PreparingEnlistment> クラスは、<xref:System.Transactions.Enlistment> クラスから <xref:System.Transactions.Enlistment.Done%2A> メソッドを継承することに注意してください。準備フェーズ中に <xref:System.Transactions.PreparingEnlistment> コールバックでこのメソッドを呼び出すと、読み取り専用の参加 \(トランザクション保護されたデータを読み取れるが更新できないリソース マネージャー\) であることが TM に通知され、RM はフェーズ 2 でのトランザクションの結果として、トランザクション マネージャーからの通知を受信しなくなります。  
  
 すべてのリソース マネージャーが <xref:System.Transactions.PreparingEnlistment.Prepared%2A> を選択すると、トランザクションのコミットメントが成功したことがアプリケーションに通知されます。  
  
### コミット フェーズ \(フェーズ 2\)  
 トランザクションの 2 番目のフェーズで、トランザクション マネージャーがすべてのリソース マネージャーから準備の成功を受信すると \(すべてのリソース マネージャーがフェーズ 1 の終わりで <xref:System.Transactions.PreparingEnlistment.Prepared%2A> を呼び出した場合\)、各リソース マネージャーに対して <xref:System.Transactions.IEnlistmentNotification.Commit%2A> メソッドを呼び出します。これにより、リソース マネージャーは、変更を永続的なものとして、コミットを完了できます。  
  
 いずれかのリソース マネージャーがフェーズ 1 での準備の失敗を報告すると、トランザクション マネージャーは、各リソース マネージャーに対して <xref:System.Transactions.IEnlistmentNotification.Rollback%2A> メソッドを呼び出し、アプリケーションにコミットの失敗を通知します。  
  
 したがって、リソース マネージャーは次のメソッドを実装する必要があります。  
  
```  
public void Commit (Enlistment enlistment)  
{  
     // Do any work necessary when commit notification is received  
  
     // Declare done on the enlistment  
     enlistment.Done();  
}  
  
public void Rollback (Enlistment enlistment)  
{  
     // Do any work necessary when rollback notification is received  
  
     // Declare done on the enlistment    
     enlistment.Done();    
}  
```  
  
 RM は、通知の種類に基づいてトランザクションを終了するために必要な作業を実行し、<xref:System.Transactions.Enlistment> パラメーターで <xref:System.Transactions.Enlistment.Done%2A> メソッドを呼び出すことにより、TM に作業の完了を通知する必要があります。これはワーカー スレッドで実行できます。フェーズ 2 の通知は、フェーズ 1 で <xref:System.Transactions.PreparingEnlistment.Prepared%2A> メソッドを呼び出した同じスレッドで、インラインで発生する可能性があることに注意してください。このため、フェーズ 2 の通知を受け取る前に既に完了したと考えられる作業 \(ロックの解除など\) は、<xref:System.Transactions.PreparingEnlistment.Prepared%2A> 呼び出しの後には実行できません。  
  
### InDoubt の実装  
 最後に、揮発性リソース マネージャーに <xref:System.Transactions.IEnlistmentNotification.InDoubt%2A> メソッドを実装する必要があります。このメソッドは、トランザクション マネージャーが 1 つ以上の参加要素と接続できなくなり、そのステータスが不明になった場合に呼び出されます。この場合は、トランザクションのいずれかの参加要素が矛盾した状態のままになっていないかどうかを後で調べることができるよう、この事実を記録しておく必要があります。  
  
```  
public void InDoubt (Enlistment enlistment)  
{  
     // log this  
     enlistment.Done();  
}  
```  
  
## 単一フェーズ コミットの最適化  
 単一フェーズ コミット プロトコルは、すべての更新が明示的な調整なしに行われるため、実行時に、より効率的です。このプロトコルの詳細については、「[単一フェーズ コミットと昇格可能単一フェーズ通知を使用した最適化 ](../../../../docs/framework/data/transactions/optimization-spc-and-promotable-spn.md)」を参照してください。  
  
## 参照  
 [単一フェーズ コミットと昇格可能単一フェーズ通知を使用した最適化 ](../../../../docs/framework/data/transactions/optimization-spc-and-promotable-spn.md)   
 [参加要素としてのリソースのトランザクションへの参加 ](../../../../docs/framework/data/transactions/enlisting-resources-as-participants-in-a-transaction.md)