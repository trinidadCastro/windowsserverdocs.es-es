---
title: makecab
description: Tema de referencia del comando MAKECAB, que empaqueta los archivos existentes en un archivo. cab.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 192471e6045a530e9deedec70cc957b9362b3ae7
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354665"
---
# <a name="makecab"></a>makecab

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Empaquetar los archivos existentes en un archivo contenedor (. cab).


> [!NOTE]
> Este comando es el mismo que el [comando diantz](diantz.md).

## <a name="syntax"></a>Sintaxis

```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<source>` | Archivo que se va a comprimir. |
| `<destination>` | Nombre de archivo que se va a asignar al archivo comprimido. Si se omite, el último carácter del nombre del archivo de código fuente se sustituye por un carácter de subrayado (_) y se usa como destino. |
| /f `<directives_file>` | Un archivo con directivas **MAKECAB** (se puede repetir). |
| /d var =`<value>` | Define la variable con el valor especificado. |
| l`<dir>` | Ubicación en la que se va a colocar el destino (el valor predeterminado es el directorio actual). |
| /v [ `<n>` ] | Establezca el nivel de detalle de depuración (0 = ninguno,..., 3 = completo). |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando diantz](diantz.md)

- [Formato de archivo. cab de Microsoft](https://docs.microsoft.com/previous-versions/bb417343(v=msdn.10))
