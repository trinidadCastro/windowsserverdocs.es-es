---
title: Add-DriverGroupPackage
description: Tema de referencia de Add-DriverGroupPackage, que agrega un paquete de controladores a un grupo de controladores.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cd323ae-9049-448e-a460-6c7d6462d4c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4baf4f16740e65c432cc09ca24270ab479346ac2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721115"
---
# <a name="add-drivergrouppackage"></a>Add-DriverGroupPackage

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Agrega un paquete de controladores a un grupo de controladores.

## <a name="syntax"></a>Sintaxis
```
wdsutil /add-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```
### <a name="parameters"></a>Parámetros

|         Parámetro         |                                                                                                                                               Descripción                                                                                                                                               |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup:<Group Name> |                                                                                                                                 Especifica el nombre del grupo de controladores.                                                                                                                                 |
|   Servidor<Server name>   |                                                                                  Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local.                                                                                  |
|   /DriverPackage:<Name>   |                                                                      Especifica el nombre del paquete de controladores que se va a agregar al grupo. Debe especificar esta opción si el paquete de controladores no se puede identificar de forma única por nombre.                                                                       |
|      PackageId<ID>      | Especifica el identificador de un paquete. Para buscar el identificador del paquete, haga clic en el grupo de controladores en el que se encuentra el paquete (o en el nodo **todos los paquetes** ), haga clic con el botón secundario en el paquete y, a continuación, haga clic en **propiedades**. El identificador del paquete se muestra en la pestaña **General** , por ejemplo: **{DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}**. |

## <a name="examples"></a>Ejemplos
Para agregar un paquete de controladores, escriba uno de los siguientes:
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /DriverPackage:XYZ
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave](command-line-syntax-key.md)
de sintaxis de línea de comandos[con el comando](using-the-add-drivergrouppackages-command.md)
Add-DriverGroupPackages mediante el comando Add-[DriverPackage](using-the-add-driverpackage-command.md)
[mediante el subcomando add-AllDriverPackages](using-the-add-alldriverpackages-subcommand.md)
