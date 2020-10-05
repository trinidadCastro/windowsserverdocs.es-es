---
title: shrink
description: Artículo de referencia para el comando de reducción de DiskPart, que reduce el tamaño del volumen seleccionado en la cantidad especificada.
ms.topic: reference
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8b03c006243f263a4f1c7fe991580d5e74f9a7a7
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718322"
---
# <a name="shrink"></a>shrink

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

El comando de reducción de Diskpart reduce el tamaño del volumen seleccionado en la cantidad especificada. Este comando hace que el espacio libre en disco esté disponible en el espacio no usado al final del volumen.

Se debe seleccionar un volumen para que esta operación se realice correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.

> [!NOTE]
> Este comando funciona en volúmenes básicos y en volúmenes dinámicos simples o distribuidos. No funciona en particiones de fabricante de equipos originales (OEM), particiones del sistema de Extensible Firmware Interface (EFI) o particiones de recuperación.

## <a name="syntax"></a>Sintaxis

```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| deseado =`<n>` | Especifica el espacio en megabytes (MB) que se desea reducir en el volumen. |
| mínimo =`<n>` | Especifica el espacio mínimo en megabytes (MB) que se desea reducir en el volumen. |
| querymax | Devuelve la cantidad máxima de espacio en MB por el que se puede reducir el volumen. Este valor puede cambiar si hay aplicaciones que están obteniendo acceso al volumen. |
| nowait | Fuerza la vuelta inmediata del comando mientras se está realizando la reducción. |
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |

#### <a name="remarks"></a>Observaciones

- El tamaño de un volumen sólo se puede reducir si se ha formateado mediante el sistema de archivos NTFS o si no contiene un sistema de archivos.

- Si no se especifica una cantidad deseada, el volumen se reduce por la cantidad mínima (si se especifica).

- Si no se especifica una cantidad mínima, el volumen se reduce según la cantidad deseada (si se especifica).

- Si no se especifica una cantidad mínima ni una cantidad deseada, el volumen se reduce tanto como sea posible.

- Si se especifica una cantidad mínima, pero no hay suficiente espacio disponible, se produce un error en el comando.

## <a name="examples"></a>Ejemplos

Para reducir el tamaño del volumen seleccionado en la cantidad más grande posible entre 250 y 500 megabytes, escriba:

```
shrink desired=500 minimum=250
```

Para mostrar el número máximo de MB por el que se puede reducir el volumen, escriba:

```
shrink querymax
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Resize-Partition](/powershell/module/storage/resize-partition?view=win10-ps&preserve-view=true)
