---
title: makecab
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46efbae1d59a85071622df51fc018d0bf73dc504
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840268"
---
# <a name="makecab"></a>makecab

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
| /f < directives_file > |                                                   Un archivo con directivas **MAKECAB** (se puede repetir).                                                   |
|    /d var =<value>    |                                                          Define la variable con el valor especificado.                                                           |
|       /l <dir>       |                                               Ubicación en la que se va a colocar el destino (el valor predeterminado es el directorio actual).                                               |
|       /v [<n>]        |                                                    Establezca el nivel de detalle de depuración (0 = ninguno,..., 3 = completo).                                                     |
|          /?          |                                                           Muestra la Ayuda en el símbolo del sistema.                                                            |

## <a name="remarks"></a>Comentarios
-   Consulte [formato de archivo. cab de Microsoft](https://go.microsoft.com/fwlink/?LinkId=226852) en MSDN para obtener información sobre directive_file.

## <a name="additional-references"></a>Referencias adicionales
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

