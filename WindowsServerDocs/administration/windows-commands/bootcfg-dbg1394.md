---
title: bootcfg dbg1394
description: 'Temas de comandos de Windows para **bootcfg dbg1394** : configura la depuración de Puerto 1394 para una entrada de sistema operativo especificada'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35724697-90dd-4dbe-85b0-337fbd369dcc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8550871c60343fdc6d797f3f81729c24270400b4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380091"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura la depuración de Puerto 1394 para una entrada de sistema operativo especificada.

## <a name="syntax"></a>Sintaxis
```
bootcfg /dbg1394 {ON | OFF}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/ch <Channel>] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                                                                                           Descripción                                                                                                                                            |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   {ON &#124; OFF}    | Especifica el valor para la depuración de Puerto 1394.<br /><br />-   **activado** : habilita la compatibilidad con la depuración remota agregando la opción/dbg1394 al <OSEntryLineNum>especificado.<br />-   **deshabilita la compatibilidad con la** depuración remota quitando la opción/dbg1394 de la <OSEntryLineNum>especificada. |
|    /s <computer>     |                                                                                        Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                                                                        |
| /u <Domain>\\<User>  |                                               Ejecuta el comando con los permisos de cuenta del usuario especificado mediante <User> o <Domain>\\<User>. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando.                                               |
|    /p <Password>     |                                                                                                      Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                                                                       |
|     /CH canal      |                                                           Especifica el canal que se va a usar para la depuración. Los valores válidos son enteros entre 1 y 64. No use el parámetro **/ch** <Channel> si se va a deshabilitar la depuración del puerto 1394.                                                           |
| /ID <OSEntryLineNum> |                                  Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini en el que se agregan las opciones de depuración de Puerto 1394. La primera línea después del encabezado de la sección [operating systems] es 1.                                  |
|          /?          |                                                                                                                               Muestra la ayuda en el símbolo del sistema.                                                                                                                               |

## <a name="BKMK_examples"></a>Example
En los siguientes ejemplos se muestra cómo se puede usar el comando **bootcfg/dbg1394**:
```
bootcfg /dbg1394 /id 2 
bootcfg /dbg1394 on /ch 1 /id 3 
bootcfg /dbg1394 edit /ch 8 /id 2 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```
#### <a name="additional-references"></a>referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
