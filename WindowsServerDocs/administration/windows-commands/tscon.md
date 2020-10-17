---
title: tscon
description: Artículo de referencia de tscon, que se conecta a otra sesión en un servidor host de sesión de Escritorio remoto.
ms.topic: reference
ms.assetid: 315a9793-cd10-4987-bb68-89a9d13f7fce
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 17b18aea265ff7c703c2ef6c9c3d0021a9d9ea00
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156393"
---
# <a name="tscon"></a>tscon

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Se conecta a otra sesión en un servidor host de sesión Escritorio remoto.

> [!IMPORTANT]
> Debe tener el permiso de **acceso de control total** o el permiso de acceso **especial Connect** para conectarse a otra sesión.

> [!NOTE]
> Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxis

```
tscon {<sessionID> | <sessionname>} [/dest:<sessionname>] [/password:<pw> | /password:*] [/v]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| `<sessionID>` | Especifica el identificador de la sesión a la que se desea conectar. Si usa el parámetro opcional `/dest:<sessionname>` , también puede especificar el nombre de la sesión actual. |
| `<sessionname>` | Especifica el nombre de la sesión a la que desea conectarse. |
| /dest`<sessionname>` | Especifica el nombre de la sesión actual. Esta sesión se desconectará cuando se conecte a la nueva sesión. También puede usar este parámetro para conectar la sesión de otro usuario a otra sesión. |
| /Password`<pw>` | Especifica la contraseña del usuario propietario de la sesión a la que desea conectarse. Esta contraseña es necesaria cuando el usuario que se conecta no es propietario de la sesión. |
| /Password`*` | Solicita la contraseña del usuario propietario de la sesión a la que desea conectarse. |
| /v | Muestra información acerca de las acciones que se llevan a cabo. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

- Este comando genera un error si no se especifica una contraseña en el parámetro **/password** y la sesión de destino pertenece a un usuario que no es el actual.

- No se puede conectar a la sesión de consola.

## <a name="examples"></a>Ejemplos

Para conectarse a la *sesión 12* en el servidor host de sesión de servicios de escritorio remoto actual y desconectar la sesión actual, escriba:

```
tscon 12
```

Para conectarse a la *sesión 23* en el servidor host de sesión de servicios de escritorio remoto actual con la contraseña, y para desconectar la sesión *actual, escriba*:

```
tscon 23 /password:mypass
```

Para conectar la sesión denominada *TERM03* a la sesión denominada *TERM05*y, a continuación, desconectar la sesión *TERM05*, escriba:

```
tscon TERM03 /v /dest:TERM05
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
