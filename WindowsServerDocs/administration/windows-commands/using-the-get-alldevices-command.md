---
title: Get-AllDevices
description: Tema de referencia de Get-AllDevices, que muestra las propiedades de los servicios de implementación de Windows de todos los equipos preconfigurados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26e114be7ecf104687da237636b54b79e4114591
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720900"
---
# <a name="get-alldevices"></a>Get-AllDevices

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
|[/ReferralServer:<Server name>]|Devuelve solo los equipos preconfigurados para el servidor especificado.|
## <a name="examples"></a>Ejemplos
Para ver todos los equipos, escriba uno de los siguientes:
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- [Subcomando de clave](command-line-syntax-key.md)
de sintaxis de línea de comandos[: set-](subcommand-set-device.md)
Device
[mediante el comando Add-](using-the-add-device-command.md)Device[mediante el comando Get-Device](using-the-get-device-command.md)
