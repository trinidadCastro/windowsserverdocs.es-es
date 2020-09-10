---
title: query session
description: Artículo de referencia del comando QUERY Session, que muestra información acerca de las sesiones de un servidor host de sesión Escritorio remoto.
ms.topic: reference
ms.assetid: abc0ace8-0b74-4b6e-a937-a78bb4b61a1f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2842aa9b0a38438a92ee2b7072b1a1054642fd62
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639883"
---
# <a name="query-session"></a>query session

Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información acerca de las sesiones en un servidor host de sesión Escritorio remoto. La lista incluye información no solo sobre las sesiones activas, sino también sobre otras sesiones que ejecuta el servidor.

> [!NOTE]
> Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
query session [<sessionname> | <username> | <sessionID>] [/server:<servername>] [/mode] [/flow] [/connect] [/counter]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<sessionname>` | Especifica el nombre de la sesión que desea consultar. |
| `<username>` | Especifica el nombre del usuario cuyas sesiones desea consultar. |
| `<sessionID>` | Especifica el identificador de la sesión que desea consultar. |
| /server:`<servername>` | Identifica el servidor host de sesión de escritorio remoto que se va a consultar. El valor predeterminado es el servidor actual. |
| /Mode | Muestra la configuración de línea actual. |
| /flow | Muestra la configuración actual del control de flujo. |
| /Connect | Muestra la configuración de conexión actual. |
| /Counter | Muestra información de los contadores actuales, incluido el número total de sesiones creadas, desconectadas y reconectadas. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Observaciones

- Un usuario siempre puede consultar la sesión en la que el usuario ha iniciado sesión actualmente. Para consultar otras sesiones, el usuario debe tener permiso de acceso especial.

- Si no especifica una sesión mediante los parámetros <*username*>, <*nombresesión*> o *SessionID* , esta consulta mostrará información acerca de todas las sesiones activas en el sistema.

- Cuando la **sesión de consulta** devuelve información, se muestra un símbolo mayor que `(>)` antes de la sesión actual. Por ejemplo:

    ```
    C:\>query session
        SESSIONNAME     USERNAME        ID STATE    TYPE    DEVICE
        console         Administrator1  0 active    wdcon
        >rdp-tcp#1      User1           1 active    wdtshare
        rdp-tcp                         2 listen    wdtshare
                                        4 idle
                                        5 idle
    ```

    Donde:
  - **Nombresesión** especifica el nombre asignado a la sesión.
  - **Username** indica el nombre de usuario del usuario conectado a la sesión.
  - **Estado** proporciona información sobre el estado actual de la sesión.
  - **Tipo** indica el tipo de sesión.
  - El **dispositivo**, que no está presente para las sesiones de consola o conectadas a la red, es el nombre del dispositivo asignado a la sesión.
  - Las sesiones en las que el estado inicial esté configurado como deshabilitada no se mostrarán en la lista de **sesiones de consulta** hasta que se habiliten.

### <a name="examples"></a>Ejemplos

Para mostrar información acerca de todas las sesiones activas en Server *servidor2*, escriba:

```
query session /server:Server2
```

Para mostrar información acerca de la sesión activa *modeM02*, escriba:

```
query session modeM02
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando de consulta](query.md)

- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
