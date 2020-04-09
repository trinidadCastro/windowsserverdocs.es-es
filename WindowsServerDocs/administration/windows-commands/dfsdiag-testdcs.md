---
title: dfsdiag TestDCs
description: Tema de comandos de Windows para dfsdiag TestDCs, que comprueba la configuración de los controladores de dominio en el dominio especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 092ce3710eb6d209f596683bd4ad054dadd11aa3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846327"
---
# <a name="dfsdiag-testdcs"></a>dfsdiag TestDCs

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba la configuración de los controladores de dominio mediante la realización de las siguientes pruebas en cada controlador de dominio del dominio especificado:  
  
-   Comprueba que el servicio de espacio de nombres Sistema de archivos distribuido (DFS) se está ejecutando y que su tipo de inicio está establecido en automático.  
  
-   Comprueba la compatibilidad de las referencias con costo de sitio para NETLOGON y SYSvol.  
  
-   Comprueba la coherencia de la Asociación del sitio por el nombre de host y la dirección IP.

## <a name="syntax"></a>Sintaxis  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
#### <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|/Domain:`<domain_name>`|Dominio que desea comprobar.|  
  
## <a name="remarks"></a>Comentarios  

/Domain es un parámetro opcional. El valor predeterminado es el dominio local al que está unido el host local.  
  
## <a name="examples"></a><a name=BKMK_Examples></a>Example  
Para comprobar la configuración de los controladores de dominio en el dominio Contoso.com, escriba:  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

