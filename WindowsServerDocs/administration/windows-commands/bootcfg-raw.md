---
title: bootcfg raw
description: Tema de los comandos de Windows para **bootcfg raw** -agrega opciones de carga del sistema operativo especificadas como una cadena a una entrada de sistema operativo en el **[sistemas operativos]** sección del archivo Boot.ini.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5334ff8a1c5d15343b4a48814b52012c641016a4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434689"
---
# <a name="bootcfg-raw"></a>bootcfg raw

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

agrega opciones de carga del sistema operativo especificadas como una cadena a una entrada de sistema operativo en el **[sistemas operativos]** sección del archivo Boot.ini.

## <a name="syntax"></a>Sintaxis
```
bootcfg /raw [/s <computer> [/u <Domain>\<User> /p <Password>]] <OSLoadOptionsString> [/id <OSEntryLineNum>] [/a]
```
## <a name="parameters"></a>Parámetros

|         Término          |                                                                                                            Definición                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s <computer>     |                                                        Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.                                                         |
| /u <Domain> \\<User>  |               Ejecuta el comando con los permisos de cuenta del usuario especificado por <User> o <Domain> \\ <User>. El valor predeterminado es los permisos de la sesión de usuario en el equipo que emite el comando actual.                |
|     /p <Password>     |                                                                       Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.                                                                       |
| <OSLoadOptionsString> | Especifica las opciones de carga del sistema operativo para agregar a la entrada de sistema operativo. Estas opciones de carga se reemplazará cualquier opción de carga existente asociado con la entrada de sistema operativo. Ninguna validación de <OSLoadOptions> se realiza. |
| / Id <OSEntryLineNum>  |                       Especifica el número de línea de entrada de sistema operativo en la sección [operating systems] del archivo Boot.ini para actualizar. La primera línea después de encabezado de sección de la sección [operating systems] es 1.                       |
|          /a           |                                                       Especifica que se deben anexar las opciones de sistema operativo que se va a agregar a las opciones de sistema operativo existente.                                                        |
|          /?           |                                                                                               Muestra la ayuda en el símbolo del sistema.                                                                                                |

##### <a name="remarks"></a>Comentarios
- **BOOTCFG raw** se utiliza para agregar texto al final de una entrada de sistema operativo, sobrescribiendo las opciones de entrada de sistema operativo existente. Este texto debe contener opciones de carga del sistema operativo válido como **/debug**, **/fastdetect**, **nodebug**, **/baudrate**, **/ /crashdebug**, y **/SOS**. Por ejemplo, el siguiente comando agrega " **/debug/fastdetect**" al final de la primera entrada de sistema operativo, reemplazando las opciones de entrada de sistema operativo anterior:
  ```
  bootcfg /raw "/debug /fastdetect" /id 1
  ```
  ## <a name="BKMK_examples"></a>Ejemplos
  Los ejemplos siguientes muestran cómo puede usar el **bootcfg / sin formato** comando:
  ```
  bootcfg /raw "/debug /sos" /id 2
  bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 "/crashdebug " /id 2
  ```
  #### <a name="additional-references"></a>Referencias adicionales
  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
