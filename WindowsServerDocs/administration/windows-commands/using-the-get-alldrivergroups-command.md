---
title: Get-AllDriverGroups
description: Artículo de referencia de Get-AllDriverGroups, que muestra información acerca de todos los grupos de controladores de un servidor.
ms.topic: reference
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bb3c8c79e95bc5617e2eff0c168b54ba039a5c14
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638194"
---
# <a name="get-alldrivergroups"></a>Get-AllDriverGroups

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información acerca de todos los grupos de controladores de un servidor.

## <a name="syntax"></a>Sintaxis
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se utiliza el servidor local.|
|[/Show: {PackageMetaData &#124; filtros &#124; todos}]|Muestra los metadatos de todos los paquetes de controladores del grupo especificado. **PackageMetaData** muestra información acerca de todos los filtros del grupo de controladores. **Filtros** muestra los metadatos de todos los paquetes de controladores y filtros del grupo.|
## <a name="examples"></a>Ejemplos
Para ver información acerca de un archivo de controlador, escriba:
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
## <a name="additional-references"></a>Referencias adicionales
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Usar el comando Get-DriverGroup](using-the-get-drivergroup-command.md)
