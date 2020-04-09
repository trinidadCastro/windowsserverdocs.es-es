---
title: Get-AllDevices
description: Temas de comandos de Windows para Get-AllDevices, que muestra las propiedades de los servicios de implementación de Windows de todos los equipos preconfigurados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 929a74b6cccaed6e85015648538c1ca875b62208
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831418"
---
# <a name="get-alldevices"></a>Get-AllDevices

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="examples"></a><a name=BKMK_examples></a>Example
Para ver todos los equipos, escriba uno de los siguientes:
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[subcomando: set-device](subcommand-set-device.md)
[mediante el comando add-device](using-the-add-device-command.md)
[mediante el comando Get-Device](using-the-get-device-command.md)
