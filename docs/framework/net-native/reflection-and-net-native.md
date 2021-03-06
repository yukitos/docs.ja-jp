---
title: "リフレクションおよび .NET ネイティブ | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 91c9eae4-c641-476c-a06e-d7ce39709763
caps.latest.revision: 16
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 16
---
# リフレクションおよび .NET ネイティブ
.NET Framework では、マネージ開発はリフレクション API を介してメタプログラミングをサポートします。  リフレクションによって、アプリ内のオブジェクトの検査、検査で検出されたオブジェクトでのメソッドの呼び出し、実行時の新しい型の生成、およびその他多数の動的コード シナリオのサポートが可能になります。  シリアル化と逆シリアル化もサポートしているため、オブジェクトのフィールド値を保持して、後で復元できます。  これらすべてのシナリオで、使用可能なメタデータに基づいてネイティブ コードを生成するために .NET Framework Just\-In\-Time \(JIT\) コンパイラが必要です。  
  
 [!INCLUDE[net_native](../../../includes/net-native-md.md)] ランタイムには JIT コンパイラは含まれていません。  そのため、必要なネイティブ コードすべてを事前に生成しておく必要があります。  生成するコードを決定するために一連のヒューリスティックが使用されますが、これらのヒューリスティックでは、可能性のあるすべてのメタプログラミング シナリオに対処できるわけではありません。  このため、[ランタイム ディレクティブ](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md)を使用して、これらのメタプログラミング シナリオにヒントを提供する必要があります。  必要なメタデータまたは実装コードを実行時に使用できない場合、アプリは [MissingMetadataException](../../../docs/framework/net-native/missingmetadataexception-class-net-native.md)、[MissingRuntimeArtifactException](../../../docs/framework/net-native/missingruntimeartifactexception-class-net-native.md)、または [MissingInteropDataException](../../../docs/framework/net-native/missinginteropdataexception-class-net-native.md) 例外をスローします。  次の 2 つのトラブルシューティング ツールを使用すると、この例外を排除するランタイム ディレクティブ ファイルの適切なエントリが生成されます。  
  
-   [MissingMetadataException のトラブルシューティング ツール](http://dotnet.github.io/native/troubleshooter/type.html) \(型の場合\)。  
  
-   [MissingMetadataException のトラブルシューティング ツール](http://dotnet.github.io/native/troubleshooter/method.html) \(メソッドの場合\)。  
  
> [!NOTE]
>  ランタイム ディレクティブ ファイルが必要となる理由の背景を含む .NET ネイティブのコンパイルの概要については、「[.NET Native とコンパイル](../../../docs/framework/net-native/net-native-and-compilation.md)」をご覧ください。  
  
 また、[!INCLUDE[net_native](../../../includes/net-native-md.md)]では、.NET Framework クラス ライブラリのプライベート メンバーに対するリフレクションを実行できません。  たとえば、.NET Framework クラス ライブラリ型のフィールドを取得するために <xref:System.Reflection.TypeInfo.DeclaredFields%2A?displayProperty=fullName> プロパティを呼び出すと、パブリックまたはプロテクト フィールドのみが返されます。  
  
 次のトピックに、アプリでリフレクションとシリアル化をサポートするために必要な概念を説明したドキュメントとリファレンス ドキュメントを示します。  
  
-   [リフレクションに依存する API](../../../docs/framework/net-native/apis-that-rely-on-reflection.md)  
  
-   [リフレクション API リファレンス](../../../docs/framework/net-native/net-native-reflection-api-reference.md)  
  
-   [ランタイム ディレクティブ \(rd.xml\) 構成ファイル リファレンス](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md)  
  
## 参照  
 [.NET ネイティブによるアプリのコンパイル](../../../docs/framework/net-native/index.md)   
 [.NET Native とコンパイル](../../../docs/framework/net-native/net-native-and-compilation.md)