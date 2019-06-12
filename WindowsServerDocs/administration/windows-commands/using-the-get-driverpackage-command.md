---
title: Mediante el comando get-DriverPackage
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0f123d281625140b3c4ba46316cb9b773bf5fee
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440513"
---
# <a name="using-the-get-driverpackage-command"></a>Mediante el comando get-DriverPackage



Muestra información sobre un paquete de controladores en el servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parámetros

|        Parámetro         |                                                                           Descripción                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/ Server:\<nombre del servidor >] |              Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se usa el servidor local.               |
| [/DriverPackage:\<Name>] |                                                        Especifica el nombre del paquete de controladores para mostrar.                                                         |
|    [/ PackageId:\<Id. >]    | Especifica el identificador de servicios de implementación de Windows del paquete de controladores para mostrar. Debe especificar el Id. Si el paquete de controladores no se identifica por nombre. |
|     [/ Mostrar: {controladores     |                                                                              Archivos                                                                               |

## <a name="BKMK_examples"></a>Ejemplos

Para ver información acerca de un paquete de controladores, escriba uno de los siguientes:
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)