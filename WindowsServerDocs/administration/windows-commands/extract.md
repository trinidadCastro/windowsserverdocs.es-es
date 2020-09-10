---
title: extract
description: Artículo de referencia para el comando Extract, que extrae archivos de una ubicación de origen.
ms.topic: reference
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 38d73bb18c210c27859add9a419adedfb4fecac4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627663"
---
# <a name="extract"></a>extract

Extrae archivos de un archivo. cab o un origen.

## <a name="syntax"></a>Sintaxis

```
extract [/y] [/a] [/d | /e] [/l dir] cabinet [filename ...]
extract [/y] source [newname]
extract [/y] /c source destination
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| archiva | Use si desea extraer dos o más archivos. |
| filename | Nombre del archivo que se va a extraer del archivo. cab. Se pueden usar caracteres comodín y varios nombres de archivo (separados por espacios en blanco). |
| source | Archivos comprimidos (un archivo. cab con un solo archivo). |
| nuevoNombre | Nuevo nombre de archivo para proporcionar el archivo extraído. Si no se proporciona, se utiliza el nombre original. |
| /a | Procesar todos los archivadores. Sigue a la cadena de contenedores empezando en el primer contenedor mencionado. |
| /C | Copiar el archivo de origen en el destino (copiar desde discos DMF). |
| /d | Mostrar el directorio del archivo. cab (use con el nombre de archivo para evitar el extracto). |
| /e | Extraer (use en lugar de *.* para extraer todos los archivos). |
| /l dir | Ubicación para colocar los archivos extraídos (el valor predeterminado es el directorio actual). |
| /y | No preguntar antes de sobrescribir un archivo existente. |

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
