---
title: Mensajes WSUS y consejos para solucionar problemas
description: 'Tema de Windows Server Update Service (WSUS): solución de problemas con mensajes de WSUS'
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f6317f7-bfe0-42d9-87ce-d8f038c728ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0c66e655ea6b6c44ee3ba375f75e6532fab74bfb
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948487"
---
# <a name="wsus-messages-and-troubleshooting-tips"></a>Mensajes WSUS y consejos para solucionar problemas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema contiene información acerca de los siguientes mensajes de WSUS:

-   "El equipo no ha comunicado el estado"

-   "ID. de mensaje 6703-error de sincronización de WSUS"

-   "Error 0x80070643: error irrecuperable durante la instalación"

-   "Algunos servicios no se están ejecutando. Compruebe los siguientes servicios [...] "

## <a name="computer-has-not-reported-status"></a>El equipo no ha comunicado el estado
Este mensaje se genera en la consola de WSUS cuando un equipo cliente WSUS no envía información al servidor WSUS para indicar su estado de actualización actual. Este problema se produce normalmente en el equipo cliente WSUS, no en el servidor WSUS.

Los motivos más comunes son:

-   El equipo ha perdido la conectividad con la red:
    -   El cable de red está desconectado.
    -   Un cable de red intermedio es defectuoso.
    -   El equipo tiene un adaptador de red defectuoso.
    -   El puerto de red al que se conecta el equipo se ha deshabilitado.
    -   El adaptador inalámbrico no puede asociarse con el punto de acceso inalámbrico corporativo y conectarse a él.
-   El equipo está apagado. (Se ha cerrado o está en modo de suspensión o hibernación).

## <a name="message-id-6703---wsus-synchronization-failed"></a>ID. de mensaje 6703-error de sincronización de WSUS
> Mensaje: error en la solicitud con el Estado HTTP 503: servicio no disponible.
> 
> Origen: Microsoft. UpdateServices. Administration. AdminProxy. createUpdateServer.

Cuando intente abrir los servicios de actualización en el servidor WSUS, recibirá el siguiente error:

> Error: error de conexión
> 
> Error al intentar conectarse al servidor WSUS. Este error puede producirse por una serie de motivos. Si el problema persiste, póngase en contacto con el administrador de red. Haga clic en el nodo restablecer servidor para conectarse de nuevo al servidor.

Además de lo anterior, se produce un error al intentar obtener acceso a la dirección URL del sitio web de administración de WSUS (es decir, `http://CM12CAS:8530`):

> Error 503 de HTTP. El servicio no está disponible

En esta situación, la causa más probable es que el grupo de aplicaciones WsusPool en IIS esté en un estado detenido.

Además, es probable que el límite de memoria privada (KB) para el grupo de aplicaciones esté establecido en el valor predeterminado de 1843200 KB. Si se produce este problema, aumente el límite de memoria privada a 4 GB (4 millones KB) y reinicie el grupo de aplicaciones. Para aumentar el límite de memoria privada, seleccione el grupo de aplicaciones de WsusPool y haga clic en configuración avanzada en editar grupo de aplicaciones. A continuación, establezca el límite de memoria privada en 4 GB (4 millones KB). Una vez reiniciado el grupo de aplicaciones, supervise el estado del componente SMS_WSUS_SYNC_MANAGER, WCM. log y archivo wsyncmgr. log en busca de errores. Tenga en cuenta que puede ser necesario aumentar el límite de memoria privada a 8 GB (8 millones KB) o superior según el entorno.

Para obtener más información, consulte: [error de HTTP 503 en la sincronización de WSUS en ConfigMgr 2012](https://blogs.technet.com/b/sus/archive/2015/03/23/configmgr-2012-support-tip-wsus-sync-fails-with-http-503-errors.aspx) .

## <a name="error-0x80070643-fatal-error-during-installation"></a>Error 0x80070643: error irrecuperable durante la instalación
El programa de instalación de WSUS usa Microsoft SQL Server para realizar la instalación. Este problema se produce porque el usuario que ejecuta el programa de instalación de WSUS no tiene permisos de administrador del sistema en SQL Server.

Para resolver este problema, conceda permisos de administrador del sistema a una cuenta de usuario o a una cuenta de grupo en SQL Server y, a continuación, ejecute de nuevo el programa de instalación de WSUS.

## <a name="some-services-are-not-running-check-the-following-services"></a>Algunos servicios no se están ejecutando. Compruebe los servicios siguientes:

- **Selfupdate:** Consulte [actualizaciones automáticas se debe actualizar](https://technet.microsoft.com/library/cc708554(v=ws.10).aspx) para obtener información acerca de la solución de problemas del servicio selfupdate.

- **WSSUService. exe:** Este servicio facilita la sincronización. Si tiene problemas con la sincronización, acceda a WSUSService. exe. para ello, haga clic en **Inicio**, seleccione **herramientas administrativas**, haga clic en **servicios**y, a continuación, busque **Windows Server Update Service** en la lista de servicios. Haz lo siguiente:
    
    -   Compruebe que este servicio se está ejecutando. Haga clic en **iniciar** si se detiene o en **reiniciar** para actualizar el servicio.
    
    -   Utilice Visor de eventos para comprobar los registros de eventos de la **aplicación**, **securit**y **del sistema** para ver si hay algún evento que pueda indicar un problema.
    
    -   También puede comprobar el archivo SoftwareDistribution. log para ver si hay eventos que puedan indicar un problema.

- **Servicio Web servicesSQL:** Los servicios web se hospedan en IIS. Si no se están ejecutando, asegúrese de que IIS se ejecuta (o se inicia). También puede intentar restablecer el servicio web escribiendo **iisreset** en un símbolo del sistema.

- **Servicio SQL:** Cada servicio, excepto el servicio selfupdate, requiere que el servicio SQL se esté ejecutando. Si alguno de los archivos de registro indica problemas de conexión de SQL, compruebe primero el servicio SQL. Para obtener acceso al servicio SQL, haga clic en **Inicio**, seleccione **herramientas administrativas**, haga clic en **servicios**y, a continuación, busque una de las siguientes opciones:
    
  - **MSSQLSERver** (si usa WMSDE o MSDE, o si usa SQL Server y usa el nombre de instancia predeterminado para el nombre de instancia)
    
  - **MSSQL $ WSUS** (si utiliza una base de datos de SQL Server y ha llamado a la instancia de base de datos "WSUS")
    
    Haga clic con el botón secundario en el servicio y, a continuación, haga clic en **iniciar** si el servicio no se está ejecutando o en **reiniciar** para actualizar el servicio si se está ejecutando.
