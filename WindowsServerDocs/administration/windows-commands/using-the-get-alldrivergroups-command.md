---
title: Mediante el comando get-AllDriverGroups
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 236a2f798fb07ee6eafb9baf9314dbf46a984cdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874006"
---
# <a name="using-the-get-alldrivergroups-command"></a>Mediante el comando get-AllDriverGroups

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información sobre todos los grupos de controladores en un servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Server:<Server name>]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se usa el servidor local.|
|[/ Mostrar: {PackageMetaData &#124; filtros &#124; todas}]|Muestra los metadatos para todos los paquetes de controladores en el grupo especificado. **PackageMetaData** muestra información acerca de todos los filtros para el grupo de controladores. **Filtros** muestra los metadatos para todos los paquetes de controladores y los filtros para el grupo.|
## <a name="BKMK_examples"></a>Ejemplos
Para ver información acerca de un archivo de controlador, escriba:
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando get-DriverGroup](using-the-get-drivergroup-command.md)
