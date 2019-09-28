---
title: Usar el comando Get-DriverPackage
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f3d31d9a02454b0f7fca06b28a4df27174f7b02e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363192"
---
# <a name="using-the-get-driverpackage-command"></a>Usar el comando Get-DriverPackage



Muestra información acerca de un paquete de controladores en el servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parámetros

|        Parámetro         |                                                                           Descripción                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server: \<Server nombre >] |              Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local.               |
| [/DriverPackage: \<Name >] |                                                        Especifica el nombre del paquete de controladores que se va a mostrar.                                                         |
|    [/PackageId: \<ID >]    | Especifica el ID. de servicios de implementación de Windows del paquete de controladores que se va a mostrar. Debe especificar el identificador si el paquete de controladores no se puede identificar de forma única por nombre. |
|     [/Show: {drivers     |                                                                              Archivos                                                                               |

## <a name="BKMK_examples"></a>Example

Para ver información acerca de un paquete de controladores, escriba uno de los siguientes:
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)