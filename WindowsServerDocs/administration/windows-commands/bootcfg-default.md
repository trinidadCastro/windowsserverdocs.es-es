---
title: bootcfg default
description: Tema de referencia para el comando bootcfg predeterminado, que especifica la entrada del sistema operativo que se debe designar como valor predeterminado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e21824d7-8278-41d7-a2c5-ce09803d513a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43a1c2b4bcbb4062a24d4c72f5856898afecd349
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709689"
---
# <a name="bootcfg-default"></a>bootcfg default

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Especifica la entrada del sistema operativo que se designará como predeterminada.

## <a name="syntax"></a>Sintaxis

```
bootcfg /default [/s <computer> [/u <domain>\<user> /p <password>]] [/id <osentrylinenum>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `/s <computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local. |
| `/u <domain>\<user>`  | Ejecuta el comando con los permisos de cuenta del usuario especificado por `<user>` o `<domain>\<user>`. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
| `/p <password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| `/id <osentrylinenum>` | Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini en el que se agregan las opciones de carga del sistema operativo. La primera línea después del encabezado de la sección [operating systems] es 1. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para usar el comando **bootcfg/default** :

```
bootcfg /default /id 2
bootcfg /default /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
