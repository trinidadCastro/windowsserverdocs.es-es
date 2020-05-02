---
title: query user
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8c095226a5445e976e47e461044ec002dc007fe
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722690"
---
# <a name="query-user"></a>query user

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra información acerca de las sesiones de usuario en un servidor host de sesión de Escritorio remoto (host de sesión de escritorio remoto).

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
> ## <a name="syntax"></a>Sintaxis
> ```
> query user [<UserName> | <SessionName> | <SessionID>] [/server:<ServerName>]
> ```
> ### <a name="parameters"></a>Parámetros
> 
> |      Parámetro       |                                                     Descripción                                                     |
> |----------------------|---------------------------------------------------------------------------------------------------------------------|
> |      <UserName>      |                            Especifica el nombre de inicio de sesión del usuario que desea consultar.                             |
> |    <SessionName>     |                              Especifica el nombre de la sesión que desea consultar.                              |
> |     <SessionID>      |                               Especifica el identificador de la sesión que desea consultar.                               |
> | /server:<ServerName> | Especifica el servidor host de sesión de escritorio remoto que desea consultar. De lo contrario, se usa el servidor host de sesión de escritorio remoto actual. |
> |          /?          |                                        Muestra la ayuda en el símbolo del sistema.                                         |
> 
> ## <a name="remarks"></a>Observaciones
> - Puede usar este comando para averiguar si un usuario específico ha iniciado sesión en un servidor host de sesión de escritorio remoto específico. **query User** devuelve la siguiente información:
>   -   El nombre del usuario
>   -   El nombre de la sesión en el servidor host de sesión de escritorio remoto
>   -   El ID. de sesión
>   -   El estado de la sesión (activo o desconectado)
>   -   Tiempo de inactividad (el número de minutos transcurridos desde la última pulsación o movimiento del mouse en la sesión)
>   -   Fecha y hora en que el usuario inició sesión
> - Para usar **query User**, debe tener el permiso control total o el permiso de acceso especial información de consulta.
> - Si utiliza **query User** sin especificar <*UserName*>, <*nombresesión*> o <*SessionID*>, se devuelve una lista de todos los usuarios que han iniciado sesión en el servidor. También puede usar la **sesión de consulta** para mostrar una lista de todas las sesiones de un servidor.
> - Cuando el usuario de la **consulta** devuelve información, se muestra un signo mayor que (>) antes de la sesión actual.
> - El parámetro **/Server** solo es necesario si se utiliza **query User** desde un servidor remoto.
>   ## <a name="examples"></a>Ejemplos
> - Para mostrar información acerca de todos los usuarios que han iniciado sesión en el sistema, escriba:
>   ```
>   query user
>   ```
> - Para mostrar información sobre el usuario USER1 en el servidor Servidor1, escriba:
>   ```
>   query user USER1 /server:SERver1
>   ```
>   ## <a name="additional-references"></a>Referencias adicionales
>   - [Referencia de comandos de servicios de escritorio remoto de](command-line-syntax-key.md)
>   [consulta](query.md)
>   de clave de sintaxis de línea de comandos[(Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
