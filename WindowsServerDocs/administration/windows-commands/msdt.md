---
title: msdt
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87c1e4e8cd6d9de036b47de590867a6531d0335a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839258"
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
| /ID \<nombre del paquete > |        Especifica el paquete de diagnóstico que se va a ejecutar. Para obtener una lista de los paquetes disponibles, consulte el ID. del paquete de solución de problemas en la sección paquetes de solución de problemas disponibles más adelante en este tema.         |
|  directorio/path \<  |                                                                                           archivo. diagpkg                                                                                            |
|   /DCI \<clave de paso >   |                                        Rellena previamente el campo clave de paso en MSDT. Este parámetro solo se usa cuando un proveedor de soporte técnico ha proporcionado una clave de paso.                                         |
|  >/DT \<   | Muestra el historial de solución de problemas en el directorio especificado. Los resultados de diagnóstico se almacenan en los directorios **%LOCALAPPDATA%\Diagnostics** o **%LOCALAPPDATA%\ElevatedDiagnostics** del usuario. |
| /AF \<archivo de respuesta >  |                                               Especifica un archivo de respuesta en formato XML que contiene respuestas a una o más interacciones de diagnóstico.                                               |

