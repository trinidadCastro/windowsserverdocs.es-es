---
title: query user
description: Artículo de referencia para el comando QUERY User, que muestra información acerca de las sesiones de usuario en un servidor host de sesión Escritorio remoto.
ms.topic: article
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea760c32cc7955c96a363c994c2cb49227bceb2e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884407"
---
# <a name="query-user"></a>query user

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información acerca de las sesiones de usuario en un servidor host de sesión Escritorio remoto. Puede usar este comando para averiguar si un usuario específico ha iniciado sesión en un servidor host de sesión de Escritorio remoto específico. Este comando devuelve la siguiente información:

- Nombre del usuario

- Nombre de la sesión en el servidor host de sesión Escritorio remoto

- Identificador de sesión

- Estado de la sesión (activo o desconectado)

- Tiempo de inactividad (el número de minutos transcurridos desde la última pulsación o movimiento del mouse en la sesión)

- Fecha y hora en que el usuario inició sesión

> [!NOTE]
> Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
query user [<username> | <sessionname> | <sessionID>] [/server:<servername>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<username>` | Especifica el nombre de inicio de sesión del usuario que desea consultar. |
| `<sessionname>` | Especifica el nombre de la sesión que desea consultar. |
| `<sessionID>` | Especifica el identificador de la sesión que desea consultar. |
| /server:`<servername>` | Especifica el servidor host de sesión de Escritorio remoto que desea consultar. De lo contrario, se usa el servidor host de sesión Escritorio remoto actual. Este parámetro solo es necesario si utiliza este comando desde un servidor remoto. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Para usar este comando, debe tener el permiso control total o el permiso de acceso especial.

- Si no especifica un usuario que use los parámetros <*username*>, <*nombresesión*> o *SessionID* , se devuelve una lista de todos los usuarios que han iniciado sesión en el servidor. También puede usar el comando **query Session** para mostrar una lista de todas las sesiones de un servidor.

- Cuando el usuario de la **consulta** devuelve información, `(>)` se muestra un símbolo mayor que antes de la sesión actual.

### <a name="examples"></a>Ejemplos

Para mostrar información acerca de todos los usuarios que han iniciado sesión en el sistema, escriba:

```
query user
```

Para mostrar información sobre el usuario *user1* en el servidor *Servidor1*, escriba:

```
query user USER1 /server:Server1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de consulta](query.md)

- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
