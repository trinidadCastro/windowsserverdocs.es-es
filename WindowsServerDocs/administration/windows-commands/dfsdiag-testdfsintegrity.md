---
title: dfsdiag TestDFSIntegrity
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7f344e2d1fecc542efc39688f20165fd3e39a04a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378429"
---
# <a name="dfsdiag-testdfsintegrity"></a>dfsdiag TestDFSIntegrity

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba la integridad del espacio de nombres Sistema de archivos distribuido \(DFS @ no__t-1 mediante la realización de las pruebas siguientes:  
  
-   Comprueba si hay incoherencias o daños en los metadatos DFS entre controladores de dominio.  
  
-   Valida la configuración de la enumeración Access @ no__t-0based para asegurarse de que es coherente entre los metadatos DFS y el recurso compartido de servidor de espacio de nombres.  
  
-   Detecta carpetas DFS superpuestas \(links @ no__t-1, carpetas duplicadas y carpetas con destinos de carpeta superpuestos.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|\/DFSRoot: <DFS root path>|Espacio de nombres DFS que se va a diagnosticar.|  
|@no__t 0Recurse|Realiza las pruebas, incluyendo los intervínculos de espacios de nombres.|  
|@no__t 0Full|comprueba la coherencia de las ACL de recursos compartidos y NTFS y la configuración del lado cliente en todos los destinos de carpeta. También comprueba que la propiedad online esté establecida.|  
  
## <a name="BKMK_Examples"></a>Example  
En TBD, escriba:  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

