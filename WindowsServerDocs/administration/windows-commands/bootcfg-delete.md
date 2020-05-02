---
title: bootcfg delete
description: Tema de referencia del comando bootcfg Delete, que elimina una entrada del sistema operativo en la sección sistemas operativos del archivo boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 71382e29-9b39-41c8-9c23-cf0ff829440a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 323e61d5d45933c518d8fee52c50330a7a15efe4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709597"
---
# <a name="bootcfg-delete"></a>bootcfg delete

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Elimina una entrada del sistema operativo en la sección [operating systems] del archivo boot. ini.

## <a name="syntax"></a>Sintaxis

```
bootcfg /delete [/s <computer> [/u <domain>\<user> /p <password>]] [/id <osentrylinenum>]
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

Para usar el comando **bootcfg/Delete** :

```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
