---
title: WDSUtil Get-alldevices
description: Artículo de referencia para WDSUtil Get-alldevices, que muestra las propiedades de los servicios de implementación de Windows de todos los equipos preconfigurados.
ms.topic: reference
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 57d565a0b4b79efd3a472505db5f57ecc0ba564f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730918"
---
# <a name="wdsutil-get-alldevices"></a>WDSUtil Get-alldevices

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra las propiedades de los servicios de implementación de Windows de todos los equipos preconfigurados. Un equipo preconfigurado es un equipo físico que se ha vinculado a una cuenta de equipo en servicios de dominio de Active Directory.

## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/Forest: {Yes &#124; no}]|Especifica si servicios de implementación de Windows debe devolver equipos en todo el bosque o en el dominio local. La configuración predeterminada es **no**, lo que significa que solo se devuelven los equipos del dominio local.|
|[/ReferralServer: <Server name> ]|Devuelve solo los equipos preconfigurados para el servidor especificado.|
## <a name="examples"></a>Ejemplos
Para ver todos los equipos, escriba uno de los siguientes:
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
- [comando set-device de WDSUtil](wdsutil-set-device.md)
- [comando de add-device de WDSUtil](wdsutil-add-device.md)
- [comando Get-Device de WDSUtil](wdsutil-get-device.md)
