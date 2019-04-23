---
title: Dfsdiag TestDFSIntegrity
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a79e034f7c60be89266eb29dcd69e8f73b2aafe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837096"
---
# <a name="dfsdiag-testdfsintegrity"></a>Dfsdiag TestDFSIntegrity

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba la integridad del sistema de archivos distribuido \(DFS\) espacio de nombres mediante la realización de las siguientes pruebas:  
  
-   Comprueba si hay daños en los metadatos DFS o incoherencias entre controladores de dominio.  
  
-   Valida la configuración de acceso\-según la enumeración para asegurarse de que es coherente entre los metadatos DFS y recurso compartido del servidor de espacio de nombres.  
  
-   Detecta que se superponen carpetas DFS \(vínculos\), carpetas duplicadas y carpetas con destinos de carpeta de superposición.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|\/DFSRoot:<DFS root path>|El espacio de nombres DFS para diagnosticar.|  
|\/Recurse|Realiza la prueba, incluidos que el espacio de nombres intervínculos.|  
|\/completo|comprueba la coherencia del recurso compartido y la configuración de lado las ACL de NTFS y el cliente en todos los destinos de carpeta. También comprueba que se establece la propiedad en línea.|  
  
## <a name="BKMK_Examples"></a>Ejemplos  
Para TBD, escriba:  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

