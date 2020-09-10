---
title: bootcfg timeout
description: Artículo de referencia del comando bootcfg timeout, que cambia el valor de tiempo de espera del sistema operativo.
ms.topic: reference
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 520889fce38eb48fe56b9b4c0a38277b5c7f8c8c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630110"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cambia el valor de tiempo de espera del sistema operativo.

## <a name="syntax"></a>Sintaxis

```
bootcfg /timeout <timeoutvalue> [/s <computer> [/u <domain>\<user> /p <password>]]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `/timeout <timeoutvalue>` | Especifica el valor de tiempo de espera en la sección [boot loader]. `<timeoutvalue>`Es el número de segundos que el usuario tiene que seleccionar un sistema operativo en la pantalla del cargador de arranque antes de que NTLDR cargue el valor predeterminado. El intervalo válido para `<timeoutvalue>` es 0-999. Si el valor es 0, NTLDR inicia inmediatamente el sistema operativo predeterminado sin mostrar la pantalla del cargador de arranque. |
| `/s <computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). La opción predeterminada es el equipo local. |
| `/u <domain>\<user>`  | Ejecuta el comando con los permisos de cuenta del usuario especificado por `<user>` o `<domain>\<user>` . El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
| `/p <password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para usar el comando **bootcfg/timeout** :

```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
