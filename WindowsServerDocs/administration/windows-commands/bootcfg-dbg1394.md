---
title: bootcfg dbg1394
description: Tema de los comandos de Windows para **bootcfg dbg1394** -configura puerto de depuración 1394 para una entrada de sistema operativo especificado
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 85a554e25d1553ea4cd9415bb180df4751966926
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434845"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura la depuración del puerto 1394 para una entrada de sistema operativo especificado.

## <a name="syntax"></a>Sintaxis
```
bootcfg /dbg1394 {ON | OFF}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/ch <Channel>] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                                                                                           Descripción                                                                                                                                            |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   {ON &AMP;#124; OFF}    | Especifica el valor de puerto de depuración 1394.<br /><br />-   **ON** -habilita la compatibilidad con la depuración remota agregando la opción/dbg1394 especificado <OSEntryLineNum>.<br />-   **DESACTIVAR** -deshabilita la compatibilidad con la depuración remota mediante la eliminación de la opción/dbg1394 especificado <OSEntryLineNum>. |
|    /s <computer>     |                                                                                        Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.                                                                                        |
| /u <Domain>\\<User>  |                                               Ejecuta el comando con los permisos de cuenta del usuario especificado por <User> o <Domain> \\ <User>. El valor predeterminado es los permisos de la sesión de usuario en el equipo que emite el comando actual.                                               |
|    /p <Password>     |                                                                                                      Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.                                                                                                       |
|     /ch Channel      |                                                           Especifica el canal que se usará para la depuración. Los valores válidos son números enteros entre 1 y 64. No utilice el **/ch** <Channel> parámetro si se va a deshabilitar la depuración 1394 del puerto.                                                           |
| / Id <OSEntryLineNum> |                                  Especifica el número de línea de entrada de sistema operativo en la sección [operating systems] del archivo Boot.ini a la que se agregan las opciones de depuración del puerto 1394. La primera línea después de encabezado de sección de la sección [operating systems] es 1.                                  |
|          /?          |                                                                                                                               Muestra la ayuda en el símbolo del sistema.                                                                                                                               |

## <a name="BKMK_examples"></a>Ejemplos
Los ejemplos siguientes muestran cómo puede usar el **/Dbg1394 bootcfg**comando:
```
bootcfg /dbg1394 /id 2 
bootcfg /dbg1394 on /ch 1 /id 3 
bootcfg /dbg1394 edit /ch 8 /id 2 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
