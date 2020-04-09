---
title: bootcfg addsw
description: Windows Commands topic for Bootcfg addsw, que agrega opciones de carga del sistema operativo para una entrada de sistema operativo especificada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9ae5175dfc3b068276f6ab95d6823699c96b2b5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848718"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

agrega opciones de carga del sistema operativo para una entrada específica del sistema operativo.

## <a name="syntax"></a>Sintaxis
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
### <a name="parameters"></a>Parámetros

|         Término         |                                                                                                            Definición                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                        Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                                        |
| /u <Domain>\\<User>  |               Ejecuta el comando con los permisos de cuenta del usuario especificado mediante <User> o <Domain>\\<User>. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando.               |
|    /p <Password>     |                                                                      Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                                       |
|   /mm <MaximumRAM>   |                                          Especifica la cantidad máxima de RAM, en megabytes, que puede usar el sistema operativo. El valor debe ser igual o mayor que 32 megabytes.                                          |
|         /bv          |                                    agrega la opción **/basevideo** al <OSEntryLineNum>especificado, dirigiendo al sistema operativo para que use el modo VGA estándar para el controlador de vídeo instalado.                                     |
|         /so          |                                      agrega la opción **/SOS** al parámetro *númeroLíneaEntradaSO*especificado y dirige al sistema operativo para que muestre los nombres de los controladores de dispositivo mientras se cargan.                                      |
|         /NG          |                                         agrega la opción **/noguiboot** al <OSEntryLineNum>especificado, deshabilitando la barra de progreso que aparece antes del símbolo del sistema de inicio de sesión Ctrl + Alt + Supr.                                          |
| /ID <OSEntryLineNum> | Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini en el que se agregan las opciones de carga del sistema operativo. La primera línea después del encabezado de la sección [operating systems] es 1. |
|          /?          |                                                                                               Muestra la Ayuda en el símbolo del sistema.                                                                                               |

## <a name="examples"></a><a name=BKMK_examples></a>Example
En los siguientes ejemplos se muestra cómo se puede usar el comando **bootcfg/addsw** :
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
