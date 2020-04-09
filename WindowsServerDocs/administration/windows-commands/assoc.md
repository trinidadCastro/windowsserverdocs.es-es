---
title: assoc
description: Windows Commands topic for Assoc, que muestra o modifica las asociaciones de extensión de nombre de archivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 237bedda-b24c-4fec-a39c-9b7eacf96417
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 442ba244a7325425df29a1ebdcdb8bc107095ebe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851298"
---
# <a name="assoc"></a>assoc

Muestra o modifica las asociaciones de extensión de nombre de archivo. Si se usa sin parámetros, **Assoc** muestra una lista de todas las asociaciones de extensión de nombre de archivo actuales.

> [!NOTE]
> Este comando solo se admite en CMD. EXE y no está disponible en PowerShell.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
assoc [<.ext>[=[<FileType>]]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<.ext>` | Especifica la extensión de nombre de archivo. |
| `<FileType>` | Especifica el tipo de archivo que se va a asociar a la extensión de nombre de archivo especificada. |
| `/?` | Muestra la Ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Comentarios

- Para quitar la Asociación de tipo de archivo para una extensión de nombre de archivo, agregue un espacio en blanco después del signo igual presionando la barra ESPACIAdora.

- Para ver los tipos de archivo actuales que tienen definidas cadenas de comandos abiertas, use el comando **ftype** .

- Para redirigir la salida de **Assoc** a un archivo de texto, utilice el operador de redireccionamiento de **>** .

## <a name="examples"></a><a name=BKMK_examples></a>Example

Para ver la Asociación de tipo de archivo actual para la extensión de nombre de archivo. txt, escriba:

```
assoc .txt
```

Para quitar la Asociación de tipo de archivo para la extensión de nombre de archivo. bak, escriba:

```
assoc .bak= 
```

> [!NOTE]
> Asegúrese de agregar un espacio después del signo igual.

Para ver la salida de **Assoc** en una pantalla a la vez, escriba:

```
assoc | more
```

Para enviar la salida de **Assoc** al archivo Assoc. txt, escriba:

```
assoc>assoc.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
