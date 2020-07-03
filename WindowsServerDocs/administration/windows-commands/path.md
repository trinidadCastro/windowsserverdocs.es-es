---
title: path
description: Artículo de referencia para establecer la ruta de acceso de comandos en la variable de entorno PATH, que especifica el conjunto de directorios que se usa para buscar archivos ejecutables (. exe).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f324c2b0fc84d2df05f7df93d83799b3ac463d5d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932023"
---
# <a name="path"></a>path

Establece la ruta de acceso del comando en la variable de entorno PATH, especificando el conjunto de directorios que se usa para buscar archivos ejecutables (. exe). Si se usa sin parámetros, este comando muestra la ruta de acceso del comando actual.

## <a name="syntax"></a>Sintaxis

```
path [[<drive>:]<path>[;...][;%PATH%]]
path ;
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `[<drive>:]<path>` | Especifica la unidad y el directorio que se establecerán en la ruta de acceso del comando. Siempre se busca en el directorio actual antes de los directorios especificados en la ruta de acceso del comando. |
| ; | Separa los directorios en la ruta de acceso del comando. Si se usa sin otros parámetros, **, borra las** rutas de comandos existentes de la variable de entorno PATH y dirige Cmd.exe para buscar solo en el directorio actual. |
| `%PATH%` | Anexa la ruta de acceso del comando al conjunto de directorios existente que aparece en la variable de entorno PATH. Si incluye este parámetro, Cmd.exe lo reemplaza por los valores de la ruta de acceso del comando que se encuentra en la variable de entorno PATH, lo que elimina la necesidad de escribir manualmente estos valores en el símbolo del sistema. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="remarks"></a>Comentarios


- El sistema operativo Windows busca con las extensiones de nombre de archivo predeterminadas en el siguiente orden de prioridad:. exe,. com,. bat y. cmd. Lo que significa que si busca un archivo por lotes denominado acct.bat, pero tiene una aplicación denominada acct.exe en el mismo directorio, debe incluir la extensión. bat en el símbolo del sistema.

- Si dos o más archivos de la ruta de acceso de comandos tienen el mismo nombre de archivo y la misma extensión, este comando busca primero el nombre de archivo especificado en el directorio actual. A continuación, busca en los directorios de la ruta de comandos en el orden en que aparecen en la variable de entorno PATH.

- Si coloca el comando **path** en el archivo Autoexec. NT, el sistema operativo Windows anexará automáticamente la ruta de acceso de búsqueda del subsistema de ms-dos especificada cada vez que inicie sesión en el equipo. Cmd.exe no utiliza el archivo Autoexec. NT. Cuando se inicia desde un acceso directo, Cmd.exe hereda las variables de entorno establecidas en Mi PC/propiedades/avanzadas/entorno.

## <a name="examples"></a>Ejemplos

Para buscar las rutas de acceso *c:\user\taxes*, *b:\user\invest*y *b:\Bin* para comandos externos, escriba:

```
path c:\user\taxes;b:\user\invest;b:\bin
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
