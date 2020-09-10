---
title: cerrar sesión
description: Artículo de referencia para el comando logoff, que cierra la sesión de un usuario en un servidor host de sesión Escritorio remoto y elimina la sesión.
ms.topic: reference
ms.assetid: 939f09cc-de8c-436c-a05d-aca5f2a06371
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b7a211f1b4bcb76ea63d79ae38d1cf78c757d6e0
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639312"
---
# <a name="logoff"></a>cerrar sesión

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Cierra la sesión de un usuario en un servidor host de sesión Escritorio remoto y elimina la sesión.

## <a name="syntax"></a>Sintaxis
```
logoff [<sessionname> | <sessionID>] [/server:<servername>] [/v]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<sessionname>` | Especifica el nombre de la sesión. Debe ser una sesión activa.|
| `<sessionID>` | Especifica el identificador numérico que identifica la sesión en el servidor. |
| /server:`<servername>` | Especifica el servidor host de sesión de Escritorio remoto que contiene la sesión cuyo usuario desea cerrar sesión. Si no se especifica, se usa el servidor en el que está activo actualmente. |
| /v | Muestra información acerca de las acciones que se llevan a cabo. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Siempre puede cerrar sesión en la sesión en la que ha iniciado sesión actualmente. Sin embargo, debe tener el permiso **control total** para cerrar la sesión de los usuarios de otras sesiones.

- El cierre de sesión de un usuario sin advertencia puede provocar la pérdida de datos en la sesión del usuario. Debe enviar un mensaje al usuario mediante el comando **MSG** para avisar al usuario antes de realizar esta acción.

- Si `<sessionID>` `<sessionname>` no se especifica o, el **cierre** de sesión cierra la sesión del usuario en la sesión actual.

- Después de cerrar la sesión de un usuario, finalizan todos los procesos y se elimina la sesión del servidor.

- No se puede cerrar la sesión de un usuario de la consola.

### <a name="examples"></a>Ejemplos

Para cerrar la sesión de un usuario de la sesión actual, escriba:

```
logoff
```

Para cerrar la sesión de un usuario mediante el identificador de la sesión, por ejemplo, la *sesión 12*, escriba:

```
logoff 12
```

Para cerrar la sesión de un usuario mediante el nombre de la sesión y el servidor, por ejemplo, *TERM04* de sesión en *server1*, escriba:

```
logoff TERM04 /server:Server1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
