---
title: logman delete
description: Artículo de referencia para el comando Logman Delete, que elimina un recopilador de datos existente.
ms.topic: reference
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbcae06f149f0ea1c443f91fec45d87b5a358136
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023809"
---
# <a name="logman-delete"></a>logman delete

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Elimina un recopilador de datos existente.

## <a name="syntax"></a>Sintaxis

```
logman delete <[-n] <name>> [options]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| -s `<computer name>` | Ejecuta el comando en el equipo remoto especificado. |
| -config `<value>` | Especifica el archivo de configuración que contiene opciones de comando. |
| [-n] `<name>` | Nombre del objeto de destino. |
| -ETS | Envía comandos a sesiones de seguimiento de eventos directamente sin guardar o programar. |
| -[-] u `<user [password]>` | Especifica el usuario que se va a ejecutar como. Al escribir un \* para la contraseña, se solicita la contraseña. La contraseña no se muestra cuando se escribe en la solicitud de contraseña. |
| /? | Muestra la ayuda contextual. |

### <a name="examples"></a>Ejemplos

Para eliminar el *perf_log*del recopilador de datos, escriba:

```
logman delete perf_log
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Logman (comando)](logman.md)
