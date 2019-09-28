---
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
title: Fsutil reparsepoint
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: fe274ad9a6dffc72607102d3430ba7527d3cc558
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376854"
---
# <a name="fsutil-reparsepoint"></a>Fsutil reparsepoint
>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Consulta o elimina puntos de reanálisis.  Los profesionales de soporte técnico suelen usar el comando **fsutil reparsepoint** .

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
fsutil reparsepoint [query] <FileName>
fsutil reparsepoint [delete] <FileName>
```

## <a name="parameters"></a>Parámetros

| Parámetro  |                                                                Descripción                                                                |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
|   query    |            Recupera los datos del punto de reanálisis asociados al archivo o directorio identificado por el identificador especificado.             |
|   Eliminar   | Elimina un punto de reanálisis del archivo o directorio identificado por el identificador especificado, pero no elimina el archivo o directorio. |
| <FileName> |             Especifica la ruta de acceso completa al archivo, incluido el nombre de archivo y la extensión, por ejemplo C:\documents\filename.txt.             |

## <a name="remarks"></a>Comentarios

-   Los puntos de reanálisis son objetos del sistema de archivos NTFS que tienen un atributo definible que contiene datos definidos por el usuario y se utilizan para extender la funcionalidad en el subsistema de entrada/salida (e/s).

-   Los puntos de reanálisis se usan para los puntos de unión de directorios y los puntos de montaje de volumen. También se usan en los controladores de filtro del sistema de archivos para marcar determinados archivos como especiales para ese controlador.

-   Cuando un programa establece un punto de reanálisis, almacena estos datos, además de una etiqueta de reanálisis, que identifica de forma única los datos almacenados. Cuando el sistema de archivos abre un archivo con un punto de reanálisis, intenta encontrar el filtro del sistema de archivos asociado. Si se encuentra el filtro del sistema de archivos, el filtro procesa el archivo según lo indicado por los datos de reanálisis. Si no se encuentra ningún filtro del sistema de archivos, se producirá un error en la operación de apertura de archivo.

## <a name="BKMK_examples"></a>Example
Para recuperar los datos de punto de reanálisis asociados a C:\Server, escriba:

```
fsutil reparsepoint query c:\server
```

Para eliminar un punto de análisis de un archivo o directorio especificado, use el formato siguiente:

```
fsutil reparsepoint delete c:\server
```

#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


