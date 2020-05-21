---
title: query session
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abc0ace8-0b74-4b6e-a937-a78bb4b61a1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b0d122beac43abfd826cb406adac4aa277fc72e
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436280"
---
# <a name="query-session"></a>query session

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información acerca de las sesiones de un servidor host de sesión de Escritorio remoto (host de sesión de escritorio remoto).
La lista incluye información no solo sobre las sesiones activas, sino también sobre otras sesiones que ejecuta el servidor.

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
> ## <a name="syntax"></a>Sintaxis
> ```
> query session [<SessionName> | <UserName> | <SessionID>] [/server:<ServerName>] [/mode] [/flow] [/connect] [/counter]
> ```
> ### <a name="parameters"></a>Parámetros
>
> |      Parámetro       |                                                      Descripción                                                      |
> |----------------------|-----------------------------------------------------------------------------------------------------------------------|
> |    <SessionName>     |                               Especifica el nombre de la sesión que desea consultar.                               |
> |      <UserName>      |                           Especifica el nombre del usuario cuyas sesiones desea consultar.                            |
> |     <SessionID>      |                                Especifica el identificador de la sesión que desea consultar.                                |
> | /server:<ServerName> |                  Identifica el servidor host de sesión de escritorio remoto que se va a consultar. El valor predeterminado es el servidor actual.                   |
> |        /Mode         |                                            Muestra la configuración de línea actual.                                            |
> |        /flow         |                                        Muestra la configuración actual del control de flujo.                                        |
> |       /Connect       |                                          Muestra la configuración de conexión actual.                                           |
> |       /Counter       | Muestra información de los contadores actuales, incluido el número total de sesiones creadas, desconectadas y reconectadas. |
> |          /?          |                                         Muestra la ayuda en el símbolo del sistema.                                          |
>
>#### <a name="remarks"></a>Observaciones
> - Un usuario siempre puede consultar la sesión en la que el usuario ha iniciado sesión actualmente. Para consultar otras sesiones, el usuario debe tener el permiso de acceso especial información de consulta.
> - Si no especifica una sesión mediante <*nombresesión*>, <*nombreusuario*> o <*SessionID*>, la sesión de la **consulta** muestra información acerca de todas las sesiones activas en el sistema.
> - Cuando la **sesión de consulta** devuelve información, se muestra un signo mayor que (>) antes de la sesión actual. A continuación se muestra una salida de ejemplo para la **sesión de consulta**:
>   ```
>   C:\>query session
>    SESSIONNAME    USERNAME       ID STATE  TYPE   DEVICE
>   console        Administrator1  0 active wdcon
>    rdp-tcp#1      User1           1 active wdtshare
>    rdp-tcp                        2 listen wdtshare
>                                   4 idle
>                                   5 idle
>   ```
>   El signo mayor que (>) indica la sesión actual. NOMBRESESIÓN especifica el nombre asignado a la sesión. USERNAME indica el nombre de usuario del usuario conectado a la sesión. Estado proporciona información sobre el estado actual de la sesión. TIPO indica el tipo de sesión. El dispositivo, que no está presente para las sesiones de consola o conectadas a la red, es el nombre del dispositivo asignado a la sesión. El comentario que sigue a la información de la sesión procede del perfil de sesión. Las sesiones en las que el estado inicial está configurado como deshabilitado no se muestran en la lista de **sesiones de consulta** hasta que se habilitan.
>   ## <a name="examples"></a>Ejemplos
> - Para mostrar información acerca de todas las sesiones activas en Server servidor2, escriba:
>   ```
>   query session /server:SERver2
>   ```
> - Para mostrar información acerca de la sesión activa modeM02, escriba:
>   ```
>   query session modeM02
>   ```
>   ## <a name="additional-references"></a>Referencias adicionales
>   - Clave de sintaxis [de línea de comandos](command-line-syntax-key.md) 
>    [consulta](query.md) 
>    de [Referencia de comandos de servicios de escritorio remoto (Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
