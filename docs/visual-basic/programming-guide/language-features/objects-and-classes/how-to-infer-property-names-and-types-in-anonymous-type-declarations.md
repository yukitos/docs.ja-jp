---
title: "方法: 匿名型の宣言におけるプロパティ名と型を推論する (Visual Basic) | Microsoft Docs"
ms.custom: ""
ms.date: "2015-07-20"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-visual-basic"
ms.topic: "article"
dev_langs: 
  - "VB"
helpviewer_keywords: 
  - "推測 (プロパティ名を) [Visual Basic]"
  - "プロパティ名と型を推論する匿名型 [Visual Basic]"
  - "推測 (プロパティ型を) [Visual Basic]"
ms.assetid: 7c748b22-913f-4d9d-b747-6b7bf296a0bc
caps.latest.revision: 19
author: "stevehoag"
ms.author: "shoag"
caps.handback.revision: 19
---
# 方法: 匿名型の宣言におけるプロパティ名と型を推論する (Visual Basic)
[!INCLUDE[vs2017banner](../../../../visual-basic/developing-apps/includes/vs2017banner.md)]

匿名型には、プロパティのデータ型を直接指定する機構はありません。 すべてのプロパティの型は、推論されます。 次の例では、`Name` と `Price` の型は、それらを初期化するために使われる値から、直接推論されます。  
  
 [!code-vb[VbVbalrAnonymousTypes#1](../../../../visual-basic/language-reference/modifiers/codesnippet/VisualBasic/how-to-infer-property-names-and-types-in-anonymous-type-declarations_1.vb)]  
  
 匿名型は、プロパティの名前と型を、他のソースから推論することもできます。 この後のセクションで、推論が可能な状況の一覧と、推論が行われない状況の例を示します。  
  
## 正常な推論  
  
#### 匿名型は、プロパティの名前と型を、次のソースから推論できます:  
  
-   変数名から。 匿名型 `anonProduct` には、`productName` と `productPrice` という 2 つのプロパティがあります。 それらのプロパティのデータ型は、それぞれ、元の変数のデータ型である `String` と `Double` になります。  
  
     [!code-vb[VbVbalrAnonymousTypes#11](../../../../visual-basic/language-reference/modifiers/codesnippet/VisualBasic/how-to-infer-property-names-and-types-in-anonymous-type-declarations_2.vb)]  
  
-   他のオブジェクトのプロパティまたはフィールドの名前から。 たとえば、`Name` プロパティと `ID` プロパティを含む `CarClass` 型の `car` オブジェクトがあるとします。 新しい匿名型のインスタンス `car1` を作成し、`car` オブジェクトの値を使用して `Name` プロパティと `ID` プロパティを初期化するには、次のように記述できます。  
  
     [!code-vb[VbVbalrAnonymousTypes#34](../../../../visual-basic/language-reference/modifiers/codesnippet/VisualBasic/how-to-infer-property-names-and-types-in-anonymous-type-declarations_3.vb)]  
  
     前の宣言は、匿名型 `car2` を定義する、次の長いコードに相当します。  
  
     [!code-vb[VbVbalrAnonymousTypes#35](../../../../visual-basic/language-reference/modifiers/codesnippet/VisualBasic/how-to-infer-property-names-and-types-in-anonymous-type-declarations_4.vb)]  
  
-   XML メンバー名から。  
  
     [!code-vb[VbVbalrAnonymousTypes#12](../../../../visual-basic/language-reference/modifiers/codesnippet/VisualBasic/how-to-infer-property-names-and-types-in-anonymous-type-declarations_5.vb)]  
  
     推論後の `anon` の型は、<xref:System.Collections.IEnumerable> \(Of XElement\) 型の 1 つのプロパティ `Book` を持ちます。  
  
-   次の例に示す `SomeFunction` など、パラメーターを持たない関数から。  
  
     `Dim sc As New SomeClass`  
  
     `Dim anon1 = New With {Key sc.SomeFunction()}`  
  
     次のコードの `anon2` 変数は、`First` という名前の 1 文字を表すプロパティを 1 つ持つ匿名型です。 このコードでは、文字 "E" が表示されます。これは関数 <xref:System.Linq.Enumerable.First%2A> から返される文字です。  
  
     [!code-vb[VbVbalrAnonymousTypes#13](../../../../visual-basic/language-reference/modifiers/codesnippet/VisualBasic/how-to-infer-property-names-and-types-in-anonymous-type-declarations_6.vb)]  
  
## 推論の失敗  
  
#### 名前の推論は、次の状況を含む、さまざまな状況で失敗します:  
  
-   推論が、引数を必要とするメソッド、コンストラクター、またはパラメーター化されたプロパティの呼び出しから派生する場合。 前の `anon1` の宣言は、`someFunction` に 1 つ以上の引数があると失敗します。  
  
     `' Not valid.`  
  
     `' Dim anon3 = New With {Key sc.someFunction(someArg)}`  
  
     この問題は、新しいプロパティ名を割り当てることによって解決できます。  
  
     `' Valid.`  
  
     `Dim anon4 = New With {Key .FunResult = sc.someFunction(someArg)}`  
  
-   推論が、複合式から派生する場合。  
  
    ```  
    Dim aString As String = "Act "  
    ' Not valid.  
    ' Dim label = New With {Key aString & "IV"}  
    ```  
  
     このエラーは、式の結果をプロパティ名に割り当てれば解決できます。  
  
     [!code-vb[VbVbalrAnonymousTypes#14](../../../../visual-basic/language-reference/modifiers/codesnippet/VisualBasic/how-to-infer-property-names-and-types-in-anonymous-type-declarations_7.vb)]  
  
-   複数のプロパティの推論で、同じ名前を持つ 2 つ以上のプロパティが生成される場合。 前の例で示した宣言で説明すると、同じ匿名型のプロパティとして`product.Name` と `car1.Name` の両方を指定することはできません。 これは、各プロパティの推論された識別子がどちらも `Name` になるためです。  
  
     `' Not valid.`  
  
     `' Dim anon5 = New With {Key product.Name, Key car1.Name}`  
  
     この問題は、別個のプロパティ名に値を割り当てることで解決できます。  
  
     [!code-vb[VbVbalrAnonymousTypes#36](../../../../visual-basic/language-reference/modifiers/codesnippet/VisualBasic/how-to-infer-property-names-and-types-in-anonymous-type-declarations_8.vb)]  
  
     大文字と小文字の違いでは、別個の名前にならないことにご注意ください。  
  
     `Dim price = 0`  
  
     `' Not valid, because Price and price are the same name.`  
  
     `' Dim anon7 = New With {Key product.Price, Key price}`  
  
-   1 つのプロパティの初期の型と値が、まだ確立されていない別のプロパティに依存している場合。 たとえば、`.IDName = .LastName` は、`.LastName` が既に初期化されていない限り、匿名型の宣言では使えません。  
  
     `' Not valid.`  
  
     `' Dim anon8 = New With {Key .IDName = .LastName, Key .LastName = "Jones"}`  
  
     この例では、プロパティを宣言する順序を逆にすることで、問題を修正できます。  
  
     [!code-vb[VbVbalrAnonymousTypes#15](../../../../visual-basic/language-reference/modifiers/codesnippet/VisualBasic/how-to-infer-property-names-and-types-in-anonymous-type-declarations_9.vb)]  
  
-   匿名型のプロパティの名前が、<xref:System.Object> のメンバーの名前と同じである場合。 たとえば、次の宣言は、`Equals` が <xref:System.Object> のメソッドなので失敗します。  
  
     `' Not valid.`  
  
     `' Dim relationsLabels1 = New With {Key .Equals = "equals", Key .Greater = _`  
  
     `'                       "greater than", Key .Less = "less than"}`  
  
     この問題は、プロパティ名を変更することで修正できます。  
  
     [!code-vb[VbVbalrAnonymousTypes#16](../../../../visual-basic/language-reference/modifiers/codesnippet/VisualBasic/how-to-infer-property-names-and-types-in-anonymous-type-declarations_10.vb)]  
  
## 参照  
 [Object Initializers: Named and Anonymous Types](../../../../visual-basic/programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)   
 [Local Type Inference](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)   
 [Anonymous Types](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)   
 [Key](../../../../visual-basic/language-reference/modifiers/key.md)