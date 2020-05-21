---
title: fsutil reparsepoint
description: Tema de referencia para el comando fsutil reparsepoint, que consulta o elimina puntos de reanálisis.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 56ca18cc4f3b4cdfd9021eb8361d980bb855bdc3
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435730"
---
# <a name="fsutil-reparsepoint"></a>fsutil reparsepoint

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Consulta o elimina puntos de reanálisis.  Los profesionales de soporte técnico suelen usar el comando **fsutil reparsepoint** .

Los puntos de reanálisis son objetos del sistema de archivos NTFS que tienen un atributo definible, que contiene datos definidos por el usuario. Se utilizan para:

- Extender la funcionalidad en el subsistema de entrada/salida (e/s).

- Actuar como puntos de unión de directorios y puntos de montaje de volumen.

- Marque ciertos archivos como especiales para un controlador de filtro del sistema de archivos.

## <a name="syntax"></a>Sintaxis

```
fsutil reparsepoint [query] <filename>
fsutil reparsepoint [delete] <filename>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| Query | Recupera los datos del punto de reanálisis asociados al archivo o directorio identificado por el identificador especificado. |
| delete | Elimina un punto de reanálisis del archivo o directorio identificado por el identificador especificado, pero no elimina el archivo o directorio. |
| `<filename>` | Especifica la ruta de acceso completa al archivo, incluido el nombre de archivo y la extensión, por ejemplo *C:\documents\filename.txt*. |

#### <a name="remarks"></a>Observaciones

- Cuando un programa establece un punto de reanálisis, almacena estos datos, además de una etiqueta de reanálisis, que identifica de forma única los datos almacenados. Cuando el sistema de archivos abre un archivo con un punto de reanálisis, intenta encontrar el filtro del sistema de archivos asociado. Si se encuentra el filtro del sistema de archivos, el filtro procesa el archivo según lo indicado por los datos de reanálisis. Si no se encuentra ningún filtro del sistema de archivos, se producirá un error en la operación de **apertura de archivo** .

### <a name="examples"></a>Ejemplos

Para recuperar los datos de punto de reanálisis asociados a *c:\server*, escriba:

```
fsutil reparsepoint query c:\server
```

Para eliminar un punto de análisis de un archivo o directorio especificado, use el formato siguiente:

```
fsutil reparsepoint delete c:\server
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [fsutil](fsutil.md)
