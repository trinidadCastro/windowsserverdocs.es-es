---
title: clip
description: Artículo de referencia del comando clip, que redirige la salida del comando desde la línea de comandos al portapapeles de Windows.
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ee6527fd66678d58e971eb12e3cb92724d50517d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880126"
---
# <a name="clip"></a>clip

Redirige la salida del comando desde la línea de comandos al portapapeles de Windows. Puede usar este comando para copiar los datos directamente en cualquier aplicación que pueda recibir texto del portapapeles. También puede pegar esta salida de texto en otros programas.

## <a name="syntax"></a>Sintaxis

```
<command> | clip
clip < <filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<command>` | Especifica un comando cuya salida se desea enviar al portapapeles de Windows. |
| `<filename>` | Especifica un archivo cuyo contenido desea enviar al portapapeles de Windows. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para copiar la lista de directorios actual en el portapapeles de Windows, escriba:

```
dir | clip
```

Para copiar la salida de un programa llamado *Generic. awk* en el portapapeles de Windows, escriba:

```
awk -f generic.awk input.txt | clip
```

Para copiar el contenido de un archivo llamado *readme.txt* en el portapapeles de Windows, escriba:

```
clip < readme.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)