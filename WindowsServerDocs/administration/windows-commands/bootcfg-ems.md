---
title: bootcfg ems
description: Tema de referencia del comando bootcfg EMS, que permite al usuario agregar o cambiar la configuración para la redirección de la consola de servicios de administración de emergencia a un equipo remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57abdc50-c64a-45f1-8470-3f8c3a51f743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c85231e094852feb673eb4f99b183f06014234b2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709523"
---
# <a name="bootcfg-ems"></a>bootcfg ems

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Permite al usuario agregar o cambiar la configuración para la redirección de la consola de servicios de administración de emergencia a un equipo remoto. Habilitar los servicios de administración de emergencia `redirect=Port#` , agrega una línea a la sección [boot loader] del archivo boot. ini junto con una opción/redirect en la línea de entrada del sistema operativo especificada. La característica servicios de administración de emergencia solo está habilitada en los servidores.

## <a name="syntax"></a>Sintaxis

```
bootcfg /ems {on | off | edit}[/s <computer> [/u <domain>\<user> /p <password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <osentrylinenum>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `{on | off | edit}` | Especifica el valor para la redirección de servicios de administración de emergencia, incluido:<ul><li>**en.** Habilita la salida remota para el `<osentrylinenum>`especificado. También agrega una opción/redirect al especificado <osentrylinenum> y un `redirect=com<X>` valor a la sección [boot loader]. El valor de `com<X>` se establece mediante el parámetro **/Port** .</li><li>**habilitar.** Deshabilita la salida en un equipo remoto. También quita la opción/redirect del especificado <osentrylinenum> y la `redirect=com<X>` configuración de la sección [boot loader].</li><li>**editar.** Permite realizar cambios en la configuración del puerto `redirect=com<X>` cambiando la configuración en la sección [cargador de arranque]. El valor de `com<X>` se establece mediante el parámetro **/Port** .</li></ul> |
| `/s <computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local. |
| `/u <domain>\<user>`  | Ejecuta el comando con los permisos de cuenta del usuario especificado por `<user>` o `<domain>\<user>`. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
| `/p <password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| `/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}` |  Especifica el puerto COM que se va a utilizar para la redirección. El parámetro BIOSSET dirige los servicios de administración de emergencia para obtener la configuración del BIOS con el fin de determinar qué puerto se debe usar para la redirección. No use este parámetro si la salida administrada de forma remota está deshabilitada. |
| `/baud {9600 | 19200 | 38400 | 57600 | 115200}` | Especifica la velocidad en baudios que se va a utilizar para la redirección. No use este parámetro si la salida administrada de forma remota está deshabilitada. |
| `/id <osentrylinenum>` | Especifica el número de línea de entrada del sistema operativo a la que se agrega la opción servicios de administración de emergencia en la sección [sistemas operativos] del archivo boot. ini. La primera línea después del encabezado de la sección [operating systems] es 1. Este parámetro es necesario cuando el valor de los servicios **de administración de** emergencia está activado o **desactivado**. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para usar el comando **bootcfg/EMS** :

```
bootcfg /ems on /port com1 /baud 19200 /id 2
bootcfg /ems on /port biosset /id 3
bootcfg /s srvmain /ems off /id 2
bootcfg /ems edit /port com2 /baud 115200
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
