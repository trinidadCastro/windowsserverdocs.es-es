---
title: getmac
description: Artículo de referencia para el comando getmac, que devuelve la dirección Media Access Control (MAC) y la lista de protocolos de red asociados a cada uno de ellos, de forma local o a través de una red.
ms.topic: reference
ms.assetid: a749a348-7cd1-4336-9f33-bb42dd0e31e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 901777744b98095e4e19ff39d9965d144ee1f1c8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034083"
---
# <a name="getmac"></a>getmac

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Devuelve la dirección Media Access Control (MAC) y la lista de protocolos de red asociados a cada dirección de todas las tarjetas de red de cada equipo, ya sea de forma local o a través de una red. Este comando es especialmente útil si desea escribir la dirección MAC en un analizador de red o si necesita saber qué protocolos se están usando actualmente en cada adaptador de red de un equipo.

## <a name="syntax"></a>Sintaxis

```
getmac[.exe][/s <computer> [/u <domain\<user> [/p <password>]]][/fo {table | list | csv}][/nh][/v]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- |------------ |
| modificado `<computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). La opción predeterminada es el equipo local. |
| /u `<domain>\<user>` | Ejecuta el comando con los permisos de cuenta del usuario especificado por *User* o *dominio\usuario*. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
| /p `<password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| /FO {Table | list | CVS | Especifica el formato que se va a usar para los resultados de la consulta. Los valores válidos son **TABLE**, **List**y **CSV**. El formato predeterminado de la salida es **TABLE**. |
| /NH | Suprime el encabezado de columna en la salida. Válido cuando el parámetro **/FO** está establecido en **TABLE** o **CSV**. |
| /v | Especifica que el resultado muestra información detallada. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

En los siguientes ejemplos se muestra cómo se puede usar el comando **getmac** :

```
getmac /fo table /nh /v
```

```
getmac /s srvmain
```

```
getmac /s srvmain /u maindom\hiropln
```

```
getmac /s srvmain /u maindom\hiropln /p p@ssW23
```

```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo list /v
```

```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo table /nh
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
