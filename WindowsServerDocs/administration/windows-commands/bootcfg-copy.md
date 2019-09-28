---
title: bootcfg copy
description: 'Tema de comandos de Windows para la **copia de Bootcfg** : realiza una copia de una entrada de arranque existente, a la que puede agregar opciones de la línea de comandos.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 42a408443cbe6722c25780f7c27d70b05da7eb8e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380122"
---
# <a name="bootcfg-copy"></a>bootcfg copy

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Realiza una copia de una entrada de arranque existente, a la que puede agregar opciones de la línea de comandos.

## <a name="syntax"></a>Sintaxis
```
bootcfg /copy [/s <computer> [/u <Domain>\<User> /p <Password>]] [/d <Description>] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                                             Descripción                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                          |
| /u <Domain> @ no__t-1 @ no__t-2  | Ejecuta el comando con los permisos de cuenta del usuario especificado por <User>or <Domain> @ no__t-2 @ no__t-3. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
|    /p <Password>     |                                                        Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                        |
|   /d <Description>   |                                                                    Especifica la descripción de la nueva entrada de sistema operativo.                                                                    |
| /ID <OSEntryLineNum> |         Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini que se va a copiar. La primera línea después del encabezado de la sección [operating systems] es 1.         |
|          /?          |                                                                                Muestra la ayuda en el símbolo del sistema.                                                                                 |

## <a name="BKMK_examples"></a>Example
En los siguientes ejemplos se muestra cómo se puede usar el comando **bootcfg/Copy** para copiar la entrada de arranque 1 y escribir "\ABC Server @ no__t-1" como Descripción:
```
bootcfg /copy /d "\ABC Server\" /id 1
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
