---
title: "How to: Write a Simple Parallel.For Loop | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "Parallel.For, How to Write"
  - "for loop, parallel construction in .NET"
  - "parallel for loops, how to use"
ms.assetid: 9029ba7f-a9d1-4526-8c84-c88716dba5d4
caps.latest.revision: 18
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 18
---
# How to: Write a Simple Parallel.For Loop
このトピックでは、<xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=fullName> メソッドを示す 2 つの例を示しています。  最初の例では <xref:System.Threading.Tasks.Parallel.For%28System.Int64%2CSystem.Int64%2CSystem.Action%7BSystem.Int64%7D%29?displayProperty=fullName> メソッドのオーバー ロードを使用し、2 番目の例では <xref:System.Threading.Tasks.Parallel.For%28System.Int32%2CSystem.Int32%2CSystem.Action%7BSystem.Int32%7D%29?displayProperty=fullName> のオーバー ロードを使用しています。これらは <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=fullName> メソッドの 2 つの最も単純なオーバーロードです。  <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=fullName> メソッドのこれらの 2 つのオーバー ロードは、ループをキャンセルする必要がない場合、ループのイテレーションから抜ける場合、またはいずれかのスレッドローカル状態を維持する場合に使用します。  
  
> [!NOTE]
>  ここでは、ラムダ式を使用して TPL でデリゲートを定義します。  C\# または Visual Basic のラムダ式についての情報が必要な場合は、「[Lambda Expressions in PLINQ and TPL](../../../docs/standard/parallel-programming/lambda-expressions-in-plinq-and-tpl.md)」を参照してください。  
  
 最初の例では、1 つのディレクトリ内のファイルのサイズを計算します。  2 つ目の例では、2 つの行列の積を計算します。  
  
## 使用例  
 この例は、ディレクトリ内のファイルの合計サイズを計算する単純なコマンド ライン ユーティリティです。  引数として 1 つのディレクトリ パスを必要とし、対象のディレクトリ内のファイルの数と合計サイズを報告します。  ディレクトリが存在することを確認したら、<xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=fullName> メソッドを使用してディレクトリ内のファイルを列挙し、そのファイルのサイズを決定します。  そして、各ファイル サイズが `totalSize` 変数に追加されます。  加算処理は、アトミックな操作として実行できるように、<xref:System.Threading.Interlocked.Add%2A?displayProperty=fullName> を呼び出して実行されることに注意してください。  そうしない場合、複数のタスクが同時に `totalSize` 変数を更新しようとすることがあります。  
  
 [!code-csharp[Conceptual.Parallel.For#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.parallel.for/cs/for1.cs#1)]
 [!code-vb[Conceptual.Parallel.For#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.parallel.for/vb/for1.vb#1)]  
  
## 使用例  
 この例では、<xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=fullName> メソッドを使用して 2 つの行列の積を計算します。  また、<xref:System.Diagnostics.Stopwatch?displayProperty=fullName> クラスを使用して並列ループと並列ではないループのパフォーマンスを比較する方法も示しています。  大量の出力が生成されるため、この例では出力をファイルにリダイレクトできることに注意してください。  
  
 [!code-csharp[TPL_Parallel#01](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/simpleparallelfor.cs#01)]
 [!code-vb[TPL_Parallel#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/simpleparallelfor.vb#01)]  
  
 ループを含む任意のコードを並列処理する場合、1 つの重要な目標は、並列処理のオーバーヘッドがパフォーマンス上の利点を無にするところまで並列化しすぎることなく、できるだけプロセッサを活用することです。  この特殊な例では、内側のループで実行される作業があまり多くないため、外側のループのみが並列化されています。  少量の作業と不適切なキャッシュの影響が組み合わさって、結果として入れ子になった並列ループのパフォーマンスが低下することがあります。  このため、大部分のシステムでは、外側のループだけを並列化することで、最大限の効果を得ることができると言えます。  
  
## デリゲート  
 この <xref:System.Threading.Tasks.Parallel.For%2A> のオーバーロードの 3 番目のパラメーターは、`Action<int>` 型 \(C\#\) または`Action(Of Integer)` 型 \(Visual Basic\) のデリゲートです。  `Action` デリゲートは、0 種類、1 種類、または 16 種類のパラメーターのいずれがあっても常に void を返します。  Visual Basic では、`Action` の動作は `Sub` で定義されます。  例では、ラムダ式を使用してデリゲートを作成していますが、他の方法でも同様にデリゲートを作成することができます。  詳細については、「[Lambda Expressions in PLINQ and TPL](../../../docs/standard/parallel-programming/lambda-expressions-in-plinq-and-tpl.md)」を参照してください。  
  
## イテレーション値  
 デリゲートは、値が現在のイテレーションである 1 つの入力パラメーターを受け取ります。  このイテレーション値はランタイムによって提供され、その開始値は、現在のスレッドで処理されているソースのセグメント \(パーティション\) の最初の要素のインデックスです。  
  
 同時実行レベルをより細かく制御する必要がある場合は、<xref:System.Threading.Tasks.Parallel.For%28System.Int32%2CSystem.Int32%2CSystem.Threading.Tasks.ParallelOptions%2CSystem.Action%7BSystem.Int32%2CSystem.Threading.Tasks.ParallelLoopState%7D%29?displayProperty=fullName> などの <xref:System.Threading.Tasks.ParallelOptions?displayProperty=fullName> 入力パラメーターを受け取るいずれかのオーバーロードを使用します。  
  
## 戻り値と例外処理  
 すべてのスレッドが完了すると、<xref:System.Threading.Tasks.Parallel.For%2A> は <xref:System.Threading.Tasks.ParallelLoopResult?displayProperty=fullName> オブジェクトを返します。  この戻り値は、手動でループのイテレーションを停止したり抜けたりする場合に役立ちます。完了まで実行した最後のイテレーションなどの情報を <xref:System.Threading.Tasks.ParallelLoopResult> が格納しているためです。  いずれかのスレッドで 1 つ以上の例外が発生した場合、<xref:System.AggregateException?displayProperty=fullName> がスローされます。  
  
 この例のコードでは、戻り値に <xref:System.Threading.Tasks.Parallel.For%2A> は使用されません。  
  
## 分析とパフォーマンス  
 パフォーマンス ウィザードを使用すると、コンピューター上の CPU 使用率を表示できます。  試しに、マトリックスの列と行の数を増やします。  マトリックスが大きいほど、並列バージョンとシーケンシャル バージョンの計算の間でパフォーマンスの違いが大きくなります。  マトリックスが小さい場合、並列ループを設定する際のオーバーヘッドがあるため、シーケンシャル バージョンのほうが高速に実行します。  
  
 コンソールやファイル システムなどの共有リソースへの同期呼び出しを行うと、並列ループのパフォーマンスが大幅に低下します。  パフォーマンスを測定する際は、ループ内の <xref:System.Console.WriteLine%2A?displayProperty=fullName> などの呼び出しを避けるようにしてください。  
  
## コードのコンパイル  
  
-   このコードを切り取って、Visual Studio 2010 プロジェクトに貼り付けます。  
  
## 参照  
 <xref:System.Threading.Tasks.Parallel.For%2A>   
 <xref:System.Threading.Tasks.Parallel.ForEach%2A>   
 [Data Parallelism](../../../docs/standard/parallel-programming/data-parallelism-task-parallel-library.md)   
 [Parallel Programming](../../../docs/standard/parallel-programming/index.md)