---
title: "Handles 句 (Visual Basic) |Microsoft ドキュメント"
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
f1_keywords:
- Handles
- vb.Handles
dev_langs:
- VB
helpviewer_keywords:
- Handles keyword
ms.assetid: 1b051c0e-f499-42f6-acb5-6f4f27824b40
caps.latest.revision: 19
author: dotnet-bot
ms.author: dotnetcontent
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: Machine Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 7c79935e7f15f31abca7efddbc443239d5db2f58
ms.contentlocale: ja-jp
ms.lasthandoff: 03/13/2017

---
# <a name="handles-clause-visual-basic"></a>Handles 句 (Visual Basic)
プロシージャが指定されたイベントを処理することを宣言します。  
  
## <a name="syntax"></a>構文  
  
```  
proceduredeclaration Handles eventlist  
```  
  
## <a name="parts"></a>指定項目  
 `proceduredeclaration`  
 イベントを処理するプロシージャの `Sub` プロシージャ宣言。  
  
 `eventlist`  
 コンマで区切られた、`proceduredeclaration` が処理するイベントの一覧。 イベントは、現在のクラスの基底クラス、または `WithEvents` キーワードを使用して宣言されたオブジェクトによって発生する必要があります。  
  
## <a name="remarks"></a>コメント  
 プロシージャ宣言の最後で `Handles` キーワードを使用すると、 `WithEvents` キーワードで宣言されたオブジェクト変数によって発生したイベントが処理されるようになります。 また、`Handles` キーワードを派生クラスで使用すると、基底クラスからのイベントを処理することもできます。  
  
 `Handles` キーワードと `AddHandler` ステートメントはどちらも特定のプロシージャで特定のイベントを処理するように指定できますが、両者には違いがあります。 `Handles` キーワードは、プロシージャの定義時に特定のイベントを処理するよう指定する場合に使用します。 `AddHandler` ステートメントは、実行時にプロシージャをイベントに接続します。 詳細については、次を参照してください。 [AddHandler ステートメント](../../../visual-basic/language-reference/statements/addhandler-statement.md)します。  
  
 カスタム イベントの場合、アプリケーションは、プロシージャをイベント ハンドラーとして追加するときにイベントの `AddHandler` アクセサーを呼び出します。 カスタム イベントの詳細については、次を参照してください。 [Event ステートメント](../../../visual-basic/language-reference/statements/event-statement.md)します。  
  
## <a name="example"></a>例  
 [!code-vb[VbVbalrEvents&#2;](../../../visual-basic/language-reference/statements/codesnippet/VisualBasic/handles-clause_1.vb)]  
  
 派生クラスで `Handles` ステートメントを使用して基底クラスからのイベントを処理する方法を次の例に示します。  
  
 [!code-vb[VbVbalrEvents&#3;](../../../visual-basic/language-reference/statements/codesnippet/VisualBasic/handles-clause_2.vb)]  
  
## <a name="example"></a>例  
 次の例は、2 つのボタンのイベント ハンドラーの**WPF アプリケーション**プロジェクトです。  
  
 [!code-vb[VbVbalrEvents #&41;](../../../visual-basic/language-reference/statements/codesnippet/VisualBasic/handles-clause_3.vb)]  
  
## <a name="example"></a>例  
 次の例は、前の例と同じです。 `Handles` 句の `eventlist` には&2; つのボタンのイベントが含まれています。  
  
 [!code-vb[VbVbalrEvents #&42;](../../../visual-basic/language-reference/statements/codesnippet/VisualBasic/handles-clause_4.vb)]  
  
## <a name="see-also"></a>関連項目  
 [WithEvents](../../../visual-basic/language-reference/modifiers/withevents.md)   
 [AddHandler ステートメント](../../../visual-basic/language-reference/statements/addhandler-statement.md)   
 [RemoveHandler ステートメント](../../../visual-basic/language-reference/statements/removehandler-statement.md)   
 [Event ステートメント](../../../visual-basic/language-reference/statements/event-statement.md)   
 [RaiseEvent ステートメント](../../../visual-basic/language-reference/statements/raiseevent-statement.md)   
 [イベント](../../../visual-basic/programming-guide/language-features/events/index.md)

