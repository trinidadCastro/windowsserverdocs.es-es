---
title: Mediante el comando get-AllDevices
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5f51bcc2332cced906be1eec3265541ffd2d225
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886376"
---
# <a name="using-the-get-alldevices-command"></a>Mediante el comando get-AllDevices

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra las propiedades de los servicios de implementación de Windows de todos los equipos preconfigurados. Un equipo preconfigurado es un equipo físico que se ha vinculado a una cuenta de equipo en servicios de dominio de active directory.
## <a name="syntax"></a>Sintaxis
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|[/ forest: {Sí &#124; n}]|Especifica si los servicios de implementación de Windows debe devolver los equipos en todo el bosque o dominio local. El valor predeterminado es **No**, lo que significa que se devuelven solo los equipos en el dominio local.|
|[/ ReferralServer:<Server name>]|Devuelve solo los equipos que están preconfigurados para el servidor especificado.|
## <a name="BKMK_examples"></a>Ejemplos
Para ver todos los equipos, escriba uno de los siguientes:
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[subcomando: set-Device](subcommand-set-device.md)
[con el comando Agregar dispositivos](using-the-add-device-command.md)
[utilizando el dispositivo de get Comando](using-the-get-device-command.md)
