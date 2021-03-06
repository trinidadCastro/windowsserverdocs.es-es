---
title: fsutil wim
description: Artículo de referencia para el comando fsutil Wim, que proporciona funciones para detectar y administrar archivos con copia de seguridad de imágenes de Windows (WIM).
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 6e1bb492911250161e1e4a57983ea9c58f3a34ff
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032873"
---
# <a name="fsutil-wim"></a>fsutil wim

> Se aplica a: Windows Server (Canal semianual), Windows Server 2019, Windows Server 2016 y Windows 10

Proporciona funciones para detectar y administrar archivos respaldados por la imagen de Windows (WIM).

## <a name="syntax"></a>Sintaxis

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| enumfiles ( | Enumera los archivos de copia de seguridad de WIM. |
| `<drive name>` | Especifica el nombre de la unidad. |
| `<data source>` | Especifica el origen de datos. |
| enumwims | Enumera los archivos WIM de respaldo. |
| queryfile | Consulta si el archivo está respaldado por WIM y, en caso afirmativo, muestra detalles sobre el archivo WIM. |
| `<filename>` | Especifica el nombre de archivo. |
| removewim | Quita un WIM de los archivos de copia de seguridad. |

### <a name="examples"></a>Ejemplos

Para enumerar los archivos de la unidad C: desde el origen de datos 0, escriba:

```
fsutil wim enumfiles C: 0
```

Para enumerar los archivos WIM de respaldo de la unidad C:, escriba:

```
fsutil wim enumwims C:
```

Para ver si un archivo está respaldado por WIM, escriba:

```
fsutil wim C:\Windows\Notepad.exe
```

Para quitar WIM de archivos de copia de seguridad para el volumen C: y el origen de datos 2, escriba:

```
fsutil wim removewims C: 2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)
