---
title: fondue
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d75d2d9fb57f8888cfc5bf50e2f7796aefc66102
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377093"
---
# <a name="fondue"></a>fondue

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita las características opcionales de Windows descargando los archivos necesarios de Windows Update u otro origen especificado por directiva de grupo. El archivo de manifiesto de la característica ya debe estar instalado en la imagen de Windows. 
## <a name="syntax"></a>Sintaxis
```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootRequest}]
```
### <a name="parameters"></a>Parámetros

|              Parámetro              |                                                                                                                                                                     Descripción                                                                                                                                                                     |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /Enable-Feature: <*feature_name*>   |                                                                               Especifica el nombre de la característica opcional de Windows que desea habilitar. Solo puede habilitar una característica por línea de comandos. Para habilitar varias características, use fondue. exe para cada característica.                                                                                |
|    /Caller-Name: <*nombre_programa*>    |                                                                                 Especifica el nombre del programa o del proceso cuando se llama a fondue. exe desde un script o un archivo por lotes. Puede usar esta opción para agregar el nombre del programa al informe de SQM si se produce un error.                                                                                 |
| /Hide-UX: {ALL &#124; rebootRequest} | Use **todo** para ocultar todos los mensajes al usuario, incluidas las solicitudes de progreso y de permiso para tener acceso a Windows Update. Si se requiere el permiso, se producirá un error en la operación.<br /><br />Use **rebootRequest** para ocultar solo los mensajes de usuario que soliciten permiso para reiniciar el equipo. Utilice esta opción si tiene un script que controla las solicitudes de reinicio. |

## <a name="BKMK_Examples"></a>Example
Para habilitar Microsoft .NET Framework 3,5, escriba:
```
fondue.exe /enable-feature:NETFX3
```
Para habilitar Microsoft .NET Framework 3,5, agregue el nombre del programa al informe de SQM y no muestre los mensajes al usuario, escriba:
```
fondue.exe /enable-feature:NETFX3 /caller-name:Admin.bat /hide-ux:all
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
  ## <a name="see-also"></a>Vea también
  [Consideraciones de implementación de Microsoft .NET Framework 3,5](https://go.microsoft.com/fwlink/?LinkId=248869)
