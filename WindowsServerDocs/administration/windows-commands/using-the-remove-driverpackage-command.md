---
title: Con el comando remove-DriverPackage
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b201e91-0d44-4e4a-8252-8b0235df1002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 217ff23b8724464670520d0b2d5b196df5a4af47
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440300"
---
# <a name="using-the-remove-driverpackage-command"></a>Con el comando remove-DriverPackage

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> 
> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quita un paquete de controladores en un servidor.
## <a name="syntax"></a>Sintaxis
```
wdsutil /remove-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Parámetros

|        Parámetro        |                                                                            Descripción                                                                             |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:<Server name>] |              Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se usa el servidor local.              |
| [/DriverPackage:<Name>] |                                                        Especifica el nombre del paquete de controladores para quitar.                                                         |
|    [/PackageId:<ID>]    | Especifica el identificador de servicios de implementación de Windows del paquete de controladores para quitar. Debe especificar el Id. Si el paquete de controladores no se identifica por nombre. |

## <a name="BKMK_examples"></a>Ejemplos
Para ver información sobre las imágenes, escriba uno de los siguientes:
```
wdsutil /remove-DriverPackage /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /remove-DriverPackage /Server:MyWdsServer /DriverPackage:MyDriverPackage
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando remove-DriverPackages](using-the-remove-driverpackages-command.md)
