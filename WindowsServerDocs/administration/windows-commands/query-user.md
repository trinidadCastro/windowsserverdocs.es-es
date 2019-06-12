---
title: consultar el usuario
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c6e0d936e03e14e134c7bfd450a878fda1a796c4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442001"
---
# <a name="query-user"></a>consultar el usuario

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información sobre las sesiones de usuario en un servidor Host de sesión de escritorio remoto (Host de sesión de rd).
Para obtener ejemplos de cómo usar este comando, consulte [ejemplos](#BKMK_examples).
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
> ## <a name="syntax"></a>Sintaxis
> ```
> query user [<UserName> | <SessionName> | <SessionID>] [/server:<ServerName>]
> ```
> ## <a name="parameters"></a>Parámetros
> 
> |      Parámetro       |                                                     Descripción                                                     |
> |----------------------|---------------------------------------------------------------------------------------------------------------------|
> |      <UserName>      |                            Especifica el nombre de inicio de sesión del usuario que desea consultar.                             |
> |    <SessionName>     |                              Especifica el nombre de la sesión que desea consultar.                              |
> |     <SessionID>      |                               Especifica el identificador de la sesión que desea consultar.                               |
> | /server:<ServerName> | Especifica el servidor Host de sesión de escritorio remoto que desea consultar. En caso contrario, se usa el servidor Host de sesión de escritorio remoto actual. |
> |          /?          |                                        Muestra la ayuda en el símbolo del sistema.                                         |
> 
> ## <a name="remarks"></a>Comentarios
> - Puede usar este comando para averiguar si un usuario específico ha iniciado sesión en un servidor Host de sesión de escritorio remoto específico. **consultar el usuario** devuelve la siguiente información:
>   -   El nombre del usuario
>   -   El nombre de la sesión en el servidor Host de sesión de escritorio remoto
>   -   El identificador de sesión
>   -   El estado de la sesión (activa o desconectada)
>   -   El tiempo de inactividad (el número de minutos desde la última pulsación de tecla o movimiento del mouse en la sesión)
>   -   La fecha y hora que el usuario inició sesión
> - Para usar **usuario consulta**, debe tener el permiso Control total o permiso de acceso especial de la información de consulta.
> - Si usas **usuario consulta** sin especificar <*UserName*>, <*SessionName*>, o <*SessionID*>, una lista de todos se devuelve a los usuarios que han iniciado sesión en el servidor. Como alternativa, también puede usar **consultar sesión** para mostrar una lista de todas las sesiones en un servidor.
> - Cuando **usuario consulta** devuelve información, un signo mayor que (>) se muestra antes de la sesión actual.
> - El **/Server** parámetro es obligatorio solo si usa **usuario consulta** desde un servidor remoto.
>   ## <a name="BKMK_examples"></a>Ejemplos
> - Para mostrar información acerca de todos los usuarios registrados en el sistema, escriba:
>   ```
>   query user
>   ```
> - Para mostrar información sobre el usuario USER1 en el servidor Servidor1, escriba:
>   ```
>   query user USER1 /server:SERver1
>   ```
>   #### <a name="additional-references"></a>Referencias adicionales
>   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
>   [consulta](query.md)
>   [servicios de escritorio remoto &#40;servicios de Terminal Server&#41; referencia de comandos](remote-desktop-services-terminal-services-command-reference.md)
