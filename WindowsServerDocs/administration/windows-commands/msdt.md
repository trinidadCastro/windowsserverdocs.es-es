---
title: msdt
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5d31b1b5a73d975aec08d675aaff04ee29c7d3c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723874"
---
# <a name="msdt"></a>msdt



Invoca un paquete de solución de problemas en la línea de comandos o como parte de un script automatizado y habilita opciones adicionales sin intervención del usuario.

## <a name="syntax"></a>Sintaxis

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

### <a name="parameters"></a>Parámetros

En la tabla siguiente se incluyen los parámetros y las opciones admitidas por MSDT. exe.


|      Parámetro      |                                                                                            Descripción                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| nombre \<del paquete/ID> |        Especifica el paquete de diagnóstico que se va a ejecutar. Para obtener una lista de los paquetes disponibles, consulte el ID. del paquete de solución de problemas en la sección paquetes de solución de problemas disponibles más adelante en este tema.         |
|  /path \<(directorio)  |                                                                                           archivo. diagpkg                                                                                            |
|   /DCI \<de clave de paso>   |                                        Rellena previamente el campo clave de paso en MSDT. Este parámetro solo se usa cuando un proveedor de soporte técnico ha proporcionado una clave de paso.                                         |
|  > \<de directorio/DT   | Muestra el historial de solución de problemas en el directorio especificado. Los resultados de diagnóstico se almacenan en los directorios **%LOCALAPPDATA%\Diagnostics** o **%LOCALAPPDATA%\ElevatedDiagnostics** del usuario. |
| > \<del archivo de respuesta/AF  |                                               Especifica un archivo de respuesta en formato XML que contiene respuestas a una o más interacciones de diagnóstico.                                               |

