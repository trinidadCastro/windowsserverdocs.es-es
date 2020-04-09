---
title: bootcfg rmsw
description: Windows Commands topic for Bootcfg Rmsw, que quita las opciones de carga del sistema operativo para una entrada de sistema operativo especificada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 956732f396e0fa353a8acd55953e46605a5c4200
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848448"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

quita las opciones de carga del sistema operativo para una entrada de sistema operativo especificada.

## <a name="syntax"></a>Sintaxis
```
bootcfg /rmsw [/s <computer> [/u <Domain>\<User> [/p <Password>]]] [/mm] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
### <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                                                      Descripción                                                                                                       |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                   Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                                   |
| /u <Domain>\\<User>  |          Ejecuta el comando con los permisos de cuenta del usuario especificado mediante <User> o <Domain>\\<User>. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando.          |
|    /p <Password>     |                                                                 Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                                  |
|         Val          |           quita la opción/MaxMem y su valor máximo de memoria asociado del <OSEntryLineNum>especificado. La opción/MaxMem especifica la cantidad máxima de memoria RAM que puede usar el sistema operativo.            |
|         /bv          |                     quita la opción/basevideo del <OSEntryLineNum>especificado. La opción/basevideo indica al sistema operativo que use el modo VGA estándar para el controlador de vídeo instalado.                     |
|         /so          |                         quita la opción/SOS del <OSEntryLineNum>especificado. La opción/SOS indica al sistema operativo que muestre los nombres de los controladores de dispositivo mientras se cargan.                          |
|         /NG          |                         quita la opción/noguiboot del <OSEntryLineNum>especificado. La opción/noguiboot deshabilita la barra de progreso que aparece antes del símbolo del sistema de inicio de sesión CTRL + ALT + SUPR.                          |
| /ID <OSEntryLineNum> | Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini del que se quitan las opciones de carga del sistema operativo. La primera línea después del encabezado de la sección [operating systems] es 1. |
|          /?          |                                                                                          Muestra la Ayuda en el símbolo del sistema.                                                                                          |

## <a name="examples"></a><a name=BKMK_examples></a>Example
En los siguientes ejemplos se muestra cómo se puede usar el comando **bootcfg/Rmsw**:
```
bootcfg /rmsw /mm 64 /id 2 
bootcfg /rmsw /so /id 3 
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /rmsw /ng /id 2 
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2       
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
