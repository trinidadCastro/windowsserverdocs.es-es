---
title: fondue
description: Artículo de referencia para el comando fondue, que habilita las características opcionales de Windows mediante la descarga de los archivos necesarios de Windows Update u otro origen especificado por directiva de grupo.
ms.topic: reference
ms.assetid: fc4467f6-ddbb-4d6d-b51e-5a50a957b8c0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c80c6b1aef9ea37bdb4ff497ff7a7b5e54f8c468
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634840"
---
# <a name="fondue"></a>fondue

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Habilita las características opcionales de Windows descargando los archivos necesarios de Windows Update u otro origen especificado por directiva de grupo. El archivo de manifiesto de la característica ya debe estar instalado en la imagen de Windows.

## <a name="syntax"></a>Sintaxis

```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootrequest}]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| /Enable-Feature`<feature_name>` | Especifica el nombre de la característica opcional de Windows que desea habilitar. Solo puede habilitar una característica por línea de comandos. Para habilitar varias características, utilice fondue.exe para cada característica. |
| /caller-name:`<program_name>` | Especifica el nombre del programa o del proceso cuando se llama a fondue.exe desde un script o un archivo por lotes. Puede usar esta opción para agregar el nombre del programa al informe de SQM si se produce un error. |
| /hide-ux:`{all | rebootrequest}` | Use **todo** para ocultar todos los mensajes al usuario, incluidas las solicitudes de progreso y de permiso para tener acceso a Windows Update. Si se requiere el permiso, se producirá un error en la operación.<p>Use **rebootrequest** para ocultar solo los mensajes de usuario que soliciten permiso para reiniciar el equipo. Utilice esta opción si tiene un script que controla las solicitudes de reinicio. |

### <a name="examples"></a>Ejemplos

Para habilitar Microsoft .NET Framework 4,8, escriba:

```
fondue.exe /enable-feature:NETFX4
```

Para habilitar Microsoft .NET Framework 4,8, agregue el nombre del programa al informe de SQM y no muestre los mensajes al usuario, escriba:

```
fondue.exe /enable-feature:NETFX4 /caller-name:Admin.bat /hide-ux:all
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Descarga de Microsoft .NET Framework 4,8](https://dotnet.microsoft.com/download/dotnet-framework/net48)
