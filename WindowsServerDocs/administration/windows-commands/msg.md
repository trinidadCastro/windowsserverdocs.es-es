---
title: msg
description: Artículo de referencia para el comando MSG, que envía un mensaje a un usuario en un servidor host de sesión Escritorio remoto
ms.topic: reference
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c2f26e12b10d0eb10197018d10d21f34f0da9cb5
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037653"
---
# <a name="msg"></a>msg

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Envía un mensaje a un usuario en un servidor host de sesión Escritorio remoto.

> [!NOTE]
> Debe tener el permiso de acceso especial de mensaje para enviar un mensaje.

## <a name="syntax"></a>Sintaxis

```
msg {<username> | <sessionname> | <sessionID>| @<filename> | *} [/server:<servername>] [/time:<seconds>] [/v] [/w] [<message>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<username>` | Especifica el nombre del usuario que desea recibir el mensaje. Si no especifica un usuario o una sesión, este comando muestra un mensaje de error. Al especificar una sesión, debe ser una activa. |
| `<sessionname>` | Especifica el nombre de la sesión que desea que reciba el mensaje. Si no especifica un usuario o una sesión, este comando muestra un mensaje de error. Al especificar una sesión, debe ser una activa. |
| `<sessionID>` | Especifica el identificador numérico de la sesión cuyo usuario desea recibir un mensaje. |
| `@<filename>` | Identifica un archivo que contiene una lista de nombres de usuario, nombres de sesión e identificadores de sesión que desea que reciban el mensaje. |
| * | Envía el mensaje a todos los nombres de usuario del sistema. |
| /server:`<servername>` | Especifica el servidor host de sesión de Escritorio remoto cuya sesión o usuario desea recibir el mensaje. Si no se especifica, **/Server** usa el servidor en el que ha iniciado sesión actualmente. |
| /Time`<seconds>` | Especifica la cantidad de tiempo que se muestra el mensaje enviado en la pantalla del usuario. Una vez alcanzado el límite de tiempo, el mensaje desaparece. Si no se establece ningún límite de tiempo, el mensaje permanece en la pantalla del usuario hasta que el usuario ve el mensaje y hace clic en **Aceptar**. |
| /v | Muestra información acerca de las acciones que se llevan a cabo. |
| /w | Espera una confirmación del usuario de que se ha recibido el mensaje. Use este parámetro con `/time:<*seconds*>` para evitar un posible retraso largo si el usuario no responde inmediatamente. También es útil usar este parámetro con **/v** . |
| `<message>` | Especifica el texto del mensaje que desea enviar. Si no se especifica ningún mensaje, se le pedirá que escriba un mensaje. Para enviar un mensaje incluido en un archivo, escriba el símbolo menor que (<) seguido del nombre de archivo. |
| /? | Muestra la ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

Para enviar un mensaje titulado, *vamos a reunirse a las 13:00 hoy* en todas las sesiones de *user1*, escriba:

```
msg User1 Let's meet at 1PM today
```

Para enviar el mismo mensaje a la sesión *modeM02*, escriba:

```
msg modem02 Let's meet at 1PM today
```

Para enviar el mensaje a todas las sesiones contenidas en el archivo *userList*, escriba:

```
msg @userlist Let's meet at 1PM today
```

Para enviar el mensaje a todos los usuarios que han iniciado sesión, escriba:

```
msg * Let's meet at 1PM today
```

Para enviar el mensaje a todos los usuarios, con un tiempo de espera de confirmación (por ejemplo, 10 segundos), escriba:

```
msg * /time:10 Let's meet at 1PM today
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
