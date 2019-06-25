---
title: msdt
description: 'Tema de los comandos de Windows para ***- '
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
ms.openlocfilehash: ba411cf73026afe9990e5c32824e3dc277507891
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437231"
---
# <a name="msdt"></a>msdt



Invoca un paquete de solución de problemas en la línea de comandos o como parte de un script automatizado y habilita opciones adicionales sin intervención del usuario.

## <a name="syntax"></a>Sintaxis

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

## <a name="parameters"></a>Parámetros

En la tabla siguiente incluye los parámetros y las opciones admitidas por msdt.exe.


|      Parámetro      |                                                                                            Descripción                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| / ID \<nombre del paquete > |        Especifica qué paquete de diagnóstico para ejecutar. ¿Para obtener una lista de paquetes disponibles, vea el identificador del paquete de solución de problemas en la "solución de problemas de paquetes disponible? sección más adelante en este tema.         |
|  / Path \<directorio  |                                                                                           archivo .diagpkg                                                                                            |
|   /dci \<passkey>   |                                        Rellena el campo de clave de paso en msdt. Este parámetro solo se usa cuando un proveedor de soporte ha suministrado una clave de paso.                                         |
|  /dt \<directory>   | Muestra el historial de la solución de problemas en el directorio especificado. Los resultados de diagnóstico se almacenan en el usuario **%LOCALAPPDATA%\Diagnostics** o **%LOCALAPPDATA%\ElevatedDiagnostics** directorios. |
| /AF \<archivo de respuesta >  |                                               Especifica un archivo de respuesta en formato XML que contiene las respuestas a las interacciones de diagnóstico de uno o más.                                               |
