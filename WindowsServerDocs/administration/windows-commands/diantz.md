---
title: diantz
description: Artículo de referencia para el comando diantz, que empaqueta los archivos existentes en un archivo. cab.
ms.topic: reference
ms.assetid: 218ed5d7-1203-4d68-ad9b-65cdd022d54f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 033bfb59bbd33fee44bb307d1a905dc6c8d658c7
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635013"
---
# <a name="diantz"></a>diantz

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Empaquetar los archivos existentes en un archivo contenedor (. cab). Este comando realiza las mismas acciones que el [comando MAKECAB](makecab.md)actualizado.

## <a name="syntax"></a>Sintaxis

```
diantz [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
diantz [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<source>` | Archivo que se va a comprimir. |
| `<destination>` | Nombre de archivo que se va a asignar al archivo comprimido. Si se omite, el último carácter del nombre del archivo de código fuente se sustituye por un carácter de subrayado (_) y se usa como destino. |
| /f `<directives_file>` | Un archivo con directivas **diantz** (se puede repetir). |
| /d var =`<value>` | Define la variable con el valor especificado. |
| l `<dir>` | Ubicación en la que se va a colocar el destino (el valor predeterminado es el directorio actual). |
| /v [ `<n>` ] | Establezca el nivel de detalle de depuración (0 = ninguno,..., 3 = completo). |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Formato de archivo. cab de Microsoft](/previous-versions/bb417343(v=msdn.10))
