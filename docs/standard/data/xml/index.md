---
title: "XML ドキュメントと XML データ | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
ms.assetid: e695047f-3c0f-4045-8708-5baea91cc380
caps.latest.revision: 9
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 9
---
# XML ドキュメントと XML データ
.NET Framework には、XML 対応アプリを容易に構築するための、包括的で統合された一連のクラスが用意されています。  次の名前空間のクラスでは、XML の解析と書き込み、メモリ内での XML データの編集、データの検証、および XSLT 変換がサポートされます。  
  
-   <xref:System.Xml>  
  
-   <xref:System.Xml.XPath>  
  
-   <xref:System.Xml.Xsl>  
  
-   <xref:System.Xml.Schema>  
  
-   <xref:System.Xml.Linq>  
  
 一覧については、「[System.Xml 名前空間](http://msdn.microsoft.com/library/gg145036.aspx)」Web ページを参照してください。  
  
 次の名前空間のクラスでは、World Wide Web Consortium \(W3C\) 勧告がサポートされます。  次に例を示します。  
  
-   <xref:System.Xml.XmlDocument?displayProperty=fullName> クラスは、[W3C ドキュメント オブジェクト モデル \(DOM\) 勧告の DOM Level 1 Core](http://www.w3.org/TR/REC-DOM-Level-1/) および [DOM Level 2 Core](http://www.w3.org/TR/DOM-Level-2-Core/) を実装しています。  
  
-   <xref:System.Xml.XmlReader?displayProperty=fullName> クラスと <xref:System.Xml.XmlWriter?displayProperty=fullName> クラスは、W3C 勧告『[XML 1.0](http://www.w3.org/TR/2006/REC-xml-20060816/)』および『[Namespaces in XML](http://www.w3.org/TR/REC-xml-names/)』をサポートしています。  
  
-   <xref:System.Xml.Schema.XmlSchemaSet?displayProperty=fullName> クラスのスキーマは、W3C 勧告『[XML Schema Part 1: Structures](http://www.w3.org/TR/xmlschema-1/)』および『[XML Schema Part 2: Datatypes](http://www.w3.org/TR/xmlschema-2/)』をサポートしています。  
  
-   <xref:System.Xml.Xsl?displayProperty=fullName> 名前空間のクラスは、W3C 勧告『[XSLT 1.0](http://www.w3.org/TR/xslt)』に準拠する XSLT 変換をサポートしています。  
  
 .NET Framework の XML クラスの利点を次に示します。  
  
-   **生産性。** [LINQ to XML](../../../../ocs/visual-basic/programming-guide/concepts/linq/linq-to-xml.md) により XML でのプログラミングがさらに容易になるほか、SQL と同じようにクエリを利用できます。  
  
-   **機能拡張。**.NET Framework の XML クラスは、抽象基本クラスと仮想メソッドを使用することによって拡張できます。  たとえば、キャッシュ ストリームをローカルのディスクに格納する <xref:System.Xml.XmlUrlResolver> クラスの派生クラスを作成できます。  
  
-   **プラグ可能なアーキテクチャ。**.NET Framework が提供するアーキテクチャでは、コンポーネントが相互に利用できます。また、コンポーネント間でデータをストリーム転送できます。  たとえば、<xref:System.Xml.XPath.XPathDocument> オブジェクト、<xref:System.Xml.XmlDocument> オブジェクトなどのデータ ストアは、<xref:System.Xml.Xsl.XslCompiledTransform> クラスを使って変換でき、その出力は、別のストアにストリーム転送することも、Web サービスからのストリームとして返すこともできます。  
  
-   **パフォーマンス。**アプリのパフォーマンスを高めるために、.NET Framework の一部の XML クラスはストリーム ベースのモデルをサポートし、次の特性を備えています。  
  
    -   キャッシング処理を最小限に抑える前方参照専用のプル モデル解析 \(<xref:System.Xml.XmlReader>\)。  
  
    -   前方参照専用の検証 \(<xref:System.Xml.XmlReader>\)。  
  
    -   作成するノードを単一の仮想ノードに抑えながら、ドキュメントへのランダム アクセスを可能にする、カーソル スタイルのナビゲーション \(<xref:System.Xml.XPath.XPathNavigator>\)。  
  
     XSLT 処理が必要になるときのパフォーマンスを常に高めるために、<xref:System.Xml.XPath.XPathDocument> クラスを使用できます。このクラスは、<xref:System.Xml.Xsl.XslCompiledTransform> クラスと効率よく連携するように設計された、XPath クエリ用の最適化された読み取り専用ストアです。  
  
-   **ADO.NET との統合。**XML クラスと [ADO.NET](../../../../docs/framework/data/adonet/index.md) が緊密に統合されることで、リレーショナル データと XML が結合されます。  <xref:System.Data.DataSet> クラスは、データベースから取得されたデータのメモリ内キャッシュです。  <xref:System.Data.DataSet> クラスは、<xref:System.Xml.XmlReader> クラスと <xref:System.Xml.XmlWriter> クラスを使用して XML を読み書きする機能、内部リレーショナル スキーマ構造を XML スキーマ \(XSD\) として永続化する機能、および XML ドキュメントからスキーマ構造を推論する機能を備えています。  
  
## このセクションの内容  
 [XML の処理オプション](../../../../docs/standard/data/xml/xml-processing-options.md)  
 XML データの処理に関するオプションについて説明します。  
  
 [メモリ内の XML データの処理](../../../../docs/standard/data/xml/processing-xml-data-in-memory.md)  
 XML データをメモリ内で処理する 3 つの方法について説明します。  [LINQ to XML](../../../../ocs/visual-basic/programming-guide/concepts/linq/linq-to-xml.md)、<xref:System.Xml.XmlDocument> クラス \(W3C ドキュメント オブジェクト モデルに基づく\)、および <xref:System.Xml.XPath.XPathDocument> クラス \(XPath データ モデルに基づく\)。  
  
 [XSLT 変換](../../../../docs/standard/data/xml/xslt-transformations.md)  
 XSLT プロセッサの使い方について説明します。  
  
 [XML スキーマ オブジェクト モデル \(SOM\)](../../../../docs/standard/data/xml/xml-schema-object-model-som.md)  
 スキーマの読み込みと編集のための <xref:System.Xml.Schema.XmlSchema> クラスを提供することで、XML スキーマ \(XSD\) の作成と操作に使用されるクラスについて説明します。  
  
 [XML とリレーショナル データおよび ADO.NET との統合](../../../../docs/standard/data/xml/xml-integration-with-relational-data-and-adonet.md)  
 .NET Framework が <xref:System.Data.DataSet> オブジェクトと <xref:System.Xml.XmlDataDocument> オブジェクトを介して、データのリレーショナル表現および階層表現の両方に対するリアルタイムの同期アクセスを実現するしくみついて説明します。  
  
 [XML ドキュメントでの名前空間の管理](../../../../docs/standard/data/xml/managing-namespaces-in-an-xml-document.md)  
 <xref:System.Xml.XmlNamespaceManager> クラスを使用して、名前空間情報を格納および保持する方法について説明します。  
  
 [System.Xml クラスでの型のサポート](../../../../docs/standard/data/xml/type-support-in-the-system-xml-classes.md)  
 XML データ型を CLR 型にマップする方法、XML データ型を変換する方法、および <xref:System.Xml> クラスのその他の型サポート機能について説明します。  
  
## 関連項目  
 [ADO.NET](../../../../docs/framework/data/adonet/index.md)  
 ADO.NET を使用してデータにアクセスする方法について説明します。  
  
 [セキュリティ](../../../../docs/standard/security/index.md)  
 .NET Framework セキュリティ システムの概要を説明します。  
  
 [XML デベロッパー センター](http://go.microsoft.com/fwlink/?linkid=42458)  
 追加の技術情報、ダウンロード、ニュースグループ、および XML 開発者向けのその他のリソースを提供します。