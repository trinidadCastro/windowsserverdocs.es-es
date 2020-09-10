---
title: systeminfo
description: Artículo de referencia de SystemInfo, que muestra información de configuración detallada sobre un equipo y su sistema operativo, incluida la configuración del sistema operativo, la información de seguridad, el ID. de producto y las propiedades de hardware (como la RAM, el espacio en disco y las tarjetas de red).
ms.topic: reference
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b8db86467c6d3190edd6c041951bf3c30eb21cc4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626741"
---
# <a name="systeminfo"></a>systeminfo

Muestra información de configuración detallada sobre un equipo y su sistema operativo, incluida la configuración del sistema operativo, la información de seguridad, el ID. de producto y las propiedades de hardware (como la RAM, el espacio en disco y las tarjetas de red).



## <a name="syntax"></a>Sintaxis

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|modificado \<Computer>|Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). La opción predeterminada es el equipo local.|
|/u \<Domain>\<UserName>|Ejecuta el comando con los permisos de cuenta de la cuenta de usuario especificada. Si no se especifica **/u** , este comando usa los permisos del usuario que ha iniciado sesión actualmente en el equipo que emite el comando.|
|/p \<Password>|Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .|
|/FO \<Format>|Especifica el formato de salida con uno de los siguientes valores:</br>TABLA: muestra la salida en una tabla.</br>LIST: muestra la salida en una lista.</br>CSV: muestra la salida en formato de valores separados por comas.|
|/NH|Suprime los encabezados de columna en la salida. Válido cuando el parámetro **/FO** está establecido en Table o CSV.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="examples"></a>Ejemplos

Para ver la información de configuración de un equipo denominado Srvmain, escriba:

**SystemInfo/s srvmain**

Para ver de forma remota la información de configuración de un equipo denominado Srvmain2 que se encuentra en el dominio Maindom, escriba:

**SystemInfo/s srvmain2/u maindom\hiropln**

Para ver de forma remota la información de configuración (en formato de lista) de un equipo denominado Srvmain2 que se encuentra en el dominio Maindom, escriba:

**SystemInfo/s srvmain2/u maindom\hiropln/p p@ssW23 /FO List**

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)