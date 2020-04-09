---
title: dfsdiag TestDFSConfig
description: Tema de comandos de Windows para dfsdiag TestDFSConfig, que comprueba la configuración de un espacio de nombres Sistema de archivos distribuido (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ffb75ba26b4ed90dbf5c8bfda80f4a81f986e46a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846338"
---
# <a name="dfsdiag-testdfsconfig"></a>dfsdiag TestDFSConfig

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba la configuración de un espacio de nombres de Sistema de archivos distribuido (DFS) mediante la realización de las siguientes acciones:  
  
-   Comprueba que el servicio de espacio de nombres DFS se está ejecutando y que su tipo de inicio está establecido en automático en todos los servidores de espacio de nombres.  
  
-   Comprueba que la configuración del registro DFS es coherente entre los servidores de espacio de nombres.  
  
-   Valida las siguientes dependencias en servidores de espacio de nombres en clúster que ejecutan Windows Server 2008 o posterior:  
  
    -   Dependencia de recursos raíz de espacio de nombres en el recurso de nombre de red.  
  
    -   Dependencia de recurso de nombre de red en el recurso de dirección IP.  
  
    -   Dependencia de recursos raíz del espacio de nombres en el recurso de disco físico.

## <a name="syntax"></a>Sintaxis  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:<namespace>  
```  
  
#### <a name="parameters"></a>Parámetros  
  
|       Parámetro       |               Descripción               |
|-----------------------|-----------------------------------------|
| /DFSRoot:`<namespace>` | Espacio de nombres (raíz DFS) que se va a diagnosticar. |
  
## <a name="examples"></a><a name=BKMK_Examples></a>Example  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

