---
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
title: fsutil reparsepoint
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: f66f09fa608fec10d7126e516f9cf2dd8a19bbfb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438996"
---
# <a name="fsutil-reparsepoint"></a>fsutil reparsepoint
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Las consultas o elimina los puntos de reanálisis.  El **fsutil reparsepoint** normalmente se usa el comando por profesionales de soporte técnico.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fsutil reparsepoint [query] <FileName>
fsutil reparsepoint [delete] <FileName>
```

## <a name="parameters"></a>Parámetros

| Parámetro  |                                                                Descripción                                                                |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
|   query    |            Recupera los datos de punto de reanálisis que está asociados con el archivo o directorio identificado por el identificador especificado.             |
|   Eliminar   | Elimina un punto de reanálisis en el archivo o directorio que se identifica por el identificador especificado, pero no elimina el archivo o directorio. |
| <FileName> |             Especifica la ruta de acceso completa al archivo incluido el nombre de archivo y la extensión, por ejemplo C:\documents\filename.txt.             |

## <a name="remarks"></a>Comentarios

-   Los puntos de reanálisis es objetos del sistema que tienen un atributo definido por el que contiene los datos definidos por el usuario de archivo de NTFS, y se usan para extender la funcionalidad en el subsistema de entrada/salida (E/S).

-   Repetición de análisis puntos se utilizan para puntos de unión de directorio y los puntos de montaje de volumen. También se usan por controladores de filtro de sistema de archivos para marcar determinados archivos especiales para el controlador.

-   Cuando un programa establece un punto de reanálisis, almacena estos datos, además de una etiqueta de reanálisis, que identifica los datos que va a guardar. Cuando el sistema de archivos abre un archivo con un punto de reanálisis, intenta buscar el filtro de sistema de archivo asociado. Si se encuentra el filtro de sistema de archivos, el filtro procesa el archivo según las indicaciones de los datos de repetición de análisis. Si no se encuentra ningún filtro de sistema de archivos, se produce un error en la operación de apertura de archivo.

## <a name="BKMK_examples"></a>Ejemplos
Para recuperar datos de punto de reanálisis asociados C:\Server, escriba:

```
fsutil reparsepoint query c:\server
```

Para eliminar un punto de reanálisis desde un archivo o directorio especificado, use el siguiente formato:

```
fsutil reparsepoint delete c:\server
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


