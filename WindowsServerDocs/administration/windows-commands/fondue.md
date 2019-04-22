---
title: fondue
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc4467f6-ddbb-4d6d-b51e-5a50a957b8c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 09708c239b5399f3284c42877970443cc2605cbe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817156"
---
# <a name="fondue"></a>fondue

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita características opcionales de Windows mediante la descarga de archivos necesarios de Windows Update u otro origen especificado por la directiva de grupo. El archivo de manifiesto de la característica ya debe instalarse en la imagen de Windows. 
## <a name="syntax"></a>Sintaxis
```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootRequest}]
```
### <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|/ Enable-Feature: <*feature_name*>|Especifica el nombre de la característica opcional de Windows que desea habilitar. Sólo se puede habilitar una característica por línea de comandos. Para habilitar varias características, use fondue.exe para cada característica.|
|/caller-name:<*program_name*>|Especifica el nombre de programa o proceso al llamar a fondue.exe desde un secuencia de comandos o archivo por lotes. Puede usar esta opción para agregar el nombre del programa a los informes SQM si se produce un error.|
|/hide-ux:{all &#124; rebootRequest}|Use **todas** para ocultar todos los mensajes al usuario, incluidas las solicitudes de progreso y el permiso para tener acceso a Windows Update. Si se requiere el permiso, se producirá un error en la operación.<br /><br />Use **rebootRequest** para ocultar sólo los mensajes de usuario que solicita permiso para reiniciar el equipo. Use esta opción si tiene una secuencia de comandos que los controles de reiniciar las solicitudes.|
## <a name="BKMK_Examples"></a>Ejemplos
Para habilitar Microsoft .NET Framework 3.5, escriba:
```
fondue.exe /enable-feature:NETFX3
```
Para habilitar Microsoft .NET Framework 3.5, agregue el nombre del programa a los informes SQM y no mostrar mensajes al usuario, tipo:
```
fondue.exe /enable-feature:NETFX3 /caller-name:Admin.bat /hide-ux:all
```
## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
## <a name="see-also"></a>Vea también
[Consideraciones de implementación de Microsoft .NET Framework 3.5](https://go.microsoft.com/fwlink/?LinkId=248869)
