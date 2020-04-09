---
title: ruta de acceso
description: Obtenga información sobre cómo establecer la variable de entorno PATH.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cb77cac3871dcf4a411638409de68d038a317d24
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837718"
---
# <a name="path"></a>ruta de acceso



Establece la ruta de acceso del comando en la variable de entorno PATH (el conjunto de directorios que se usa para buscar archivos ejecutables). Si se usa sin parámetros, la **ruta de acceso** muestra la ruta de acceso del comando actual.

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
path [[<Drive>:]<Path>[;...][;%PATH%]]
path ;
```

### <a name="parameters"></a>Parámetros

|     Parámetro     |                                                                                                     Descripción                                                                                                      |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<> de unidad:]<Path> |                                                                            Especifica la unidad y el directorio que se establecerán en la ruta de acceso del comando.                                                                             |
|         ;         | Separa los directorios en la ruta de acceso del comando. Si se usa sin otros parámetros **, se** borran las rutas de comandos existentes de la variable de entorno PATH y se indica a cmd. exe que busque solo en el directorio actual. |
|      CAMINO       |                                                         Anexa la ruta de acceso del comando al conjunto de directorios existente que aparece en la variable de entorno PATH.                                                         |
|        /?         |                                                                                         Muestra la Ayuda en el símbolo del sistema.                                                                                         |

## <a name="remarks"></a>Comentarios

-   Cuando se incluye **% path%** en la sintaxis, cmd. exe lo reemplaza con los valores de ruta de acceso de comando que se encuentran en la variable de entorno PATH, lo que elimina la necesidad de escribir manualmente estos valores en el símbolo del sistema.
-   Siempre se busca en el directorio actual antes de los directorios especificados en la ruta de acceso del comando.
-   Es posible que tenga archivos en un directorio que compartan el mismo nombre de archivo, pero que tengan extensiones diferentes. Por ejemplo, podría tener un archivo denominado Accnt.com que inicia un programa de contabilidad y otro archivo denominado Accnt. bat que conecta el servidor a la red de sistema de contabilidad.

    El sistema operativo Windows busca un archivo con las extensiones de nombre de archivo predeterminadas en el siguiente orden de prioridad:. exe,. com,. bat y. cmd. Para ejecutar Accnt. bat cuando Accnt.com existe en el mismo directorio, debe incluir la extensión. bat en el símbolo del sistema.
-   Si dos o más archivos de la ruta de acceso de comandos tienen el mismo nombre de archivo y la misma extensión, **path** busca primero el nombre de archivo especificado en el directorio actual. A continuación, busca en los directorios de la ruta de comandos en el orden en que aparecen en la variable de entorno PATH.
-   Si coloca el comando **path** en el archivo Autoexec. NT, el sistema operativo Windows anexará automáticamente la ruta de acceso de búsqueda del subsistema de ms-dos especificada cada vez que inicie sesión en el equipo. Cmd. exe no utiliza el archivo Autoexec. NT. Cuando se inicia desde un acceso directo, cmd. exe hereda las variables de entorno establecidas en Mi PC/propiedades/avanzado/entorno.

## <a name="examples"></a><a name="BKMK_examples"></a>Example

Para buscar las rutas de acceso C:\User\Taxes, B:\User\Invest y B:\Bin para comandos externos, escriba:

`path c:\user\taxes;b:\user\invest;b:\bin`

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)