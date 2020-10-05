---
title: WDSUtil Remove-multicasttransmission
description: Artículo de referencia de WDSUtil Remove-multicasttransmission, que deshabilita la transmisión por multidifusión para una imagen.
ms.topic: reference
ms.assetid: 9a7f5c31-bfbf-425d-9129-a6f9173fe83d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a7ed8d9ef26deb0a50baa8511cd07f250561d579
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731379"
---
# <a name="wdsutil-remove-multicasttransmission"></a>WDSUtil Remove-multicasttransmission

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Deshabilita la transmisión por multidifusión para una imagen. A menos que especifique **/Force**, los clientes existentes completarán la transferencia de imágenes, pero no se permitirá que se unan a los clientes nuevos.

## <a name="syntax"></a>Sintaxis
**Windows Server 2008**
```
wdsutil /remove-MulticastTransmission:<Image name> [/Server:<Server name> mediatype:Install Group:<Image Group>] [/Filename:<File name>] [/force]
```
**Windows Server 2008 R2** para imágenes de arranque:
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
\x20    [/Server:<Server name>]
\x20  mediatype:Boot
\x20    /Architecture:{x86 | ia64 | x64}
\x20    [/Filename:<File name>]
```
para imágenes de instalación:
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group
        [/Filename:<File name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|soporte<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utiliza el servidor local.|
mediatype: {instalar&#124;arranque}|Especifica el tipo de imagen. Tenga en cuenta que esta opción debe estar establecida en **instalar** para Windows Server 2008.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|Especifica la arquitectura de la imagen de arranque que está asociada a la transmisión que se va a iniciar. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en diferentes arquitecturas, debe especificar la arquitectura para asegurarse de que se utiliza la transmisión correcta.|
|\mediaGroup: <Image group name> ]|Especifica el grupo de imágenes que contiene la imagen. Si no se especifica ningún nombre de grupo de imágenes y solo existe un grupo de imágenes en el servidor, se utiliza ese grupo de imágenes. Si existe más de un grupo de imágenes en el servidor, debe usar esta opción para especificar el nombre del grupo de imágenes.|
|[/Filename:<File name>]|especifica el nombre de archivo. Si la imagen de origen no se puede identificar de forma única mediante el nombre, debe usar esta opción para especificar el nombre de archivo.|
|/Force.|quita la transmisión y finaliza todos los clientes. A menos que especifique un valor para la opción **/Force** , los clientes existentes pueden completar la transferencia de imágenes, pero los clientes nuevos no podrán unirse.|
## <a name="examples"></a>Ejemplos
Para detener un espacio de nombres (los clientes actuales completarán la transmisión, pero los clientes nuevos no podrán unirse), escriba:
```
wdsutil /remove-MulticastTransmission:Vista with Office
/Imagetype:Install
```
```
wdsutil /remove-MulticastTransmission:x64 Boot Image
/Imagetype:Boot /Architecture:x64
```
Para forzar la finalización de todos los clientes, escriba:
```
wdsutil /remove-MulticastTransmission /Server:MyWDSServer
/Image:Vista with Officemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /force
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Get-allmulticasttransmissions (comando)](wdsutil-get-allmulticasttransmissions.md)
- [WDSUtil Get-multicasttransmission (comando)](wdsutil-get-multicasttransmission.md)
- [WDSUtil New-multicasttransmission (comando)](wdsutil-new-multicasttransmission.md)
- [WDSUtil Start-multicasttransmission (comando)](wdsutil-start-multicasttransmission.md)
