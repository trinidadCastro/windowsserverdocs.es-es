---
title: load metadata
description: Artículo de referencia para el comando cargar metadatos, que carga un archivo Metadata. cab antes de importar una instantánea transportable o carga los metadatos del escritor en el caso de una restauración.
ms.topic: reference
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9953b06b46b82466dbf4fcba97c2d902c88d301c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037893"
---
# <a name="load-metadata"></a>Cargar metadatos

Carga un archivo Metadata. cab antes de importar una instantánea transportable o carga los metadatos del escritor en el caso de una restauración. Si se usa sin parámetros, **cargar metadatos** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
load metadata [<drive>:][<path>]<metadata.cab>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `[<drive>:][<path>]` | Especifica la ubicación del archivo de metadatos. |
| metadata.cab | Especifica el archivo Metadata. cab que se va a cargar. |

## <a name="remarks"></a>Observaciones

- Puede usar el comando **importar** para importar una instantánea transportable en función de los metadatos especificados por los **metadatos de carga**.

- Debe ejecutar este comando antes del comando **Begin restore** para cargar los escritores y componentes seleccionados para la restauración.

## <a name="examples"></a>Ejemplos

Para cargar un archivo de metadatos llamado metafile.cab desde la ubicación predeterminada, escriba:

```
load metadata metafile.cab
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [importar comando de DiskShadow](import.md)

- [comando Begin restore](begin-restore.md)
