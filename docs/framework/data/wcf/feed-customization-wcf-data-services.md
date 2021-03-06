---
title: "フィードのカスタマイズ (WCF Data Services) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-oob"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "WCF Data Services、フィード"
  - "WCF Data Services、Atom プロトコル"
  - "Atom 公開プロトコル [WCF Data Services]"
  - "WCF Data Services、フィードのカスタマイズ"
ms.assetid: 0d1a39bc-6462-4683-bd7d-e74e0fd28a85
caps.latest.revision: 11
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 11
---
# フィードのカスタマイズ (WCF Data Services)
[!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)]使用して、[!INCLUDE[ssODataFull](../../../../includes/ssodatafull-md.md)]をフィードとしてデータを公開します。 [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)]データ フィードを Atom と JavaScript Object Notation (JSON) の両方の形式をサポートします。 Atom フィードを使用すると[!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)]エンティティおよびリレーションシップを HTTP メッセージの本文に含めることのできる XML 形式などのデータをシリアル化の標準的な方法を提供します。 [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)]エンティティに含まれるデータと Atom 要素間の既定のエンティティとプロパティのマッピングを定義します。 詳細については、次を参照してください。 [OData: Atom 形式](http://go.microsoft.com/fwlink/?LinkID=185794)します。  
  
 場合によっては、データ サービスから返されるプロパティのデータを、標準のフィードの形式ではなくカスタマイズした方法でシリアル化する必要があります。 [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)]、またはフィード内のエントリのカスタム要素にエントリの未使用の要素と属性には、エンティティのプロパティをマップできますが、データ フィードのシリアル化をカスタマイズできます。  
  
> [!NOTE]
>  フィードのカスタマイズは、Atom フィードでのみサポートされます。 返されたフィードに対して JSON 形式が要求されたときは、カスタム フィードが返されません。  
  
 [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] を使用すると、Atom ペイロードに対するエンティティとプロパティの代替マッピングは、属性をデータ モデルのエンティティ型に手動で適用することによって定義できます。 これらの属性の適用方法は、データ サービスのデータ ソース プロバイダーによって決定されます。  
  
> [!IMPORTANT]
>  カスタム フィードを定義する場合、カスタム マッピングが定義されているすべてのエンティティ プロパティが投影に含まれることを保証する必要があります。 マップされているエンティティ プロパティがこの射影に含まれていない場合、データの損失が発生することがあります。 詳細については、次を参照してください。[クエリ プロジェクション](../../../../docs/framework/data/wcf/query-projections-wcf-data-services.md)します。  
  
## <a name="customizing-feeds-with-the-entity-framework-provider"></a>Entity Framework プロバイダーを使用したフィードのカスタマイズ  
 [!INCLUDE[adonet_ef](../../../../includes/adonet-ef-md.md)] プロバイダーで使用されるデータ モデルは、.edmx ファイルの XML として表現されます。 この場合、カスタム フィードを定義する属性が、データ モデルのエンティティ型とプロパティを表現する `EntityType` および `Property` 要素に追加されます。 これらのフィードのカスタマイズ属性が定義されていない[ \[MC-CSDL\]: 概念スキーマ定義ファイル形式](http://go.microsoft.com/fwlink/?LinkId=159072)、形式であること、[!INCLUDE[adonet_ef](../../../../includes/adonet-ef-md.md)]プロバイダーを使用してデータ モデルを定義します。 したがって、フィードのカスタマイズ属性を特定のスキーマ名前空間で宣言する必要があります。これは、`m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata"` として定義されます。 次の XML フラグメントは、`Property`、`Products`、および `ProductName` プロパティを定義する `ReorderLevel` エンティティ型の `UnitsInStock` 要素に適用されたフィードのカスタマイズ属性を示します。  
  
  [Astoria カスタム フィード #EdmFeedAttributes](../CodeSnippet/VS_Snippets_Misc/astoria custom feeds#edmfeedattributes)]  
  
 これらの属性によって `Products` エンティティ セットに対して次のカスタム データ フィードが生成されます。 このカスタム データ フィードでは、`ProductName` プロパティ値が `author` 要素内に加えて、`ProductName` プロパティ要素としても表示されます。`UnitsInStock` プロパティは、一意の名前空間、および属性としての `ReorderLevel` プロパティと共にカスタム要素に表示されます。  
  
  [Astoria カスタム フィード #EdmFeedResultProduct](../CodeSnippet/VS_Snippets_Misc/astoria custom feeds#edmfeedresultproduct)]  
  
 詳細については、次を参照してください。[方法: Entity Framework プロバイダーでフィードをカスタマイズする](../../../../docs/framework/data/wcf/how-to-customize-feeds-with-ef-provider-wcf-data-services.md)です。  
  
> [!NOTE]
>  データ モデルへの拡張はエンティティ デザイナーでサポートされていないので、データ モデルを含む XML ファイルを手動で変更する必要があります。 [!INCLUDE[crabout](../../../../includes/crabout-md.md)]によって生成される .edmx ファイル、[!INCLUDE[adonet_edm](../../../../includes/adonet-edm-md.md)]ツールを参照してください[.edmx ファイルの概要](http://msdn.microsoft.com/ja-jp/f4c8e7ce-1db6-417e-9759-15f8b55155d4)します。  
  
### <a name="custom-feed-attributes"></a>カスタム フィード属性  
 次の表に、データ モデルを定義する概念スキーマ定義言語 (CSDL) に追加できるフィードをカスタマイズする XML 属性を示します。 これらの属性が同等のプロパティには、 <xref:System.Data.Services.Common.EntityPropertyMappingAttribute>リフレクション プロバイダーと共に使用します。  
  
|属性名|説明|  
|--------------------|-----------------|  
|`FC_ContentKind`|コンテンツの種類を示します。 次のキーワードは、配信コンテンツの種類を定義します。<br /><br /> `text:`プロパティの値はフィード内でテキストとして表示されます。<br /><br /> `html:`プロパティの値はフィード内で HTML として表示されます。<br /><br /> `xhtml:`プロパティの値はフィード内で XML 形式の HTML として表示されます。<br /><br /> これらのキーワードの値に相当、 <xref:System.Data.Services.Common.SyndicationTextContentKind>リフレクション プロバイダーと共に使用される列挙体です。<br /><br /> この属性は、`FC_NsPrefix` および `FC_NsUri` 属性が使用されているときはサポートされません。<br /><br /> `xhtml` 属性の値として `FC_ContentKind` を指定する場合、そのプロパティ値に正しい形式の XML が含まれることを確認する必要があります。 データ サービスは、変換を行わずに値を返します。 さらに、返された XML 内の XML 要素のプレフィックスに名前空間 URI、およびマップされたフィード内で定義されたプレフィックスが含まれている必要があります。|  
|`FC_KeepInContent`|参照されたプロパティ値をフィードのコンテンツ セクションおよびマップされた場所の両方に含める必要があることを示します。 有効値は `true` または `false` です。 結果のフィードの以前のバージョンとの下位互換性のために[!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)]の値を指定して`true`フィードのコンテンツ セクションで、値が含まれていることを確認します。|  
|`FC_NsPrefix`|非配信マッピング内の XML 要素の名前空間プレフィックス。 この属性は、`FC_NsUri` 属性と一緒に使用する必要があります。この属性は、`FC_ContentKind` 属性と一緒に使用できません。|  
|`FC_NsUri`|非配信マッピング内の XML 要素の名前空間 URI。 この属性は、`FC_NsPrefix` 属性と一緒に使用する必要があります。この属性は、`FC_ContentKind` 属性と一緒に使用できません。|  
|`FC_SourcePath`|このフィード マッピング規則が適用されるエンティティのプロパティのパス。 この属性は、`EntityType` 要素で使用されている場合にのみサポートされます。<br /><br /> <xref:System.Data.Services.Common.EntityPropertyMappingAttribute.SourcePath%2A>プロパティが複合型を直接参照できません。 複合型の場合は、プロパティ名が円記号 (`/`) で区切られたパス式を使用する必要があります。 エンティティ型に対して次の値を許可するなどの`Person`整数プロパティを使用して`Age`と複雑なプロパティ<br /><br /> `Address`:<br /><br /> `Age`<br /><br /> `Address/Street`<br /><br /> <xref:System.Data.Services.Common.EntityPropertyMappingAttribute.SourcePath%2A>プロパティはスペースまたはその他のプロパティ名に無効な文字を含む値に設定することはできません。|  
|`FC_TargetPath`|プロパティのマップ先となる結果のフィードのターゲット要素の名前。 この要素は、Atom 仕様またはカスタム要素によって定義された要素である場合があります。<br /><br /> 次のキーワードは、[!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)] フィードの特定の場所をポイントする定義済みの配信ターゲット パス値です。<br /><br /> `SyndicationAuthorEmail:``atom:email`の子要素、`atom:author`要素。<br /><br /> `SyndicationAuthorName:``atom:name`の子要素、`atom:author`要素。<br /><br /> `SyndicationAuthorUri:``atom:uri`の子要素、`atom:author`要素。<br /><br /> `SyndicationContributorEmail:``atom:email`の子要素、`atom:contributor`要素。<br /><br /> `SyndicationContributorName:``atom:name`の子要素、`atom:contributor`要素。<br /><br /> `SyndicationContributorUri:``atom:uri`の子要素、`atom:contributor`要素。<br /><br /> `SyndicationCustomProperty:`カスタム プロパティ要素。 カスタム要素にマップする際、ターゲットは、ネストされた要素が円記号 (`/`) で区切られ、属性がアンパサンド (`@`) で指定されたパス式である必要があります。 次の例では、文字列 `UnitsInStock/@ReorderLevel` によってプロパティ値がルート エントリ要素の `ReorderLevel` という子要素の `UnitsInStock` という属性にマップされます。<br /><br /> `<Property Name="ReorderLevel" Type="Int16"               m:FC_TargetPath="UnitsInStock/@ReorderLevel"               m:FC_NsPrefix="Northwind"               m:FC_NsUri="http://schemas.examples.microsoft.com/dataservices"               m:FC_KeepInContent="false"               />`<br /><br /> ターゲットがカスタム要素名の場合、`FC_NsPrefix` および `FC_NsUri` 属性も指定する必要があります。<br /><br /> `SyndicationPublished:``atom:published`要素。<br /><br /> `SyndicationRights:``atom:rights`要素。<br /><br /> `SyndicationSummary:``atom:summary`要素。<br /><br /> `SyndicationTitle:``atom:title`要素。<br /><br /> `SyndicationUpdated:``atom:updated`要素。<br /><br /> これらのキーワードの値に相当、 <xref:System.Data.Services.Common.SyndicationItemProperty>リフレクション プロバイダーと共に使用される列挙体です。|  
  
> [!NOTE]
>  属性名および値では、大文字と小文字が区別されます。 属性は、`EntityType` 要素または&1; つ以上の `Property` 要素に適用されます (両方には適用されません)。  
  
## <a name="customizing-feeds-with-the-reflection-provider"></a>リフレクション プロバイダーを使用したフィードのカスタマイズ  
 リフレクション プロバイダーを使用して実装されたデータ モデル用にフィードをカスタマイズするには、1 つまたは複数のインスタンスを追加、 <xref:System.Data.Services.Common.EntityPropertyMappingAttribute>属性をデータ モデルのエンティティ型を表すクラス。 プロパティ、 <xref:System.Data.Services.Common.EntityPropertyMappingAttribute>クラスは、前のセクションで説明されているフィードのカスタマイズ属性に対応します。 両方のプロパティに対して定義されたカスタム フィード マッピングと共に `Order` 型の宣言の例を次に示します。  
  
> [!NOTE]
>  この例のデータ モデルの定義は、トピックで[方法: リフレクション プロバイダーを使用してデータ サービスを作成](../../../../docs/framework/data/wcf/create-a-data-service-using-rp-wcf-data-services.md)します。  
  
  [Astoria カスタム フィード #CustomOrderFeed](../CodeSnippet/VS_Snippets_Misc/astoria custom feeds#customorderfeed)]  
  
 これらの属性によって `Orders` エンティティ セットに対して次のカスタム データ フィードが生成されます。 このカスタム フィード、`OrderId`プロパティの値がでのみ表示されます、`title`の要素、`entry`と`Customer`プロパティの値では、両方が表示されます、`author`要素として、`Customer`プロパティ要素。  
  
  [Astoria カスタム フィード #IQueryableFeedResult](../CodeSnippet/VS_Snippets_Misc/astoria custom feeds#iqueryablefeedresult)]  
  
 詳細については、次を参照してください。[方法: リフレクション プロバイダーでフィードをカスタマイズする](../../../../docs/framework/data/wcf/how-to-customize-feeds-with-the-reflection-provider-wcf-data-services.md)です。  
  
## <a name="customizing-feeds-with-a-custom-data-service-provider"></a>カスタム データ サービス プロバイダーを使用したフィードのカスタマイズ  
 カスタム データ サービス プロバイダーを使用して定義されたデータ モデルが呼び出すことによってリソースの種類の定義用のフィードのカスタマイズ、 <xref:System.Data.Services.Providers.ResourceType.AddEntityPropertyMappingAttribute%2A>上、 <xref:System.Data.Services.Providers.ResourceType>を表すデータ モデルのエンティティ型。 詳細については、次を参照してください。[カスタム データ サービス プロバイダー](../../../../docs/framework/data/wcf/custom-data-service-providers-wcf-data-services.md)します。  
  
## <a name="consuming-custom-feeds"></a>カスタム フィードの処理  
 アプリケーションが直接処理する場合、[!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)]フィード、その必要がありますのカスタム要素および返されたフィード内の属性を処理できません。 データ サービス プロバイダーに関係なく、データ モデルにカスタム フィードを実装した場合、`$metadata` エンドポイントは、データ サービスによって返される CSDL のカスタム フィード属性としてカスタム フィード情報を返します。 使用すると、**サービス参照の追加**ダイアログまたは[datasvcutil.exe](../../../../docs/framework/data/wcf/wcf-data-service-client-utility-datasvcutil-exe.md)クライアント データ サービス クラスを生成するツール、カスタム フィード属性が要求と、データ サービスへの応答が正しく処理することを保証するために使用します。  
  
## <a name="feed-customization-considerations"></a>フィードのカスタマイズに関する考慮事項  
 カスタム フィードのマッピングを定義する場合は、以下を検討してください。  
  
-   [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] クライアントは、フィード内のマップされた要素に含まれているのが空白のみの場合、その要素を空であると見なします。 そのため、空白のみを含むマップされた要素は、同じ空白を使用するクライアントで具体化されません。 この空白をクライアントで維持するには、フィード マッピング属性で `KeepInContext` の値を `true` に設定する必要があります。  
  
## <a name="versioning-requirements"></a>バージョン管理の要件  
 フィードのカスタマイズには、次に示す [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)] プロトコルのバージョン管理の要件があります。  
  
-   フィードのカスタマイズを行う場合は、クライアントとデータ サービスの両方でバージョン 2.0 以降の [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)] プロトコルがサポートされている必要があります。  
  
 詳細については、次を参照してください。[データ サービスのバージョン管理](../../../../docs/framework/data/wcf/data-service-versioning-wcf-data-services.md)します。  
  
## <a name="see-also"></a>関連項目  
 [リフレクション プロバイダー](../../../../docs/framework/data/wcf/reflection-provider-wcf-data-services.md)   
 [Entity Framework プロバイダー](../../../../docs/framework/data/wcf/entity-framework-provider-wcf-data-services.md)