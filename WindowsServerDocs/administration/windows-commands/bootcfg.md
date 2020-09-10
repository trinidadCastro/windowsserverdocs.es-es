---
title: bootcfg
description: Artículo de referencia para el comando bootcfg, que configura, consulta o cambia Boot.ini configuración del archivo.
ms.topic: reference
ms.assetid: 3deb354c-5717-4066-bc79-b9323d559e44
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6f187015ab3c8cba18e34601faebc1637ecf7896
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630094"
---
# <a name="bootcfg"></a>bootcfg

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Configura, consulta o cambia la configuración del archivo Boot.ini.

## <a name="syntax"></a>Sintaxis

```
bootcfg <parameter> [arguments...]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| [bootcfg addsw](bootcfg-addsw.md) | Agrega opciones de carga del sistema operativo para una entrada específica del sistema operativo. |
| [bootcfg copy](bootcfg-copy.md) | Realiza una copia de una entrada de arranque existente, a la que puede agregar opciones de la línea de comandos. |
| [bootcfg dbg1394](bootcfg-dbg1394.md) | Configura la depuración de Puerto 1394 para una entrada de sistema operativo especificada. |
| [bootcfg debug](bootcfg-debug.md) | Agrega o cambia la configuración de depuración de una entrada de sistema operativo especificada. |
| [bootcfg default](bootcfg-default.md) | Especifica la entrada del sistema operativo que se designará como predeterminada. |
| [bootcfg delete](bootcfg-delete.md) | Elimina una entrada de sistema operativo en la sección [sistemas operativos] del archivo de Boot.ini. |
| [bootcfg ems](bootcfg-ems.md) | Permite al usuario agregar o cambiar la configuración para la redirección de la consola de servicios de administración de emergencia a un equipo remoto. |
| [bootcfg query](bootcfg-query.md) | Consulta y muestra las entradas de la sección [cargador de arranque] y [sistemas operativos] de Boot.ini. |
| [bootcfg raw](bootcfg-raw.md) | Agrega opciones de carga del sistema operativo especificadas como una cadena a una entrada del sistema operativo en la sección [sistemas operativos] del archivo de Boot.ini. |
| [bootcfg rmsw](bootcfg-rmsw.md) | Quita las opciones de carga del sistema operativo para una entrada de sistema operativo especificada. |
| [bootcfg timeout](bootcfg-timeout.md) | Cambia el valor de tiempo de espera del sistema operativo. |
