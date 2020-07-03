---
title: ', donde'
description: Artículo de referencia de Where, que muestra la ubicación de los archivos que coinciden con el patrón de búsqueda especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0b3486a5-896b-4d92-84b8-e463a0b76487
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 019f38bb47b9aa479a53a824823aba548431e5d3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936599"
---
# <a name="where"></a>, donde



Muestra la ubicación de los archivos que coinciden con el patrón de búsqueda especificado.



## <a name="syntax"></a>Sintaxis

```
where [/r <Dir>] [/q] [/f] [/t] [$<ENV>:|<Path>:]<Pattern>[ ...]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/r\<Dir>|Indica una búsqueda recursiva, empezando por el directorio especificado.|
|/q|Devuelve un código de salida (**0** para Success, **1** para error) sin mostrar la lista de archivos coincidentes.|
|/f|Muestra los resultados del comando **Where** entre comillas.|
|/t|Muestra el tamaño del archivo y la fecha y hora de la última modificación de cada archivo coincidente.|
|[$\<ENV>:\|\<Path>:]\<Pattern>[ ...]|Especifica el patrón de búsqueda de los archivos que deben coincidir. Se requiere al menos un patrón, y el patrón puede incluir caracteres comodín (**&#42;** y **?**). De forma predeterminada, **donde** busca en el directorio actual y las rutas de acceso especificadas en la variable de entorno PATH. Puede especificar una ruta de acceso diferente para la búsqueda con el formato $*env*:*Pattern* (donde *env* es una variable de entorno existente que contiene una o más rutas de acceso) o mediante el uso de format *path*:*Pattern* (donde *path* es la ruta de acceso al directorio que quiere buscar). Estos formatos opcionales no deben usarse con la opción de línea de comandos **/r** .|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Si no especifica una extensión de nombre de archivo, las extensiones enumeradas en la variable de entorno PATHEXT se anexan al patrón de forma predeterminada.
-   **Donde** puede ejecutar búsquedas recursivas, Mostrar información de archivos como la fecha o el tamaño, y aceptar variables de entorno en lugar de rutas de acceso en equipos locales.

## <a name="examples"></a>Ejemplos

Para buscar todos los archivos denominados test en la unidad C del equipo actual y sus subdirectorios, escriba:
```
where /r c:\ test
```
Para enumerar todos los archivos del directorio público, escriba:
```
where $public:*.*
```
Para buscar todos los archivos denominados Notepad en la unidad C del equipo remoto, Equipo1 y sus subdirectorios, escriba:
```
where /r \\computer1\c notepad.*
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)