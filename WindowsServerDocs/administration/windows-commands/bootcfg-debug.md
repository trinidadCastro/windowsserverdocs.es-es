---
title: bootcfg debug
description: Tema de referencia para el comando de depuración Bootcfg, que agrega o cambia la configuración de depuración de una entrada de sistema operativo especificada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 28afa5fb-a236-46e2-b1a4-a3c43a49c437
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8059aaefd1b23b3e74f4c27ba96e322c44b5cb6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709714"
---
# <a name="bootcfg-debug"></a>bootcfg debug

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Agrega o cambia la configuración de depuración de una entrada de sistema operativo especificada.

>[!NOTE]
> Si está intentando depurar el puerto 1394, use en su lugar el comando [bootcfg dbg1394](bootcfg-dbg1394.md) .

## <a name="syntax"></a>Sintaxis

```
bootcfg /debug {on | off | edit}[/s <computer> [/u <domain>\<user> /p <password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <osentrylinenum>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `{on | off | edit}` | Especifica el valor para la depuración de puertos, incluido:<ul><li>**en.** Habilita la compatibilidad con la depuración remota agregando la opción `<osentrylinenum>`/Debug al especificado.</li><li>**habilitar.** Deshabilita la compatibilidad con la depuración remota quitando la opción <osentrylinenum>/Debug del especificado.</li><li>**editar.** Permite realizar cambios en la configuración de puerto y velocidad de baudios cambiando los valores asociados a la opción <osentrylinenum>/debug para el especificado.</li></ul> |
| `/s <computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local. |
| `/u <domain>\<user>`  | Ejecuta el comando con los permisos de cuenta del usuario especificado por `<user>` o `<domain>\<user>`. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
| `/p <password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| `/port {COM1 | COM2 | COM3 | COM4}` |  Especifica el puerto COM que se va a utilizar para la depuración. No utilice este parámetro si está deshabilitada la depuración. |
| `/baud {9600 | 19200 | 38400 | 57600 | 115200}` | Especifica la velocidad en baudios que se va a utilizar para la depuración. No utilice este parámetro si está deshabilitada la depuración. |
| `/id <osentrylinenum>` | Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini en el que se agregan las opciones de carga del sistema operativo. La primera línea después del encabezado de la sección [operating systems] es 1. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para usar el comando **bootcfg/Debug** :

```
bootcfg /debug on /port com1 /id 2
bootcfg /debug edit /port com2 /baud 19200 /id 2
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
