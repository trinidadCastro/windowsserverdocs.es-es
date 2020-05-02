---
title: makecab
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a0bf4afda09f0e0e8416777a2cfd1404d4bf59a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724222"
---
# <a name="makecab"></a>makecab

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Empaquetar los archivos existentes en un archivo contenedor (. cab).
## <a name="syntax"></a>Sintaxis
```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```
#### <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                        Descripción                                                                        |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|       <source>       |                                                                     Archivo que se va a comprimir.                                                                     |
|    <destination>     | Nombre de archivo que se va a asignar al archivo comprimido. Si se omite, el último carácter del nombre del archivo de código fuente se sustituye por un carácter de subrayado (_) y se usa como destino. |
| /f <directives_file> |                                                   Un archivo con directivas **MAKECAB** (se puede repetir).                                                   |
|    /d var =<value>    |                                                          Define la variable con el valor especificado.                                                           |
|       l<dir>       |                                               Ubicación en la que se va a colocar el destino (el valor predeterminado es el directorio actual).                                               |
|       /v [<n>]        |                                                    Establezca el nivel de detalle de depuración (0 = ninguno,..., 3 = completo).                                                     |
|          /?          |                                                           Muestra la ayuda en el símbolo del sistema.                                                            |

## <a name="remarks"></a>Observaciones
-   Consulte [formato de archivo. cab de Microsoft](https://go.microsoft.com/fwlink/?LinkId=226852) en MSDN para obtener información sobre directive_file.

## <a name="additional-references"></a>Referencias adicionales
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

