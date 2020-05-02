---
title: Establecer metadatos
description: Tema de referencia para conjuntos de metadatos, que establece el nombre y la ubicación del archivo de metadatos de creación de instantáneas que se usa para transferir instantáneas de un equipo a otro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 683e54a7efc072d8709d6257771ba6bc5bde206e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721911"
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
|[\<> de unidad:] [<Path>]|Especifica la ubicación en la que se va a crear el archivo de metadatos.|
|\<MetaData. cab>|Especifica el nombre del archivo. cab para almacenar los metadatos de creación de instantáneas.|

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)