---
title: systeminfo
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 32a84a33c5339e9949648a4e40d71daf25c055d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370695"
---
# <a name="systeminfo"></a>systeminfo



Muestra información de configuración detallada sobre un equipo y su sistema operativo, incluida la configuración del sistema operativo, la información de seguridad, el ID. de producto y las propiedades de hardware (como la RAM, el espacio en disco y las tarjetas de red).

Para obtener ejemplos de cómo utilizar este comando, consulte [Ejemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxis

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

## <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/s \<Computer >|Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local.|
|/u \<Domain > \<UserName >|Ejecuta el comando con los permisos de cuenta de la cuenta de usuario especificada. Si no se especifica **/u** , este comando usa los permisos del usuario que ha iniciado sesión actualmente en el equipo que emite el comando.|
|/p \<contraseña >|Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** .|
|/FO @no__t 0Format >|Especifica el formato de salida con uno de los siguientes valores:</br>CUADRO Muestra la salida en una tabla.</br>LISTA Muestra la salida en una lista.</br>CVS Muestra la salida en formato de valores separados por comas.|
|/NH|Suprime los encabezados de columna en la salida. Válido cuando el parámetro **/FO** está establecido en Table o CSV.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="BKMK_examples"></a>Example

Para ver la información de configuración de un equipo denominado Srvmain, escriba:

**SystemInfo/s srvmain**

Para ver de forma remota la información de configuración de un equipo denominado Srvmain2 que se encuentra en el dominio Maindom, escriba:

**SystemInfo/s srvmain2/u maindom\hiropln**

Para ver de forma remota la información de configuración (en formato de lista) de un equipo denominado Srvmain2 que se encuentra en el dominio Maindom, escriba:

**SystemInfo/s srvmain2/u maindom\hiropln/p p@ssW23/FO List**

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)