---
title: bootcfg delete
description: Tema de los comandos de Windows para **bootcfg eliminar** -elimina una entrada de sistema operativo en la sección sistemas operativos del archivo Boot.ini.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0605e8dfaaf1631da02ac320aa8748d7a34f3fed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840406"
---
# <a name="bootcfg-delete"></a>bootcfg delete

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

elimina una entrada de sistema operativo en la sección [operating systems] del archivo Boot.ini.

## <a name="syntax"></a>Sintaxis
```
bootcfg /delete [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parámetros
|Término|Definición|
|----|-------|
|/s <computer>|Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.|
|/u <Domain>\\<User>|Ejecuta el comando con los permisos de cuenta del usuario especificado por <User>o <Domain> \\ <User>. El valor predeterminado es los permisos de la sesión de usuario en el equipo que emite el comando actual.|
|/p <Password>|Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.|
|/ Id <OSEntryLineNum>|Especifica el número de línea de entrada de sistema operativo en la sección [operating systems] del archivo Boot.ini para eliminar. La primera línea después de encabezado de sección de la sección [operating systems] es 1.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="BKMK_examples"></a>Ejemplos
Los ejemplos siguientes muestran cómo puede usar el **bootcfg.exe-delete**comando:
```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
