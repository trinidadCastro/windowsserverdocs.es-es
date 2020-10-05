---
title: Subcomando set-DriverPackage
description: Artículo de referencia para el subcomando set-DriverPackage, que cambia el nombre de un paquete de controladores o lo habilita o deshabilita en un servidor.
ms.topic: reference
ms.assetid: 11804bb6-ca29-4461-8c63-5131748cd742
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a4b0b3154f6cc7cee34e6fdcc91f332295164541
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731309"
---
# <a name="subcommand-set-driverpackage"></a>Subcomando: set-DriverPackage

Cambia el nombre o habilita o deshabilita un paquete de controladores en un servidor.

## <a name="syntax"></a>Sintaxis

```
WDSUTIL /Set-DriverPackage [/Server:<Server name>] {/DriverPackage:<Name> | /PackageId:<ID>} [/Name:<New Name>] [/Enabled:{Yes | No}
```

### <a name="parameters"></a>Parámetros

|        Parámetro         |                                                                                                                                                                                                               Descripción                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:\<Server name>] |                                                                                                                                                 Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se utiliza el servidor local.                                                                                                                                                 |
| [/DriverPackage: \<Name> ] |                                                                                                                                                                                       Especifica el nombre actual del paquete de controladores que se va a modificar.                                                                                                                                                                                        |
|    [/PackageId: \<ID> ]    | Especifica el ID. de servicios de implementación de Windows del paquete de controladores. Debe especificar esta opción si el paquete de controladores no se puede identificar de forma única por nombre. Para encontrar este ID. para un paquete, haga clic en el grupo de controladores en el que se encuentra el paquete (o en el nodo **todos los paquetes** ), haga clic con el botón secundario en el paquete y, a continuación, haga clic en **propiedades**. El identificador del paquete se muestra en la pestaña **General** . Por ejemplo: {DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}. |
|   [/Name: \<New Name> ]    |                                                                                                                                                                                              Especifica el nuevo nombre del paquete de controladores.                                                                                                                                                                                              |
|      [/Enabled: {Yes      |                                                                                                                                                                                                                   Ninguno                                                                                                                                                                                                                    |

## <a name="examples"></a>Ejemplos

Para cambiar la configuración de un paquete, escriba uno de los siguientes:
```
WDSUTIL /Set-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318} /Name:MyDriverPackage
```
```
WDSUTIL /Set-DriverPackage /DriverPackage:MyDriverPackage /Name:NewName /Enabled:Yes
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)