---
title: Get-DriverGroup
description: Temas de comandos de Windows para Get-DriverGroup, que muestra información acerca de los grupos de controladores en un servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cfe10c3-a63f-48e7-bef9-f6b474b4ddbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60a0649e98a0af0cbbd12db9a07f97dade66344c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831098"
---
# <a name="get-drivergroup"></a>Get-DriverGroup

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de los grupos de controladores en un servidor.

## <a name="syntax"></a>Sintaxis
```
wdsutil /Get-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/DriverGroup:<Group Name>|Especifica el nombre del grupo de controladores.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN.  Si no se especifica un nombre de servidor, se utiliza el servidor local.|
|[/Show: {PackageMetaData &#124; filters &#124; All}]|Muestra los metadatos de todos los paquetes de controladores del grupo especificado. **PackageMetaData** muestra información acerca de todos los filtros del grupo de controladores. **Filtros** muestra los metadatos de todos los paquetes de controladores y filtros del grupo.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para ver información acerca de un archivo de controlador, escriba:
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Show:PackageMetaData
```
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Server:MyWdsServer /Show:Filters
```
## <a name="additional-references"></a>Referencias adicionales
- 
[de la clave de sintaxis de línea de comandos](command-line-syntax-key.md) [mediante el comando Get-AllDriverGroups](using-the-get-alldrivergroups-command.md)
