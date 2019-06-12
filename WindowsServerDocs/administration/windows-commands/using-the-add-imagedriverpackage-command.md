---
title: Con el comando add-ImageDriverPackage
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c2a4833-6427-47f8-9ffb-20b3786cb406
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38d6c032f347f9945701f17b9289f3e3ff474031
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440623"
---
# <a name="using-the-add-imagedriverpackage-command"></a>Con el comando add-ImageDriverPackage

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Agrega un paquete de controladores que se encuentra en el almacén de controladores a una imagen de arranque existente en el servidor. La versión de la imagen debe ser Windows 7 o Windows Server 2008 R2 o posterior.
## <a name="syntax"></a>Sintaxis
```
wdsutil /add-ImageDriverPackage [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} 
```
```
[/Filename:<File name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Parámetros

|                 Parámetro                  |                                                                                                                                                                                                            Descripción                                                                                                                                                                                                             |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           [/ Server:<Server name>           |                                                                                                                                               Especifica el nombre del servidor. Esto puede ser el nombre NetBIOS o el FQDN. Si no se especifica ningún nombre de servidor, se usa el servidor local.                                                                                                                                                |
|             Medio:<Image name>             |                                                                                                                                                                                       Especifica el nombre de la imagen para agregar el controlador.                                                                                                                                                                                        |
|               mediatype:Boot               |                                                                                                                                                                Especifica el tipo de imagen para agregar el controlador. Solo se pueden agregar paquetes de controladores a imágenes de arranque.                                                                                                                                                                 |
| / Arquitectura: {x86 &#124; ia64 &#124; x64} |                                                                                                       Especifica la arquitectura de la imagen de arranque. Dado que es posible tener el mismo nombre de imagen para las imágenes de arranque en arquitecturas diferentes, debe especificar la arquitectura para asegurarse de que se usa la imagen correcta.                                                                                                        |
|           /Filename:<File name>]           |                                                                                                                                                        Especifica el nombre del archivo. Si la imagen no se identifica por nombre, debe especificarse el nombre de archivo.                                                                                                                                                        |
|           [/DriverPackage:<Name>           |                                                                                                                                                                                   Especifica el nombre del paquete de controladores para agregar a la imagen.                                                                                                                                                                                    |
|             [/PackageId:<ID>]              | Especifica el identificador de servicios de implementación de Windows del paquete de controladores. Debe especificar esta opción si el paquete de controladores no se identifica por nombre. Para buscar el identificador del paquete, haga clic en el grupo de controladores que se encuentra el paquete (o el **todos los paquetes** nodo), haga clic en el paquete y, a continuación, haga clic en **propiedades**. El identificador del paquete se muestra en el **General** ficha. Por ejemplo: {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}. |

## <a name="BKMK_examples"></a>Ejemplos
Para agregar un paquete de controladores a una imagen de arranque, escriba uno de los siguientes:
```
wdsutil /add-ImageDriverPackagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```
```
wdsutil /verbose /add-ImageDriverPackagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[con el comando add-ImageDriverPackages](using-the-add-imagedriverpackages-command.md)
