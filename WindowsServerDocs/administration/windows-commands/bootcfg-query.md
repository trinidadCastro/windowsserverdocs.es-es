---
title: bootcfg query
description: Tema de los comandos de Windows para **bootcfg consulta** -consultas y muestra [el cargador de arranque] y [sistemas operativos] sección entradas del archivo Boot.ini.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a6886fbdfe541b39e530d2b7f71d4fc40909a2a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862936"
---
# <a name="bootcfg-query"></a>bootcfg query

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consulta y muestra [el cargador de arranque] y las entradas del archivo Boot.ini de sección [operating systems].

## <a name="syntax"></a>Sintaxis
```
bootcfg /query [/s <computer> [/u <Domain>\<User> /p <Password>]]
```
## <a name="parameters"></a>Parámetros
|Término|Definición|
|----|-------|
|/s <computer>|Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.|
|/u <Domain>\\<User>|Ejecuta el comando con los permisos de cuenta del usuario especificado por <User>o <Domain> \\ <User>. El valor predeterminado es los permisos de la sesión de usuario en el equipo que emite el comando actual.|
|/p <Password>|Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.|
|/?|Muestra la ayuda en el símbolo del sistema.|
##### <a name="remarks"></a>Comentarios
-   El siguiente es un ejemplo de **bootcfg /query** salida:
    ```
    Boot Loader Settings
    ----------
    timeout: 30
    default: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
    Boot Entries
    ------
    Boot entry ID:   1
    Friendly Name:   ""
    path:            multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
    OS Load Options: /fastdetect /debug /debugport=com1:
    ```
-   La parte de la configuración del cargador de arranque de la **bootcfg consulta** salida muestra cada entrada en la sección [boot loader] del archivo Boot.ini.
-   La parte de las entradas de arranque de la **bootcfg consulta** salida muestra los detalles siguientes para cada entrada de sistema operativo en la sección [operating systems] del archivo Boot.ini: Id. de entrada de arranque, un nombre descriptivo, ruta de acceso y las opciones de carga del sistema operativo.
## <a name="BKMK_examples"></a>Ejemplos
Los ejemplos siguientes muestran cómo puede usar el **bootcfg /query** comando:
```
bootcfg /query
bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
bootcfg /query /u hiropln /p p@ssW23
```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
