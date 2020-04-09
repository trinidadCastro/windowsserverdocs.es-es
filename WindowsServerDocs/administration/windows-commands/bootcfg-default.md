---
title: bootcfg default
description: Comando de comandos de Windows para Bootcfg predeterminado, que especifica la entrada del sistema operativo que se debe designar como valor predeterminado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e21824d7-8278-41d7-a2c5-ce09803d513a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 517cf444a5517b3d612266b57b428e47ac60d4ef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848569"
---
# <a name="bootcfg-default"></a>bootcfg default

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Especifica la entrada del sistema operativo que se designará como predeterminada.

## <a name="syntax"></a>Sintaxis
```
bootcfg /default [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
### <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                                             Descripción                                                                                              |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                          Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                          |
| /u <Domain>\\<User>  | Ejecuta el comando con los permisos de cuenta del usuario especificado mediante <User> o <Domain>\\<User>. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
|    /p <Password>     |                                                        Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                         |
| /ID <OSEntryLineNum> | Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini para designar como predeterminado. La primera línea después del encabezado de la sección [operating systems] es 1.  |
|          /?          |                                                                                 Muestra la Ayuda en el símbolo del sistema.                                                                                 |

## <a name="examples"></a><a name=BKMK_examples></a>Example
En los siguientes ejemplos se muestra cómo se puede usar el comando **bootcfg/default**:
```
bootcfg /default /id 2
bootcfg /default /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
