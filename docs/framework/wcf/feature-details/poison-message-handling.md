---
title: "有害メッセージ処理 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 8d1c5e5a-7928-4a80-95ed-d8da211b8595
caps.latest.revision: 29
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 29
---
# 有害メッセージ処理
A*有害なメッセージ*をアプリケーションに配信が試行の最大数を超えたメッセージです。 この状況は、キュー ベースのアプリケーションがエラーによってメッセージを処理できないときに発生する可能性があります。 信頼性に対する要求を満たすために、キューに置かれたアプリケーションはトランザクションの下でメッセージを受信します。 キューに置かれたメッセージを受信したトランザクションを中止すると、メッセージはそのままキューに残り、新しいトランザクションの下で再試行されます。 トランザクションを中止させた問題が解決されなければ、受信側のアプリケーションは、配信試行回数の最大値を超えるまで同じメッセージの受信と中止を繰り返す悪循環に陥る可能性があり、その結果、有害メッセージが生じることになります。  
  
 メッセージは、さまざまな理由で有害メッセージになる可能性があります。 最も一般的な理由は、アプリケーション固有の理由です。 たとえば、アプリケーションがキューからメッセージを読み取り、なんらかのデータベース処理を実行する場合は、アプリケーションがデータベースをロックできないことによって、トランザクションが中止されることが考えられます。 データベース トランザクションが中止されたために、メッセージはキューに残ります。これにより、アプリケーションではメッセージを再度読み取り、データベースのロックの取得を再試行します。 メッセージに無効な情報が含まれている場合にも、有害メッセージになる可能性があります。 たとえば、発注書に無効な顧客番号が含まれている場合があります。 このような場合、アプリケーションは自発的にトランザクションを中止し、メッセージを強制的に有害メッセージにすることがあります。  
  
 まれに、メッセージをアプリケーションにディスパッチできないことがあります。 メッセージに不適切なフレームがあったり、無効なメッセージ資格情報が割り当てられていたり、無効なアクション ヘッダーが含まれていたりする場合などは、[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] レイヤーがメッセージの問題を検出することがあります。 このような場合、アプリケーションはメッセージを受信しません。ただし、メッセージは有害メッセージとなるため、手動で処理できます。  
  
## <a name="handling-poison-messages"></a>有害メッセージの処理  
 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] の有害メッセージ処理は、アプリケーションにディスパッチできないメッセージや、アプリケーションにディスパッチされても、アプリケーション固有の理由によって処理できないメッセージを、受信側アプリケーションで処理する機構を提供します。 有害メッセージ処理は、キューに置かれた使用可能な各バインディングの次のプロパティで構成されます。  
  
-   `ReceiveRetryCount`. アプリケーション キューからアプリケーションへのメッセージの配信を再試行する最大回数を示す整数値。 既定値は 5 です。 データベースの一時的なデッドロックなどの問題を即時再試行で解決する場合は、この値で十分です。  
  
-   `MaxRetryCycles`. 再試行サイクルの最大数を示す整数値。 再試行サイクルは、アプリケーション キューから再試行サブキューにメッセージを転送し、構成可能な遅延の後に再試行サブキューからアプリケーション キューにメッセージを転送して配信を再試行するプロセスで構成されます。 既定値は 2 です。 [!INCLUDE[wv](../../../../includes/wv-md.md)] では、メッセージが最大で (`ReceiveRetryCount` + 1) × (`MaxRetryCycles` + 1) 回試行されます。 `MaxRetryCycles` は、[!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] および [!INCLUDE[wxp](../../../../includes/wxp-md.md)] では無視されます。  
  
-   `RetryCycleDelay`. 再試行サイクル間の遅延時間。 既定値は 30 分です。 `MaxRetryCycles` と `RetryCycleDelay` の組み合わせにより、問題に対処する機構が提供されます。この場合、定期的な遅延後に再試行が行われ、問題が解決されます。 たとえば、SQL Server の保留中のトランザクションのコミットでロックされた行セットが処理されます。  
  
-   `ReceiveErrorHandling`. 再試行を最大回数実行しても配信できなかったメッセージに対して実行するアクションを示す列挙値。 値には、Fault、Drop、Reject、および Move の&4; つがあります。 既定のオプションは Fault です。  
  
-   Fault :  このオプションでは、`ServiceHost` のエラーの原因となったリスナーにエラーが送信されます。 アプリケーションでキューのメッセージの処理を継続するには、なんらかの外部機構によってアプリケーション キューからメッセージを削除する必要があります。  
  
-   Drop :  このオプションでは、有害メッセージが破棄されます。メッセージがアプリケーションに配信されることはありません。 この時点でメッセージの `TimeToLive` プロパティの有効期限が既に切れている場合は、メッセージが送信側の配信不能キューに表示されることがあります。 有効期限が切れていない場合は、どこにも表示されません。 このオプションは、メッセージが失われたときにユーザーが実行する操作が指定されていないことを示します。  
  
-   Reject :  このオプションは、[!INCLUDE[wv](../../../../includes/wv-md.md)] 専用です。 アプリケーションがメッセージを受信できないという否定受信確認を送信側キュー マネージャーに返信するように、メッセージ キュー (MSMQ) に指示します。 メッセージは、送信側キュー マネージャーの配信不能キューに置かれます。  
  
-   Move :  このオプションは、[!INCLUDE[wv](../../../../includes/wv-md.md)] 専用です。 有害メッセージを有害メッセージ キューに移動して、後で有害メッセージ処理アプリケーションで処理できるようにします。 有害メッセージ キューは、アプリケーション キューのサブキューです。 有害メッセージ処理アプリケーションを、有害キューからメッセージを読み取る [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] サービスにすることができます。 有害キューはアプリケーション キューのサブキューであり、net.msmq:// としてアドレス指定できます*マシン名*>/*applicationQueue*有害、;、*マシン名*キューが存在するコンピューターの名前を指定します、 *applicationQueue*アプリケーションに固有のキューの名前を指定します。  
  
 メッセージに対して実行される配信の最大試行回数は、次のようになります。  
  
-   [!INCLUDE[wv](../../../../includes/wv-md.md)] の場合、((ReceiveRetryCount + 1) × (MaxRetryCycles + 1))。  
  
-   [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] および [!INCLUDE[wxp](../../../../includes/wxp-md.md)] の場合、(ReceiveRetryCount + 1)。  
  
> [!NOTE]
>  正常に配信されたメッセージについては、再試行は行われません。  
  
 メッセージの読み取りが試行された回数を追跡するために、[!INCLUDE[wv](../../../../includes/wv-md.md)] では、中止回数をカウントする永続的なメッセージ プロパティと、アプリケーション キューとサブキューの間でメッセージが移動された回数をカウントする移動回数プロパティを保持します。 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] チャネルはこれらを使用して、受信再試行回数と再試行サイクル数を計算します。 [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] と [!INCLUDE[wxp](../../../../includes/wxp-md.md)] では、中止回数が [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] チャネルによってメモリに保持され、アプリケーションでエラーが発生した場合にリセットされます。 また、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] チャネルはいつでも、最大 256 のメッセージの中止回数をメモリに保持できます。 257 番目のメッセージを読み取ると、最も古いメッセージの中止回数がリセットされます。  
  
 中止回数と移動回数のプロパティは、操作コンテキストを通じてサービス操作で使用できます。 これらのプロパティにアクセスする方法を次のコード例に示します。  
  
 [!code-csharp[S_UE_MSMQ_Poison#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ue_msmq_poison/cs/service.cs#1)]  
  
 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] は、標準の、キューに置かれたバインディングを&2; つ提供します。  
  
-   <xref:System.ServiceModel.NetMsmqBinding>します。 他の [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] エンドポイントとのキュー ベースの通信を実行するのに適した [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] バインディング。  
  
-   <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>します。 既存のメッセージ キュー アプリケーションとの通信に適したバインディング。  
  
> [!NOTE]
>  これらのバインディングでは、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] サービスの要件に基づいてプロパティを変更できます。 有害メッセージ処理機構全体は、受信側アプリケーションに対してローカルです。 このプロセスは、受信側アプリケーションが最終的に処理を停止し、送信側に否定受信確認を返信する場合を除き、送信元アプリケーションには認識されません。 この場合、メッセージは、送信元の配信不能キューに送られます。  
  
## <a name="best-practice-handling-msmqpoisonmessageexception"></a>ベスト プラクティス : MsmqPoisonMessageException の処理  
 キューに置かれたトランスポートによってスロー、サービス、メッセージが有害であると判断した場合、 <xref:System.ServiceModel.MsmqPoisonMessageException>を含む、`LookupId`有害メッセージのです。  
  
 受信側アプリケーションを実装する、 <xref:System.ServiceModel.Dispatcher.IErrorHandler>インターフェイスをアプリケーションに必要なエラーがあれば処理します。 [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)][エラー処理およびレポートに対する制御の拡張](../../../../docs/framework/wcf/samples/extending-control-over-error-handling-and-reporting.md)します。  
  
 アプリケーションでは、有害メッセージを有害メッセージ キューに移動し、サービスがキュー内の残りのメッセージにアクセスできるようにする、なんらかの有害メッセージ自動処理を必要とする場合があります。 エラー処理機構を使用して、有害メッセージの例外をリッスンする唯一のシナリオは、 <xref:System.ServiceModel.Configuration.MsmqBindingElementBase.ReceiveErrorHandling%2A>に設定した<xref:System.ServiceModel.ReceiveErrorHandling>します。 メッセージ キュー 3.0 の有害メッセージ サンプルは、この動作を示しています。 ベスト プラクティスを含め、有害メッセージを処理するために必要な手順の概要を以下に示します。  
  
1.  有害メッセージの設定がアプリケーションの要件を反映していることを確認します。 設定を処理するときは、[!INCLUDE[wv](../../../../includes/wv-md.md)]、[!INCLUDE[ws2003](../../../../includes/ws2003-md.md)]、および [!INCLUDE[wxp](../../../../includes/wxp-md.md)] でのメッセージ キューの機能の相違点を理解してください。  
  
2.  必要に応じて `IErrorHandler` を実装し、有害メッセージのエラーを処理します。 `ReceiveErrorHandling` を `Fault` に設定する場合、有害メッセージをキューから取り除いたり、外部の従属的な問題を解決したりする手動の機構が必要となります。そのため、`IErrorHandler` を `ReceiveErrorHandling` に設定するときは、次のコードに示すように `Fault` を実装するのが一般的です。  
  
     [!code-csharp[S_UE_MSMQ_Poison#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ue_msmq_poison/cs/poisonerrorhandler.cs#2)]  
  
3.  サービス動作が使用できる `PoisonBehaviorAttribute` を作成します。 この動作により、`IErrorHandler` がディスパッチャーにインストールされます。 このコード例を次に示します。  
  
     [!code-csharp[S_UE_MSMQ_Poison#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ue_msmq_poison/cs/poisonbehaviorattribute.cs#3)]  
  
4.  サービスに有害動作属性による注釈が付けられていることを確認します。  
  
  
  
 また、`ReceiveErrorHandling` を `Fault` に設定した場合は、`ServiceHost` が有害メッセージを検出するとエラーになります。 この場合、faulted イベントにフックしてサービスを終了し、是正措置を講じた後に再起動できます。 など、`LookupId`で、 <xref:System.ServiceModel.MsmqPoisonMessageException>に反映、`IErrorHandler`ことに注意して、サービス ホストはエラーに使用するとでしたし、 `System.Messaging` API を使用してキューからメッセージを受信する、`LookupId`キューからメッセージを削除して、外部ストアまたは別のキューにメッセージを格納します。 これで `ServiceHost` を再起動して、通常の処理を再開できるようになります。 [の有害メッセージ処理で MSMQ 4.0](../../../../docs/framework/wcf/samples/poison-message-handling-in-msmq-4-0.md)この動作を示します。  
  
## <a name="transaction-time-out-and-poison-messages"></a>トランザクション タイムアウトと有害メッセージ  
 キューに置かれたトランスポート チャネルとユーザー コードの間では、特定の部類のエラーが発生する可能性があります。 これらのエラーは、メッセージ セキュリティ レイヤーやサービス ディスパッチ ロジックなど、中間のレイヤーで検出される場合があります。 たとえば、SOAP セキュリティ レイヤーで X.509 証明書がないことが検出された場合やアクションがない場合は、メッセージがアプリケーションにディスパッチされます。 この場合、サービス モデルはそのメッセージを破棄します。 メッセージはトランザクションで読み取られますが、そのトランザクションの結果は提供されないため、トランザクションが最終的にタイムアウトになって中止され、メッセージはキューに戻されます。 つまり、エラーの部類によっては、トランザクションがすぐに中止されるわけではなく、タイムアウトになるまで待機します。 使用してサービスのトランザクション タイムアウトを変更する<xref:System.ServiceModel.ServiceBehaviorAttribute>します。  
  
 トランザクション タイムアウトをコンピューター全体で変更するには、machine.config ファイルを変更し、適切なトランザクション タイムアウトを設定します。 トランザクションに設定されたタイムアウトに従って、トランザクションは最終的に中止され、キューに戻されることになり、中止回数がインクリメントされることに注意してください。 最終的に、メッセージは有害メッセージになり、ユーザー設定に従って適切な処理が行われます。  
  
## <a name="sessions-and-poison-messages"></a>セッションと有害メッセージ  
 セッションでは、単一のメッセージと同じ再試行および有害メッセージ処理手順が行われます。 有害メッセージに対する上記のプロパティは、セッション全体に適用されます。 つまり、セッション全体が再試行され、メッセージは最終有害メッセージ キューに送られます。または、メッセージが拒否された場合は、送信側の配信不能キューに送られます。  
  
## <a name="batching-and-poison-messages"></a>バッチ処理と有害メッセージ  
 バッチの一部であるメッセージが有害メッセージになった場合は、バッチ全体がロールバックされ、チャネルはメッセージを&1; つずつ読み取る処理に戻ります。 [!INCLUDE[crabout](../../../../includes/crabout-md.md)]バッチ処理を参照してください[トランザクションでメッセージのバッチ処理](../../../../docs/framework/wcf/feature-details/batching-messages-in-a-transaction.md)  
  
## <a name="poison-message-handling-for-messages-in-a-poison-queue"></a>有害キュー内のメッセージに対する有害メッセージ処理  
 有害メッセージ処理は、メッセージが有害メッセージ キューに配置された時点では終了しません。 有害メッセージ キューに置かれたメッセージは、依然として読み取り、処理する必要があります。 最終有害サブキューからメッセージを読み取るときは、有害メッセージ処理設定のサブセットを使用できます。 適用できる設定は、`ReceiveRetryCount` と `ReceiveErrorHandling` です。 `ReceiveErrorHandling` は Drop、Rreject、Fault のいずれかに設定できます。 `MaxRetryCycles` が Move に設定されている場合は、`ReceiveErrorHandling` が無視され、例外がスローされます。  
  
## <a name="windows-vista-windows-server-2003-and-windows-xp-differences"></a>Windows Vista、Windows Server 2003、および Windows XP の相違点  
 前述のように、[!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] と [!INCLUDE[wxp](../../../../includes/wxp-md.md)] には、すべての有害メッセージ処理設定が適用されるわけではありません。 [!INCLUDE[ws2003](../../../../includes/ws2003-md.md)]、[!INCLUDE[wxp](../../../../includes/wxp-md.md)]、および [!INCLUDE[wv](../../../../includes/wv-md.md)] のメッセージ キューには、有害メッセージの処理に関連する次のような重要な違いがあります。  
  
-   [!INCLUDE[wv](../../../../includes/wv-md.md)] のメッセージ キューはサブキューをサポートしていますが、[!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] と [!INCLUDE[wxp](../../../../includes/wxp-md.md)] はサポートしていません。 サブキューは、有害メッセージ処理で使用されます。 再試行キューと有害キューは、有害メッセージ処理の設定に基づいて作成されるアプリケーション キューのサブキューです。 作成する再試行サブキューの数は、`MaxRetryCycles` で指定します。 したがって、[!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] または [!INCLUDE[wxp](../../../../includes/wxp-md.md)] で実行している場合、`MaxRetryCycles` は無視されるため、`ReceiveErrorHandling.Move` は使用できません。  
  
-   [!INCLUDE[wv](../../../../includes/wv-md.md)] のメッセージ キューは否定受信確認をサポートしていますが、[!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] と [!INCLUDE[wxp](../../../../includes/wxp-md.md)] はサポートしていません。 受信側キュー マネージャーから否定受信確認を受け取ると、送信側キュー マネージャーは拒否されたメッセージを配信不能キューに入れます。 そのため、`ReceiveErrorHandling.Reject` は、[!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] と [!INCLUDE[wxp](../../../../includes/wxp-md.md)] では使用できません。  
  
-   [!INCLUDE[wv](../../../../includes/wv-md.md)] のメッセージ キューは、メッセージの配信試行回数を保持するメッセージ プロパティをサポートしています。 この中止回数のプロパティは、[!INCLUDE[ws2003](../../../../includes/ws2003-md.md)] と [!INCLUDE[wxp](../../../../includes/wxp-md.md)] では使用できません。 [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] は、中止回数をメモリで保持するため、同じメッセージがファーム内の複数の [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] サービスによって読み取られた場合、このプロパティは、正確な値を格納できない可能性があります。  
  
## <a name="see-also"></a>関連項目  
 [キューの概要](../../../../docs/framework/wcf/feature-details/queues-overview.md)   
 [Windows Vista、Windows Server 2003、および Windows XP におけるキュー機能の相違点](../../../../docs/framework/wcf/feature-details/diff-in-queue-in-vista-server-2003-windows-xp.md)   
 [指定して、コントラクトおよびサービスでエラーを処理](../../../../docs/framework/wcf/specifying-and-handling-faults-in-contracts-and-services.md)