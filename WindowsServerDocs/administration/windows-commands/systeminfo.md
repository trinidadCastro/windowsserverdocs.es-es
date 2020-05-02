---
title: systeminfo
description: Tema de referencia de SystemInfo, que muestra información de configuración detallada sobre un equipo y su sistema operativo, incluida la configuración del sistema operativo, la información de seguridad, el ID. de producto y las propiedades de hardware (como la RAM, el espacio en disco y las tarjetas de red).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e95a61e4312e37288bc587264cf35240920abd3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721582"
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
|/s \<equipo>|Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.|
|/u \<dominio>\<nombredeusuario>|Ejecuta el comando con los permisos de cuenta de la cuenta de usuario especificada. Si no se especifica **/u** , este comando usa los permisos del usuario que ha iniciado sesión actualmente en el equipo que emite el comando.|
|/p \<contraseña>|Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .|
|/FO \<formato>|Especifica el formato de salida con uno de los siguientes valores:</br>TABLA: muestra la salida en una tabla.</br>LIST: muestra la salida en una lista.</br>CSV: muestra la salida en formato de valores separados por comas.|
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