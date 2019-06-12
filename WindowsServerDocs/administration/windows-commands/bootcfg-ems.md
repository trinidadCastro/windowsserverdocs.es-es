---
title: bootcfg ems
description: Tema de los comandos de Windows para **bootcfg ems** -permite al usuario agregar o cambiar la configuración de redirección de la consola de servicios de administración de emergencia en un equipo remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57abdc50-c64a-45f1-8470-3f8c3a51f743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d971fab552b321d54899b418eac4f54c4665ccb7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434737"
---
# <a name="bootcfg-ems"></a>bootcfg ems

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite al usuario agregar o cambiar la configuración de redirección de la consola de servicios de administración de emergencia en un equipo remoto. Al habilitar servicios de administración de emergencia, se agrega un "redirect = número de puerto" línea a la sección [boot loader] del archivo Boot.ini y la opción /redirect a la línea de entrada de sistema operativo especificado. La característica Servicios de administración de emergencia está habilitada solo en los servidores.

## <a name="syntax"></a>Sintaxis
```
bootcfg /ems {ON | OFF | edit} [/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parámetros

|                            Parámetro                             |                                                                                                                                                                                                                                                                                                                                                              Descripción                                                                                                                                                                                                                                                                                                                                                              |
|------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    {ON &#124; OFF&#124; edit}                    | Especifica el valor para la redirección de los servicios de administración de emergencia.<br /><br />**ON** -habilita la salida remota para el elemento especificado <OSEntryLineNum>. Agrega una opción /redirect a especificado <OSEntryLineNum> y una redirección = com<X> si se establece en la sección [boot loader]. El valor de com<X> se establece mediante la **/puerto** parámetro.<br /><br />**DESACTIVAR** -deshabilita la salida a un equipo remoto. Quita la opción /redirect especificado <OSEntryLineNum> la redirección = com<X> configuración desde la sección [boot loader].<br /><br />**Editar** -permite que los cambios de configuración de puerto cambiando la redirección = com<X> en la sección [boot loader]. El valor de com<X> se restablece al valor especificado por el **/puerto** parámetro. |
|                          /s <computer>                           |                                                                                                                                                                                                                                                                                                          Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.                                                                                                                                                                                                                                                                                                           |
|                       /u <Domain>\\<User>                        |                                                                                                                                                                                                                                                                 Ejecuta el comando con los permisos de cuenta del usuario especificado por <User> o <Domain> \\ <User>. El valor predeterminado es los permisos de la sesión de usuario en el equipo que emite el comando actual.                                                                                                                                                                                                                                                                  |
|                          /p <Password>                           |                                                                                                                                                                                                                                                                                                                         Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.                                                                                                                                                                                                                                                                                                                         |
| /port {COM1 &#124; COM2 &#124; COM3 &#124; COM4 &#124; BIOSSET}  |                                                                                                                                                                                                                              Especifica el puerto COM que se usará para la redirección. **BIOSSET** dirige los servicios de administración de emergencia para obtener la configuración del BIOS para determinar qué puerto se debe usar para la redirección. No utilice el **/puerto** parámetro si se administran de forma remota salida que se va a deshabilitar.                                                                                                                                                                                                                              |
| /baud {9600 &#124; 19200 &#124; 38400&#124; 57600 &#124; 115200} |                                                                                                                                                                                                                                                                                               Especifica la velocidad en baudios que se usará para la redirección. No utilice el **/baud** parámetro si se administran de forma remota salida que se va a deshabilitar.                                                                                                                                                                                                                                                                                               |
|                       / Id <OSEntryLineNum>                       |                                                                                                                                                                                              Especifica el número de línea de entrada de sistema operativo al que se agrega la opción Emergency Management Services en la sección [operating systems] del archivo Boot.ini. La primera línea después de encabezado de sección de la sección [operating systems] es 1. Este parámetro es obligatorio cuando se establece el valor de los servicios de administración de emergencia en **ON** o **OFF**.                                                                                                                                                                                              |
|                                /?                                |                                                                                                                                                                                                                                                                                                                                                 Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                                                                                                                                                                  |

## <a name="BKMK_examples"></a>Ejemplos
Los ejemplos siguientes muestran cómo puede usar el **bootcfg /ems** comando:
```
bootcfg /ems on /port com1 /baud 19200 /id 2 
bootcfg /ems on /port biosset /id 3 
bootcfg /s srvmain /ems off /id 2 
bootcfg /ems edit /port com2 /baud 115200 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
