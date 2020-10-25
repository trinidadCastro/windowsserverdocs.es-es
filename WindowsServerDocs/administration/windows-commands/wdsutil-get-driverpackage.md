---
title: Get-DriverPackage
description: Artículo de referencia de Get-DriverPackage, que muestra información acerca de un paquete de controladores en el servidor.
ms.topic: reference
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 86f8b2cdc7a0bc62f42c02c9ff7c6852e9b159f0
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524440"
---
# <a name="get-driverpackage"></a>Get-DriverPackage

Muestra información acerca de un paquete de controladores en el servidor.

## <a name="syntax"></a>Sintaxis

```
wdsutil /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>Parámetros

|        Parámetro         |                                                                           Descripción                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:\<Server name>] |              Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local.               |
| [/DriverPackage: \<Name> ] |                                                        Especifica el nombre del paquete de controladores que se va a mostrar.                                                         |
|    [/PackageId: \<ID> ]    | Especifica el ID. de servicios de implementación de Windows del paquete de controladores que se va a mostrar. Debe especificar el identificador si el paquete de controladores no se puede identificar de forma única por nombre. |
|     [/Show: {drivers     |                                                                              Archivos                                                                               |

## <a name="examples"></a>Ejemplos

Para ver información acerca de un paquete de controladores, escriba uno de los siguientes:
```
wdsutil /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
wdsutil /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)