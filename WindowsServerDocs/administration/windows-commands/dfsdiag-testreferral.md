---
title: dfsdiag TestReferral
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af22520d2c89f9d9f9d91ea6f43a33f3ff9c57f1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378359"
---
# <a name="dfsdiag-testreferral"></a>dfsdiag TestReferral

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba Sistema de archivos distribuido \(las referencias\) DFS mediante la realización de las pruebas siguientes:  
  
-   Cuando se usa el parámetro DFSpath sin argumentos, este comando valida que la lista de referencias incluye todos los dominios de confianza.  
  
-   Cuando se especifica un dominio, el comando realiza una comprobación de estado de los controladores de dominio \(dfsdiag \/testdcs\) y comprueba las asociaciones de sitio y la caché de dominio del host local.  
  
-   Cuando se especifica un dominio y \\SYSvol o \\NETLOGON, además de realizar las mismas comprobaciones de estado que cuando se especifica un dominio, el comando comprueba que el período de vida \(TTL\) de las referencias de SYSvol o NETLOGON coincide con el valor predeterminado de 900 segundos.  
  
-   Al especificar una raíz de espacio de nombres, además de realizar las mismas comprobaciones de estado que cuando se especifica un dominio, el comando realiza una comprobación de configuración de DFS \(dfsdiag \/TestDFSConfig\) y una comprobación de integridad del espacio de nombres \(dfsdiag \/TestDFSIntegrity\).  
  
-   Cuando se especifica una carpeta DFS \(vínculo\), además de realizar las mismas comprobaciones de estado que cuando se especifica una raíz de espacio de nombres, el comando valida la configuración del sitio para los destinos de carpeta \(dfsdiag \/testsites\) y valida la Asociación de sitio del host local.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|\/DFSpath:<path for getting referrals>|Esta ruta de acceso DFS puede ser una de las siguientes:<br /><br />-   \(\)en blanco: comprueba los dominios de confianza.<br />-   \\\\dominio: referencias del controlador de dominio.<br />-   \\\\dominio\\SYSvol: referencias SYSvol.<br />-   \\dominio \\\\NETLOGON: referencias NETLOGON.<br />-   \\\\<Domain or server>\\<Namespace Root>: referencias a la raíz del espacio de nombres.<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>: carpeta DFS \(vincula\) referencias.|  
|\/completo|Solo se aplica a las referencias de dominio y raíz. comprueba la coherencia de la información de asociación del sitio entre el registro y los servicios de dominio de Active Directory \(AD DS\).|  
  
## <a name="BKMK_Examples"></a>Example  
En TBD, escriba:  
  
```  
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace  
```  
  
En TBD, escriba:  
  
```  
dfsdiag /TestReferral /DFSpath:  
```  
  
## <a name="additional-references"></a>referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

