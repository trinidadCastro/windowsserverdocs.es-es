---
title: cargar metadatos
description: Tema de referencia para el comando cargar metadatos, que carga un archivo Metadata. cab antes de importar una instantánea transportable o cargar los metadatos del escritor en el caso de una restauración.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c7dc967476412261e7afc228088566f74ec4208c
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820195"
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
| Metadata. cab | Especifica el archivo Metadata. cab que se va a cargar. |

## <a name="remarks"></a>Observaciones

- Puede usar el comando **importar** para importar una instantánea transportable en función de los metadatos especificados por los **metadatos de carga**.

- Debe ejecutar este comando antes del comando **Begin restore** para cargar los escritores y componentes seleccionados para la restauración.

## <a name="examples"></a>Ejemplos

Para cargar un archivo de metadatos denominado Metafile. cab desde la ubicación predeterminada, escriba:

```
load metadata metafile.cab
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [importar comando de DiskShadow](import.md)

- [comando Begin restore](begin-restore.md)
