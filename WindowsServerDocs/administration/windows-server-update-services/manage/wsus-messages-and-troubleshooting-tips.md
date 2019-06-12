---
title: Sugerencias para solucionar problemas y mensajes WSUS
description: Tema de Windows Server Update Service (WSUS) - solución de problemas mediante mensajes WSUS
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4acc5e284d5ca7a62335a1c52f341cda3dfb547e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439733"
---
# <a name="wsus-messages-and-troubleshooting-tips"></a>Sugerencias para solucionar problemas y mensajes WSUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema contiene información acerca de los siguientes mensajes WSUS:

-   "Equipo no ha notificado el estado"

-   "Id. de mensaje 6703 - error de sincronización de WSUS"

-   "Error 0x80070643: Error irrecuperable durante la instalación"

-   "Algunos servicios no se está ejecutando. Compruebe los siguientes servicios [...]"

## <a name="computer-has-not-reported-status"></a>Equipo no ha informado de su estado
Cuando un equipo cliente WSUS no envía información al servidor WSUS para indicar su estado actual de la actualización, se genera este mensaje en la consola de WSUS. Este problema se suele producir por el equipo cliente WSUS, no en el servidor WSUS.

Las razones más comunes son:

-   El equipo ha perdido la conectividad a la red:
    -   Se desconecta el cable de red.
    -   Un cable de red que intervengan es defectuoso.
    -   El equipo tiene un adaptador de red defectuoso.
    -   Se deshabilitó el puerto de red al que se conecta el equipo.
    -   El adaptador inalámbrico no puede asociar y conectarse al punto de acceso inalámbrico corporativa.
-   El equipo está apagado. (Se ha cerrado o está en modo de suspensión o hibernación.)

## <a name="message-id-6703---wsus-synchronization-failed"></a>Id. de mensaje 6703 - sincronización de WSUS no se pudo
> Mensaje: Error en la solicitud con estado HTTP 503: Servicio no disponible.
> 
> Origen: Microsoft.UpdateServices.Administration.AdminProxy.createUpdateServer.

Cuando se intenta abrir Servicios de actualización en el servidor WSUS recibirá el error siguiente:

> Error: Error de conexión
> 
> Se produjo un error al intentar conectar con el servidor WSUS. Este error puede ocurrir por varios motivos. Si el problema persiste, póngase en contacto con el Administrador de red. Haga clic en el restablecimiento del nodo de servidor para volver a conectar con el servidor.

Además, intenta obtener acceso a la dirección URL para el sitio Web de administración de WSUS (es decir, `http://CM12CAS:8530`) genera el error:

> Error HTTP 503. El servicio no está disponible

En esta situación, la causa más probable es que el grupo de aplicaciones de WsusPool en IIS está en estado detenido.

Además, el límite de memoria privada (KB) para el grupo de aplicaciones probablemente se establece en el valor predeterminado de 1843200 KB. Si se produce este problema, aumente el límite de memoria privada a 4GB (4000000 KB) y reinicie el grupo de aplicaciones. Para aumentar el límite de memoria privada, seleccione el grupo de aplicaciones de WsusPool y haga clic en Configuración avanzada en Editar grupo de aplicaciones. A continuación, establezca el límite de memoria privada a 4GB (4000000 KB). Una vez reiniciado el grupo de aplicaciones, supervisar el estado del componente SMS_WSUS_SYNC_MANAGER, wcm.log y wsyncmgr.log para errores. Tenga en cuenta que puede ser necesario aumentar el límite de memoria privada (KB 8000000) de 8 GB o superior, según el entorno.

Para obtener más información, consulte: [Se produce un error de sincronización WSUS en Configuration Manager 2012 con errores HTTP 503](http://blogs.technet.com/b/sus/archive/2015/03/23/configmgr-2012-support-tip-wsus-sync-fails-with-http-503-errors.aspx)

## <a name="error-0x80070643-fatal-error-during-installation"></a>Error 0x80070643: Error irrecuperable durante la instalación
El programa de instalación de WSUS usa Microsoft SQL Server para realizar la instalación. Este problema se produce porque el usuario que se está ejecutando el programa de instalación de WSUS no tiene permisos de administrador del sistema en SQL Server.

Para resolver este problema, conceda permisos de administrador del sistema para una cuenta de usuario o una cuenta de grupo en SQL Server y, a continuación, ejecute de nuevo el programa de instalación de WSUS.

## <a name="some-services-are-not-running-check-the-following-services"></a>Algunos servicios no se está ejecutando. Compruebe los siguientes servicios:

- **SelfUpdate:** Consulte [automática las actualizaciones se deben actualizar](https://technet.microsoft.com/library/cc708554(v=ws.10).aspx) para obtener información sobre cómo solucionar problemas del servicio Selfupdate.

- **WSSUService.exe:** Este servicio facilita la sincronización. Si tiene problemas con la sincronización, acceder a WSUSService.exe haciendo **iniciar**, seleccionando **herramientas administrativas**, haga clic en **servicios**y, a continuación, buscar **Windows Server Update Services** en la lista de servicios. Haga lo siguiente:
    
    -   Compruebe que este servicio se está ejecutando. Haga clic en **iniciar** si está detenido o **reiniciar** para actualizar el servicio.
    
    -   Use el Visor de eventos para comprobar la **aplicación**, **nivel de seguridad**y, y **sistema** registros de eventos para ver si hay cualquier evento que podría indicar un problema.
    
    -   También puede comprobar el registro SoftwareDistribution.log para ver si hay eventos que podrían indicar un problema.

- **ServicesSQL Web Service:** Servicios Web se hospedan en IIS. Si no se están ejecutando, asegúrese de que IIS está en ejecución (o iniciado). También puede intentar restablecer el servicio Web escribiendo **iisreset** en un símbolo del sistema.

- **Servicio SQL:** Todos los servicios excepto el servicio selfupdate requiere que se está ejecutando el servicio SQL. Si cualquiera de los archivos de registro indican problemas de conexión de SQL, compruebe primero el servicio SQL. Para obtener acceso al servicio SQL, haga clic en **iniciar**, apunte a **herramientas administrativas**, haga clic en **servicios**y, a continuación, seleccione una de las siguientes acciones:
    
  - **MSSQLSERver** (si está utilizando WMSDE o MSDE, o si está utilizando SQL Server y usa el nombre de instancia predeterminado para el nombre de instancia)
    
  - **MSSQL$ WSUS** (si está usando una base de datos de SQL Server y ha denominado "WSUS" de la instancia de base de datos)
    
    Haga clic en el servicio y, a continuación, haga clic en **iniciar** si no se está ejecutando el servicio, o **reiniciar** para actualizar el servicio si se está ejecutando.
