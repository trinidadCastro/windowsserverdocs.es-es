---
title: Establecer metadatos
description: Temas de comandos de Windows para conjuntos de metadatos, que establece el nombre y la ubicación del archivo de metadatos de creación de instantáneas que se usa para transferir instantáneas de un equipo a otro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5bd728650cf163f98a82ff1e6f88755c4cc1aea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834528"
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
|\<MetaData. cab >|Especifica el nombre del archivo. cab para almacenar los metadatos de creación de instantáneas.|

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)