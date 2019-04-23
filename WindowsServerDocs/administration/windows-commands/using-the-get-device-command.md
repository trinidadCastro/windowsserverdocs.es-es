---
title: Usa el comando get-dispositivo
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 117720c5102da5713c1b0bb5e59cbcc099f3aa8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871036"
---
# <a name="using-the-get-device-command"></a>Usa el comando get-dispositivo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera información de servicios de implementación de Windows sobre un equipo preconfigurado (es decir, un equipo físico que se ha preparado para una cuenta de equipo en servicios de dominio de active directory.
## <a name="syntax"></a>Sintaxis
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/ Dispositivo:<Device name>|Especifica el nombre del equipo (SAMAccountName).|
|ID.:<MAC or UUID>|Especifica la dirección MAC o el UUID (GUID) del equipo, como se muestra en los ejemplos siguientes. Tenga en cuenta que debe ser un GUID válido en uno de los dos formatos de cadena binaria o una cadena GUID<br /><br />-   **Cadena binaria**: /ID:ACEFA3E81F20694E953EB2DAA1E8B1B6<br />-   **Dirección MAC**: 00B056882FDC (sin guiones) o 00-B0-56-88-2F-DC (con guiones)<br />-   **Cadena GUID**: /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/Domain:<Domain>]|Especifica el dominio que se va a buscar el equipo preconfigurado. El valor predeterminado para este parámetro es el dominio local.|
|[/ forest: {Sí &#124; n}]|Especifica si los servicios de implementación de Windows debe buscar todo el bosque o dominio local. El valor predeterminado es **No**, lo que significa que se buscará solo en el dominio local.|
## <a name="BKMK_examples"></a>Ejemplos
Para obtener información con el nombre de equipo, escriba:
```
wdsutil /Get-Device /Device:computer1
```
Para obtener información mediante el uso de la dirección MAC, escriba:
```
wdsutil /verbose /Get-Device /ID:"00-B0-56-88-2F-DC" /Domain:MyDomain
```
Para obtener información mediante el uso de la cadena de GUID, escriba:
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[subcomando: set-Device](subcommand-set-device.md)
[con el comando Agregar dispositivos](using-the-add-device-command.md)
[mediante el Comando Get-AllDevices](using-the-get-alldevices-command.md)
