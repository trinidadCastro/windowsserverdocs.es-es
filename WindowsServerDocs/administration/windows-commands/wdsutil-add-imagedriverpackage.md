---
title: WDSUtil Add-imagedriverpackage
description: Artículo de referencia del comando WDSUtil Add-imagedriverpackage, que agrega un paquete de controladores que se encuentra en el almacén de controladores a una imagen de arranque existente en el servidor.
ms.topic: reference
ms.assetid: 6c2a4833-6427-47f8-9ffb-20b3786cb406
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5d441007f673a194799e245bb704d31add81a483
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524510"
---
# <a name="wdsutil-add-imagedriverpackage"></a>WDSUtil Add-imagedriverpackage

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Agrega un paquete de controladores que se encuentra en el almacén de controladores a una imagen de arranque existente en el servidor.

## <a name="syntax"></a>Sintaxis

```
wdsutil /add-ImageDriverPackage [/Server:<Servername>] [media:<Imagename>] [mediatype:Boot] [/Architecture:{x86 | ia64 | x64}] [/Filename:<Filename>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| [/Server:`<Servername>`] | Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica un nombre de servidor, se utiliza el servidor local. |
| [medios: `<Imagename>` ] | Especifica el nombre de la imagen a la que se va a agregar el controlador. |
| [mediatype: boot] | Especifica el tipo de imagen a la que se va a agregar el controlador. Los paquetes de controladores solo se pueden agregar a imágenes de arranque. |
| [/Architecture: `{x86 | ia64 | x64}` ] | Especifica la arquitectura de la imagen de arranque. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en diferentes arquitecturas, debe especificar la arquitectura para asegurarse de que se usa la imagen correcta. |
| [/Filename:`<Filename>`] | Especifica el nombre del archivo. Si la imagen no se puede identificar de forma única por nombre, se debe especificar el nombre de archivo. |
| [/DriverPackage:`<Name>` | Especifica el nombre del paquete de controladores que se va a agregar a la imagen. |
| [/PackageId: `<ID>` ] | Especifica el ID. de servicios de implementación de Windows del paquete de controladores. Debe especificar esta opción si el paquete de controladores no se puede identificar de forma única por nombre. Para buscar el identificador del paquete, seleccione el grupo de controladores en el que se encuentra el paquete (o el nodo **todos los paquetes** ), haga clic con el botón secundario en el paquete y, a continuación, seleccione **propiedades**. El identificador del paquete se muestra en la pestaña **General** . Por ejemplo: {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}. |

## <a name="examples"></a>Ejemplos

Para agregar un paquete de controladores a una imagen de arranque, escriba:

```
wdsutil /add-ImageDriverPackagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```

```
wdsutil /verbose /add-ImageDriverPackagmedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [WDSUtil Add-imagedriverpackages (comando)](wdsutil-add-imagedriverpackages.md)

- [Cmdlets de servicios de implementación de Windows](/powershell/module/wds)
