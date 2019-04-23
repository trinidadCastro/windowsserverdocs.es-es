---
title: Dfsdiag TestDCs
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62956ae65d2311939ac0db6a4b86950f21dba407
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836606"
---
# <a name="dfsdiag-testdcs"></a>Dfsdiag TestDCs

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba la configuración de controladores de dominio mediante la realización de las siguientes pruebas en cada controlador de dominio en el dominio especificado:  
  
-   comprueba que el sistema de archivos distribuido \(DFS\) Namespace servicio se está ejecutando y que su tipo de inicio esté establecido en automático.  
  
-   Comprueba la compatibilidad de sitio\-calcula el costo de las referencias de NETLOGON y SYSvol.  
  
-   comprueba la coherencia de la asociación del sitio mediante el nombre de host y la dirección IP.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|\/Dominio:<Domain name>|Dominio que desea comprobar.|  
  
## <a name="remarks"></a>Comentarios  
\/Dominio es un parámetro opcional. El valor predeterminado es el dominio local que forme parte de host local.  
  
## <a name="BKMK_Examples"></a>Ejemplos  
Para comprobar la configuración de controladores de dominio en el dominio Contoso.com, escriba:  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

