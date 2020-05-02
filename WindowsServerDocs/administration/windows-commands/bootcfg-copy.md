---
title: bootcfg copy
description: Tema de referencia para el comando bootcfg Copy, que realiza una copia de una entrada de arranque existente, a la que puede agregar opciones de la línea de comandos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2a236c2a-8675-444d-b695-9cbc9aff643b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 033227ab1d9efcdf1d58708a75085067766a6af2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709681"
---
# <a name="bootcfg-copy"></a>bootcfg copy

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Realiza una copia de una entrada de arranque existente, a la que puede agregar opciones de la línea de comandos.

## <a name="syntax"></a>Sintaxis

```
bootcfg /copy [/s <computer> [/u <domain>\<user> /p <password>]] [/d <description>] [/id <osentrylinenum>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `/s <computer>` | Especifica el nombre o la dirección IP de un equipo remoto (no use barras diagonales inversas). El valor predeterminado es el equipo local. |
| `/u <domain>\<user>`  | Ejecuta el comando con los permisos de cuenta del usuario especificado por `<user>` o `<domain>\<user>`. El valor predeterminado son los permisos del usuario que ha iniciado la sesión actual en el equipo que emite el comando. |
| `/p <password>` | Especifica la contraseña de la cuenta de usuario que se especifica en el parámetro **/u** . |
| `/d <description>` | Especifica la descripción de la nueva entrada de sistema operativo. |
| `/id <osentrylinenum>` | Especifica el número de línea de entrada del sistema operativo en la sección [operating systems] del archivo boot. ini en el que se agregan las opciones de carga del sistema operativo. La primera línea después del encabezado de la sección [operating systems] es 1. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para copiar la entrada de arranque 1 y escriba \ABC Server \ como Descripción:

```
bootcfg /copy /d \ABC Server\ /id 1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
