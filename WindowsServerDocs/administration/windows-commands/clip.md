---
title: clip
description: Artículo de referencia del comando clip, que redirige la salida del comando desde la línea de comandos al portapapeles de Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3c0e23c18d356740a639af760fc7433db59d012a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929894"
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