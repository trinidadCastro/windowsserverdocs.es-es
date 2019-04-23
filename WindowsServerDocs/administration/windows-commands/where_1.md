---
title: ', donde'
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0b3486a5-896b-4d92-84b8-e463a0b76487
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff50405dd53ee383abc8e13f67befecf73e37c1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832456"
---
# <a name="where"></a>, donde



Muestra la ubicación de archivos que coinciden con el patrón de búsqueda determinada.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
where [/r <Dir>] [/q] [/f] [/t] [$<ENV>:|<Path>:]<Pattern>[ ...] 
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/r \<Dir >|Indica una búsqueda recursiva, empezando por el directorio especificado.|
|/q|Devuelve un código de salida (**0** para lograr el éxito, **1** error) sin mostrar la lista de archivos coincidentes.|
|/f|Muestra los resultados de la **donde** comando entre comillas.|
|/t|Muestra el tamaño del archivo y la fecha de última modificación y la hora de cada archivo coincidente.|
|[$\<ENV >:\|\<ruta de acceso >:]\<patrón > [...]|Especifica el patrón de búsqueda para que coincidan con los archivos. Se requiere al menos un patrón y el patrón puede incluir caracteres comodín (**&#42;** y **?**). De forma predeterminada, **donde** busca en el directorio actual y las rutas de acceso que se especifican en la variable de entorno PATH. Puede especificar una ruta de acceso diferentes para buscar mediante el formato $*ENV*:*patrón* (donde *ENV* es una variable de entorno que contiene uno o más rutas de acceso) o mediante el formato *ruta*:*patrón* (donde *ruta* es la ruta de acceso del directorio que desea buscar). No se deben usar estos formatos opcionales con el **/r** opción de línea de comandos.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   Si no especifica una extensión de nombre de archivo, las extensiones que aparecen en la variable de entorno PATHEXT se anexan al patrón de forma predeterminada.
-   **Donde** puede ejecutar las búsquedas recursivas, mostrar información como la fecha o el tamaño del archivo y acepta variables de entorno en lugar de las rutas de acceso en equipos locales.

## <a name="BKMK_examples"></a>Ejemplos

Para buscar todos los archivos con el nombre de prueba de la unidad C del equipo actual y sus subdirectorios, escriba:
```
where /r c:\ test 
```
Para obtener una lista de todos los archivos en el directorio público, escriba:
```
where $public:*.*
```
Para buscar todos los archivos denominados el Bloc de notas de la unidad C del equipo remoto, Equipo1 y sus subdirectorios, escriba:
```
where /r \\computer1\c notepad.*
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)