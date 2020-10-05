---
title: WDSUtil Start-multicasttransmission
description: Artículo de referencia para el subcomando Start-MulticastTransmission, que inicia una transmisión de difusión programada de una imagen.
ms.topic: reference
ms.assetid: a1b2d459-1ece-49d4-997c-9d206c463b61
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a60f708dcba51cad109c050c8cc6fa331db293fe
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91731292"
---
# <a name="wdsutil-start-multicasttransmission"></a>WDSUtil Start-multicasttransmission

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Inicia una transmisión de difusión programada de una imagen.

## <a name="syntax"></a>Sintaxis
**Windows Server 2008**
```
wdsutil /start-MulticastTransmissiomedia:<Image name> [/Server:<Server namemediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
**Windows Server 2008 R2** para imágenes de arranque:
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
para imágenes de instalación:
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
soporte<Image name>|Especifica el nombre de la imagen.|
|[/Server:<Server name>]|Especifica el nombre del servidor. Puede ser el nombre de NetBIOS o el nombre de dominio completo (FQDN). Si no se especifica ningún nombre de servidor, se utilizará el servidor local.|
mediatype: {instalar&#124;arranque}|Especifica el tipo de imagen. Tenga en cuenta que esta opción debe estar establecida en **instalar** para Windows Server 2008.|
|/Architecture: {x86 &#124; ia64 &#124; x64}|La arquitectura de la imagen de arranque que está asociada a la transmisión que se va a iniciar. Dado que es posible tener el mismo nombre de imagen para imágenes de arranque en diferentes arquitecturas, debe especificar la arquitectura para asegurarse de que se utiliza la transmisión correcta.|
|\mediaGroup: <Image group name> ]|Especifica el grupo de imágenes de la imagen. Si no se especifica ningún nombre de grupo de imágenes y solo existe un grupo de imágenes en el servidor, se utilizará ese grupo de imágenes. Si existe más de un grupo de imágenes en el servidor, debe usar esta opción para especificar el nombre del grupo de imágenes.|
|[/Filename:<File name>]|Especifica el nombre del archivo que contiene la imagen. Si la imagen no se puede identificar de forma única por nombre, debe usar esta opción para especificar el nombre de archivo.|
## <a name="examples"></a>Ejemplos
Para iniciar una transmisión por multidifusión, escriba uno de los siguientes:
```
wdsutil /start-MulticastTransmissiomedia:Vista with Office
/Imagetype:Install
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
Para iniciar una transmisión por multidifusión de imagen de arranque para Windows Server 2008 R2, escriba:
```
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:X64 Boot Imagemediatype:Boot /Architecture:x64
/Filename:boot.wim\n\
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [WDSUtil Get-allmulticasttransmissions (comando)](wdsutil-get-allmulticasttransmissions.md)
- [WDSUtil Get-multicasttransmission (comando)](wdsutil-get-multicasttransmission.md)
- [WDSUtil New-multicasttransmission (comando)](wdsutil-new-multicasttransmission.md)
- [WDSUtil Remove-multicasttransmission (comando)](wdsutil-remove-multicasttransmission.md)
