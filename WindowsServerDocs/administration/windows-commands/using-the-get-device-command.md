---
title: Get-Device
description: Comando comandos de Windows para Get-Device, que recupera información de servicios de implementación de Windows sobre un equipo preconfigurado (es decir, un equipo físico que se ha alineado con una cuenta de equipo en servicios de dominio de Active Directory).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b9554ed6236d02be0be3502f42552bafbbfe1cb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831118"
---
# <a name="get-device"></a>Get-Device

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera información de servicios de implementación de Windows sobre un equipo preconfigurado (es decir, un equipo físico que se ha alineado con una cuenta de equipo en servicios de dominio de Active Directory).

## <a name="syntax"></a>Sintaxis
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/Device:<Device name>|Especifica el nombre del equipo (SAMAccountName).|
|/ID:<MAC or UUID>|Especifica la dirección MAC o el UUID (GUID) del equipo, tal y como se muestra en los ejemplos siguientes. Tenga en cuenta que un GUID válido debe estar en uno de los dos formatos cadena binaria o cadena GUID<p>-   **cadena binaria**:/ID: ACEFA3E81F20694E953EB2DAA1E8B1B6<br />**dirección MAC**-   : 00B056882FDC (sin guiones) o 00-B0-56-88-2F-DC (con guiones)<br />-   **cadena guid**:/ID: E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/Domain:<Domain>]|Especifica el dominio en el que se va a buscar el equipo preconfigurado. El valor predeterminado de este parámetro es el dominio local.|
|[/Forest: {Yes &#124; no}]|Especifica si servicios de implementación de Windows debe buscar en todo el bosque o en el dominio local. El valor predeterminado es **no**, lo que significa que solo se buscará en el dominio local.|
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para obtener información mediante el nombre del equipo, escriba:
```
wdsutil /Get-Device /Device:computer1
```
Para obtener información mediante la dirección MAC, escriba:
```
wdsutil /verbose /Get-Device /ID:00-B0-56-88-2F-DC /Domain:MyDomain
```
Para obtener información mediante la cadena GUID, escriba:
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[subcomando: set-device](subcommand-set-device.md)
[mediante el comando add-device](using-the-add-device-command.md)
[mediante el comando Get-AllDevices](using-the-get-alldevices-command.md)
