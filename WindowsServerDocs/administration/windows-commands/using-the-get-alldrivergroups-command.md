---
title: Get-AllDriverGroups
description: Temas de comandos de Windows para Get-AllDriverGroups, que muestra información acerca de todos los grupos de controladores de un servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6112c38ec8e89effe5cdecaa99c6b75becd2e90d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831368"
---
# <a name="get-alldrivergroups"></a>Get-AllDriverGroups

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de todos los grupos de controladores de un servidor.

## <a name="syntax"></a>Sintaxis
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se utiliza el servidor local.|
|[/Show: {PackageMetaData &#124; filters &#124; All}]|Muestra los metadatos de todos los paquetes de controladores del grupo especificado. **PackageMetaData** muestra información acerca de todos los filtros del grupo de controladores. **Filtros** muestra los metadatos de todos los paquetes de controladores y filtros del grupo.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para ver información acerca de un archivo de controlador, escriba:
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
## <a name="additional-references"></a>Referencias adicionales
- 
[de la clave de sintaxis de línea de comandos](command-line-syntax-key.md) [mediante el comando Get-DriverGroup](using-the-get-drivergroup-command.md)
