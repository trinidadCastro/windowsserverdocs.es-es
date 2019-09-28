---
title: Uso del comando Get-Device
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6451e0a55a72fc88867a3f3be0e1317d881391aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363199"
---
# <a name="using-the-get-device-command"></a>Uso del comando Get-Device

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera información de servicios de implementación de Windows sobre un equipo preconfigurado (es decir, un equipo físico que se ha alineado con una cuenta de equipo en servicios de dominio de Active Directory).
## <a name="syntax"></a>Sintaxis
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/Device: <Device name>|Especifica el nombre del equipo (SAMAccountName).|
|/ID: <MAC or UUID>|Especifica la dirección MAC o el UUID (GUID) del equipo, tal y como se muestra en los ejemplos siguientes. Tenga en cuenta que un GUID válido debe estar en uno de los dos formatos cadena binaria o cadena GUID<br /><br />**cadena binaria**-   :/ID: ACEFA3E81F20694E953EB2DAA1E8B1B6<br />**dirección MAC**-   : 00B056882FDC (sin guiones) o 00-B0-56-88-2F-DC (con guiones)<br />-   **cadena de GUID**:/ID: E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/Domain: <Domain>]|Especifica el dominio en el que se va a buscar el equipo preconfigurado. El valor predeterminado de este parámetro es el dominio local.|
|[/Forest: {Yes &#124; no}]|Especifica si servicios de implementación de Windows debe buscar en todo el bosque o en el dominio local. El valor predeterminado es **no**, lo que significa que solo se buscará en el dominio local.|
## <a name="BKMK_examples"></a>Example
Para obtener información mediante el nombre del equipo, escriba:
```
wdsutil /Get-Device /Device:computer1
```
Para obtener información mediante la dirección MAC, escriba:
```
wdsutil /verbose /Get-Device /ID:"00-B0-56-88-2F-DC" /Domain:MyDomain
```
Para obtener información mediante la cadena GUID, escriba:
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[subcomando: set-device](subcommand-set-device.md)
[mediante el comando add-device](using-the-add-device-command.md)
[mediante el comando Get-AllDevices](using-the-get-alldevices-command.md)
