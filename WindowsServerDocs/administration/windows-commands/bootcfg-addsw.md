---
title: bootcfg addsw
description: Windows Commands topic for **bootcfg addsw** -agrega opciones de carga del sistema operativo para una entrada de sistema operativo especificada.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2dd727c839babe1ae4f7743285844f35cf5bf76e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380183"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

agrega opciones de carga del sistema operativo para una entrada específica del sistema operativo.

## <a name="syntax"></a>Sintaxis
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Parámetros

|         Término         |                                                                                                            Definición                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                        Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                                        |
| /u <Domain> @ no__t-1 @ no__t-2  |               Ejecuta el comando con los permisos de cuenta del usuario especificado por <User> o <Domain> @ no__t-2 @ no__t-3. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando.               |
|    /p <Password>     |                                                                      Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                                       |
|   /mm <MaximumRAM>   |                                          Especifica la cantidad máxima de RAM, en megabytes, que puede usar el sistema operativo. El valor debe ser igual o mayor que 32 megabytes.                                          |
|         /bv          |                                    agrega la opción **/basevideo** al @no__t especificado, dirigiendo al sistema operativo para que use el modo VGA estándar para el controlador de vídeo instalado.                                     |
|         /so          |                                      agrega la opción **/SOS** al parámetro *númeroLíneaEntradaSO*especificado y dirige al sistema operativo para que muestre los nombres de los controladores de dispositivo mientras se cargan.                                      |
|         /NG          |                                         agrega la opción **/noguiboot** al @no__t especificado, deshabilitando la barra de progreso que aparece antes del símbolo del sistema de inicio de sesión Ctrl + Alt + Supr.                                          |
| /ID <OSEntryLineNum> | Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini en el que se agregan las opciones de carga del sistema operativo. La primera línea después del encabezado de la sección [operating systems] es 1. |
|          /?          |                                                                                               Muestra la ayuda en el símbolo del sistema.                                                                                               |

## <a name="BKMK_examples"></a>Example
En los siguientes ejemplos se muestra cómo se puede usar el comando **bootcfg/addsw** :
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
