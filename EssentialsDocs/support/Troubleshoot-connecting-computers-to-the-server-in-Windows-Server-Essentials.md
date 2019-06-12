---
title: Solucionar problemas de conexión de equipos al servidor en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e45b3d89-c057-4c70-a627-86fb06dd22aa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 52ec9bf1caa4cb4c7ed661eec1448f3f2a4be980
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436050"
---
# <a name="troubleshoot-connecting-computers-to-the-server-in-windows-server-essentials"></a>Solucionar problemas de conexión de equipos al servidor en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Este tema contiene la Guía de solución de problemas que pueden surgir cuando se conecta un equipo en el servidor que ejecuta Windows Server Essentials o Windows Server Essentials.  
  
> [!NOTE]
>  Para la información de solución de problemas más reciente de la Comunidad de Windows Server Essentials y Windows Server Essentials, le recomendamos que visite el [foro de Windows Server Essentials](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserveressentials). El foro de Windows Server Essentials es un buen lugar para buscar ayuda o realizar una pregunta.  
  
 Este tema proporciona soluciones para los problemas siguientes:  
  

-   Problema 1: [Problema 1](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problema 2: [Issue 2](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problema 3: [Problema 3](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problema 4: [Problema 4](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problema 5: [Problema 5](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problema 6: [Problema 6](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problema 7: [Problema 7](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problema 8: [Problema 8](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problema 9: [Problema 9](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problema 10: [Problema 10](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problema 11: [Problema 11](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

-   Problema 1: [Problema 1](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problema 2: [Issue 2](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problema 3: [Problema 3](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problema 4: [Problema 4](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problema 5: [Problema 5](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problema 6: [Problema 6](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problema 7: [Problema 7](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problema 8: [Problema 8](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problema 9: [Problema 9](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problema 10: [Problema 10](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problema 11: [Problema 11](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

  
##  <a name="BMRK_Package"></a> Problema 1  
 **Problema**  
  
 Obtengo un paquete de instalación no se realizó correctamente. Intente instalar el Conector de Windows Server Essentials de nuevo. Si el problema persiste, póngase en contacto con el error del Administrador de red al conectar un equipo al servidor  
  
 **Descripción**  
  
 Esto puede ocurrir si se conecta un equipo a un servidor que ejecuta Windows Server Essentials mientras están pendientes otras actualizaciones de Windows o las instalaciones de aplicaciones y se cancela la instalación del conector.  
  
 **Solución**  
  
 Complete todas las demás instalaciones de aplicaciones o actualizaciones. Si se le pide, reinicie los equipos.  
  
##  <a name="BKMK_ConnectorIssue2"></a> Issue 2  
 **Problema**  
  
 No se puede unir un equipo a Windows Server Essentials  
  
 **Descripción**  
  
 No se pueden unir los equipos que tienen caracteres no ASCII en el nombre del equipo a Windows Server Essentials. Si el nombre de equipo incluye caracteres que no son ASCII, recibirá el mensaje de error "Se produjo un error inesperado".  
  
 **Solución**  
  
 Cambiar el nombre de su equipo cliente con un nombre que contiene únicamente caracteres ASCII e intente agregar de nuevo el equipo a Windows Server Essentials.  
  
##  <a name="BKMK_ConnectorIssue2a"></a> Problema 3  
 **Problema**  
  
 Obtengo un conector de la instalación de software cancelado error al conectar un equipo al servidor  
  
 **Descripción**  
  
 Para poder conectar un equipo con el servidor, la cuenta del sistema debe tener permisos de Control total en las carpetas del servidor aparece en el panel de Windows Server Essentials. Si no se concedieron los permisos necesarios, recibe el mensaje de error "Se canceló la instalación del software del conector".  
  
 **Solución**  
  
 Conceda a la cuenta del sistema permisos de **control total** en cada carpeta del servidor.  
  
#### <a name="to-grant-the-system-account-full-control-permissions-on-a-server-folder"></a>Para conceder a la cuenta del sistema permisos de control total en cada carpeta del servidor  
  
1.  Abra el panel de Windows Server Essentials.  
  
2.  Haga clic en **Almacenamiento** y después, en **Carpetas del servidor**.  
  
3.  Haga clic con el botón derecho en la carpeta del servidor y, a continuación, haga clic en **Abrir la carpeta**. (Si no ve la opción **Abrir la carpeta** , no es necesario establecer permisos en la carpeta).  
  
4.  En la ruta de acceso de red en la parte superior del Explorador de Windows, haga clic en el recurso compartido del servidor para mostrar las carpetas compartidas del servidor. Por ejemplo, si la ruta de acceso es **Red > Server01 > Copias de seguridad del historial de archivos**, haga clic en **Server01**.  
  
5.  Haga clic con el botón derecho en una carpeta del servidor y, a continuación, haga clic en **Propiedades**.  
  
6.  Haga clic en la pestaña **Seguridad**.  
  
7.  Si no se permite **Control total** para la cuenta del sistema, haga clic en **Editar** y, a continuación, haga clic en **Sistema**. En **Permisos del sistema**, seleccione la casilla **Permitir** situada junto a **Control total**.  
  
8.  Haga clic dos veces en **Aceptar** para actualizar los permisos y cierre **Propiedades**.  
  
##  <a name="BKMK_ConnectorIssueNetFramework"></a> Problema 4  
 **Problema**  
  
 Obtengo a para ejecutar esta aplicación, debe instalar una de las siguientes versiones de .NET Framework: V4.5.50709" al conectar un equipo al servidor  
  
 **Descripción**  
  
 Cuando se conecta un equipo a un servidor que ejecuta Windows Server Essentials o Windows Server Essentials, el asistente intenta instalar .NET Framework versión 4.5.50709 en el equipo. Sin embargo, si hay una versión anterior de .NET Framework versión 4.5, no se puede instalar la versión actualizada y se produce un error en el intento de conexión con este mensaje de error: Para ejecutar esta aplicación, debe instalar una de las siguientes versiones de .NET Framework: V4.5.50709. Para obtener instrucciones acerca de cómo obtener la versión adecuada de .NET Framework, póngase en contacto con el publicador de asignación.  
  
 **Solución**  
  
 Desinstale .NET Framework 4.5 del equipo y, a continuación, conecte el equipo al servidor.  
  
#### <a name="to-uninstall-net-framework-45"></a>Para desinstalar .NET Framework 4.5  
  
1.  En la página **Inicio** del equipo cliente, abra el **Panel de control**.  
  
2.  En el Panel de control, en **Programas**, haga clic en **Desinstalar un programa**.  
  
3.  Haga clic con el botón derecho en **Microsoft .NET Framework 4.5** y, a continuación, haga clic en **Desinstalar**.  
  
4.  Después de desinstalar .NET Framework 4.5 correctamente, conecte el equipo al servidor. La versión correcta de .NET Framework 4.5 se instala junto con el software del conector.  
  
##  <a name="BKMK_Time"></a> Problema 5  
 **Problema**  
  
 Obtener una el servidor no está disponible. Para resolver este problema, póngase en contacto con la persona responsable de la red. al conectar un equipo al servidor  
  
 **Descripción**  
  
 Esto puede suceder si la fecha y la hora del equipo conectado no están sincronizadas con la fecha y la hora del servidor.  Windows Server Essentials y Windows Server Essentials usan el servicio de sincronización para sincronizar la fecha y hora de los equipos que se ejecutan en una red de Windows Server Essentials o Windows Server Essentials. La sincronización de la hora es fundamental, ya que el protocolo de autenticación predeterminado utiliza la hora del servidor como parte del proceso de autenticación. Por ejemplo, si no se sincroniza el reloj en un equipo cliente a la fecha y hora correctas, Windows Server Essentials o autenticación de Windows Server Essentials falsamente podría interpretar una solicitud de inicio de sesión como un intento de intrusión y denegar el acceso al usuario.  
  
 Esto puede ocurrir si la memoria libre del servidor es inferior al 5 por ciento.  
  
 Esto puede suceder si ya tiene una conexión VPN a Windows Server Essentials e intenta configurar el software del conector de forma remota mediante una dirección de dominio.  
  
 **Solución**  
  
1.  Sincronice la fecha y la hora del equipo cliente con las del servidor. A continuación, vuelva a conectar el equipo al servidor.  
  
2.  Cierre algunas aplicaciones del servidor y después conecte el equipo al servidor.  
  
3.  Cierre la conexión VPN y, a continuación, vuelva a conectar el equipo al servidor.  
  
#### <a name="to-change-the-date-and-time-on-the-client-computer"></a>Para cambiar la fecha y la hora del equipo cliente  
  
1. En la página Inicio del equipo cliente, abra el **Panel de control**.  
  
2. En el Panel de control, haga clic en **Reloj, idioma y región**y en **Fecha y hora**.  
  
3. Haga clic en **Cambiar fecha y hora**, establezca la fecha y la hora correctas y haga clic en **Aceptar**.  
  
4. Haga clic en **Aceptar** y cierre el Panel de control.  
  
5. Intente conectar de nuevo el equipo cliente al servidor. Para obtener instrucciones, consulte Conectar equipos al servidor.  
  
   Si todavía no se puede conectar el equipo cliente al servidor, asegúrese de que la fecha y la hora del servidor son correctas. Si la fecha y la hora no son correctas, cámbielas.  
  
#### <a name="to-change-the-date-and-time-on-the-server"></a>Para cambiar la fecha y la hora del servidor  
  
1.  Inicie sesión en el servidor con la contraseña que estableció durante la instalación de Windows Server Essentials o Windows Server Essentials y la configuración.  
  
    > [!NOTE]
    >  Si administra el servidor de forma remota, debe iniciar sesión en el servidor mediante la conexión a Escritorio remoto.  
  
2.  En la página **Inicio** , abra el **Panel de control**.  
  
3.  En el Panel de control, haga clic en **Reloj, idioma y región**y en **Fecha y hora**.  
  
4.  Haga clic en **Cambiar fecha y hora**, establezca la fecha y la hora correctas y haga clic en **Aceptar**.  
  
5.  Haga clic en **Aceptar** y cierre el Panel de control.  
  
6.  En el equipo cliente, vuelva a intentar conectar el equipo cliente al servidor. Para obtener instrucciones, consulte Conectar equipos al servidor.  
  
##  <a name="BKMK_ServiceStopped"></a> Issue 6  
 **Problema**  
  
 Obtengo un An error inesperado. Para resolver este problema, póngase en contacto con la persona responsable de la red. al conectar un equipo al servidor  
  
 **Descripción**  
  
 Si recibe este mensaje de error, es posible que WSS Certificate Web Service no se esté ejecutando.  
  
 **Solución**  
  
 Inicie WSS Certificate Web Service.  
  
#### <a name="to-start-the-wss-certificate-web-service"></a>Para iniciar WSS Certificate Web Service  
  
1.  En la página **Inicio** del servidor, abra **Herramientas administrativas**.  
  
     En **Herramientas administrativas**, haga clic con el botón derecho en **Administrador de Internet Information Services (IIS)** y, a continuación, haga clic en **Abrir**.  
  
2.  En el panel de navegación, haga clic en **WSS Certificate Web Service**.  
  
3.  En el panel de **Acciones**, haga clic en **Inicio**.  
  
##  <a name="BKMK_ConnectorIssueReconnect"></a> Issue 7  
 **Problema**  
  
 Cuando se intenta conectar un equipo en el servidor de nuevo después de un intento de conexión incorrecto, aparece la advertencia en que un equipo con este nombre ya está conectado al servidor  
  
 **Descripción**  
  
 Si se canceló o se interrumpió un intento anterior de conectar un equipo al servidor, puede recibir la siguiente advertencia al intentar conectarlo de nuevo: Un equipo con este nombre ya está conectado al servidor. Esto sucede porque se emitió un certificado al intentar conectarse al servidor la primera vez.  
  
 **Solución** Si está seguro de que no hay ningún otro equipo con el mismo nombre conectado al servidor, haga clic en **Siguiente** y siga las instrucciones para completar el asistente para **Conectar el equipo al servidor**.  
  
##  <a name="BKMK_JoinWin7"></a> Issue 8  
 **Problema**  
  
 Al intentar conectar al servidor un equipo cliente que ejecuta Windows 7 Home, se abre la página web para ejecutar el software del conector, pero el equipo cliente no se puede conectar al servidor  
  
 **Descripción**  
  
 Si el enrutador de la red tiene habilitada la multidifusión, las comunicaciones entre el servidor y un equipo cliente que ejecuta Windows 7 Home Basic o Windows 7 Home Premium no funcionarán correctamente.  
  
 **Solución**  
  
 Deshabilite la multidifusión en el enrutador. En algunos enrutadores, esto podría incluir deshabilitar el protocolo de enrutamiento RIP-2M. Para obtener más información, consulte la documentación proporcionada por el fabricante del enrutador.  
  
##  <a name="BKMK_ConnectorIssueAutologon"></a> Issue 9  
 **Problema**  
  
 El inicio de sesión automático dejó de funcionar después de que el equipo se conectase al servidor  
  
 **Descripción**  
  
 Si el inicio de sesión automático se establece para la cuenta de usuario cuando se conecta el equipo a Windows Server Essentials, la configuración se sobrescribe cuando se instala el software del conector en el equipo.  
  
 **Solución** Para resolver este problema, cuando conecte el equipo al servidor, tenga en cuenta la contraseña que se usa para la cuenta de usuario. Después de instalar el software del conector, configure el inicio de sesión automático para que use la cuenta.  
  
> [!NOTE]
>  La cuenta de dominio de Windows Server Essentials requiere una contraseña que cumpla los requisitos de directiva de contraseñas predeterminada.  
  
##  <a name="BKMK_ConnectorIssueOldLogs"></a> Issue 10  
 **Problema**  
  
 Al desinstalar una versión preliminar del software del conector, no se eliminan los registros existentes  
  
 **Descripción**  
  
 Después de actualizar desde una versión preliminar (Beta o RC) de Windows Server Essentials a la versión de lanzamiento, debe quitar el software del conector de cada equipo que se ha conectado al servidor y, a continuación, conecte el equipo nuevo para instalar la versión de versión del software del conector.  
  
 Sin embargo, al quitar el software del conector de un equipo de red, no se eliminan los archivos de registro de la carpeta %ProgramData%\Microsoft\Windows Server\Logs\ de ese equipo. Si no elimina la carpeta de registros, los archivos de registro pueden dañarse cuando conecte el equipo a la versión de lanzamiento de Windows Server Essentials.  
  
 **Solución** Para evitar daños en los archivos de registro, elimine manualmente la carpeta Logs antes de volver a conectar el equipo cliente al servidor actualizado.  
  
#### <a name="to-reconnect-a-computer-after-a-server-update-without-corrupting-the-log-files"></a>Para volver a conectar un equipo después de actualizar un servidor sin dañar los archivos de registro  
  
1.  Desinstale el software del conector del equipo cliente.  
  
2.  Elimine la carpeta Logs de la carpeta %ProgramData%\Microsoft\Windows Server\.  
  
3.  Vuelva a conectar el equipo al servidor. De este modo se instala la versión de lanzamiento del software del conector y se crean una nueva carpeta Logs y archivos de registro.  
  
##  <a name="BKMK_UpgradeClientOS"></a> Issue 11  
 **Problema**  
  
 Deseo actualizar el sistema operativo de un equipo cliente  
  
 **Descripción**  
  
 Durante la instalación del software del conector, se realizan numerosas comprobaciones en el sistema operativo del cliente para asegurarse de que el cliente cumple todos los requisitos previos del conector. Si actualiza el sistema operativo del cliente después de instalar el software del conector, algunos requisitos previos podrían no cumplirse y el conector del cliente podría producir un error.  
  
 **Solución**  
  
 Antes de actualizar el sistema operativo del cliente a una versión diferente (por ejemplo, actualizar Windows XP a Windows Vista, o Windows Vista a Windows 7), debe desinstalar el software del conector. Use **Agregar o quitar programas** en el Panel de control. Una vez completada la actualización del sistema operativo cliente, puede volver a instalar el conector de cliente abriendo http://&lt*server*> /Connect en un explorador Web, donde <*server*> es el nombre de la  Servidor de Windows Server Essentials.  
  
 Si ya actualizó el cliente con el software del conector instalado, use **Agregar o quitar programas** o **Programas y características** para desinstalar el software del conector. A continuación, vuelva a instalar el software del conector.  
  
## <a name="see-also"></a>Vea también  
  
-   [Administrar Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  
-   [Windows 2012 Server Essentials http://%s/ConnectComputer solución de problemas (Wiki de TechNet)](https://social.technet.microsoft.com/wiki/contents/articles/14370.windows-2012-server-essentials-connectcomputer-troubleshooting.aspx)
