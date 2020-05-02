---
title: dfsdiag TestReferral
description: Tema de referencia de dfsdiag TestReferral, que comprueba las referencias Sistema de archivos distribuido (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b4c616181d367a8a95efe6484f74af0ff88cc5f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719568"
---
# <a name="dfsdiag-testreferral"></a>dfsdiag TestReferral

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Comprueba las referencias de Sistema de archivos distribuido (DFS) mediante la realización de las siguientes pruebas:

- Cuando se usa el parámetro DFSpath sin argumentos, este comando valida que la lista de referencias incluye todos los dominios de confianza.

- Cuando se especifica un dominio, el comando realiza una comprobación de estado de los controladores de dominio (dfsdiag/testdcs) y prueba las asociaciones de sitio y la caché de dominio del host local.

- Cuando se especifica un dominio y \SYSvol o \NETLOGON, además de realizar las mismas comprobaciones de estado que cuando se especifica un dominio, el comando comprueba que el período de vida (\TTL) de las referencias de SYSvol o NETLOGON coincide con el valor predeterminado de 900 segundos.

- Cuando se especifica una raíz de espacio de nombres, además de realizar las mismas comprobaciones de estado que cuando se especifica un dominio, el comando realiza una comprobación de configuración de DFS (dfsdiag/TestDFSConfig) y una comprobación de integridad del espacio de nombres (dfsdiag/TestDFSIntegrity).

- Al especificar una carpeta DFS (vínculo), además de realizar las mismas comprobaciones de estado que cuando se especifica una raíz de espacio de nombres, el comando valida la configuración del sitio para los destinos de la carpeta (dfsdiag/testsites) y valida la Asociación del sitio del host local.

## <a name="syntax"></a>Sintaxis

```
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|-------|--------|
| /DFSpath:<path for getting referrals>|Esta ruta de acceso DFS puede ser una de las siguientes:<p>-   \(en\)blanco: comprueba los dominios de confianza.<br />-   \\\\Dominio: referencias del controlador de dominio.<br />-   \\\\Dominio\\SYSVOL: referencias de SYSVOL.<br />-   \\\\Nombre en\\Netlogon: referencias de netlogon.<br />-   \\\\<Domain or server>\\<Namespace Root>: Referencias a la raíz del espacio de nombres.<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>: Referencias de \(vínculo\) de carpetas DFS.|
|/Full|Solo se aplica a las referencias de dominio y raíz. comprueba la coherencia de la información de asociación del sitio entre el AD DS \(\)del registro y de servicios de dominio de Active Directory.|

## <a name="examples"></a>Ejemplos

```
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace
```

```
dfsdiag /TestReferral /DFSpath:
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)


