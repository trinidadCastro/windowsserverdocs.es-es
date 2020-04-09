---
title: lodctr
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a849abd-6b31-4833-bc8a-306c05eca29a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 33c14970a669d24f1cc803003e8530712311c564
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840958"
---
# <a name="lodctr"></a>lodctr

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite registrar o guardar la configuración del registro y el nombre del contador de rendimiento en un archivo y designar los servicios de confianza.
## <a name="syntax"></a>Sintaxis
```
lodctr <filename> [/s:<filename>] [/r:<filename>] [/t:<servicename>]
```
#### <a name="parameters"></a>Parámetros

|    Parámetro     |                                                                                                                                         Descripción                                                                                                                                          |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <filename>    |                                                                                          Registra la configuración del nombre del contador de rendimiento y el texto explicativo que se proporciona en el archivo de inicialización FileName.                                                                                          |
|  /s:<filename>   |                                                                                                       Guarda la configuración del registro del contador de rendimiento y el texto explicativo en el <filename>de archivo.                                                                                                       |
|        /r        |                                Restaura la configuración del registro de contadores y el texto explicativo de la configuración actual del registro y los archivos de rendimiento almacenados en caché relacionados con el registro.<p>Esta opción solo está disponible en el sistema operativo Windows Server 2003.                                |
|  /r:<filename>   | Restaura la configuración del registro del contador de rendimiento y el texto explicativo del archivo <filename>. **ADVERTENCIA:** si usa el comando **LODCTR/r** , sobrescribirá todos los valores del registro de contadores de rendimiento y el texto explicativo y los reemplazará por la configuración definida en el archivo especificado. |
| /t:<servicename> |                                                                                                                       Indica que el <servicename> del servicio es de confianza.                                                                                                                       |
|        /?        |                                                                                                                             Muestra la Ayuda en el símbolo del sistema.                                                                                                                             |

## <a name="remarks"></a>Comentarios
Si la información proporcionada contiene espacios, utilice comillas alrededor del texto (por ejemplo, <filename>).
## <a name="examples"></a><a name=BKMK_Examples></a>Example
Para guardar la configuración actual del registro de rendimiento y el texto de explicación del contador en el archivo **Perf 1. txt**:
```
lodctr /s:perf backup1.txt
```
## <a name="additional-references"></a>Referencias adicionales
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

