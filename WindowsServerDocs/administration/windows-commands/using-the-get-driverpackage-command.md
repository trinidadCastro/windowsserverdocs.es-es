---
title: Get-DriverPackage
description: Temas de comandos de Windows para Get-DriverPackage, que muestra información acerca de un paquete de controladores en el servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1906a109d22b24b5a44227d56c726996e6532bd6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831058"
---
# <a name="get-driverpackage"></a>Get-DriverPackage

Muestra información acerca de un paquete de controladores en el servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>Parámetros

|        Parámetro         |                                                                           Descripción                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:\<nombre de servidor >] |              Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local.               |
| [/DriverPackage: nombre de\<>] |                                                        Especifica el nombre del paquete de controladores que se va a mostrar.                                                         |
|    [/PackageId: ID. de\<>]    | Especifica el ID. de servicios de implementación de Windows del paquete de controladores que se va a mostrar. Debe especificar el identificador si el paquete de controladores no se puede identificar de forma única por nombre. |
|     [/Show: {drivers     |                                                                              Files                                                                               |

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para ver información acerca de un paquete de controladores, escriba uno de los siguientes:
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)