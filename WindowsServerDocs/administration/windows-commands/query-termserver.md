---
title: consulta termserver
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b89d3b4-236f-4376-90b6-939a0ec4b288
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8a0a4608a16df0336b90ea5df281278ae47a503
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722702"
---
# <a name="query-termserver"></a>consulta termserver

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Muestra una lista de todos los servidores host de sesión de Escritorio remoto (host de sesión de escritorio remoto) de la red.

> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
> ## <a name="syntax"></a>Sintaxis
> ```
> query termserver [<ServerName>] [/domain:<Domain>] [/address] [/continue]
> ```
> ### <a name="parameters"></a>Parámetros
> 
> |    Parámetro     |                                                                        Descripción                                                                         |
> |------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |   <ServerName>   |                                               Especifica el nombre que identifica el servidor host de sesión de escritorio remoto.                                               |
> | /Domain<Domain> | Especifica el dominio que se va a consultar para los servidores de Terminal Server. No es necesario especificar un dominio si está consultando el dominio en el que está trabajando actualmente. |
> |     /Address     |                                                  Muestra las direcciones de red y nodo para cada servidor.                                                  |
> |    /Continue     |                                              Impide que se realice una pausa después de mostrar cada pantalla de información.                                               |
> |        /?        |                                                            Muestra la ayuda en el símbolo del sistema.                                                            |
> 
> ## <a name="remarks"></a>Observaciones
> - **query termserver** busca en la red todos los servidores host de sesión de escritorio remoto asociados y devuelve la siguiente información:
>   - El nombre del servidor.
>   - La red (y la dirección del nodo si se usa la opción/Address)
>     ## <a name="examples"></a>Ejemplos
> - Para mostrar información acerca de todos los servidores host de sesión de escritorio remoto en la red, escriba:
>   ```
>   query termserver
>   ```
> - Para mostrar información sobre el servidor host de sesión de escritorio remoto llamado Server3, escriba:
>   ```
>   query termserver Server3
>   ```
> - Para mostrar información sobre todos los servidores host de sesión de escritorio remoto en el dominio CONTOSO, escriba:
>   ```
>   query termserver /domain:CONTOSO
>   ```
> - Para mostrar la dirección de red y de nodo del servidor host de sesión de escritorio remoto llamado Server3, escriba:
>   ```
>   query termserver Server3 /address
>   ```
>   ## <a name="additional-references"></a>Referencias adicionales
>   - [Referencia de comandos de servicios de escritorio remoto de](command-line-syntax-key.md)
>   [consulta](query.md)
>   de clave de sintaxis de línea de comandos[(Terminal Services)](remote-desktop-services-terminal-services-command-reference.md)
