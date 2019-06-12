---
title: bootcfg debug
description: 'Tema de los comandos de Windows para **bootcfg depuración** : agrega o cambia la configuración de depuración para una entrada de sistema operativo especificado.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 28afa5fb-a236-46e2-b1a4-a3c43a49c437
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44a1145384a62d30f055cb48fd7ed6adccd2c69b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434823"
---
# <a name="bootcfg-debug"></a>bootcfg debug

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Agrega o cambia la configuración de depuración para una entrada de sistema operativo especificado.

## <a name="syntax"></a>Sintaxis
```
bootcfg /debug {ON | OFF | edit}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parámetros

|                           Parámetro                           |                                                                                                                                                                                                                    Descripción                                                                                                                                                                                                                    |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  {ON &#124; OFF&#124; edit}                   | Especifica el valor para la depuración.<br /><br />**ON** -habilita la compatibilidad con la depuración remota agregando la opción/debug a especificado <OSEntryLineNum>.<br /><br />**DESACTIVAR** -deshabilita la compatibilidad con la depuración remota mediante la eliminación de la opción/debug especificado <OSEntryLineNum>.<br /><br />**Editar** -permite la configuración de velocidad de cambios en baudios y el puerto cambiando los valores asociados con la opción/debug para el elemento especificado <OSEntryLineNum>. |
|                         /s <computer>                         |                                                                                                                                                                Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.                                                                                                                                                                 |
|                      /u <Domain>\\<User>                      |                                                                                                                       Ejecuta el comando con los permisos de cuenta del usuario especificado por <User> o <Domain> \\ <User>. El valor predeterminado es los permisos de la sesión de usuario en el equipo que emite el comando actual.                                                                                                                        |
|                         /p <Password>                         |                                                                                                                                                                               Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.                                                                                                                                                                               |
|       /port {COM1 &#124; COM2 &#124; COM3 &#124; COM4}        |                                                                                                                                                                Especifica el puerto COM que se usará para la depuración. No utilice el **/puerto** parámetro si la depuración se va a deshabilitar.                                                                                                                                                                |
| /baud {9600&#124; 19200&#124; 38400&#124; 57600&#124; 115200} |                                                                                                                                                               Especifica la velocidad en baudios que se usará para la depuración. No utilice el **/baud** parámetro si la depuración se va a deshabilitar.                                                                                                                                                                |
|                     / Id <OSEntryLineNum>                      |                                                                                                               Especifica el número de línea de entrada de sistema operativo en la sección [operating systems] del archivo Boot.ini a la que se agregan las opciones de depuración. La primera línea después de encabezado de sección de la sección [operating systems] es 1.                                                                                                                |
|                              /?                               |                                                                                                                                                                                                       Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                        |

##### <a name="remarks"></a>Comentarios
- Si el puerto de depuración 1394 es necesario, utilice [bootcfg dbg1394](bootcfg-dbg1394.md).
  ## <a name="BKMK_examples"></a>Ejemplos
  Los ejemplos siguientes muestran cómo puede usar el **/Debug bootcfg**comando:
  ```
  bootcfg /debug on /port com1 /id 2 
  bootcfg /debug edit /port com2 /baud 19200 /id 2 
  bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
  ```
  #### <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
