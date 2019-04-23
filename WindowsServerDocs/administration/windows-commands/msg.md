---
title: msg
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b6c6193fc439140fa559643427067066e819c502
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879556"
---
# <a name="msg"></a>msg

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envía un mensaje a un usuario en un servidor Host de sesión de escritorio remoto (Host de sesión de rd).
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
msg {<UserName> | <SessionName> | <SessionID>| @<FileName> | *} [/server:<ServerName>] [/time:<Seconds>] [/v] [/w] [<Message>]
```

## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|<UserName>|Especifica el nombre del usuario que desea recibir el mensaje.|
|<SessionName>|Especifica el nombre de la sesión que desea recibir el mensaje.|
|<SessionID>|Especifica el identificador numérico de la sesión de usuario que desea recibir un mensaje.|
|@<FileName>|Identifica un archivo que contiene una lista de nombres de usuario, los nombres de sesión y los identificadores de sesión que desea recibir el mensaje.|
|*|Envía el mensaje a todos los nombres de usuario en el sistema.|
|/server:<ServerName>|Especifica el servidor Host de sesión de escritorio remoto cuya sesión o usuario que desea recibir el mensaje. Si no se especifica, **/Server** usa el servidor al que han iniciado sesión.|
|/Time:<Seconds>|Especifica la cantidad de tiempo que se muestra el mensaje enviado en la pantalla del usuario. Cuando se alcanza el límite de tiempo, el mensaje desaparece. Si no se establece ningún límite de tiempo, el mensaje permanece en la pantalla del usuario hasta que el usuario verá el mensaje y hace clic en **Aceptar**.|
|/v|Muestra información sobre las acciones que se va a realizar.|
|/w|Espera una confirmación del usuario que se ha recibido el mensaje. Use este parámetro con **/time:**<*segundos*> para evitar un retraso largo posible si el usuario no responde inmediatamente. Usa este parámetro con **/v** también es útil.|
|<Message>|Especifica el texto del mensaje que desea enviar. Si no se especifica ningún mensaje, se le pedirá que escriba un mensaje. Para enviar un mensaje que se encuentra en un archivo, escriba el símbolo menor que (<) seguido del nombre de archivo.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios
-   Si no especifica un usuario o una sesión, **msg** muestra un mensaje de error. Al especificar una sesión, debe ser uno activo.
-   El usuario debe tener permiso de acceso especial mensaje para enviar un mensaje.

## <a name="BKMK_examples"></a>Ejemplos
-   Para enviar que el mensaje titulado "Quedemos hoy a la 1 P.M." a todas las sesiones de User1, escriba:
    ```
    msg User1 Let's meet at 1PM today
    ```
-   Para enviar el mismo mensaje a la sesión modeM02, escriba:
    ```
    msg modem02 Let's meet at 1PM today
    ```
-   Para enviar el mensaje a la sesión 12, escriba:
    ```
    msg 12 Let's meet at 1PM today
    ```
-   Para enviar el mensaje a todas las sesiones contenidas en el archivo USERlist, escriba:
    ```
    msg @userlist Let's meet at 1PM today
    ```
-   Para enviar el mensaje a todos los usuarios que han iniciado sesión, escriba:
    ```
    msg * Let's meet at 1PM today
    ```
-   Para enviar el mensaje a todos los usuarios, con un tiempo de espera de confirmación (por ejemplo, 10 segundos), escriba:
    ```
    msg * /time:10 Let's meet at 1PM today
    ```
    
#### <a name="additional-references"></a>Referencias adicionales
-  [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-  [Servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia del comando](remote-desktop-services-terminal-services-command-reference.md)
