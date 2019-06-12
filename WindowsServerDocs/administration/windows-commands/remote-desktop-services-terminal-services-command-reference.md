---
title: Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f371848-5c48-470c-908c-afbc95d3a805
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7524973c50b597b8fd8798c5268746dd1d991d7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441837"
---
# <a name="remote-desktop-services-terminal-services-command-reference"></a>Referencia de comandos (Terminal Services) de Servicios de Escritorio remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La siguiente es una lista de herramientas de línea de comandos de servicios de escritorio remoto.
> [!NOTE]
> En Windows Server 2008 R2, el nombre de Terminal Services se cambió a Servicios de Escritorio remoto. Para descubrir las novedades de la versión más reciente, consulte [novedades nuevos servicios de escritorio remoto en Windows Server 2012](https://technet.microsoft.com/library/hh831527) en la biblioteca de TechNet de Windows Server.
> 
> |                 Comando                 |                                                      Descripción                                                       |
> |-----------------------------------------|------------------------------------------------------------------------------------------------------------------------|
> |           [change](change.md)           | cambia la configuración del servidor Host de sesión de escritorio remoto (Host de sesión de rd) para los inicios de sesión, las asignaciones de puertos COM y el modo de instalación. |
> |     [change logon](change-logon.md)     |    Habilita o deshabilita los inicios de sesión de sesiones de cliente en un servidor Host de sesión de escritorio remoto o muestra el estado de inicio de sesión actual.     |
> |      [change port](change-port.md)      |                   Muestra o cambia las asignaciones de puerto COM para que sean compatibles con las aplicaciones MS-DOS.                    |
> |      [change user](change-user.md)      |                                cambia el modo de instalación para el servidor Host de sesión de escritorio remoto.                                |
> |         [chglogon](chglogon.md)         |    Habilita o deshabilita los inicios de sesión de sesiones de cliente en un servidor Host de sesión de escritorio remoto o muestra el estado de inicio de sesión actual.     |
> |          [chgport](chgport.md)          |                   Muestra o cambia las asignaciones de puerto COM para que sean compatibles con las aplicaciones MS-DOS.                    |
> |           [chgusr](chgusr.md)           |                                cambia el modo de instalación para el servidor Host de sesión de escritorio remoto.                                |
> |         [flattemp](flattemp.md)         |                                      Habilita o deshabilita las carpetas temporales lineales.                                       |
> |           [logoff](logoff.md)           |          Cierra la sesión en un servidor Host de sesión de escritorio remoto de un usuario y elimina la sesión del servidor.          |
> |              [msg](msg.md)              |                                Envía un mensaje a un usuario en un servidor Host de sesión de escritorio remoto.                                 |
> |            [mstsc](mstsc.md)            |                       crea las conexiones a servidores Host de sesión de escritorio remoto o en otros equipos remotos.                        |
> |          [qappsrv](qappsrv.md)          |                             Muestra una lista de todos los servidores Host de sesión de escritorio remoto en la red.                             |
> |         [qprocess](qprocess.md)         |                  Muestra información acerca de los procesos que se ejecutan en un servidor Host de sesión de escritorio remoto.                   |
> |            [query](query.md)            |                      Muestra información acerca de los procesos, sesiones y los servidores Host de sesión de escritorio remoto.                      |
> |    [proceso de consulta](query-process.md)    |                  Muestra información acerca de los procesos que se ejecutan en un servidor Host de sesión de escritorio remoto.                   |
> |    [sesión de consulta](query-session.md)    |                           Muestra información acerca de las sesiones en un servidor Host de sesión de escritorio remoto.                            |
> | [consulta termserver](query-termserver.md) |                             Muestra una lista de todos los servidores Host de sesión de escritorio remoto en la red.                             |
> |       [consultar el usuario](query-user.md)       |                         Muestra información sobre las sesiones de usuario en un servidor Host de sesión de escritorio remoto.                         |
> |            [quser](quser.md)            |                         Muestra información sobre las sesiones de usuario en un servidor Host de sesión de escritorio remoto.                         |
> |          [qwinsta](qwinsta.md)          |                           Muestra información acerca de las sesiones en un servidor Host de sesión de escritorio remoto.                            |
> |          [rdpsign](rdpsign.md)          |                          Le permite firmar digitalmente un archivo de protocolo de escritorio remoto (.rdp).                          |
> |    [reset session](reset-session.md)    |                         Permite restablecer (eliminar) una sesión en un servidor Host de sesión de escritorio remoto.                          |
> |          [rwinsta](rwinsta.md)          |                         Permite restablecer (eliminar) una sesión en un servidor Host de sesión de escritorio remoto.                          |
> |           [shadow](shadow.md)           |            Le permite controlar remotamente una sesión activa de otro usuario en un servidor Host de sesión de escritorio remoto.             |
> |            [tscon](tscon.md)            |                               Se conecta a otra sesión de un servidor Host de sesión de escritorio remoto.                                |
> |         [tsdiscon](tsdiscon.md)         |                                 Desconecta una sesión de un servidor Host de sesión de escritorio remoto.                                  |
> |           [tskill](tskill.md)           |                           Finaliza un proceso que se ejecuta en una sesión en un servidor Host de sesión de escritorio remoto.                            |
> |           [tsprof](tsprof.md)           |              Copia la información de configuración de usuario de servicios de escritorio remoto de un usuario a otro.               |
