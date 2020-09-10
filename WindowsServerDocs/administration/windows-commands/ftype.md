---
title: ftype
description: Artículo de referencia del comando ftype, que muestra o modifica el tipo de archivo utilizado en las asociaciones de extensión de nombre de archivo.
ms.topic: reference
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: db5781eccb4fc54fea42586b5e7aab779509bffd
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636652"
---
# <a name="ftype"></a>ftype

Muestra o modifica los tipos de archivo que se usan en las asociaciones de extensión de nombre de archivo. Si se utiliza sin un operador de asignación (=), este comando muestra la cadena de comandos Open actual para el tipo de archivo especificado. Si se usa sin parámetros, este comando muestra los tipos de archivo que tienen definidas cadenas de comandos abiertas.

> [!NOTE]
> Este comando solo se admite dentro de cmd.exe y no está disponible en PowerShell.
> Aunque puede usar `cmd /c ftype` como solución alternativa.

## <a name="syntax"></a>Sintaxis

```
ftype [<filetype>[=[<opencommandstring>]]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<filetype>` | Especifica el tipo de archivo que se va a mostrar o cambiar. |
| `<opencommandstring>` | Especifica la cadena de comandos abierta que se va a usar al abrir archivos del tipo de archivo especificado.|
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

En la tabla siguiente se describe cómo **ftype** sustituye las variables dentro de una cadena de comandos abierta:

| Variable | Valor de reemplazo |
| -------- | ----------------- |
| `%0` o `%1` | Se sustituye por el nombre de archivo que se inicia a través de la asociación. |
| `%*` | Obtiene todos los parámetros. |
| `%2`, `%3`, ... | Obtiene el primer parámetro ( `%2` ), el segundo parámetro ( `%3` ), etc. |
| `%~<n>` | Obtiene todos los parámetros restantes a partir del parámetro *n*, donde *n* puede ser cualquier número comprendido entre 2 y 9. |

### <a name="examples"></a>Ejemplos

Para mostrar los tipos de archivo actuales que tienen definidas cadenas de comandos abiertas, escriba:

```
ftype
```

Para mostrar la cadena de comando abierta actual para el tipo de archivo *TXTfile* , escriba:

```
ftype txtfile
```

Este comando produce un resultado similar al siguiente:

`txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1`

Para eliminar la cadena de comandos Open para un tipo de archivo llamado *example*, escriba:

```
ftype example=
```

Para asociar la extensión de nombre de archivo. pl con el tipo de archivo PerlScript y habilitar el tipo de archivo PerlScript para que se ejecute PERL.EXE, escriba los siguientes comandos:

```
assoc .pl=PerlScript
ftype PerlScript=perl.exe %1 %*
```

Para eliminar la necesidad de escribir la extensión de nombre de archivo. pl al invocar un script Perl, escriba:

```
set PATHEXT=.pl;%PATHEXT%
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
