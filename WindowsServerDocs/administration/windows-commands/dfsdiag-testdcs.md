---
title: dfsdiag TestDCs
description: Tema de referencia de dfsdiag TestDCs, que comprueba la configuración de los controladores de dominio en el dominio especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ac7fe1a7bae6a7b3dab9004b6212b7d93774ade
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719595"
---
# <a name="dfsdiag-testdcs"></a>dfsdiag TestDCs

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

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
|Dominio`<domain_name>`|Dominio que desea comprobar.|  
  
## <a name="remarks"></a>Observaciones  

/Domain es un parámetro opcional. El valor predeterminado es el dominio local al que está unido el host local.  
  
## <a name="examples"></a>Ejemplos  
Para comprobar la configuración de los controladores de dominio en el dominio Contoso.com, escriba:  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

