---
title: bootcfg default
description: Tema de los comandos de Windows para **bootcfg predeterminada** -especifica la entrada de sistema operativo para designar como el valor predeterminado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e21824d7-8278-41d7-a2c5-ce09803d513a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63aaa7a044634d29c61f3085b1f0c015f4e64444
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434830"
---
# <a name="bootcfg-default"></a>bootcfg default

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Especifica la entrada de sistema operativo para designar como el valor predeterminado.

## <a name="syntax"></a>Sintaxis
```
bootcfg /default [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                                             Descripción                                                                                              |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                          Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.                                          |
| /u <Domain>\\<User>  | Ejecuta el comando con los permisos de cuenta del usuario especificado por <User> o <Domain> \\ <User>. El valor predeterminado es los permisos de la sesión de usuario en el equipo que emite el comando actual. |
|    /p <Password>     |                                                        Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.                                                         |
| / Id <OSEntryLineNum> | Especifica el número de línea de entrada de sistema operativo en la sección [operating systems] del archivo Boot.ini para designar como predeterminado. La primera línea después de encabezado de sección de la sección [operating systems] es 1.  |
|          /?          |                                                                                 Muestra la ayuda en el símbolo del sistema.                                                                                 |

## <a name="BKMK_examples"></a>Ejemplos
Los ejemplos siguientes muestran cómo puede usar el **/default bootcfg**comando:
```
bootcfg /default /id 2
bootcfg /default /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
