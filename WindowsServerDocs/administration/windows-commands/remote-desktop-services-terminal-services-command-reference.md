---
title: Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto
description: Artículo de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2f371848-5c48-470c-908c-afbc95d3a805
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd054e59c7245eddfb66725bd5d95333f602b2c6
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956307"
---
# <a name="remote-desktop-services-terminal-services-command-reference"></a>Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

A continuación se muestra una lista de herramientas de línea de comandos de Servicios de Escritorio remoto.
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para conocer las novedades de la versión más reciente, consulte [novedades de servicios de escritorio remoto en Windows server 2012](/previous-versions/orphan-topics/ws.11/hh831527(v=ws.11)) en la biblioteca de TechNet de Windows Server.
>
> |                 Comando                 |                                                      Descripción                                                       |
> |-----------------------------------------|------------------------------------------------------------------------------------------------------------------------|
> |           [change](change.md)           | cambia Escritorio remoto configuración del servidor host de sesión de escritorio remoto para los inicios de sesión, las asignaciones de puertos COM y el modo de instalación. |
> |     [change logon](change-logon.md)     |    Habilita o deshabilita los inicios de sesión de las sesiones de cliente en un servidor host de sesión de escritorio remoto o muestra el estado de inicio de sesión actual.     |
> |      [change port](change-port.md)      |                   enumera o cambia las asignaciones de puertos COM para que sean compatibles con las aplicaciones de MS-DOS.                    |
> |      [change user](change-user.md)      |                                cambia el modo de instalación del servidor host de sesión de escritorio remoto.                                |
> |         [chglogon](chglogon.md)         |    Habilita o deshabilita los inicios de sesión de las sesiones de cliente en un servidor host de sesión de escritorio remoto o muestra el estado de inicio de sesión actual.     |
> |          [chgport](chgport.md)          |                   enumera o cambia las asignaciones de puertos COM para que sean compatibles con las aplicaciones de MS-DOS.                    |
> |           [chgusr](chgusr.md)           |                                cambia el modo de instalación del servidor host de sesión de escritorio remoto.                                |
> |         [flattemp](flattemp.md)         |                                      Habilita o deshabilita las carpetas temporales sin formato.                                       |
> |           [logoff](logoff.md)           |          Cierra la sesión de un usuario de una sesión en un servidor host de sesión de escritorio remoto y elimina la sesión del servidor.          |
> |              [msg](msg.md)              |                                Envía un mensaje a un usuario en un servidor host de sesión de escritorio remoto.                                 |
> |            [mstsc](mstsc.md)            |                       crea conexiones a servidores host de sesión de escritorio remoto o a otros equipos remotos.                        |
> |          [qappsrv](qappsrv.md)          |                             Muestra una lista de todos los servidores host de sesión de escritorio remoto en la red.                             |
> |         [qprocess](qprocess.md)         |                  Muestra información sobre los procesos que se ejecutan en un servidor host de sesión de escritorio remoto.                   |
> |            [consulta](query.md)            |                      Muestra información acerca de los procesos, las sesiones y los servidores host de sesión de escritorio remoto.                      |
> |    [query process](query-process.md)    |                  Muestra información sobre los procesos que se ejecutan en un servidor host de sesión de escritorio remoto.                   |
> |    [query session](query-session.md)    |                           Muestra información acerca de las sesiones en un servidor host de sesión de escritorio remoto.                            |
> | [query termserver](query-termserver.md) |                             Muestra una lista de todos los servidores host de sesión de escritorio remoto en la red.                             |
> |       [query user](query-user.md)       |                         Muestra información acerca de las sesiones de usuario en un servidor host de sesión de escritorio remoto.                         |
> |            [quser](quser.md)            |                         Muestra información acerca de las sesiones de usuario en un servidor host de sesión de escritorio remoto.                         |
> |          [qwinsta](qwinsta.md)          |                           Muestra información acerca de las sesiones en un servidor host de sesión de escritorio remoto.                            |
> |          [rdpsign](rdpsign.md)          |                          Permite firmar digitalmente un archivo Protocolo de escritorio remoto (. RDP).                          |
> |    [reset session](reset-session.md)    |                         Permite restablecer (eliminar) una sesión en un servidor host de sesión de escritorio remoto.                          |
> |          [rwinsta](rwinsta.md)          |                         Permite restablecer (eliminar) una sesión en un servidor host de sesión de escritorio remoto.                          |
> |           [shadow](shadow.md)           |            Permite controlar de forma remota una sesión activa de otro usuario en un servidor host de sesión de escritorio remoto.             |
> |            [tscon](tscon.md)            |                               Se conecta a otra sesión en un servidor host de sesión de escritorio remoto.                                |
> |         [tsdiscon](tsdiscon.md)         |                                 Desconecta una sesión de un servidor host de sesión de escritorio remoto.                                  |
> |           [tskill](tskill.md)           |                           Finaliza un proceso que se ejecuta en una sesión en un servidor host de sesión de escritorio remoto.                            |
> |           [tsprof](tsprof.md)           |              Copia la información de configuración de usuario de Servicios de Escritorio remoto de un usuario a otro.               |
