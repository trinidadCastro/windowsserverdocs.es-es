---
title: Dfsdiag TestSites
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6486c79cfa58bb262fd3161ad0801e84185b6629
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873976"
---
# <a name="dfsdiag-testsites"></a>Dfsdiag TestSites

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba la configuración de servicios de dominio de Active Directory \(AD DS\) sitios mediante la comprobación de que los servidores que actúan como servidores de espacio de nombres o la carpeta \(vínculo\) destinos tienen las mismas asociaciones de sitio en todos los dominios controladores.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|\/Equipo:<server name>|El nombre del servidor en el que se va a comprobar la asociación del sitio.|  
|\/DFSpath:<namespace root or DFS folder>|El espacio de nombres raíz o el sistema de archivos distribuido \(DFS\) carpeta \(vínculo\) con destinos para el que se va a comprobar la asociación del sitio.|  
|\/Recurse|Enumera y comprueba las asociaciones de sitio para todos los destinos de carpeta bajo la raíz del espacio de nombres especificado.|  
|\/completo|comprueba que AD DS y el registro del servidor contienen la misma información de asociación del sitio.|  
  
## <a name="BKMK_Examples"></a>Ejemplos  
Para TBD, escriba:  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
  
Para TBD, escriba:  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
Para TBD, escriba:  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

