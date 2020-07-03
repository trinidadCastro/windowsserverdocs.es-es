---
title: Establecer metadatos
description: Artículo de referencia para conjuntos de metadatos, que establece el nombre y la ubicación del archivo de metadatos de creación de instantáneas que se usa para transferir instantáneas de un equipo a otro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50c9ceebf072db2e7cefada1601accc97b5d0f7f
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937097"
---
# <a name="set-metadata"></a>Establecer metadatos

Establece el nombre y la ubicación del archivo de metadatos de creación de instantáneas que se usa para transferir instantáneas de un equipo a otro. Si se usa sin parámetros, el comando **set Metadata** muestra la ayuda en el símbolo del sistema.

## <a name="syntax"></a>Sintaxis

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|[\<Drive>:][<Path>]|Especifica la ubicación en la que se va a crear el archivo de metadatos.|
|\<MetaData.cab>|Especifica el nombre del archivo. cab para almacenar los metadatos de creación de instantáneas.|

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)