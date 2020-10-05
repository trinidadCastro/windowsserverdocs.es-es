---
title: systeminfo
description: Artículo de referencia del comando SystemInfo, que muestra información de configuración detallada sobre un equipo y su sistema operativo, incluida la configuración del sistema operativo, la información de seguridad, el ID. de producto y las propiedades de hardware (como la RAM, el espacio en disco y las tarjetas de red).
ms.topic: reference
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9c104d3114eae6170e3fbbd60ab718fa0013ea85
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718202"
---
# <a name="systeminfo"></a>systeminfo

Muestra información de configuración detallada sobre un equipo y su sistema operativo, incluida la configuración del sistema operativo, la información de seguridad, el ID. de producto y las propiedades de hardware (como la RAM, el espacio en disco y las tarjetas de red).

## <a name="syntax"></a>Sintaxis

```
systeminfo [/s <computer> [/u <domain>\<username> [/p <password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). La opción predeterminada es el equipo local. |
| /u `<domain>\<username>` | Ejecuta el comando con los permisos de cuenta de la cuenta de usuario especificada. Si no se especifica **/u** , este comando usa los permisos del usuario que ha iniciado sesión actualmente en el equipo que emite el comando. |
| /p `<password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| /FO `<format>` | Especifica el formato de salida con uno de los siguientes valores:<ul><li>**Tabla** : muestra la salida en una tabla.</li><li>**List** : muestra la salida en una lista.</li><li>**CSV** : muestra la salida en formato de valores separados por comas (. csv).</li></ul> |
| /NH | Suprime los encabezados de columna en la salida. Válido cuando el parámetro **/FO** está establecido en Table o CSV. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para ver la información de configuración de un equipo denominado *Srvmain*, escriba:

```
systeminfo /s srvmain
```

Para ver de forma remota la información de configuración de un equipo denominado *Srvmain2* que se encuentra en el dominio *Maindom* , escriba:

```
systeminfo /s srvmain2 /u maindom\hiropln
```

Para ver de forma remota la información de configuración (en formato de lista) de un equipo denominado *Srvmain2* que se encuentra en el dominio *Maindom* , escriba:

```
systeminfo /s srvmain2 /u maindom\hiropln /p p@ssW23 /fo list
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
