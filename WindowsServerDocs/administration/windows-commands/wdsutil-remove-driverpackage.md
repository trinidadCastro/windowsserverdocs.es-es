---
title: WDSUtil Remove-driverpackage
description: Artículo de referencia de WDSUtil Remove-driverpackage, que quita un paquete de controladores de un servidor.
ms.topic: reference
ms.assetid: 6b201e91-0d44-4e4a-8252-8b0235df1002
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fe40c9a1dbbf4a5f0bd09e61f39e6b9314a09337
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730629"
---
# <a name="wdsutil-remove-driverpackage"></a>WDSUtil Remove-driverpackage

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Quita un paquete de controladores de un servidor.

## <a name="syntax"></a>Sintaxis
```
wdsutil /remove-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
### <a name="parameters"></a>Parámetros

|        Parámetro        |                                                                            Descripción                                                                             |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:<Server name>] |              Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica un nombre de servidor, se utiliza el servidor local.              |
| [/DriverPackage: <Name> ] |                                                        Especifica el nombre del paquete de controladores que se va a quitar.                                                         |
|    [/PackageId: <ID> ]    | Especifica el ID. de servicios de implementación de Windows del paquete de controladores que se va a quitar. Debe especificar el identificador si el paquete de controladores no se puede identificar de forma única por nombre. |

## <a name="examples"></a>Ejemplos
Para ver información acerca de las imágenes, escriba una de las siguientes opciones:
```
wdsutil /remove-DriverPackage /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /remove-DriverPackage /Server:MyWdsServer /DriverPackage:MyDriverPackage
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Remove-driverpackages (comando)](wdsutil-remove-driverpackages.md)
