---
title: bootcfg ems
description: 'Tema de comandos de Windows para **bootcfg EMS** : permite al usuario agregar o cambiar la configuración para la redirección de la consola de servicios de administración de emergencia a un equipo remoto.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c0e761f3796121cc71be88d23e25371369a3ee90
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379957"
---
# <a name="bootcfg-ems"></a>bootcfg ems

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite al usuario agregar o cambiar la configuración para la redirección de la consola de servicios de administración de emergencia a un equipo remoto. Al habilitar los servicios de administración de emergencia, se agrega una línea "Redirect = Port #" a la sección [boot loader] del archivo boot. ini y una opción/redirect en la línea de entrada del sistema operativo especificada. La característica servicios de administración de emergencia solo está habilitada en los servidores.

## <a name="syntax"></a>Sintaxis
```
bootcfg /ems {ON | OFF | edit} [/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parámetros

|                            Parámetro                             |                                                                                                                                                                                                                                                                                                                                                              Descripción                                                                                                                                                                                                                                                                                                                                                              |
|------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    {ON &#124; OFF&#124; Edit}                    | Especifica el valor para la redirección de servicios de administración de emergencia.<br /><br />**On** : habilita la salida remota para el @no__t especificado-1. agrega una opción/redirect al parámetro <OSEntryLineNum> especificado y un valor redirect = com @ no__t-1 a la sección [boot loader]. El valor de com @ no__t-0 se establece mediante el parámetro **/Port** .<br /><br />**Desactivado** : deshabilita la salida en un equipo remoto. quita la opción/redirect del parámetro especificado <OSEntryLineNum> y redirect = com @ no__t-1 de la sección [boot loader].<br /><br />**Editar** : permite realizar cambios en la configuración del puerto cambiando el valor redirect = com @ no__t-1 en la sección [boot loader]. El valor de com @ no__t-0 se restablece en el valor especificado por el parámetro **/Port** . |
|                          /s <computer>                           |                                                                                                                                                                                                                                                                                                          Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                                                                                                                                                                                                                                                                                           |
|                       /u <Domain> @ no__t-1 @ no__t-2                        |                                                                                                                                                                                                                                                                 Ejecuta el comando con los permisos de cuenta del usuario especificado por <User> o <Domain> @ no__t-2 @ no__t-3. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando.                                                                                                                                                                                                                                                                  |
|                          /p <Password>                           |                                                                                                                                                                                                                                                                                                                         Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                                                                                                                                                                                                                                                                                         |
| /Port {COM1 &#124; COM2 &#124; COM3 &#124; COM3 &#124; COM4 BIOSSET}  |                                                                                                                                                                                                                              Especifica el puerto COM que se va a utilizar para la redirección. **BIOSSET** dirige los servicios de administración de emergencia para obtener la configuración del BIOS con el fin de determinar qué puerto se debe usar para la redirección. No use el parámetro **/Port** si se va a deshabilitar la salida administrada de forma remota.                                                                                                                                                                                                                              |
| /Baud {9600 &#124; 19200 &#124; 38400&#124; 57600 &#124; 115200} |                                                                                                                                                                                                                                                                                               Especifica la velocidad en baudios que se va a utilizar para la redirección. No use el parámetro **/Baud** si la salida administrada de forma remota está deshabilitada.                                                                                                                                                                                                                                                                                               |
|                       /ID <OSEntryLineNum>                       |                                                                                                                                                                                              Especifica el número de línea de entrada del sistema operativo a la que se agrega la opción servicios de administración de emergencia en la sección [sistemas operativos] del archivo boot. ini. La primera línea después del encabezado de la sección [operating systems] es 1. Este parámetro es necesario cuando el valor de los servicios **de administración de** emergencia está activado o **desactivado**.                                                                                                                                                                                              |
|                                /?                                |                                                                                                                                                                                                                                                                                                                                                 Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                                                                                                                                                                  |

## <a name="BKMK_examples"></a>Example
En los siguientes ejemplos se muestra cómo se puede usar el comando **bootcfg/EMS** :
```
bootcfg /ems on /port com1 /baud 19200 /id 2 
bootcfg /ems on /port biosset /id 3 
bootcfg /s srvmain /ems off /id 2 
bootcfg /ems edit /port com2 /baud 115200 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
