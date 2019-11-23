---
title: Usar el comando New-Imagendedetección
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ede9fbbb-0bba-4309-8c21-3cc13e1dc3cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b17777eb4d8541ce5669ee6becbd51e2b9da236
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362992"
---
# <a name="using-the-new-discoverimage-command"></a>Usar el comando New-Imagendedetección



Crea una nueva imagen de detección a partir de una imagen de arranque existente. Las imágenes de detección son imágenes de arranque que obligan a que el programa Setup. exe se inicie en el modo de servicios de implementación de Windows y, a continuación, detecta un servidor de servicios de implementación de Windows. Normalmente, estas imágenes se usan para implementar imágenes en equipos que no tienen la capacidad de arrancar en PXE. Para obtener más información, consulte crear imágenes ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)).

## <a name="syntax"></a>Sintaxis

```
WDSUTIL [Options] /New-DiscoverImage [/Server:<Server name>]
     /Image:<Image name>
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /DestinationImage
         /FilePath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
         [/WDSServer:<Server name>]
         [/Overwrite:{Yes | No | Append}]
```

## <a name="parameters"></a>Parámetros

|        Parámetro         |                                                                                                                                                                                                                                                                                                                                                                                                                       Descripción                                                                                                                                                                                                                                                                                                                                                                                                                       |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:\<nombre de servidor >] |                                                                                                                                                                                                                                                                                                                                     Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.                                                                                                                                                                                                                                                                                                                                     |
|   /Image:\<nombre de imagen >   |                                                                                                                                                                                                                                                                                                                                                                                                      Especifica el nombre de la imagen de arranque de origen.                                                                                                                                                                                                                                                                                                                                                                                                       |
|    /Architecture: {x86    |                                                                                                                                                                                                                                                                                                                                                                                                                          64                                                                                                                                                                                                                                                                                                                                                                                                                           |
| [/Filename:\<nombre de archivo >] |                                                                                                                                                                                                                                                                                                                                                                         Si la imagen no se puede identificar de forma única por nombre, debe usar esta opción para especificar el nombre de archivo.                                                                                                                                                                                                                                                                                                                                                                          |
|    /DestinationImage     | Especifica la configuración de la imagen de destino. Puede especificar la configuración con las siguientes opciones:</br>-/FilePath: < ruta de acceso y el nombre del archivo > establece la ruta de acceso completa del archivo para la nueva imagen.</br>-[/Name:\<nombre >]: establece el nombre para mostrar de la imagen. Si no se especifica ningún nombre para mostrar, se usará el nombre para mostrar de la imagen de origen.</br>-[/Description: \<Descripción >]: establece la descripción de la imagen.</br>-[/WDSServer: \<nombre de servidor >]: especifica el nombre del servidor que todos los clientes que arrancan desde la imagen especificada deben ponerse en contacto para descargar la imagen de instalación. De forma predeterminada, todos los clientes que arrancan esta imagen detectarán un servidor de servicios de implementación de Windows válido. El uso de esta opción omite la funcionalidad de detección y obliga al cliente arrancado a ponerse en contacto con el servidor especificado.</br>-[/Overwrite: {Yes |

## <a name="BKMK_examples"></a>Example

Para crear una imagen de detección fuera de la imagen de arranque y asignarle el nombre WinPEDiscover. Wim, escriba:
```
WDSUTIL /New-DiscoverImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPEDiscover.wim"
```
Para crear una imagen de detección fuera de la imagen de arranque y asignarle el nombre WinPEDiscover. Wim con la configuración especificada, escriba:
```
WDSUTIL /Verbose /Progress /New-DiscoverImage /Server:MyWDSServer
/Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim /DestinationImage /FilePath:"\\Server\Share\WinPEDiscover.wim" 
/Name:"New WinPE image" /Description:"WinPE image for WDS Client discovery" /Overwrite:No
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)