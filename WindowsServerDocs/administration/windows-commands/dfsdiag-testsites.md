---
title: dfsdiag TestSites
description: Temas de comandos de Windows para dfsdiag TestSites, que comprueba la configuración de los sitios de servicios de dominio de Active Directory (AD DS) comprobando que los servidores que actúan como destinos de carpetas o servidores de espacio de nombres (vínculo) tienen las mismas asociaciones de sitio en todos los controladores de dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80cc9095748dafb030b204130bfa2ccb61ec69ea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846228"
---
# <a name="dfsdiag-testsites"></a>dfsdiag TestSites

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba la configuración de los sitios de servicios de dominio de Active Directory (AD DS) comprobando que los servidores que actúan como servidores de espacio de nombres o destinos de carpeta (vínculo) tienen las mismas asociaciones de sitio en todos los controladores de dominio.

## <a name="syntax"></a>Sintaxis  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
#### <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|\/máquina:<server name>|Nombre del servidor en el que se va a comprobar la Asociación del sitio.|  
|\/DFSpath:<namespace root or DFS folder>|La raíz del espacio de nombres o la carpeta del Sistema de archivos distribuido (DFS) con destinos para los que se va a comprobar la Asociación del sitio.|  
|\/recurse|Enumera y comprueba las asociaciones del sitio para todos los destinos de carpeta en la raíz del espacio de nombres especificada.|  
|\/completo|comprueba que AD DS y el registro del servidor contienen la misma información de asociación del sitio.|  
  
## <a name="examples"></a><a name=BKMK_Examples></a>Example  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
 
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

