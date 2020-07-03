---
title: dfsdiag testsites
description: Artículo de referencia para dfsdiag testsites, que comprueba la configuración de los sitios de servicios de dominio de Active Directory (AD DS) comprobando que los servidores que actúan como destinos de carpeta (vínculo) o servidores de espacio de nombres tienen las mismas asociaciones de sitio en todos los controladores de dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7942b1535957366af9485580d75c9eec17120f4d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928689"
---
# <a name="dfsdiag-testsites"></a>dfsdiag testsites

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Comprueba la configuración de los sitios de servicios de dominio de Active Directory (AD DS) comprobando que los servidores que actúan como servidores de espacio de nombres o destinos de carpeta (vínculo) tienen las mismas asociaciones de sitio en todos los controladores de dominio.

## <a name="syntax"></a>Sintaxis

```
dfsdiag /testsites </machine:<server name>| /DFSpath:<namespace root or DFS folder> [/recurse]> [/full]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `/machine:<server name>` | Nombre del servidor en el que se va a comprobar la Asociación del sitio. |
| `/DFSpath:<namespace root or DFS folder>` | La raíz del espacio de nombres o la carpeta del Sistema de archivos distribuido (DFS) con destinos para los que se va a comprobar la Asociación del sitio. |
| /recurse | Enumera y comprueba las asociaciones del sitio para todos los destinos de carpeta en la raíz del espacio de nombres especificada. |
| /Full | Comprueba que AD DS y el registro del servidor contienen la misma información de asociación del sitio. |

## <a name="examples"></a>Ejemplos

Para comprobar las asociaciones de sitio en *machine\MyServer*, escriba:

```
dfsdiag /testsites /machine:MyServer
```

Para comprobar una carpeta de Sistema de archivos distribuido (DFS) para comprobar la Asociación del sitio, junto con la comprobación de que AD DS y el registro del servidor contienen la misma información de asociación del sitio, escriba:

```
dfsdiag /TestSites /DFSpath:\\contoso.com\namespace1\folder1 /full
```

Para comprobar una raíz de espacio de nombres para comprobar la Asociación del sitio, junto con la enumeración y comprobación de las asociaciones del sitio para todos los destinos de carpeta en la raíz del espacio de nombres especificada, y la comprobación de que AD DS y el registro del servidor contienen la misma información de asociación del sitio, escriba:

```
dfsdiag /testsites /DFSpath:\\contoso.com\namespace2 /recurse /full
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando dfsdiag](dfsdiag.md)
