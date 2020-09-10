---
title: dfsdiag testreferral
description: Artículo de referencia para el comando dfsdiag testreferral, que comprueba las referencias Sistema de archivos distribuido (DFS).
ms.topic: reference
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e1fb5526f49994ddbfb35c0c64f53933ab67674f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634133"
---
# <a name="dfsdiag-testreferral"></a>dfsdiag testreferral

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Comprueba las referencias de Sistema de archivos distribuido (DFS) mediante la realización de las siguientes pruebas:

- Si usa el parámetro **DFSpath*** sin argumentos, el comando valida que la lista de referencias incluya todos los dominios de confianza.

- Si especifica un dominio, el comando realiza una comprobación de estado de los controladores de dominio ( `dfsdiag /testdcs` ) y prueba las asociaciones de sitio y la caché de dominio del host local.

- Si especifica un dominio y \SYSvol o \NETLOGON, el comando realiza las mismas comprobaciones de estado del controlador de dominio, además de comprobar que el período de vida **(TTL)** de las referencias de SYSVOL o Netlogon coincide con el valor predeterminado de 900 segundos.

- Si especifica una raíz de espacio de nombres, el comando realiza las mismas comprobaciones de estado del controlador de dominio, además de realizar una comprobación de la configuración de DFS ( `dfsdiag /testdfsconfig` ) y una comprobación de integridad del espacio de nombres ( `dfsdiag /testdfsintegrity` ).

- Si especifica una carpeta DFS (vínculo), el comando realiza las mismas comprobaciones de estado de la raíz del espacio de nombres, junto con la validación de la configuración del sitio para los destinos de la carpeta (dfsdiag/testsites) y la validación de la Asociación del sitio del host local.

## <a name="syntax"></a>Sintaxis

```
dfsdiag /testreferral /DFSpath:<DFS path to get referrals> [/full]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /DFSpath:`<path to get referrals>` | Puede ser uno de los siguientes:<ul><li>**En blanco:** Solo comprueba dominios de confianza.</li><li>`\\Domain:` Comprueba solo las referencias del controlador de dominio.</li><li>`\\Domain\SYSvol:` Comprueba solo las referencias de SYSvol.</li><li>`\\Domain\NETLOGON:` Comprueba solo las referencias de NETLOGON.</li><li>`\\<domain or server>\<namespace root>:` Solo comprueba las referencias a la raíz del espacio de nombres.</li><li>`\\<domain or server>\<namespace root>\<DFS folder>:` Comprueba solo las referencias a la carpeta DFS (vínculo).</li></ul> |
| /Full | Solo se aplica a las referencias de dominio y raíz. Comprueba la coherencia de la información de asociación del sitio entre el registro y los servicios de dominio de Active Directory (AD DS). |

## <a name="examples"></a>Ejemplos

Para comprobar las referencias de Sistema de archivos distribuido (DFS) en *contoso. com\MyNamespace*, escriba:

```
dfsdiag /testreferral /DFSpath:\\contoso.com\MyNamespace
```

Para comprobar las referencias de Sistema de archivos distribuido (DFS) en todos los dominios de confianza, escriba:

```
dfsdiag /testreferral /DFSpath:
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando dfsdiag](dfsdiag.md)
