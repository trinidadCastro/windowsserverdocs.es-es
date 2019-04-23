---
title: extract
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9113c34b61b98fb738bc0aff03193ab73b1abbd7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882316"
---
# <a name="extract"></a>extract



## <a name="syntax"></a>Sintaxis

```
EXTRACT [/Y] [/A] [/D | /E] [/L dir] cabinet [filename ...]
EXTRACT [/Y] source [newname]
EXTRACT [/Y] /C source destination
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|cabinet|Archivo contiene dos o más archivos.|
|nombreDeArchivo|Nombre del archivo para extraer el contenedor. Se pueden usar caracteres comodín y varios nombres de archivo (separadas por espacios en blanco).|
|Código fuente|Archivo comprimido (un contenedor con un solo archivo).|
|NewName|Nuevo nombre de archivo para proporcionar el archivo extraído. Si no se proporciona, se usa el nombre original.|
|/A|Procesar todos los archivadores. Sigue la cadena de contenedores comenzando en el primer archivo .cab que se ha mencionado.|
|/C|Copie el archivo de origen al destino (para copiar desde discos DMF).|
|/D|Mostrar el directorio (se usa con el nombre de archivo para evitar la extracción).|
|/E|Extraer (usar en lugar de *.* para extraer todos los archivos).|
|/L dir|Ubicación para colocar los archivos extraídos (valor predeterminado es el directorio actual).|
|/Y|No preguntar antes de sobrescribir un archivo existente.|

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)