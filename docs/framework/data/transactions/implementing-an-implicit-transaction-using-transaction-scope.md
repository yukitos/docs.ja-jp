---
title: "トランザクション スコープを使用した暗黙的なトランザクションの実装  | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 49d1706a-1e0c-4c85-9704-75c908372eb9
caps.latest.revision: 4
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 2
---
# トランザクション スコープを使用した暗黙的なトランザクションの実装 
<xref:System.Transactions.TransactionScope> クラスを使用すると、コード ブロックがトランザクションに参加しているものとして簡単にマークすることができ、トランザクション自体と対話する必要がありません。トランザクション スコープは、アンビエント トランザクションを自動的に選択して管理することができます。トランザクション アプリケーションを開発する際は、使いやすさと効率の点から、<xref:System.Transactions.TransactionScope> クラスを使用することをお勧めします。  
  
 また、リソースをトランザクションに明示的に参加させる必要がありません。<xref:System.Transactions> リソース マネージャー \(SQL Server 2005 など\) は、スコープによって作成されたアンビエント トランザクションを検出して、自動的に参加することができます。  
  
## トランザクション スコープの作成  
 次のサンプルは、<xref:System.Transactions.TransactionScope> クラスの簡単な使用法を示しています。  
  
 [!code-csharp[TransactionScope#1](../../../../samples/snippets/csharp/VS_Snippets_Remoting/TransactionScope/cs/ScopeWithSQL.cs#1)]
 [!code-vb[TransactionScope#1](../../../../samples/snippets/visualbasic/VS_Snippets_Remoting/TransactionScope/vb/ScopeWithSQL.vb#1)]  
  
 新しい <xref:System.Transactions.TransactionScope> オブジェクトを作成すると、トランザクション スコープが開始されます。コード サンプルに示すように、**using** ステートメントを使用してスコープを作成することをお勧めします。**using** ステートメントは C\# と Visual Basic の両方で使用でき、**try...finally** ブロック同様、スコープが正しく破棄されるように機能します。  
  
 <xref:System.Transactions.TransactionScope> をインスタンス化するとき、トランザクション マネージャーは参加するトランザクションを決定します。確認されると、そのスコープは常にそのトランザクションに参加します。この決定は 2 つの要因に基づいて行われます。1 つはアンビエント トランザクションが存在するかどうか、もう 1 つはコンストラクターの **TransactionScopeOption** パラメーターの値です。アンビエント トランザクションとは、実行するコードが含まれているトランザクションのことです。<xref:System.Transactions.Transaction> クラスの静的 <xref:System.Transactions.Transaction.Current%2A> プロパティを呼び出すことによってアンビエント トランザクションへの参照を取得できます。このパラメーターの使用法の詳細については、このトピックの「TransactionScopeOption を使用したトランザクション フローの管理」を参照してください。  
  
## トランザクション スコープの完了  
 アプリケーションがトランザクション内で実行する必要のあるすべての作業を完了したら、トランザクションをコミットできることをトランザクション マネージャーに知らせるために、<xref:System.Transactions.TransactionScope.Complete%2A> メソッドを一度だけ呼び出す必要があります。<xref:System.Transactions.TransactionScope.Complete%2A> の呼び出しを **using** ブロックの最後のステートメントとして配置することを強くお勧めします。  
  
 この呼び出しを行わないと、トランザクション マネージャーは、システム障害が発生したかまたはトランザクションのスコープ内で例外がスローされたと解釈するため、トランザクションが中止されます。ただし、このメソッドを呼び出したからといって必ずしもトランザクションのコミットが保証されるわけではありません。これはトランザクション マネージャーにステータスを通知する手段にすぎません。<xref:System.Transactions.TransactionScope.Complete%2A> メソッドを呼び出した後は、<xref:System.Transactions.Transaction.Current%2A> プロパティを使用してアンビエント トランザクションにアクセスできなくなります。アクセスしようとすると例外がスローされます。  
  
 <xref:System.Transactions.TransactionScope> オブジェクトが最初にトランザクションを作成した場合、トランザクション マネージャーがトランザクションを実際にコミットする作業は **using** ブロックの最後のコード行の後で行われます。このオブジェクトによってトランザクションが作成されていない場合、<xref:System.Transactions.CommittableTransaction> オブジェクトの所有者によって <xref:System.Transactions.CommittableTransaction.Commit%2A> が呼び出されるたびにコミットが発生します。その時点で、トランザクション マネージャーがリソース マネージャーを呼び出し、<xref:System.Transactions.TransactionScope> オブジェクトの <xref:System.Transactions.TransactionScope.Complete%2A> メソッドが呼び出されたかどうかに基づいて、コミットするかロールバックするかを通知します。  
  
 **using** ステートメントにより、例外が発生した場合でも必ず <xref:System.Transactions.TransactionScope> オブジェクトの <xref:System.Transactions.TransactionScope.Dispose%2A> メソッドが呼び出されます。<xref:System.Transactions.TransactionScope.Dispose%2A> メソッドは、トランザクション スコープの末尾を表します。このメソッドの呼び出し後に発生した例外は、トランザクションに影響しない場合があります。また、このメソッドはアンビエント トランザクションを前の状態に復元します。  
  
 スコープがトランザクションを作成し、そのトランザクションが中止された場合は、<xref:System.Transactions.TransactionAbortedException> がスローされます。トランザクション マネージャーがコミットを判断できない場合は、<xref:System.Transactions.TransactionIndoubtException> がスローされます。トランザクションがコミットされた場合は、例外はスローされません。  
  
## トランザクションのロールバック  
 トランザクションをロールバックする場合は、トランザクション スコープ内で <xref:System.Transactions.TransactionScope.Complete%2A> メソッドを呼び出さないようにしてください。たとえば、スコープ内で例外をスローすると、スコープが参加しているトランザクションがロールバックされます。  
  
##  <a name="ManageTxFlow"></a> TransactionScopeOption を使用したトランザクション フローの管理  
 次の例にある `RootMethod` メソッドのように、独自のスコープを使用するメソッド内から、<xref:System.Transactions.TransactionScope> を使用するメソッドを呼び出すことによって、トランザクション スコープを入れ子にすることができます。  
  
```csharp  
void RootMethod()  
{  
     using(TransactionScope scope = new TransactionScope())  
     {  
          /* Perform transactional work here */  
          SomeMethod();  
          scope.Complete();  
     }  
}  
  
void SomeMethod()  
{  
     using(TransactionScope scope = new TransactionScope())  
     {  
          /* Perform transactional work here */  
          scope.Complete();  
     }  
}  
```  
  
 最上位のトランザクション スコープをルート スコープと呼びます。  
  
 <xref:System.Transactions.TransactionScope> クラスには、スコープのトランザクション動作を定義する <xref:System.Transactions.TransactionScopeOption> 型の列挙体を受け入れる、いくつかのオーバーロードされたコンストラクターがあります。  
  
 <xref:System.Transactions.TransactionScope> オブジェクトには次の 3 つのオプションがあります。  
  
-   アンビエント トランザクションに参加します \(存在しない場合は新規に作成します\)。  
  
-   新しいルート スコープになります。つまり、新しいトランザクションを開始して、そのトランザクションをそれ自身のスコープ内の新しいアンビエント トランザクションにします。  
  
-   どのトランザクションにも参加しません。その結果、アンビエント トランザクションは存在しません。  
  
 スコープが <xref:System.Transactions.TransactionScopeOption> でインスタンス化された場合、アンビエント トランザクションが存在しているときは、スコープはそのトランザクションに参加します。一方、アンビエント トランザクションが存在しないときは、スコープは新しいトランザクションを作成して、ルート スコープになります。これが既定値です。<xref:System.Transactions.TransactionScopeOption> を使用した場合、スコープがルートのときでも、アンビエント トランザクションに参加するだけのときでも、スコープ内のコードは異なる動作をする必要がありません。いずれの場合にもスコープ内のコードは同じ動作をします。  
  
 スコープが <xref:System.Transactions.TransactionScopeOption> でインスタンス化された場合は、常にルート スコープです。スコープが新しいトランザクションを開始し、そのトランザクションがスコープ内の新しいアンビエント トランザクションになります。  
  
 スコープが <xref:System.Transactions.TransactionScopeOption> でインスタンス化された場合は、アンビエント トランザクションの有無にかかわらず、スコープがトランザクションに参加することはありません。この値でインスタンス化されたスコープの場合は、スコープのアンビエント トランザクションは常に **null** になります。  
  
 上記のオプションを要約すると、次の表のようになります。  
  
|TransactionScopeOption|アンビエント トランザクション|スコープの参加|  
|----------------------------|---------------------|-------------|  
|Required|いいえ|新規トランザクション \(ルートになる\)|  
|RequiresNew|いいえ|新規トランザクション \(ルートになる\)|  
|Suppress|いいえ|トランザクションなし|  
|Required|はい|アンビエント トランザクション|  
|RequiresNew|はい|新規トランザクション \(ルートになる\)|  
|Suppress|はい|トランザクションなし|  
  
 <xref:System.Transactions.TransactionScope> オブジェクトが既存のアンビエント トランザクションに参加した場合、スコープ オブジェクトを破棄してもトランザクションが終了しないことがあります \(スコープがトランザクションを中止した場合を除く\)。アンビエント トランザクションがルート スコープによって作成されたものである場合は、ルート スコープが破棄されたときのみ、トランザクションの <xref:System.Transactions.CommittableTransaction.Commit%2A> が呼び出されます。トランザクションが手動で作成されたものである場合は、中止されるか、その作成者によってコミットされたときに、そのトランザクションは終了します。  
  
 次の例は、入れ子になった 3 つのスコープ オブジェクトを作成する <xref:System.Transactions.TransactionScope> オブジェクトを示しています。各スコープ オブジェクトは異なる <xref:System.Transactions.TransactionScopeOption> 値でインスタンス化しています。  
  
```csharp  
using(TransactionScope scope1 = new TransactionScope())   
//Default is Required   
{   
     using(TransactionScope scope2 = new   
      TransactionScope(TransactionScopeOption.Required))   
     {  
     ...  
     }   
  
     using(TransactionScope scope3 = new TransactionScope(TransactionScopeOption.RequiresNew))   
     {  
     ...  
     }   
  
     using(TransactionScope scope4 = new   
        TransactionScope(TransactionScopeOption.Suppress))   
    {  
     ...  
    }   
}  
```  
  
 このコード例では、アンビエント トランザクションがない状態で、新しいスコープ `scope1` を <xref:System.Transactions.TransactionScopeOption> で作成しています。スコープ `scope1` は、新しいトランザクション \(トランザクション A\) を作成し、トランザクション A をアンビエント トランザクションにするので、ルート スコープです。`Scope1` は次に、おのおのが異なる <xref:System.Transactions.TransactionScopeOption> 値を持つ 3 つのオブジェクトをさらに作成します。たとえば、`scope2` は <xref:System.Transactions.TransactionScopeOption> で作成されますが、アンビエント トランザクションがあるため、`scope1` によって作成された最初のトランザクションに参加します。`scope3` は新しいトランザクションのルート スコープです。`scope4` にはアンビエント トランザクションがありません。  
  
 <xref:System.Transactions.TransactionScopeOption> の既定値でかつ最もよく使用される値は <xref:System.Transactions.TransactionScopeOption> ですが、その他の各値にはそれぞれ固有の用途があります。  
  
 <xref:System.Transactions.TransactionScopeOption> は、コード セクションで実行された操作を保持したり、操作が失敗した場合でもアンビエント トランザクションを中止しない場合に役立ちます。たとえば、ログの記録や監査操作を実行する場合や、アンビエント トランザクションがコミットしても中止してもサブスクライバーにイベントを公開する場合などです。次の例で示すように、この値を使用すれば、トランザクション スコープ内に非トランザクション コード セクションを置くことができます。  
  
```csharp  
using(TransactionScope scope1 = new TransactionScope())  
{  
     try  
     {  
          //Start of non-transactional section   
          using(TransactionScope scope2 = new  
             TransactionScope(TransactionScopeOption.Suppress))  
          {  
               //Do non-transactional work here  
          }  
          //Restores ambient transaction here  
   }  
     catch  
     {}  
   //Rest of scope1  
}  
  
```  
  
### 入れ子になったスコープ内での選択  
 入れ子になったスコープはルート スコープのアンビエント トランザクションに参加できますが、入れ子になったスコープ内で <xref:System.Transactions.TransactionScope.Complete%2A> を呼び出してもルート スコープには影響がありません。ルート スコープから、入れ子になった最後のスコープまで、すべてのスコープがトランザクションのコミットを選択した場合にのみ、トランザクションはコミットされます。入れ子になったスコープで <xref:System.Transactions.TransactionScope.Complete%2A> を呼び出さないと、アンビエント トランザクションが即時に中止されるため、ルート スコープに影響します。  
  
## TransactionScope タイムアウトの設定  
 <xref:System.Transactions.TransactionScope> のオーバーロードされたコンストラクターのいくつかは、トランザクションのタイムアウトを制御するために使用される <xref:System.TimeSpan> 型の値を受け入れます。タイムアウトをゼロに設定すると、タイムアウトは無期限になります。無期限のタイムアウトは主にデバッグに役立ちます。つまり、コードをステップ実行することによってビジネス ロジックの問題を切り分け、問題の究明を試みている間はデバッグするトランザクションがタイムアウトにならないようにすることができます。タイムアウト値を無期限にすると、トランザクションのデッドロックに対する保護機能が無効になるため、上記以外の目的でこれを使用する場合は十分注意する必要があります。  
  
 次の 2 つの場合は、通常、<xref:System.Transactions.TransactionScope> タイムアウトを既定以外の値に設定します。第 1 は、開発時に、中止されたトランザクションをアプリケーションがどう処理するかをテストする場合です。タイムアウトを小さい値 \(1 ミリ秒など\) に設定すると、トランザクションが失敗するため、エラー処理コードを確認できます。第 2 は、スコープがリソース競合に関与した結果、デッドロックの発生が考えられるときに、既定のタイムアウト値より小さい値に設定する場合です。この場合は、できるだけ早くトランザクションを中止して、既定のタイムアウトが満了するのを待ちません。  
  
 アンビエント トランザクションに参加するスコープが、アンビエント トランザクションのタイムアウト設定値より小さいタイムアウト値を指定すると、<xref:System.Transactions.TransactionScope> オブジェクトに対して、後から指定した短い方のタイムアウトが適用され、スコープは指定した時間内に終了する必要があります。終了しない場合、トランザクションは自動的に中止されます。入れ子になったスコープのタイムアウトがアンビエント トランザクションのタイムアウトより長い場合は、効果はありません。  
  
## TransactionScope 分離レベルの設定  
 <xref:System.Transactions.TransactionScope> のオーバーロードされたコンストラクターのいくつかは、タイムアウト値の他にも、分離レベルを指定する <xref:System.Transactions.TransactionOptions> 型の構造体を受け入れます。既定では、トランザクションは <xref:System.Transactions.IsolationLevel> に設定された分離レベルで実行されます。<xref:System.Transactions.IsolationLevel> 以外の分離レベルは、読み取り集中型のシステムの場合によく使用されます。そのためには、トランザクション処理理論とトランザクション自体の動作、関連する同時実行の問題、およびシステム整合性の影響をよく理解する必要があります。  
  
 さらに、すべてのリソース マネージャーがすべての分離レベルをサポートするわけではなく、構成されたレベルより高いレベルのトランザクションへの参加をリソース マネージャーが選択する場合もあります。  
  
 <xref:System.Transactions.IsolationLevel> を除くすべての分離レベルは、他のトランザクションが同じ情報にアクセスすることで生じる不整合の影響を受けやすくなっています。分離レベルごとの違いは、読み取りロックおよび書き込みロックの使用方法にあります。ロックには、トランザクションがリソース マネージャーのデータにアクセスするときのみ保持できるか、トランザクションがコミットまたは中止されるまで保持できるかの 2 種類があります。前者はスループットの面で優れており、後者は整合性の面で優れています。2 種類のロックと 2 種類の操作 \(読み取り\/書き込み\) から、4 つの基本分離レベルが構成されます。詳細については、「<xref:System.Transactions.IsolationLevel>」を参照してください。  
  
 入れ子になった <xref:System.Transactions.TransactionScope> オブジェクトを使用する場合、入れ子になったすべてのスコープがアンビエント トランザクションに参加するときには、必ず同じ分離レベルを使用するようにそれらのスコープを構成する必要があります。入れ子になった <xref:System.Transactions.TransactionScope> オブジェクトがアンビエント トランザクションに参加するときに、別の分離レベルが指定された場合は、<xref:System.ArgumentException> がスローされます。  
  
## COM\+ との相互運用  
 新しい <xref:System.Transactions.TransactionScope> インスタンスを作成する際には、いずれかのコンストラクターで <xref:System.Transactions.EnterpriseServicesInteropOption> 列挙体を使用して、COM\+ との対話方法を指定することができます。詳細については、「[Enterprise Services と COM\+ トランザクションの相互運用性 ](../../../../docs/framework/data/transactions/interoperability-with-enterprise-services-and-com-transactions.md)」を参照してください。  
  
## 参照  
 <xref:System.Transactions.Transaction.Clone%2A>   
 <xref:System.Transactions.TransactionScope>