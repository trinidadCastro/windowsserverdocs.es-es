---
title: dfsdiag TestDCs
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a193e68b6f015b1535a98e20b52deb2a4a14034c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378440"
---
# <a name="dfsdiag-testdcs"></a>dfsdiag TestDCs

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba la configuración de los controladores de dominio mediante la realización de las siguientes pruebas en cada controlador de dominio del dominio especificado:  
  
-   comprueba que el Sistema de archivos distribuido \(DFS\) el servicio de espacio de nombres se está ejecutando y que su tipo de inicio está establecido en automático.  
  
-   Comprueba la compatibilidad del sitio\-referencias costosas para NETLOGON y SYSvol.  
  
-   comprueba la coherencia de la Asociación del sitio por el nombre de host y la dirección IP.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|Dominio de \/:<Domain name>|Dominio que desea comprobar.|  
  
## <a name="remarks"></a>Observaciones  
\/dominio es un parámetro opcional. El valor predeterminado es el dominio local al que está unido el host local.  
  
## <a name="BKMK_Examples"></a>Example  
Para comprobar la configuración de los controladores de dominio en el dominio Contoso.com, escriba:  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

