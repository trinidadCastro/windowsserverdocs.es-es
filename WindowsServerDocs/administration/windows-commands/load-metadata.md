---
title: Cargar metadatos
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d3132bca86533ec3f2d0a27247bd3c116cf55b6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724464"
---
# <a name="load-metadata"></a>Cargar metadatos



Carga un archivo Metadata. cab antes de importar una instantánea transportable o carga los metadatos del escritor en el caso de una restauración. Si se usa sin parámetros, **cargar metadatos** muestra la ayuda en el símbolo del sistema.



## <a name="syntax"></a>Sintaxis

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<> de unidad:] [<Path>]|Especifica la ubicación del archivo de metadatos.|
|MetaData. cab|Especifica el archivo Metadata. cab que se va a cargar.|

## <a name="remarks"></a>Observaciones

-   Puede usar el comando **importar** para importar una instantánea transportable en función de los metadatos especificados por los **metadatos de carga**.
-   Este comando es necesario antes del comando **Begin restore** para cargar los escritores y componentes seleccionados para la restauración.

## <a name="examples"></a>Ejemplos

Para cargar un archivo de metadatos denominado Metafile. cab desde la ubicación predeterminada, escriba:
```
load metadata metafile.cab
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)