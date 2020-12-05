---
title: WDSUtil Get-alldrivergroups
description: Artículo de referencia del comando WDSUtil Get-alldrivergroups, que muestra información acerca de todos los grupos de controladores de un servidor.
ms.topic: reference
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b6d407e91af9132d6d2bc87ccb86320e3fe42b76
ms.sourcegitcommit: 28b5ab74cb0b40539ccc1a83998d6391e87fe51f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2020
ms.locfileid: "96614947"
---
# <a name="wdsutil-get-alldrivergroups"></a>WDSUtil Get-alldrivergroups

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información acerca de todos los grupos de controladores de un servidor.

## <a name="syntax"></a>Sintaxis

```
wdsutil /get-alldrivergroups [/server:<servername>] [/show:{packagemetadata | filters | all}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `[/server:<servername>]` | Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se utiliza el servidor local. |
| `/show:{packagemetadata | filters | all}]` | Muestra los metadatos de todos los paquetes de controladores del grupo especificado. **PackageMetaData** muestra información acerca de todos los filtros del grupo de controladores. **Filtros** muestra los metadatos de todos los paquetes de controladores y filtros del grupo. |

## <a name="examples"></a>Ejemplos

Para ver información acerca de un archivo de controlador, escriba:

```
wdsutil /get-alldrivergroups /server:MyWdsServer /show:All
```

```
wdsutil /get-alldrivergroups [/show:packagemetadata]
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-
- [WDSUtil Get-drivergroup (comando)](wdsutil-get-drivergroup.md)
