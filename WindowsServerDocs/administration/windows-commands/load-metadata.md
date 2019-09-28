---
title: Cargar metadatos
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 025f75743d61889c4b987e9a2a575d1c599f04c1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374619"
---
# <a name="load-metadata"></a>Cargar metadatos



Carga un archivo Metadata. cab antes de importar una instantánea transportable o carga los metadatos del escritor en el caso de una restauración. Si se usa sin parámetros, **cargar metadatos** muestra la ayuda en el símbolo del sistema.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive >:] [<Path>]|Especifica la ubicación del archivo de metadatos.|
|MetaData. cab|Especifica el archivo Metadata. cab que se va a cargar.|

## <a name="remarks"></a>Comentarios

-   Puede usar el comando **importar** para importar una instantánea transportable en función de los metadatos especificados por los **metadatos de carga**.
-   Este comando es necesario antes del comando **Begin restore** para cargar los escritores y componentes seleccionados para la restauración.

## <a name="BKMK_examples"></a>Example

Para cargar un archivo de metadatos denominado Metafile. cab desde la ubicación predeterminada, escriba:
```
load metadata metafile.cab
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)