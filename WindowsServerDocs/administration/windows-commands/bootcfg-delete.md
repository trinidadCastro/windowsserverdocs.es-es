---
title: bootcfg delete
description: 'Temas de comandos de Windows para **bootcfg Delete** : elimina una entrada del sistema operativo en la sección sistemas operativos del archivo boot. ini.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71382e29-9b39-41c8-9c23-cf0ff829440a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d2298c07af32e66a2ffcebb85ec780da762be58
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380006"
---
# <a name="bootcfg-delete"></a>bootcfg delete

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

elimina una entrada del sistema operativo en la sección [operating systems] del archivo boot. ini.

## <a name="syntax"></a>Sintaxis
```
bootcfg /delete [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parámetros

|         Término         |                                                                                             Definición                                                                                              |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                          |
| /u <Domain> @ no__t-1 @ no__t-2  | Ejecuta el comando con los permisos de cuenta del usuario especificado por <User>or <Domain> @ no__t-2 @ no__t-3. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
|    /p <Password>     |                                                        Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                        |
| /ID <OSEntryLineNum> |        Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini que se va a eliminar. La primera línea después del encabezado de la sección [operating systems] es 1.        |
|          /?          |                                                                                Muestra la ayuda en el símbolo del sistema.                                                                                 |

## <a name="BKMK_examples"></a>Example
En los siguientes ejemplos se muestra cómo se puede usar el comando **bootcfg/Delete**:
```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
