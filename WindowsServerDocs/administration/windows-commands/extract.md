---
title: extract
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 967f08e271019cc33970419179c9ddbf902b1882
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377261"
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
|archiva|El archivo contiene dos o más archivos.|
|nombreDeArchivo|Nombre del archivo que se va a extraer del archivo. cab. Se pueden usar caracteres comodín y varios nombres de archivo (separados por espacios en blanco).|
|Fuentes|Archivos comprimidos (un archivo. cab con un solo archivo).|
|NewName|Nuevo nombre de archivo para proporcionar el archivo extraído. Si no se proporciona, se utiliza el nombre original.|
|/A|Procesar todos los archivadores. Sigue a la cadena de contenedores empezando en el primer contenedor mencionado.|
|/C|Copiar el archivo de origen en el destino (copiar desde discos DMF).|
|/D.|Mostrar el directorio del archivo. cab (use con el nombre de archivo para evitar el extracto).|
|/E|Extraer (use en lugar de *.* para extraer todos los archivos).|
|/L dir|Ubicación para colocar los archivos extraídos (el valor predeterminado es el directorio actual).|
|/Y|No preguntar antes de sobrescribir un archivo existente.|

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)