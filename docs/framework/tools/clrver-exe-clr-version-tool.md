---
title: "Clrver.exe (CLR バージョン ツール) | Microsoft Docs"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
- CSharp
- C++
- jsharp
helpviewer_keywords:
- Clrver.exe
- CLR Version tool
ms.assetid: cbc2ee86-bdc8-4a65-a8f1-ba23bce3a699
caps.latest.revision: 13
author: mairaw
ms.author: mairaw
manager: wpickett
ms.translationtype: Machine Translation
ms.sourcegitcommit: 14abadaf548e228244a1ff7ca72fa3896ef4eb5d
ms.openlocfilehash: 2a3f1cf4d82aefc171d256dec166f0441e4fd57e
ms.contentlocale: ja-jp
ms.lasthandoff: 06/02/2017

---
# <a name="clrverexe-clr-version-tool"></a>Clrver.exe (CLR バージョン ツール)
CLR バージョン ツール (Clrver.exe) は、コンピューターにインストールされている共通言語ランタイム (CLR: Common Language Runtime) のすべてのバージョンを報告します。  
  
 このツールは、Visual Studio と共に自動的にインストールされます。 このツールを実行するには、開発者コマンド プロンプト (または、Windows 7 の Visual Studio コマンド プロンプト) を使用します。 詳細については、「[コマンド プロンプト](../../../docs/framework/tools/developer-command-prompt-for-vs.md)」を参照してください。  
  
 コマンド プロンプトに次のように入力します。  
  
## <a name="syntax"></a>構文  
  
```  
clrver [option]  
```  
  
## <a name="options"></a>オプション  
  
|オプション|説明|  
|------------|-----------------|  
|`-all`|CLR を使用しているコンピューター上のすべてのプロセスを表示します。|  
|*pid*|指定したプロセス ID (PID) のプロセスで使用されている CLR のバージョンを表示します。|  
|`-?`|このツールのコマンド構文とオプションを表示します。|  
  
## <a name="remarks"></a>コメント  
 オプションを指定せずに Clrver.exe を呼び出した場合、インストールされている CLR のすべてのバージョンが表示されます。 別のユーザーの PID を指定する場合、バージョン情報を取得するには、管理アクセス許可が必要です。  
  
> [!NOTE]
>  Windows Vista 以降では、ユーザー アカウント制御 (UAC: User Account Control) でユーザーの権限が決定されます。 ユーザーが組み込みの Administrators グループのメンバーである場合、そのユーザーには標準ユーザー アクセス トークンおよび管理者アクセス トークンの 2 つのランタイム アクセス トークンが割り当てられています。 既定では、ユーザーは標準ユーザー ロールに所属します。 管理アクセス許可を必要とするコードを実行するには、最初に、ユーザーの権限を標準ユーザーから管理者に昇格させる必要があります。 この操作は、コマンド プロンプトの起動時にコマンド プロンプト アイコンを右クリックし、管理者として実行することを指定して行うことができます。  
  
 SYSTEM、LOCAL SERVICE、および NETWORK SERVICE の各プロセスの CLR バージョンを確認しようとすると、PID が存在しないことを示すメッセージが表示されます。  
  
## <a name="examples"></a>例  
 コンピューターにインストールされている CLR のすべてのバージョンを表示するコマンドを次に示します。  
  
 `clrver`  
  
 プロセス 128 で使用されている CLR のバージョンを表示するコマンドを次に示します。  
  
 `clrver 128`  
  
 すべてのマネージ プロセスとそれらのプロセスで使用されている CLR のバージョンを表示するコマンドを次に示します。  
  
 `Clrver -all`  
  
## <a name="see-also"></a>関連項目  
 [ツール](../../../docs/framework/tools/index.md)   
 [コマンド プロンプト](../../../docs/framework/tools/developer-command-prompt-for-vs.md)

