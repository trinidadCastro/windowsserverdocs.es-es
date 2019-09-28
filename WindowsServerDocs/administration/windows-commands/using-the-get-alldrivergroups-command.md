---
title: Usar el comando Get-AllDriverGroups
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bed6c784b2fafa30f2beb0394b64fe570ddd8ff7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363380"
---
# <a name="using-the-get-alldrivergroups-command"></a>Usar el comando Get-AllDriverGroups

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de todos los grupos de controladores de un servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se utiliza el servidor local.|
|[/Show: {PackageMetaData &#124; filters &#124; All}]|Muestra los metadatos de todos los paquetes de controladores del grupo especificado. **PackageMetaData** muestra información acerca de todos los filtros del grupo de controladores. **Filtros** muestra los metadatos de todos los paquetes de controladores y filtros del grupo.|
## <a name="BKMK_examples"></a>Example
Para ver información acerca de un archivo de controlador, escriba:
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando Get-DriverGroup](using-the-get-drivergroup-command.md)
