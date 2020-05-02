---
title: extract
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1cca89a356530e49fbf2b0610ff3ced1c5733847
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725656"
---
# <a name="extract"></a>extract



## <a name="syntax"></a>Sintaxis

```
EXTRACT [/Y] [/A] [/D | /E] [/L dir] cabinet [filename ...]
EXTRACT [/Y] source [newname]
EXTRACT [/Y] /C source destination
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|archiva|El archivo contiene dos o más archivos.|
|filename|Nombre del archivo que se va a extraer del archivo. cab. Se pueden usar caracteres comodín y varios nombres de archivo (separados por espacios en blanco).|
|source|Archivos comprimidos (un archivo. cab con un solo archivo).|
|nuevoNombre|Nuevo nombre de archivo para proporcionar el archivo extraído. Si no se proporciona, se utiliza el nombre original.|
|/A|Procesar todos los archivadores. Sigue a la cadena de contenedores empezando en el primer contenedor mencionado.|
|/C|Copiar el archivo de origen en el destino (copiar desde discos DMF).|
|/D|Mostrar el directorio del archivo. cab (use con el nombre de archivo para evitar el extracto).|
|/E|Extraer (use en lugar de *.* para extraer todos los archivos).|
|/L dir|Ubicación para colocar los archivos extraídos (el valor predeterminado es el directorio actual).|
|/Y|No preguntar antes de sobrescribir un archivo existente.|

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)