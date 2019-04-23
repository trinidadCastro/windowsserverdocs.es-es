---
title: Con el comando nuevo Imagendedetección
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1212571fc07b912ab9fa3344b973ce5a25085d86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835776"
---
# <a name="using-the-new-discoverimage-command"></a>Con el comando nuevo Imagendedetección



Crea una nueva imagen de detección desde una imagen de arranque existente. Descubra son imágenes de arranque que forzar que el programa Setup.exe para iniciar en modo de servicios de implementación de Windows y, a continuación, detectar un servidor de servicios de implementación de Windows. Normalmente, estas imágenes se usan para implementar imágenes en equipos que no son capaces de arranque PXE. Para obtener más información, vea Creación de imágenes ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)).

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

|Parámetro|Descripción|
|---------|-----------|
|[/ Server:\<nombre del servidor >]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se usará el servidor local.|
|/ Image:\<nombre de la imagen >|Especifica el nombre de la imagen de arranque de origen.|
|/ Arquitectura: {x86 | ia64 | x64}|Especifica la arquitectura de la imagen que se va a devolver. Dado que es posible tener el mismo nombre de imagen para las imágenes de arranque diferentes en distintas arquitecturas, especificando el valor de la arquitectura garantiza que WDSUTIL / devuelve la imagen correcta.|
|[/ Nombre de archivo:\<nombreDeArchivo >]|Si la imagen no se identifica por nombre, debe usar esta opción para especificar el nombre de archivo.|
|/DestinationImage|Especifica la configuración de la imagen de destino. Puede especificar la configuración mediante las siguientes opciones:</br>-/ FilePath: < ruta de acceso y nombre del archivo > - ruta de acceso completa de conjuntos para la nueva imagen.</br>-[/ Name:\<nombre >]-establece el nombre para mostrar de la imagen. Si no se especifica ningún nombre para mostrar, se usará el nombre para mostrar de la imagen de origen.</br>-[/ Descripción: \<Descripción >]-establece la descripción de la imagen.</br>-   [/WDSServer: \<Nombre del servidor >]: especifica el nombre del servidor que deben ponerse en contacto con todos los clientes que arrancan desde la imagen especificada para descargar la imagen de instalación. De forma predeterminada, todos los clientes que esta imagen de arranque detectará un servidor de servicios de implementación de Windows válido. Con esta opción omite la funcionalidad de detección y fuerza el cliente arrancado ponerse en contacto con el servidor especificado.</br>-   [/Overwrite:{Yes | No | Append}]: determina si el archivo especificado en **/DestinationImage** se debe sobrescribir si ya existe otro archivo con ese nombre en/filepath. **Sí** sobrescribe el archivo existente. **No** (valor predeterminado) provoca un error si ya existe otro archivo con el mismo nombre. **Anexar** adjunta la imagen generada como una nueva imagen en el archivo .wim existente.|

## <a name="BKMK_examples"></a>Ejemplos

Para crear una imagen de detección fuera de la imagen de arranque y asígnele el nombre WinPEDiscover.wim, escriba:
```
WDSUTIL /New-DiscoverImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPEDiscover.wim"
```
Para crear una imagen de detección fuera de la imagen de arranque y asígnele el nombre WinPEDiscover.wim con la configuración especificada, escriba:
```
WDSUTIL /Verbose /Progress /New-DiscoverImage /Server:MyWDSServer
/Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim /DestinationImage /FilePath:"\\Server\Share\WinPEDiscover.wim" 
/Name:"New WinPE image" /Description:"WinPE image for WDS Client discovery" /Overwrite:No
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)