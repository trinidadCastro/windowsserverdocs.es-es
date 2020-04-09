---
title: dfsdiag TestReferral
description: Tema de comandos de Windows para dfsdiag TestReferral, que comprueba las referencias Sistema de archivos distribuido (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5c0a75d557d816ac9e19a1e22b3273195b93f53
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846258"
---
# <a name="dfsdiag-testreferral"></a>dfsdiag TestReferral

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
| /DFSpath:<path for getting referrals>|Esta ruta de acceso DFS puede ser una de las siguientes:<p>-   \(\)en blanco: comprueba los dominios de confianza.<br />-   \\\\dominio: referencias del controlador de dominio.<br />-   \\\\dominio\\SYSvol: referencias SYSvol.<br />-   \\\\nombre en\\las referencias NETLOGON: NETLOGON.<br />-   \\\\<Domain or server>\\<Namespace Root>: referencias a la raíz del espacio de nombres.<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>: carpeta DFS \(vincula\) referencias.|
|/Full|Solo se aplica a las referencias de dominio y raíz. comprueba la coherencia de la información de asociación del sitio entre el registro y los servicios de dominio de Active Directory \(AD DS\).|

## <a name="examples"></a><a name=BKMK_Examples></a>Example

```
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace
```

```
dfsdiag /TestReferral /DFSpath:
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)


