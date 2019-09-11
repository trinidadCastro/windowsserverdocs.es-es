---
title: msdt
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7bec16ab3f716148bb009dd56be475fcd058a897
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868900"
---
# <a name="msdt"></a>msdt



Invoca un paquete de solución de problemas en la línea de comandos o como parte de un script automatizado y habilita opciones adicionales sin intervención del usuario.

## <a name="syntax"></a>Sintaxis

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

## <a name="parameters"></a>Parámetros

En la tabla siguiente se incluyen los parámetros y las opciones admitidas por MSDT. exe.


|      Parámetro      |                                                                                            Descripción                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| nombre \<del paquete/ID > |        Especifica el paquete de diagnóstico que se va a ejecutar. Para obtener una lista de los paquetes disponibles, vea el ID. del paquete de solución de problemas en la sección "módulos de solución de problemas disponibles" más adelante en este tema.         |
|  /path \<(directorio)  |                                                                                           archivo. diagpkg                                                                                            |
|   /DCI \<de clave de paso >   |                                        Rellena previamente el campo clave de paso en MSDT. Este parámetro solo se usa cuando un proveedor de soporte técnico ha proporcionado una clave de paso.                                         |
|  > \<de directorio/DT   | Muestra el historial de solución de problemas en el directorio especificado. Los resultados de diagnóstico se almacenan en los directorios **%LOCALAPPDATA%\Diagnostics** o **%LOCALAPPDATA%\ElevatedDiagnostics** del usuario. |
| > \<del archivo de respuesta/AF  |                                               Especifica un archivo de respuesta en formato XML que contiene respuestas a una o más interacciones de diagnóstico.                                               |

