---
title: bootcfg dbg1394
description: Artículo de referencia para el comando bootcfg dbg1394, que configura la depuración de Puerto 1394 para una entrada de sistema operativo especificada
ms.topic: reference
ms.assetid: 35724697-90dd-4dbe-85b0-337fbd369dcc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 56d91256a74bc247749956bc8948245636b085d1
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630278"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Configura la depuración de Puerto 1394 para una entrada de sistema operativo especificada.

## <a name="syntax"></a>Sintaxis

```
bootcfg /dbg1394 {on | off}[/s <computer> [/u <domain>\<user> /p <password>]] [/ch <channel>] /id <osentrylinenum>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `{on | off}` | Especifica el valor para la depuración de Puerto 1394, incluido:<ul><li>**en.** Habilita la compatibilidad con la depuración remota agregando la opción/dbg1394 al especificado `<osentrylinenum>` .</li><li>**habilitar.** Deshabilita la compatibilidad con la depuración remota quitando la opción/dbg1394 del especificado <osentrylinenum> .</li></ul> |
| `/s <computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). La opción predeterminada es el equipo local. |
| `/u <domain>\<user>`  | Ejecuta el comando con los permisos de cuenta del usuario especificado por `<user>` o `<domain>\<user>` . El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
| `/p <password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| `/ch <channel>` | Especifica el canal que se va a usar para la depuración. Los valores válidos incluyen enteros, entre 1 y 64. No utilice este parámetro si está deshabilitada la depuración de Puerto 1394. |
| `/id <osentrylinenum>` | Especifica el número de línea de entrada del sistema operativo en la sección [sistemas operativos] del archivo de Boot.ini al que se agregan las opciones de carga del sistema operativo. La primera línea después del encabezado de la sección [operating systems] es 1. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para usar el comando **bootcfg/dbg1394**:

```
bootcfg /dbg1394 /id 2
bootcfg /dbg1394 on /ch 1 /id 3
bootcfg /dbg1394 edit /ch 8 /id 2
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
