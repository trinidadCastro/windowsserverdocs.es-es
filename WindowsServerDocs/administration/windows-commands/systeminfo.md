---
title: systeminfo
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d08d3f86bdbd176aa4de157f58a58c9ea418470
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841506"
---
# <a name="systeminfo"></a>systeminfo



Muestra configuración información detallada sobre un equipo y su sistema operativo, incluida la configuración del sistema operativo, información de seguridad, Id. de producto y las propiedades de hardware (por ejemplo, RAM, espacio en disco y tarjetas de red).

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/s \<equipo >|Especifica el nombre o dirección IP de un equipo remoto (no utilice las barras diagonales inversas). El valor predeterminado es el equipo local.|
|/u \<Domain>\<UserName>|Ejecuta el comando con los permisos de cuenta de la cuenta de usuario especificado. Si **/u** no se especifica, este comando usa los permisos del usuario que ha iniciado sesión actualmente en el equipo que está emitiendo el comando.|
|/p \<contraseña >|Especifica la contraseña de la cuenta de usuario que se especifica en el **/u** parámetro.|
|/Fo \<formato >|Especifica el formato de salida con uno de los siguientes valores:</br>TABLA: Muestra los resultados en una tabla.</br>LISTA: Muestra los resultados en una lista.</br>CSV: Muestra la salida en formato de valores separados por comas.|
|/nh|Suprime los encabezados de columna en la salida. Es válido cuando el **/fo** parámetro está establecido en la tabla o CSV.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_examples"></a>Ejemplos

Para ver información de configuración para un equipo denominado Srvmain, escriba:

**systeminfo /s srvmain**

Para ver información de configuración para un equipo denominado Srvmain2 que se encuentra en el dominio Maindom de forma remota, escriba:

**systeminfo /s srvmain2 /u maindom\hiropln**

Para ver información de configuración (en formato de lista) para un equipo denominado Srvmain2 que se encuentra en el dominio Maindom de forma remota, escriba:

**systeminfo /s srvmain2 /u maindom\hiropln /p p@ssW23 /fo list**

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)