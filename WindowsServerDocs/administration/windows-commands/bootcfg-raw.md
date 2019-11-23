---
title: bootcfg raw
description: 'Windows Commands topic for **bootcfg RAW** : agrega opciones de carga del sistema operativo especificadas como una cadena a una entrada del sistema operativo en la sección **[operating systems]** del archivo boot. ini.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb5c052f85f54656c54a9e534f867d287407d2d4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379902"
---
# <a name="bootcfg-raw"></a>bootcfg raw

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

agrega opciones de carga del sistema operativo especificadas como una cadena a una entrada del sistema operativo en la sección **[operating systems]** del archivo boot. ini.

## <a name="syntax"></a>Sintaxis
```
bootcfg /raw [/s <computer> [/u <Domain>\<User> /p <Password>]] <OSLoadOptionsString> [/id <OSEntryLineNum>] [/a]
```
## <a name="parameters"></a>Parámetros

|         Término          |                                                                                                            Definición                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s <computer>     |                                                        Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                                         |
| /u <Domain> \\<User>  |               Ejecuta el comando con los permisos de cuenta del usuario especificado mediante <User> o <Domain>\\<User>. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando.                |
|     /p <Password>     |                                                                       Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                                       |
| <OSLoadOptionsString> | Especifica las opciones de carga del sistema operativo que se van a agregar a la entrada de sistema operativo. Estas opciones de carga reemplazarán las opciones de carga existentes asociadas a la entrada del sistema operativo. No se realiza ninguna validación de <OSLoadOptions>. |
| /ID <OSEntryLineNum>  |                       Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini que se va a actualizar. La primera línea después del encabezado de la sección [operating systems] es 1.                       |
|          /a           |                                                       Especifica que las opciones del sistema operativo que se van a agregar se deben anexar a las opciones del sistema operativo existentes.                                                        |
|          /?           |                                                                                               Muestra la ayuda en el símbolo del sistema.                                                                                                |

##### <a name="remarks"></a>Observaciones
- **bootcfg RAW** se usa para agregar texto al final de una entrada de sistema operativo, sobrescribiendo las opciones de entrada existentes del sistema operativo. Este texto debe contener opciones de carga del sistema operativo válidas como **/Debug**, **/fastdetect**, **/nodebug**, **/Baudrate**, **/crashdebug**y **/SOS**. Por ejemplo, el comando siguiente agrega " **/Debug/fastdetect**" al final de la primera entrada del sistema operativo, reemplazando las opciones de entrada del sistema operativo anteriores:
  ```
  bootcfg /raw "/debug /fastdetect" /id 1
  ```
  ## <a name="BKMK_examples"></a>Example
  En los siguientes ejemplos se muestra cómo se puede usar el comando **bootcfg/RAW** :
  ```
  bootcfg /raw "/debug /sos" /id 2
  bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 "/crashdebug " /id 2
  ```
  #### <a name="additional-references"></a>referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
