---
title: Con el comando add-DriverGroupPackage
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cd323ae-9049-448e-a460-6c7d6462d4c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b32d72a1317683e4c1bbeb2d6101d1315b69148e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862646"
---
# <a name="using-the-add-drivergrouppackage-command"></a>Con el comando add-DriverGroupPackage

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Agrega un paquete de controladores a un grupo de controladores.
## <a name="syntax"></a>Sintaxis
```
wdsutil /add-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/DriverGroup:<Group Name>|Especifica el nombre del grupo de controladores.|
|/ Server:<Server name>|Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se usa el servidor local.|
|/DriverPackage:<Name>|Especifica el nombre del paquete de controladores para agregarse al grupo. Debe especificar esta opción si el paquete de controladores no se identifica por nombre.|
|/PackageId:<ID>|Especifica el identificador de un paquete. Para buscar el identificador del paquete, haga clic en el grupo de controladores que se encuentra el paquete (o el **todos los paquetes** nodo), haga clic en el paquete y, a continuación, haga clic en **propiedades**. El identificador del paquete se muestra en el **General** pestaña, por ejemplo: **{DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}**.|
## <a name="BKMK_examples"></a>Ejemplos
Para agregar un paquete de controladores, escriba uno de los siguientes:
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /DriverPackage:XYZ
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando add-DriverGroupPackages](using-the-add-drivergrouppackages-command.md)
[con el comando add-DriverPackage](using-the-add-driverpackage-command.md) 
 [Mediante el subcomando add AllDriverPackages](using-the-add-alldriverpackages-subcommand.md)
