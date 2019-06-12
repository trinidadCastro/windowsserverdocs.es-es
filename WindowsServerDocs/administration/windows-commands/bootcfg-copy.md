---
title: bootcfg copy
description: Tema de los comandos de Windows para **bootcfg copia** -realiza una copia de una entrada de arranque existente, al que puede agregar opciones de línea de comandos.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a236c2a-8675-444d-b695-9cbc9aff643b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b76ecfe953d1a462e311fdaaeba35e8f962165c4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434862"
---
# <a name="bootcfg-copy"></a>bootcfg copy

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Realiza una copia de una entrada de arranque existente, al que puede agregar opciones de línea de comandos.

## <a name="syntax"></a>Sintaxis
```
bootcfg /copy [/s <computer> [/u <Domain>\<User> /p <Password>]] [/d <Description>] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                                             Descripción                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.                                          |
| /u <Domain>\\<User>  | Ejecuta el comando con los permisos de cuenta del usuario especificado por <User>o <Domain> \\ <User>. El valor predeterminado es los permisos de la sesión de usuario en el equipo que emite el comando actual. |
|    /p <Password>     |                                                        Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.                                                        |
|   /d <Description>   |                                                                    Especifica la descripción de la nueva entrada de sistema operativo.                                                                    |
| / Id <OSEntryLineNum> |         Especifica el número de línea de entrada de sistema operativo en la sección [operating systems] del archivo Boot.ini para copiar. La primera línea después de encabezado de sección de la sección [operating systems] es 1.         |
|          /?          |                                                                                Muestra la ayuda en el símbolo del sistema.                                                                                 |

## <a name="BKMK_examples"></a>Ejemplos
Los ejemplos siguientes muestran cómo puede usar el **bootcfg /copy** comando para copiar la entrada de inicio 1 y escriba "\ABC Server\\" como descripción:
```
bootcfg /copy /d "\ABC Server\" /id 1
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
