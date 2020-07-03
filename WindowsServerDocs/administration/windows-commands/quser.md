---
title: quser
description: Artículo de referencia para el comando quser, que muestra información acerca de las sesiones de usuario en un servidor host de sesión Escritorio remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8056204f-ed11-4c91-bb1d-c799283a48a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7df52ea8d2b30d9e365d6dc79d53aad9bd0782f9
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931110"
---
# <a name="quser"></a>quser

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información acerca de las sesiones de usuario en un servidor host de sesión Escritorio remoto. Puede usar este comando para averiguar si un usuario específico ha iniciado sesión en un servidor host de sesión de Escritorio remoto específico. Este comando devuelve la siguiente información:

- Nombre del usuario

- Nombre de la sesión en el servidor host de sesión Escritorio remoto

- Identificador de sesión

- Estado de la sesión (activo o desconectado)

- Tiempo de inactividad (el número de minutos transcurridos desde la última pulsación o movimiento del mouse en la sesión)

- Fecha y hora en que el usuario inició sesión

> [!NOTE]
> Este comando es el mismo que el [comando QUERY User](query-user.md). Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows Server](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
quser [<username> | <sessionname> | <sessionID>] [/server:<servername>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<username>` | Especifica el nombre de inicio de sesión del usuario que desea consultar. |
| `<sessionname>` | Especifica el nombre de la sesión que desea consultar. |
| `<sessionID>` | Especifica el identificador de la sesión que desea consultar. |
| /server:`<servername>` | Especifica el servidor host de sesión de Escritorio remoto que desea consultar. De lo contrario, se usa el servidor host de sesión Escritorio remoto actual. Este parámetro solo es necesario si utiliza este comando desde un servidor remoto. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Para usar este comando, debe tener el permiso control total o el permiso de acceso especial.

- Si no especifica un usuario que use los parámetros <*username*>, <*nombresesión*> o *SessionID* , se devuelve una lista de todos los usuarios que han iniciado sesión en el servidor. También puede usar el comando **query Session** para mostrar una lista de todas las sesiones de un servidor.

- Cuando **quser** devuelve información, `(>)` se muestra un símbolo mayor que antes de la sesión actual.

### <a name="examples"></a>Ejemplos

Para mostrar información acerca de todos los usuarios que han iniciado sesión en el sistema, escriba:

```
quser
```

Para mostrar información sobre el usuario *user1* en el servidor *Servidor1*, escriba:

```
quser USER1 /server:Server1
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Query User (comando)](query-user.md)

- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
