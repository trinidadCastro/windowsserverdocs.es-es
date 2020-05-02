---
title: tipo FTP
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e96dcd4-08f8-4e7b-90b7-1e1761fea4c7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5531da30118914599ed0f85bfd10bd02ae89ffcf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725087"
---
# <a name="ftp-type"></a>FTP: tipo

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Establece o muestra el tipo de transferencia de archivos.   
## <a name="syntax"></a>Sintaxis  
```  
type [<typeName>]  
```  
#### <a name="parameters"></a>Parámetros  

|  Parámetro   |            Descripción            |
|--------------|-----------------------------------|
| [<typeName>] | Especifica el tipo de transferencia de archivo. |

## <a name="remarks"></a>Observaciones  
- Si no se especifica *TypeName* , se muestra el tipo actual.  
- **FTP** admite dos tipos de transferencia de archivos: ASCII y binario.  
  El tipo de transferencia de archivo predeterminado es ASCII.  El comando **ASCII** se debe utilizar al transferir archivos de texto. En el modo ASCII, se realizan las conversiones de caracteres a y desde el juego de caracteres estándar de red. Por ejemplo, los caracteres de fin de línea se convierten según sea necesario, en función del sistema operativo en el destino.  
  El comando **binario** debe usarse al transferir archivos ejecutables. En el modo binario, el archivo se mueve en unidades de un byte.  
  ## <a name="examples"></a>Ejemplos  
  Establezca el tipo de transferencia de archivo en ASCII.  
  ```  
  type ascii  
  ```  
  Establezca el tipo de archivo de transferencia en binario.  
  ```  
  type binary  
  ```  
  ## <a name="additional-references"></a>Referencias adicionales  
- - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
