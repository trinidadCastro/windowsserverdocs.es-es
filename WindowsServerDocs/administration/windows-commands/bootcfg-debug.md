---
title: bootcfg debug
description: 'Temas de comandos de Windows para la **depuración Bootcfg** : agrega o cambia la configuración de depuración de una entrada de sistema operativo especificada.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f6659cf2bfdf83b1b2fe6f6c811365775526768a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380054"
---
# <a name="bootcfg-debug"></a>bootcfg debug

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

agrega o cambia la configuración de depuración de una entrada de sistema operativo especificada.

## <a name="syntax"></a>Sintaxis
```
bootcfg /debug {ON | OFF | edit}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parámetros

|                           Parámetro                           |                                                                                                                                                                                                                    Descripción                                                                                                                                                                                                                    |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  {ON &#124; OFF&#124; Edit}                   | Especifica el valor para la depuración.<br /><br />**On** : habilita la compatibilidad con la depuración remota agregando la opción/debug al @no__t especificado-1.<br /><br />**OFF** : deshabilita la compatibilidad con la depuración remota quitando la opción/debug del <OSEntryLineNum> especificado.<br /><br />**Editar** : permite realizar cambios en la configuración de velocidad de puerto y baudios cambiando los valores asociados a la opción/debug para el <OSEntryLineNum> especificado. |
|                         /s <computer>                         |                                                                                                                                                                Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                                                                                                                                                 |
|                      /u <Domain> @ no__t-1 @ no__t-2                      |                                                                                                                       Ejecuta el comando con los permisos de cuenta del usuario especificado por <User> o <Domain> @ no__t-2 @ no__t-3. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando.                                                                                                                        |
|                         /p <Password>                         |                                                                                                                                                                               Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                                                                                                                                               |
|       /Port {COM1 &#124; COM2 &#124; COM3 &#124; COM3 COM4}        |                                                                                                                                                                Especifica el puerto COM que se va a utilizar para la depuración. No use el parámetro **/Port** si se deshabilita la depuración.                                                                                                                                                                |
| /Baud {9600&#124; 19200&#124; 38400&#124; 57600&#124; 115200} |                                                                                                                                                               Especifica la velocidad en baudios que se va a utilizar para la depuración. No use el parámetro **/Baud** si se deshabilita la depuración.                                                                                                                                                                |
|                     /ID <OSEntryLineNum>                      |                                                                                                               Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini al que se agregan las opciones de depuración. La primera línea después del encabezado de la sección [operating systems] es 1.                                                                                                                |
|                              /?                               |                                                                                                                                                                                                       Muestra la ayuda en el símbolo del sistema.                                                                                                                                                                                                        |

##### <a name="remarks"></a>Comentarios
- Si se requiere la depuración de Puerto 1394, use [bootcfg dbg1394](bootcfg-dbg1394.md).
  ## <a name="BKMK_examples"></a>Example
  En los siguientes ejemplos se muestra cómo se puede usar el comando **bootcfg/Debug**:
  ```
  bootcfg /debug on /port com1 /id 2 
  bootcfg /debug edit /port com2 /baud 19200 /id 2 
  bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
  ```
  #### <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
