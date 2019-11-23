---
title: dfsdiag TestSites
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af72da64dd20d4b37824355a494cb8f97f597b28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378380"
---
# <a name="dfsdiag-testsites"></a>dfsdiag TestSites

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba la configuración de los servicios de dominio de Active Directory \(sitios de AD DS\) comprobando que los servidores que actúan como servidores de espacio de nombres o \(de vínculo\) destinos tienen las mismas asociaciones de sitio en todos los controladores de dominio.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|\/máquina:<server name>|Nombre del servidor en el que se va a comprobar la Asociación del sitio.|  
|\/DFSpath:<namespace root or DFS folder>|La raíz del espacio de nombres o el Sistema de archivos distribuido \(carpeta DFS\) \(vínculo\) con destinos para los que se va a comprobar la Asociación del sitio.|  
|\/recurse|Enumera y comprueba las asociaciones del sitio para todos los destinos de carpeta en la raíz del espacio de nombres especificada.|  
|\/completo|comprueba que AD DS y el registro del servidor contienen la misma información de asociación del sitio.|  
  
## <a name="BKMK_Examples"></a>Example  
En TBD, escriba:  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
  
En TBD, escriba:  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
En TBD, escriba:  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

