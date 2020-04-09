---
title: bootcfg delete
description: Comando de comandos de Windows para Bootcfg Delete, que elimina una entrada del sistema operativo en la sección sistemas operativos del archivo boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 71382e29-9b39-41c8-9c23-cf0ff829440a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 01ec7a4dde1e22982f2cf0fa30245c33e09cc0ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848548"
---
# <a name="bootcfg-delete"></a>bootcfg delete

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina una entrada del sistema operativo en la sección [operating systems] del archivo boot. ini.

## <a name="syntax"></a>Sintaxis
```
bootcfg /delete [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
### <a name="parameters"></a>Parámetros

|         Término         |                                                                                             Definición                                                                                              |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.                                          |
| /u <Domain>\\<User>  | Ejecuta el comando con los permisos de cuenta del usuario especificado mediante <User>o <Domain>\\<User>. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
|    /p <Password>     |                                                        Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .                                                        |
| /ID <OSEntryLineNum> |        Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini que se va a eliminar. La primera línea después del encabezado de la sección [operating systems] es 1.        |
|          /?          |                                                                                Muestra la Ayuda en el símbolo del sistema.                                                                                 |

## <a name="examples"></a><a name=BKMK_examples></a>Example
En los siguientes ejemplos se muestra cómo se puede usar el comando **bootcfg/Delete**:
```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```
## <a name="additional-references"></a>Referencias adicionales
- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
