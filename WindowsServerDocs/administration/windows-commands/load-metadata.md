---
title: Cargar metadatos
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b52b5040fc8c834b04cad83ca4b0cfab103fdc43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871336"
---
# <a name="load-metadata"></a>Cargar metadatos



Carga un archivo .cab de metadatos antes de importar una copia de instantáneas transportables o carga los metadatos del escritor en el caso de una restauración. Si se utiliza sin parámetros, **cargar metadatos** muestra la Ayuda en el símbolo del sistema.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive>:][<Path>]|Especifica la ubicación del archivo de metadatos.|
|MetaData.cab|Especifica el archivo .cab de metadatos para cargar.|

## <a name="remarks"></a>Comentarios

-   Puede usar el **importar** comando para importar una copia de instantáneas transportables basándose en los metadatos especificados por **cargar metadatos**.
-   Este comando es necesaria antes de la **comenzar restauración** comando para cargar los componentes para la restauración y escritores seleccionados.

## <a name="BKMK_examples"></a>Ejemplos

Para cargar un archivo de metadatos denominado metafile.cab desde la ubicación predeterminada, escriba:
```
load metadata metafile.cab
```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)