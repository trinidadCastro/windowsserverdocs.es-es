---
title: bootcfg query
description: Artículo de referencia para el comando de consulta Bootcfg, que consulta y muestra las entradas de la sección del cargador de arranque y del sistema operativo de Boot.ini.
ms.topic: reference
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8debee9f799a89f80c82b25242127c42fd00189f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630191"
---
# <a name="bootcfg-query"></a>bootcfg query

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Consulta y muestra las entradas de la sección [cargador de arranque] y [sistemas operativos] de Boot.ini.

## <a name="syntax"></a>Sintaxis

```
bootcfg /query [/s <computer> [/u <domain>\<user> /p <password>]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `/s <computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). La opción predeterminada es el equipo local. |
| `/u <domain>\<user>`  | Ejecuta el comando con los permisos de cuenta del usuario especificado por `<user>` o `<domain>\<user>` . El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
| `/p <password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="sample-output"></a>Salida de ejemplo

Salida de ejemplo para el comando **bootcfg/Query** :

```
Boot Loader Settings
----------
timeout: 30
default: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
Boot Entries
------
Boot entry ID:   1
Friendly Name:
path: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
OS Load Options: /fastdetect /debug /debugport=com1:
```

- En el área **configuración del cargador de arranque** se muestra cada entrada de la sección [cargador de arranque] de Boot.ini.

- En el área **entradas de arranque** se muestran más detalles de cada entrada del sistema operativo en la sección [sistemas operativos] de la Boot.ini

## <a name="examples"></a>Ejemplos

Para usar el comando **bootcfg/Query** :

```
bootcfg /query
bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
bootcfg /query /u hiropln /p p@ssW23
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
