---
title: edición
description: Artículo de referencia para el comando Editar, que inicia el editor de MS-DOS, para que pueda crear y cambiar archivos de texto ASCII.
ms.topic: reference
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b3723c65456fd7e17395cd7a9bc931ddc56a09b3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636173"
---
# <a name="edit"></a>edición

Inicia el editor de MS-DOS, que crea y cambia archivos de texto ASCII.

## <a name="syntax"></a>Sintaxis

```
edit [/b] [/h] [/r] [/s] [/<nnn>] [[<drive>:][<path>]<filename> [<filename2> [...]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `[<drive>:][<path>]<filename> [<filename2> [...]]` | Especifica la ubicación y el nombre de uno o más archivos de texto ASCII. Si el archivo no existe, el editor de MS-DOS lo crea. Si el archivo existe, el editor de MS-DOS lo abre y muestra su contenido en la pantalla. La opción *filename* puede contener caracteres comodín (**&#42;** y **?**). Separe varios nombres de archivo con espacios. |
| /b | Fuerza el modo monocromo, de modo que el editor de MS-DOS se muestre en blanco y negro. |
| /h | Muestra el número máximo de líneas posibles para el monitor actual. |
| /r | Carga archivos en modo de solo lectura. |
| /s | Fuerza el uso de nombres de archivo cortos. |
| `<nnn>` | Carga los archivos binarios, ajustando las líneas a *nnn* caracteres de ancho. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Para obtener ayuda adicional, abra el editor de MS-DOS y presione la tecla F1.

- Algunos monitores no admiten la presentación de teclas de método abreviado de forma predeterminada. Si el monitor no muestra las teclas de método abreviado, use **/b**.

### <a name="examples"></a>Ejemplos

Para abrir el editor de MS-DOS, escriba:

```
edit
```

Para crear y editar un archivo llamado *newtextfile.txt* en el directorio actual, escriba:

```
edit newtextfile.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
