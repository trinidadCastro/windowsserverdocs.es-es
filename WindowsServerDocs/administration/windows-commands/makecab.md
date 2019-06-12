---
title: makecab
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7b120cf990abe2024fd6c96ca2f1ef11fa2350ae
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437530"
---
# <a name="makecab"></a>makecab

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Empaquetar los archivos existentes en un archivo contenedor (.cab).
## <a name="syntax"></a>Sintaxis
```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```
### <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                        Descripción                                                                        |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|       <source>       |                                                                     Archivo que se va a comprimir.                                                                     |
|    <destination>     | Nombre de archivo para proporcionar el archivo comprimido. Si se omite, el último carácter del nombre del archivo de origen es reemplazado por un carácter de subrayado (_) y se usa como destino. |
| /f <directives_file> |                                                   Un archivo con **makecab** directivas (puede repetirse).                                                   |
|    /d var=<value>    |                                                          Define la variable con el valor especificado.                                                           |
|       /l <dir>       |                                               Ubicación de destino (el valor predeterminado es el directorio actual).                                               |
|       /v[<n>]        |                                                    Establecer la depuración de nivel de detalle (0 = none,..., 3 = full).                                                     |
|          /?          |                                                           Muestra la ayuda en el símbolo del sistema.                                                            |

## <a name="remarks"></a>Comentarios
-   Consulte [formato de Microsoft Cabinet](https://go.microsoft.com/fwlink/?LinkId=226852) en MSDN para obtener información sobre directive_file.

## <a name="additional-references"></a>Referencias adicionales
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

