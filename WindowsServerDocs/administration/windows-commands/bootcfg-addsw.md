---
title: bootcfg addsw
description: Tema de los comandos de Windows para **addsw bootcfg** -agrega opciones de carga del sistema operativo para una entrada de sistema operativo especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a056cec15bf804dafed4c4d39a80386e58c87fea
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434886"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

agrega opciones de carga del sistema operativo para una entrada de sistema operativo especificado.

## <a name="syntax"></a>Sintaxis
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parámetros

|         Término         |                                                                                                            Definición                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                        Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.                                                        |
| /u <Domain>\\<User>  |               Ejecuta el comando con los permisos de cuenta del usuario especificado por <User> o <Domain> \\ <User>. El valor predeterminado es los permisos de la sesión de usuario en el equipo que emite el comando actual.               |
|    /p <Password>     |                                                                      Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.                                                                       |
|   /mm <MaximumRAM>   |                                          Especifica la cantidad máxima de RAM, en megabytes, que puede usar el sistema operativo. El valor debe ser igual o mayor que 32 Megabytes.                                          |
|         /bv          |                                    Agrega el **basevideo** opción especificado <OSEntryLineNum>, indicar al sistema operativo que se usará el modo VGA estándar para el controlador de vídeo instalado.                                     |
|         /so          |                                      Agrega el **/SOS** opción especificado *númeroLíneaEntradaSO*, indicar al sistema operativo para mostrar los nombres de controlador de dispositivo mientras se cargan.                                      |
|         /ng          |                                         Agrega el **noguiboot** opción especificado <OSEntryLineNum>, deshabilitar la barra de progreso que aparece antes de la CTRL + ALT + inicio de sesión del símbolo del sistema.                                          |
| / Id <OSEntryLineNum> | Especifica el número de línea de entrada de sistema operativo en la sección [operating systems] del archivo Boot.ini a la que se agregan las opciones de carga del sistema operativo. La primera línea después de encabezado de sección de la sección [operating systems] es 1. |
|          /?          |                                                                                               Muestra la ayuda en el símbolo del sistema.                                                                                               |

## <a name="BKMK_examples"></a>Ejemplos
Los ejemplos siguientes muestran cómo puede usar el **bootcfg /addsw** comando:
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)