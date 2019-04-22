---
title: Subcomando conjunto DriverPackage
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 11804bb6-ca29-4461-8c63-5131748cd742
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0513d3de2416f93f69b1e1cc286c38b12be94d15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818826"
---
# <a name="subcommand-set-driverpackage"></a>Subcomando: set-DriverPackage



Cambia el nombre o habilita o deshabilita un paquete de controladores en un servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Set-DriverPackage [/Server:<Server name>] {/DriverPackage:<Name> | /PackageId:<ID>} [/Name:<New Name>] [/Enabled:{Yes | No}
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[/ Server:\<nombre del servidor >]|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se usa el servidor local.|
|[/DriverPackage:\<Name>]|Especifica el nombre actual del paquete de controladores para modificar.|
|[/ PackageId:\<Id. >]|Especifica el identificador de servicios de implementación de Windows del paquete de controladores. Debe especificar esta opción si el paquete de controladores no se identifica por nombre. Para encontrar este identificador de un paquete, haga clic en el grupo de controladores que se encuentra el paquete (o el **todos los paquetes** nodo), haga clic en el paquete y, a continuación, haga clic en **propiedades**. El identificador del paquete se muestra en el **General** ficha. Por ejemplo: {DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}.|
|[/ Name:\<nuevo nombre >]|Especifica el nuevo nombre para el paquete de controladores.|
|[/Enabled:{Yes | No}|Habilita o deshabilita el paquete.|

## <a name="BKMK_examples"></a>Ejemplos

Para cambiar la configuración de un paquete, escriba uno de los siguientes:
```
WDSUTIL /Set-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318} /Name:MyDriverPackage
```
```
WDSUTIL /Set-DriverPackage /DriverPackage:MyDriverPackage /Name:NewName /Enabled:Yes
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)