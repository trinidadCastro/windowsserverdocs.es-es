---
title: extract
description: Artículo de referencia para el comando Extract, que extrae archivos de una ubicación de origen.
ms.topic: reference
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 557ee765a40703d2781613549992c7668753f5a9
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036643"
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
