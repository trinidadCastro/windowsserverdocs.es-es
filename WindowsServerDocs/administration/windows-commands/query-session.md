---
title: sesión de consulta
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abc0ace8-0b74-4b6e-a937-a78bb4b61a1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b96f7e6a6252b40e5a32910d161b1fa0ff94e11
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868896"
---
# <a name="query-session"></a>sesión de consulta

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de las sesiones en un servidor Host de sesión de escritorio remoto (Host de sesión de rd).
La lista incluye información no sólo sobre las sesiones activas, sino también acerca de otras sesiones que se ejecuta el servidor.
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
## <a name="syntax"></a>Sintaxis
```
query session [<SessionName> | <UserName> | <SessionID>] [/server:<ServerName>] [/mode] [/flow] [/connect] [/counter]
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|<SessionName>|Especifica el nombre de la sesión que desea consultar.|
|<UserName>|Especifica el nombre del usuario cuyas sesiones quiere consultar.|
|<SessionID>|Especifica el identificador de la sesión que desea consultar.|
|/server:<ServerName>|Identifica el servidor Host de sesión de escritorio remoto a la consulta. El valor predeterminado es el servidor actual.|
|/ Mode|Muestra la configuración de línea actual.|
|/flow|Muestra la configuración de control de flujo actual.|
|/connect|Muestra actual conectar configuración.|
|/counter|Actual contadores muestra información, incluido el número total de sesiones creadas, desconectado y vuelto a conectar.|
|/?|Muestra la ayuda en el símbolo del sistema.|
## <a name="remarks"></a>Comentarios
-   Un usuario siempre puede consultar la sesión a la que el usuario se ha iniciado sesión actualmente. Para consultar otras sesiones, el usuario debe tener permiso de acceso especial de información de consulta.
-   Si no especifica una sesión mediante el uso de <*SessionName*>, <*UserName*>, o <*SessionID*>, **consultar sesión** Muestra información sobre todas las sesiones activas en el sistema.
-   Cuando **consultar sesión** devuelve información, un signo mayor que (>) se muestra antes de la sesión actual. Siguiente es la salida de ejemplo para **consultar sesión**:
    ```
    C:\>query session
     SESSIONNAME    USERNAME       ID STATE  TYPE   DEVICE
    >console        Administrator1  0 active wdcon
     rdp-tcp#1      User1           1 active wdtshare
     rdp-tcp                        2 listen wdtshare
                                    4 idle
                                    5 idle
    ```
    El signo mayor que (>) indica que la sesión actual. SESSIONNAME especifica el nombre asignado a la sesión. Nombre de usuario indica el nombre de usuario del usuario conectado a la sesión. ESTADO proporciona información sobre el estado actual de la sesión. TIPO indica el tipo de sesión. DISPOSITIVO, lo que no está presente para la consola o en sesiones conectadas a la red, es el nombre del dispositivo asignado a la sesión. El comentario que sigue a la información de sesión es desde el perfil de la sesión. Las sesiones en el que el estado inicial está configurado como deshabilitado no se muestran en el **consultar sesión** lista hasta que se habilitan.
## <a name="BKMK_examples"></a>Ejemplos
-   Para mostrar información sobre todas las sesiones activas en el servidor Servidor2, escriba:
    ```
    query session /server:SERver2
    ```
-   Para mostrar información acerca de la sesión activa Módem02, escriba:
    ```
    query session modeM02
    ```
#### <a name="additional-references"></a>Referencias adicionales
[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
[consulta](query.md)
[servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia de comandos](remote-desktop-services-terminal-services-command-reference.md)
