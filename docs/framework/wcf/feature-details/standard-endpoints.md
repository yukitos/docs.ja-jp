---
title: "標準エンドポイント | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 3fcb4225-addc-44f2-935d-30e4943a8812
caps.latest.revision: 11
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 11
---
# 標準エンドポイント
エンドポイントは、アドレス、バインディング、およびコントラクトを指定して定義します。エンドポイントに設定できるその他のパラメーターには、動作構成、ヘッダー、リッスン URI などがあります。特定の種類のエンドポイントでは、これらの値が変化しません。たとえば、メタデータ交換エンドポイントでは、常に <xref:System.ServiceModel.Description.IMetadataExchange> コントラクトを使用します。<xref:System.ServiceModel.Web.WebHttpEndpoint> などのその他のエンドポイントでは、指定されたエンドポイント動作が必要です。一般的に使用されるエンドポイント プロパティの既定の値を設定することによって、エンドポイントの操作性を向上させることができます。開発者は、標準エンドポイントを使用して、既定値を持つエンドポイント、またはエンドポイントの 1 つ以上のプロパティが変化しないエンドポイントを定義できます。これらのエンドポイントによって、静的な性質の情報を指定する必要がないエンドポイントの使用が可能になります。標準エンドポイントは、インフラストラクチャ エンドポイントおよびアプリケーション エンドポイントに使用できます。  
  
## インフラストラクチャ エンドポイント  
 サービスの作成者が明示的に実装していないいくつかのプロパティを持つエンドポイントを、サービスが公開することがあります。たとえば、メタデータ交換エンドポイントは <xref:System.ServiceModel.Description.IMetadataExchange> コントラクトを公開しますが、そのインターフェイスを実装するのは、サービスの作成者ではなく、WCF です。このようなインフラストラクチャ エンドポイントには、1 つ以上のプロパティの既定値があり、そのいくつかは変更できません。メタデータ交換エンドポイントの <xref:System.ServiceModel.Description.ServiceEndpoint.Contract%2A> プロパティは <xref:System.ServiceModel.Description.IMetadataExchange> である必要がありますが、バインディングなど、その他のプロパティは開発者が提供できます。インフラストラクチャ エンドポイントは、<xref:System.ServiceModel.Description.ServiceEndpoint.IsSystemEndpoint%2A> プロパティを `true` に設定することによって識別されます。  
  
## アプリケーション エンドポイント  
 アプリケーション開発者は独自の標準エンドポイントを定義でき、このエンドポイントでアドレス、バインディング、またはコントラクトの既定値が指定されます。標準エンドポイントを定義するには、<xref:System.ServiceModel.Description.ServiceEndpoint> からクラスを派生させ、適切なエンドポイント プロパティを設定します。変更可能な既定値をプロパティに設定できます。他のいくつかのプロパティには、変更できない静的な値が設定されます。次の例は、標準エンドポイントの実装方法を示しています。  
  
```  
public class CustomEndpoint : ServiceEndpoint  
    {  
        public CustomEndpoint()  
            : this(string.Empty)  
        {  
        }  
  
        public CustomEndpoint(string address)  
            : this(address, ContractDescription.GetContract(typeof(ICalculator)))  
        {  
        }  
  
        // Create the custom endpoint with a fixed binding  
        public CustomEndpoint(string address, ContractDescription contract)  
            : base(contract)  
        {  
            this.Binding = new BasicHttpBinding();  
            this.IsSystemEndpoint = false;  
        }  
  
        // Definition of the additional property of this endpoint  
        public bool Property  
        {  
            get;  
            set;  
        }  
    }  
```  
  
 ユーザー定義のカスタム エンドポイントを構成ファイルで使用するには、<xref:System.ServiceModel.Configuration.StandardEndpointElement> から 1 つのクラスを派生させ、<xref:System.ServiceModel.Configuration.StandardEndpointCollectionElement> から 1 つのクラスを派生させて、新しい標準エンドポイントを app.config または machine.config の拡張セクションに登録します。<xref:System.ServiceModel.Configuration.StandardEndpointElement> は、次の例に示すように、標準エンドポイントの構成サポートを提供します。  
  
```  
public class CustomEndpointElement : StandardEndpointElement  
    {  
        // Definition of the additional property for the standard endpoint element  
        public bool Property  
        {  
            get { return (bool)base["property"]; }  
            set { base["property"] = value; }  
        }  
  
        // The additional property needs to be added to the properties of the standard endpoint element  
        protected override ConfigurationPropertyCollection Properties  
        {  
            get  
            {  
                ConfigurationPropertyCollection properties = base.Properties;  
                properties.Add(new ConfigurationProperty("property", typeof(bool), false, ConfigurationPropertyOptions.None));  
                return properties;  
            }  
        }  
  
        // Return the type of this standard endpoint  
        protected override Type EndpointType  
        {  
            get { return typeof(CustomEndpoint); }  
        }  
  
        // Create the custom service endpoint  
        protected override ServiceEndpoint CreateServiceEndpoint(ContractDescription contract)  
        {  
            return new CustomEndpoint();  
        }  
  
        // Read the value given to the property in config and save it  
        protected override void OnApplyConfiguration(ServiceEndpoint endpoint, ServiceEndpointElement serviceEndpointElement)  
        {  
            CustomEndpoint customEndpoint = (CustomEndpoint)endpoint;  
            customEndpoint.Property = this.Property;  
        }  
  
        // Read the value given to the property in config and save it  
        protected override void OnApplyConfiguration(ServiceEndpoint endpoint, ChannelEndpointElement channelEndpointElement)  
        {  
            CustomEndpoint customEndpoint = (CustomEndpoint)endpoint;  
            customEndpoint.Property = this.Property;  
        }  
  
        // No validation in this sample  
        protected override void OnInitializeAndValidate(ServiceEndpointElement serviceEndpointElement)  
        {  
        }  
  
        // No validation in this sample  
        protected override void OnInitializeAndValidate(ChannelEndpointElement channelEndpointElement)  
        {  
        }  
    }  
  
```  
  
 <xref:System.ServiceModel.Configuration.StandardEndpointCollectionElement> は、標準エンドポイントの構成の \<`standardEndpoints`\> セクションの下に表示されるコレクションのバッキング型を提供します。次の例は、このクラスを実装する方法を示しています。  
  
```  
public class CustomEndpointCollectionElement : StandardEndpointCollectionElement<CustomEndpoint, CustomEndpointElement>  
    {  
         // ...  
  
    }  
```  
  
 次の例は、拡張セクションで標準エンドポイントを登録する方法を示しています。  
  
```  
<extensions>  
      <standardEndpointExtensions>  
        <add  
          name="customStandardEndpoint"  
          type="CustomEndpointCollectionElement, Example.dll,  
                Version=1.0.0.0, Culture=neutral, PublicKeyToken=ffffffffffffffff"/>  
  
```  
  
## 標準エンドポイントの構成  
 標準エンドポイントは、コードまたは構成で追加できます。標準エンドポイントをコードで追加するには、次の例に示すように、適切な標準エンドポイントの種類をインスタンス化し、それをサービス ホストに追加します。  
  
```csharp  
serviceHost.AddServiceEndpoint(new CustomEndpoint());  
```  
  
 標準エンドポイントを構成で追加するには、\<`endpoint`\> 要素を、\<`service`\> 要素、および \<`standardEndpoints`\> 要素内の必要な構成設定に追加します。次の例は、<xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> を追加する方法を示しています。これは、[!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] に付属する標準エンドポイントのいずれかです。  
  
```xml  
<services>  
  <service>  
    <endpoint isSystemEndpoint=”true” kind=”udpDiscoveryEndpoint” />  
  </service>  
</services>  
<standardEndpoints>    
  <udpDiscoveryEndpoint>  
     <standardEndpoint multicastAddress="soap.udp://239.255.255.250:3702" />   
  </udpDiscoveryEndpoint>  
</ standardEndpoints >  
```  
  
 標準エンドポイントの種類は、\<`endpoint`\> 要素の種類属性を使用して指定します。エンドポイントは、\<`standardEndpoints`\> 要素内で構成されます。前の例では、<xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> エンドポイントが追加され、構成されます。\<`udpDiscoveryEndpoint`\> 要素は、<xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> の <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint.MulticastAddress%2A> プロパティを設定する \<`standardEndpoint`\> を格納します。  
  
## .NET Framework に付属する標準エンドポイント  
 次の表は、[!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] に付属する標準エンドポイントを示しています。  
  
 `Mex Endpoint`  
 サービス メタデータの公開に使用する標準エンドポイント。  
  
 <xref:System.ServiceModel.Discovery.AnnouncementEndpoint>  
 サービスがアナウンス メッセージを送信するために使用する標準エンドポイント。  
  
 <xref:System.ServiceModel.Discovery.DiscoveryEndpoint>  
 サービスが探索メッセージを送信するために使用する標準エンドポイント。  
  
 <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint>  
 UDP マルチキャスト バインディング上での探索操作用に事前構成済みの標準エンドポイント。  
  
 <xref:System.ServiceModel.Discovery.UdpAnnouncementEndpoint>  
 サービスが UDP バインディングでアナウンス メッセージを送信するために使用する標準エンドポイント。  
  
 <xref:System.ServiceModel.Discovery.DynamicEndpoint>  
 実行時にエンドポイント アドレスを動的に検索するために WS\-Discovery を使用する標準エンドポイント。  
  
 <xref:System.ServiceModel.Description.ServiceMetadataEndpoint>  
 メタデータ交換用の標準エンドポイント。  
  
 <xref:System.ServiceModel.Description.WebHttpEndpoint>  
 <xref:System.ServiceModel.Description.WebHttpBehavior> の動作を自動的に追加する <xref:System.ServiceModel.WebHttpBinding> バインディングを持つ標準エンドポイント。  
  
 <xref:System.ServiceModel.Description.WebScriptEndpoint>  
 <xref:System.ServiceModel.Description.WebScriptEnablingBehavior> の動作を自動的に追加する <xref:System.ServiceModel.WebHttpBinding> バインディングを持つ標準エンドポイント。  
  
 <xref:System.ServiceModel.Description.WebServiceEndpoint>  
 <xref:System.ServiceModel.WebHttpBinding> バインディングを持つ標準エンドポイント。  
  
 <xref:System.ServiceModel.Activities.WorkflowControlEndpoint>  
 ワークフロー インスタンスでの制御操作の呼び出しを可能にする標準エンドポイント。  
  
 <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint>  
 ワークフローの作成とブックマークの再開をサポートする標準エンドポイント。