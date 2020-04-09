---
title: query user
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6624c559bc85263da955f993ae7e4ad7e8b9ee2d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836808"
---
# <a name="query-user"></a>query user

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra información acerca de las sesiones de usuario en un servidor host de sesión de Escritorio remoto (host de sesión de escritorio remoto).
para obtener ejemplos de cómo usar este comando, vea [ejemplos](#BKMK_examples).
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services ha cambiado a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
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
> |          /?          |                                        Muestra la Ayuda en el símbolo del sistema.                                         |
> 
> ## <a name="remarks"></a>Comentarios
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
>   ## <a name="examples"></a><a name=BKMK_examples></a>Example
> - Para mostrar información acerca de todos los usuarios que han iniciado sesión en el sistema, escriba:
>   ```
>   query user
>   ```
> - Para mostrar información sobre el usuario USER1 en el servidor Servidor1, escriba:
>   ```
>   query user USER1 /server:SERver1
>   ```
>   ## <a name="additional-references"></a>Referencias adicionales
>   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
>   referencia de comandos de la
>   de [consulta](query.md) [servicios de escritorio remoto (Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
