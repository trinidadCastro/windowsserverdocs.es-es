---
title: Get-Device
description: Artículo de referencia de Get-Device, que recupera información de servicios de implementación de Windows sobre un equipo preconfigurado (es decir, un equipo físico que se ha alineado con una cuenta de equipo en servicios de dominio de Active Directory).
ms.topic: reference
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4fdbdfa843376b328cac0fe06e68a5a215ddd915
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622595"
---
# <a name="get-device"></a>Get-Device

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Recupera información de servicios de implementación de Windows sobre un equipo preconfigurado (es decir, un equipo físico que se ha alineado con una cuenta de equipo en servicios de dominio de Active Directory).

## <a name="syntax"></a>Sintaxis
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|Dispositivos<Device name>|Especifica el nombre del equipo (SAMAccountName).|
|Sesión<MAC or UUID>|Especifica la dirección MAC o el UUID (GUID) del equipo, tal y como se muestra en los ejemplos siguientes. Tenga en cuenta que un GUID válido debe estar en uno de los dos formatos cadena binaria o cadena GUID<p>-   **Cadena binaria**:/ID: ACEFA3E81F20694E953EB2DAA1E8B1B6<br />-   **Dirección Mac**: 00B056882FDC (sin guiones) o 00-B0-56-88-2F-DC (con guiones)<br />-   **Cadena guid**:/ID: E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/Domain: <Domain> ]|Especifica el dominio en el que se va a buscar el equipo preconfigurado. El valor predeterminado de este parámetro es el dominio local.|
|[/Forest: {Yes &#124; no}]|Especifica si servicios de implementación de Windows debe buscar en todo el bosque o en el dominio local. El valor predeterminado es **no**, lo que significa que solo se buscará en el dominio local.|
## <a name="examples"></a>Ejemplos
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
- Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
 [Subcomando: set-device](subcommand-set-device.md) 
 [Usar el comando](using-the-add-device-command.md) 
 add-device [Usar el comando Get-AllDevices](using-the-get-alldevices-command.md)
