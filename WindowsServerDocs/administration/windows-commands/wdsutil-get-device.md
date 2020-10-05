---
title: Get-Device de WDSUtil
description: Artículo de referencia de WDSUtil Get-Device, que recupera información de servicios de implementación de Windows sobre un equipo preconfigurado (es decir, un equipo físico que se ha alineado con una cuenta de equipo en servicios de dominio de Active Directory).
ms.topic: reference
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 875ab5053ebbb1708f7b51896c9d82b9eba8f725
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730845"
---
# <a name="wdsutil-get-device"></a>Get-Device de WDSUtil

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
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [comando set-device de WDSUtil](wdsutil-set-device.md)
- [comando de add-device de WDSUtil](wdsutil-add-device.md)
- [WDSUtil Get-alldevices (comando)](wdsutil-get-alldevices.md)
