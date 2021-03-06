---
title: "-target:exe (C# コンパイラ オプション) | Microsoft Docs"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
f1_keywords:
- /exe
dev_langs:
- CSharp
helpviewer_keywords:
- target compiler options [C#], /target:exe
- /target compiler options [C#], /target:exe
- -target compiler options [C#], /target:exe
ms.assetid: bda5717d-1b91-4848-956b-fcf85c30e432
caps.latest.revision: 12
author: BillWagner
ms.author: wiwagn
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
ms.translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 9d5784186f564241c896333d518e297c3c094f28
ms.contentlocale: ja-jp
ms.lasthandoff: 03/13/2017

---
# <a name="targetexe-c-compiler-options"></a>/target:exe (C# コンパイラ オプション)
**/target:exe** オプションを指定した場合、コンパイラは実行可能な (EXE) コンソール アプリケーションを作成します。  
  
## <a name="syntax"></a>構文  
  
```console  
/target:exe  
```  
  
## <a name="remarks"></a>コメント  
 **/target:exe** オプションは既定で有効になっています。 .exe 拡張子の実行可能ファイルが作成されます。  
  
 Windows プログラムの実行可能ファイルを作成する場合は、[/target:winexe](../../../csharp/language-reference/compiler-options/target-winexe-compiler-option.md) を使用します。  
  
 [/out](../../../csharp/language-reference/compiler-options/out-compiler-option.md) オプションで特に指定しない限り、出力ファイル名は [Main](../../../csharp/programming-guide/main-and-command-args/index.md) メソッドを含む入力ファイルと同じになります。  
  
 コマンド ラインで指定すると、次の **/out** または **/target:module** オプションまでのすべてのファイルが .exe ファイルを作成するために使用されます。  
  
 **Main** メソッドは、.exe ファイルにコンパイルされるソース コード ファイル内で 1 つだけ必要になります。 コードに **Main** メソッドを含むクラスが複数ある場合は、[/main](../../../csharp/language-reference/compiler-options/main-compiler-option.md) コンパイラ オプションを使用して、**Main** メソッドを含めるクラスを指定できます。  
  
### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>Visual Studio 開発環境でこのコンパイラ オプションを設定するには  
  
1.  プロジェクトの **[プロパティ]** ページを開きます。  
  
2.  **[アプリケーション]** プロパティ ページをクリックします。  
  
3.  **[出力の種類]** プロパティを変更します。  
  
 このコンパイラ オプションをプログラムで設定する方法については、「<xref:VSLangProj80.ProjectProperties3.OutputType%2A>」を参照してください。  
  
## <a name="example"></a>例  
 次のコマンド ラインのいずれかで `in.cs` がコンパイルされ、`in.exe` が作成されます。  
  
```console  
csc /target:exe in.cs  
csc in.cs  
```  
  
## <a name="see-also"></a>関連項目  
 [/target (C# コンパイラ オプション)](../../../csharp/language-reference/compiler-options/target-compiler-option.md)   
 [C# コンパイラ オプション](../../../csharp/language-reference/compiler-options/index.md)
