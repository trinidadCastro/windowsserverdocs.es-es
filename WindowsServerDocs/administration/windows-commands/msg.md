---
title: msg
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91a85cd5d043b6c613f88e199670f55f6e0e72a7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839148"
---
# <a name="msg"></a>msg

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envía un mensaje a un usuario en un servidor host de sesión de Escritorio remoto (host de sesión de escritorio remoto).
para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services ha cambiado a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.

## <a name="syntax"></a>Sintaxis
```
msg {<UserName> | <SessionName> | <SessionID>| @<FileName> | *} [/server:<ServerName>] [/time:<Seconds>] [/v] [/w] [<Message>]
```

### <a name="parameters"></a>Parámetros

|      Parámetro       |                                                                                                                               Descripción                                                                                                                               |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      <UserName>      |                                                                                                  Especifica el nombre del usuario que desea recibir el mensaje.                                                                                                   |
|    <SessionName>     |                                                                                                 Especifica el nombre de la sesión que desea que reciba el mensaje.                                                                                                 |
|     <SessionID>      |                                                                                            Especifica el identificador numérico de la sesión cuyo usuario desea recibir un mensaje.                                                                                            |
|     @<FileName>      |                                                                         Identifica un archivo que contiene una lista de nombres de usuario, nombres de sesión e identificadores de sesión que desea que reciban el mensaje.                                                                         |
|          \*          |                                                                                                           Envía el mensaje a todos los nombres de usuario del sistema.                                                                                                            |
| /server:<ServerName> |                                              Especifica el servidor host de sesión de escritorio remoto cuya sesión o usuario desea recibir el mensaje. Si no se especifica, **/Server** usa el servidor en el que ha iniciado sesión actualmente.                                              |
|   /Time:<Seconds>    | Especifica la cantidad de tiempo que se muestra el mensaje enviado en la pantalla del usuario. Una vez alcanzado el límite de tiempo, el mensaje desaparece. Si no se establece ningún límite de tiempo, el mensaje permanece en la pantalla del usuario hasta que el usuario ve el mensaje y hace clic en **Aceptar**. |
|          /v          |                                                                                                         Muestra información acerca de las acciones que se llevan a cabo.                                                                                                         |
|          /w          |         Espera una confirmación del usuario de que se ha recibido el mensaje. Use este parámetro con **/Time:** <*segundos*> para evitar un posible retraso largo si el usuario no responde inmediatamente. También es útil usar este parámetro con **/v** .          |
|      <Message>       |                  Especifica el texto del mensaje que desea enviar. Si no se especifica ningún mensaje, se le pedirá que escriba un mensaje. Para enviar un mensaje incluido en un archivo, escriba el símbolo menor que (<) seguido del nombre de archivo.                  |
|          /?          |                                                                                                                  Muestra la Ayuda en el símbolo del sistema.                                                                                                                   |

## <a name="remarks"></a>Comentarios
-   Si no especifica un usuario o una sesión, **MSG** muestra un mensaje de error. Al especificar una sesión, debe ser una activa.
-   El usuario debe tener el permiso de acceso especial de mensaje para enviar un mensaje.

## <a name="examples"></a><a name=BKMK_examples></a>Example
-   Para enviar el mensaje titulado vamos a reunirse a las 13:00 hoy en todas las sesiones de user1, escriba:
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

## <a name="additional-references"></a>Referencias adicionales
-  - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
-  [Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto](remote-desktop-services-terminal-services-command-reference.md)
