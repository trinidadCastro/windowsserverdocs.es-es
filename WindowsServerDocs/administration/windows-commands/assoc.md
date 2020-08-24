---
title: assoc
description: Artículo de referencia para el comando Assoc, que muestra o modifica las asociaciones de la extensión de nombre de archivo.
ms.topic: article
ms.assetid: 237bedda-b24c-4fec-a39c-9b7eacf96417
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 682375733bdd269150cb6d557db730283ee3267e
ms.sourcegitcommit: a868f7d8bb9c5becffc688fd9b75c80802af71ba
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2020
ms.locfileid: "88778606"
---
# <a name="assoc"></a>assoc

Muestra o modifica las asociaciones de extensión de nombre de archivo. Si se usa sin parámetros, **Assoc** muestra una lista de todas las asociaciones de extensión de nombre de archivo actuales.

> [!NOTE]
> Este comando solo se admite dentro de cmd.exe y no está disponible en PowerShell.
> Aunque puede usar `cmd /c assoc` como solución alternativa.

## <a name="syntax"></a>Sintaxis

```
assoc [<.ext>[=[<filetype>]]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<.ext>` | Especifica la extensión de nombre de archivo. |
| `<filetype>` | Especifica el tipo de archivo que se va a asociar a la extensión de nombre de archivo especificada. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="remarks"></a>Observaciones

- Para quitar la Asociación de tipo de archivo para una extensión de nombre de archivo, agregue un espacio en blanco después del signo igual presionando la barra ESPACIAdora.

- Para ver los tipos de archivo actuales que tienen definidas cadenas de comandos abiertas, use el comando **ftype** .

- Para redirigir la salida de **Assoc** a un archivo de texto, utilice el `>` operador de redireccionamiento.

## <a name="examples"></a>Ejemplos

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

Para enviar la salida de **Assoc** al archivo assoc.txt, escriba:

```
assoc>assoc.txt
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ftype (comando)](ftype.md)
