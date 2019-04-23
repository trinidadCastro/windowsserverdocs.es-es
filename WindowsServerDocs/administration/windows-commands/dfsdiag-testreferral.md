---
title: Dfsdiag TestReferral
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cd1b87befa8a9cfda5ea27a4ce5a5105ea1a1009
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848026"
---
# <a name="dfsdiag-testreferral"></a>Dfsdiag TestReferral

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprueba el sistema de archivos distribuido \(DFS\) referencias mediante la realización de las siguientes pruebas:  
  
-   Cuando se usa el parámetro DFSpath sin argumentos, este comando valida que la lista de referencia incluye los dominios de confianza.  
  
-   Cuando se especifica un dominio, el comando realiza una comprobación de estado de los controladores de dominio \(dfsdiag \/testdcs\) y comprueba las asociaciones de sitio y la memoria caché del host local de dominio.  
  
-   Cuando se especifica un dominio y \\SYSvol o \\NETLOGON, además de realizar el mismo estado comprueba cuando se especifica un dominio, el comando comprueba que el tiempo de vida \(TTL\) de referencias de SYSvol o NETLOGON coincide con el valor predeterminado de 900 segundos.  
  
-   Cuando se especifica un espacio de nombres root, además de realizar el mismo estado comprueba cuando se especifica un dominio, el comando realiza una comprobación de la configuración de DFS \(dfsdiag \/TestDFSConfig\) y una comprobación de integridad del espacio de nombres \(dfsdiag \/TestDFSIntegrity\).  
  
-   Al especificar una carpeta DFS \(vínculo\), además de realizar las mismo comprobaciones de estado como cuando se especifica un espacio de nombres root, el comando valida la configuración del sitio para destinos de carpeta \(dfsdiag \/ testsites\) y valida la asociación del sitio del host local.  
  
  
  
## <a name="syntax"></a>Sintaxis  
  
```  
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]  
```  
  
### <a name="parameters"></a>Parámetros  
  
|Parámetro|Descripción|  
|-------|--------|  
|\/DFSpath:<path for getting referrals>|Esta ruta de acceso DFS puede ser uno de los siguientes:<br /><br />-   \(en blanco\): Dominios de confianza de las pruebas.<br />-   \\\\Dominio: Referencias de controlador de dominio.<br />-   \\\\Domain\\SYSvol: Referencias de SYSvol.<br />-   \\\\Dominio\\NETLOGON: Referencias de NETLOGON.<br />-   \\\\<Domain or server>\\<Namespace Root>: Referencias a la raíz de Namespace.<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>: Carpeta DFS \(vínculo\) las referencias.|  
|\/completo|Se aplica solo a las referencias de raíz y el dominio. comprueba la coherencia de la información de asociación de sitio entre el registro y los servicios de dominio de Active Directory \(AD DS\).|  
  
## <a name="BKMK_Examples"></a>Ejemplos  
Para TBD, escriba:  
  
```  
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace  
```  
  
Para TBD, escriba:  
  
```  
dfsdiag /TestReferral /DFSpath:  
```  
  
## <a name="additional-references"></a>Referencias adicionales  
  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)  
  

