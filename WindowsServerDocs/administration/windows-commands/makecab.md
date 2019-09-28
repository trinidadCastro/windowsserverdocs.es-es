---
title: makecab
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0231b6f1ddd3e81caa7544587f764e2308015b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374148"
---
# <a name="makecab"></a>makecab

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Empaquetar los archivos existentes en un archivo contenedor (. cab).
## <a name="syntax"></a>Sintaxis
```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```
### <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                        Descripción                                                                        |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|       <source>       |                                                                     Archivo que se va a comprimir.                                                                     |
|    <destination>     | Nombre de archivo que se va a asignar al archivo comprimido. Si se omite, el último carácter del nombre del archivo de código fuente se sustituye por un carácter de subrayado (_) y se usa como destino. |
| /f < directives_file > |                                                   Un archivo con directivas **MAKECAB** (se puede repetir).                                                   |
|    /d var =<value>    |                                                          Define la variable con el valor especificado.                                                           |
|       /l <dir>       |                                               Ubicación en la que se va a colocar el destino (el valor predeterminado es el directorio actual).                                               |
|       /v [<n>]        |                                                    Establezca el nivel de detalle de depuración (0 = ninguno,..., 3 = completo).                                                     |
|          /?          |                                                           Muestra la ayuda en el símbolo del sistema.                                                            |

## <a name="remarks"></a>Comentarios
-   Consulte [formato de archivo. cab de Microsoft](https://go.microsoft.com/fwlink/?LinkId=226852) en MSDN para obtener información sobre directive_file.

## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

