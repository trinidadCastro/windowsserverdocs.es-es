---
title: Add-ImageDriverPackage
description: Temas de comandos de Windows para Add-ImageDriverPackage, que agrega un paquete de controladores que se encuentra en el almacén de controladores a una imagen de arranque existente en el servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c2a4833-6427-47f8-9ffb-20b3786cb406
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b61a5beabed655ae629ad5c1e43d681568d07db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831928"
---
# <a name="add-imagedriverpackage"></a>Add-ImageDriverPackage

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

agrega un paquete de controladores que se encuentra en el almacén de controladores a una imagen de arranque existente en el servidor. La versión de la imagen debe ser Windows 7 o Windows Server 2008 R2 o posterior.

## <a name="syntax"></a>Sintaxis
```
wdsutil /add-ImageDriverPackage [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} 
```
```
[/Filename:<File name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
### <a name="parameters"></a>Parámetros

|                 Parámetro                  |                                                                                                                                                                                                            Descripción                                                                                                                                                                                                             |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           [/Server:<Server name>           |                                                                                                                                               Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se utiliza el servidor local.                                                                                                                                                |
|             medios:<Image name>             |                                                                                                                                                                                       Especifica el nombre de la imagen a la que se va a agregar el controlador.                                                                                                                                                                                        |
|               mediatype: boot               |                                                                                                                                                                Especifica el tipo de imagen a la que se va a agregar el controlador. Los paquetes de controladores solo se pueden agregar a imágenes de arranque.                                                                                                                                                                 |
| /Architecture: {x86 &#124; ia64 &#124; x64} |                                                                                                       Especifica la arquitectura de la imagen de arranque. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en diferentes arquitecturas, debe especificar la arquitectura para asegurarse de que se utiliza la imagen correcta.                                                                                                        |
|           /Filename:<File name>]           |                                                                                                                                                        Especifica el nombre del archivo. Si la imagen no se puede identificar de forma única por nombre, se debe especificar el nombre de archivo.                                                                                                                                                        |
|           [/DriverPackage:<Name>           |                                                                                                                                                                                   Especifica el nombre del paquete de controladores que se va a agregar a la imagen.                                                                                                                                                                                    |
|             [/PackageId:<ID>]              | Especifica el ID. de servicios de implementación de Windows del paquete de controladores. Debe especificar esta opción si el paquete de controladores no se puede identificar de forma única por nombre. Para buscar el identificador del paquete, haga clic en el grupo de controladores en el que se encuentra el paquete (o en el nodo **todos los paquetes** ), haga clic con el botón secundario en el paquete y, a continuación, haga clic en **propiedades**. El identificador del paquete se muestra en la pestaña **General** . Por ejemplo: {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}. |

## <a name="examples"></a><a name=BKMK_examples></a>Example
Para agregar un paquete de controladores a una imagen de arranque, escriba uno de los siguientes:
```
wdsutil /add-ImageDriverPackagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```
```
wdsutil /verbose /add-ImageDriverPackagmedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
## <a name="additional-references"></a>Referencias adicionales
- 
[de la clave de sintaxis de línea de comandos](command-line-syntax-key.md) [con el comando Add-ImageDriverPackages](using-the-add-imagedriverpackages-command.md)
