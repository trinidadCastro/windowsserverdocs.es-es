---
title: Solucionar problemas de equipos que se conectan al servidor de Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
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
ms.openlocfilehash: 9b0d11be08840ecedabab6fd4e96f5d453ea4857
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-connecting-computers-to-the-server-in-windows-server-essentials"></a>Solucionar problemas de equipos que se conectan al servidor de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Este tema contiene la Guía de solución de problemas que podrían surgir al conectar un equipo con el servidor que ejecuta Windows Server Essentials o Windows Server Essentials.  
  
> [!NOTE]
>  Para obtener información sobre solución de problemas más reciente de la Comunidad de Windows Server Essentials y Windows Server Essentials, te sugerimos que visite el [foro de Windows Server Essentials](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserveressentials). El foro de Windows Server Essentials es un lugar excelente para buscar ayuda, o hacer una pregunta.  
  
 En este tema proporciona soluciones a los siguientes problemas:  
  

-   Problema 1: [número 1](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problema 2: [emitir 2](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problema 3: [emitir 3](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problema 4: [emitir 4](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problema 5: [emitir 5](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problema 6: [emitir 6](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problema 7: [emitir 7](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problema 8: [emitir 8](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problema 9: [emitir 9](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problema 10: [emitir 10](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problema 11: [emitir 11](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

-   Problema 1: [número 1](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problema 2: [emitir 2](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problema 3: [emitir 3](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problema 4: [emitir 4](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problema 5: [emitir 5](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problema 6: [emitir 6](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problema 7: [emitir 7](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problema 8: [emitir 8](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problema 9: [emitir 9](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problema 10: [emitir 10](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problema 11: [emitir 11](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

  
##  <a name="BMRK_Package"></a>Problema 1  
 **Problema**  
  
 Me aparece un paquete de instalación no se realizó correctamente. Intente volver a instalar el conector de Windows Server Essentials. Si el problema persiste, ponte en contacto con el error del Administrador de red al conectar un equipo con el servidor  
  
 **Descripción**  
  
 Este problema puede producirse si se conecta un equipo a un servidor que ejecuta Windows Server Essentials mientras otras actualizaciones de Windows o instalaciones de aplicaciones están pendientes y se cancela la instalación del conector.  
  
 **Solución**  
  
 Completar todas las demás actualizaciones o instalaciones de aplicaciones. Si se te solicita, reinicia los equipos.  
  
##  <a name="BKMK_ConnectorIssue2"></a>Problema 2  
 **Problema**  
  
 No pueden unirse a un equipo a Windows Server Essentials  
  
 **Descripción**  
  
 Equipos que tienen los caracteres que no sean ASCII en el nombre del equipo no pueden unirse a Windows Server Essentials. Si el nombre del equipo incluye caracteres no ASCII, recibes el mensaje de error "se ha producido un error inesperado."  
  
 **Solución**  
  
 Cambiar el nombre de equipo cliente con un nombre que contenga caracteres ASCII solo e intenta agregar de nuevo el equipo a Windows Server Essentials.  
  
##  <a name="BKMK_ConnectorIssue2a"></a>Problema 3  
 **Problema**  
  
 Veo un conector de la instalación de software cancelada error al conectar un equipo con el servidor  
  
 **Descripción**  
  
 Para poder conectarse al servidor en un equipo, la cuenta del sistema debe tener permisos de Control total en carpetas de servidor que se muestra en el escritorio de Windows Server Essentials. Si no se han concedido los permisos necesarios, recibe el error "se cancela la instalación de software del conector" mensaje.  
  
 **Solución**  
  
 Conceder a la cuenta de sistema **control total** permisos en cada carpeta del servidor.  
  
#### <a name="to-grant-the-system-account-full-control-permissions-on-a-server-folder"></a>Para conceder permisos de control total en una carpeta del servidor de la cuenta del sistema  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  Haz clic en **almacenamiento**y, a continuación, haz clic en **carpetas del servidor**.  
  
3.  Haz clic en la carpeta del servidor y, a continuación, haz clic en **abre la carpeta**. (Si no ves la **abre la carpeta** opción, no es necesario establecer permisos en la carpeta.)  
  
4.  En la ruta de acceso de red en la parte superior del explorador de Windows, haz clic en el recurso compartido de servidor para que muestre las carpetas compartidas en el servidor. Por ejemplo, si es la ruta de acceso **red > Server01 > copias de seguridad de historial de archivos**, haz clic en **Server01**.  
  
5.  Haz clic en una carpeta del servidor y, a continuación, haz clic en **propiedades**.  
  
6.  Haz clic en el **seguridad** pestaña.  
  
7.  Si **control total** no es se permite para la cuenta del sistema, haga clic en **editar**y, a continuación, haz clic en **sistema**. En **permisos para el sistema**, selecciona el **permitir** casilla de verificación junto a **control total**.  
  
8.  Haz clic en **Aceptar** dos veces para actualizar los permisos y cerrar **propiedades**.  
  
##  <a name="BKMK_ConnectorIssueNetFramework"></a>Problema 4  
 **Problema**  
  
 Se obtiene a para ejecutar esta aplicación, debes instalar una de las siguientes versiones de .NET Framework: V4.5.50709 "error al conectar un equipo con el servidor  
  
 **Descripción**  
  
 Cuando un equipo se conecta a un servidor que ejecuta Windows Server Essentials o Windows Server Essentials, el asistente intenta instalar .NET Framework versión 4.5.50709 en el equipo. Sin embargo, si hay una versión anterior de .NET Framework 4.5, no se puede instalar la versión actualizada y se produce un error en el intento de conexión con este mensaje de error: para ejecutar esta aplicación, debes instalar una de las siguientes versiones de .NET Framework: V4.5.50709. Para obtener instrucciones acerca de cómo obtener la versión apropiada de .NET Framework, ponte en contacto con el Editor de asignación.  
  
 **Solución**  
  
 Desinstala .NET Framework 4.5 desde el equipo y, a continuación, conecta el equipo al servidor.  
  
#### <a name="to-uninstall-net-framework-45"></a>Para desinstalar .NET Framework 4.5  
  
1.  En la **inicio** página del equipo cliente, abre **Panel de Control**.  
  
2.  En el Panel de Control, en **programas**, haz clic en **desinstalar un programa**.  
  
3.  Haz clic en **Microsoft .NET Framework 4.5**y, a continuación, haz clic en **desinstalar**.  
  
4.  Después de desinstalar correctamente .NET Framework 4.5, conecta el equipo al servidor. Se instala la versión correcta de .NET Framework 4.5 junto con el software del conector.  
  
##  <a name="BKMK_Time"></a>Problema 5  
 **Problema**  
  
 Obtengo un el servidor no está disponible. Para resolver este problema, ponte en contacto con el responsable de la red. Error al conectar un equipo con el servidor  
  
 **Descripción**  
  
 Esto puede suceder si la fecha y hora en el equipo conectado no se sincronizan con la fecha y hora en el servidor.  Windows Server Essentials y Windows Server Essentials utilizan el servicio de sincronización de hora para sincronizar la fecha y hora de equipos que ejecutan en una red de Windows Server Essentials o Windows Server Essentials. Sincronización de la hora es fundamental porque el protocolo de autenticación predeterminado usa tiempo del servidor como parte del proceso de autenticación. Por ejemplo, si el reloj en un equipo cliente no se sincroniza con la fecha y hora correctas, Windows Server Essentials o autenticación de Windows Server Essentials, es posible que erróneamente interpretar una solicitud de inicio de sesión como un intento de intrusiones y denegar el acceso al usuario.  
  
 Esto puede suceder si el servidor s memoria es menos del 5%.  
  
 Esto puede suceder si ya tienes una conexión VPN en el servidor de Windows Essentials e intenta configurar el conector software fuera de las instalaciones mediante una dirección de dominio.  
  
 **Solución**  
  
1.  Sincronizar la fecha y hora en el equipo cliente con los que estén en el servidor. A continuación, conecta el equipo al servidor.  
  
2.  Cierre algunas aplicaciones en el servidor y, a continuación, conecta el equipo al servidor.  
  
3.  Cierra la conexión VPN y, a continuación, conecta el equipo al servidor.  
  
#### <a name="to-change-the-date-and-time-on-the-client-computer"></a>Para cambiar la fecha y hora en el equipo cliente  
  
1.  En la página de inicio del equipo cliente, abre **Panel de Control**.  
  
2.  En el Panel de Control, haz clic en **reloj, idioma y región**y, a continuación, haz clic en **fecha y hora**.  
  
3.  Haz clic en **Cambiar fecha y hora**, Establece la fecha y hora en la fecha y hora correctas y, a continuación, haz clic en **Aceptar**.  
  
4.  Haz clic en **Aceptar**y, a continuación, cierra el Panel de Control.  
  
5.  Vuelva a conectar el equipo cliente con el servidor. Para obtener instrucciones, consulta los equipos de conectarse al servidor.  
  
 Si aún no se puede conectar el equipo cliente al servidor, asegúrate de que la fecha y hora en el servidor son correctos. Si la fecha y hora no son correctos, cambiarlos.  
  
#### <a name="to-change-the-date-and-time-on-the-server"></a>Para cambiar la fecha y hora en el servidor  
  
1.  Iniciar sesión en el servidor mediante la contraseña que configuración durante la configuración o instalación de Windows Server Essentials y Windows Server Essentials.  
  
    > [!NOTE]
    >  Si va a administrar el servidor de forma remota, debe iniciar sesión en el servidor mediante conexión a Escritorio remoto.  
  
2.  En la **inicio** página abierta **Panel de Control**.  
  
3.  En el Panel de Control, haz clic en **reloj, idioma y región**y, a continuación, haz clic en **fecha y hora**.  
  
4.  Haz clic en **Cambiar fecha y hora**, Establece la fecha y hora en la fecha y hora correctas y, a continuación, haz clic en **Aceptar**.  
  
5.  Haz clic en **Aceptar**y, a continuación, cierra el Panel de Control.  
  
6.  En el equipo cliente, vuelva a conectar el equipo cliente con el servidor. Para obtener instrucciones, consulta los equipos de conectarse al servidor.  
  
##  <a name="BKMK_ServiceStopped"></a>Problema 6  
 **Problema**  
  
 Obtengo un un error inesperado. Para resolver este problema, ponte en contacto con el responsable de la red. Error al conectar un equipo con el servidor  
  
 **Descripción**  
  
 Si recibes este mensaje de error, es posible que no se estén ejecutando el servicio Web de certificados de WSS.  
  
 **Solución**  
  
 Inicia el servicio Web de certificado WSS.  
  
#### <a name="to-start-the-wss-certificate-web-service"></a>Para iniciar el servicio Web de certificados de WSS  
  
1.  En el servidor **inicio** página abierta **herramientas administrativas**.  
  
     En **herramientas administrativas**, haz clic en **Internet Information Services (IIS) Manager**y, a continuación, haz clic en **abrir**.  
  
2.  En el panel de navegación, haz clic en **servicio Web de certificados WSS**.  
  
3.  En la **acciones** panel, haz clic en **inicio**.  
  
##  <a name="BKMK_ConnectorIssueReconnect"></a>Problema 7  
 **Problema**  
  
 Cuando se intenta conectar un equipo con el servidor después de un intento de conexión incorrecta, aparece la advertencia de que un equipo con este nombre ya está conectado al servidor  
  
 **Descripción**  
  
 Si se cancela o se interrumpa anterior intentar conectarse al servidor en un equipo, es posible que reciba la siguiente advertencia cuando intentas conectarte de nuevo: un equipo con este nombre ya está conectado al servidor. Esto ocurre porque el certificado emitido al intentar conectarse al servidor la primera vez.  
  
 **Solución** si estás seguro de que ningún otro equipo con el mismo nombre ya está conectado al servidor, haz clic en **siguiente**y, a continuación, sigue las instrucciones para completar la **conectar mi equipo con el servidor** asistente.  
  
##  <a name="BKMK_JoinWin7"></a>Problema 8  
 **Problema**  
  
 Cuando se intenta conectar un equipo cliente que ejecuta Windows 7 Home al servidor, se abrirá la página web para ejecutar el software del conector, pero el equipo cliente no puede conectarse al servidor  
  
 **Descripción**  
  
 Si el enrutador de la red tiene habilitada la multidifusión, comunicaciones entre el servidor y equipo cliente que ejecuta Windows 7 Home Basic o Windows 7 Home Premium no funcionan correctamente.  
  
 **Solución**  
  
 Deshabilitar la multidifusión en el enrutador. En algunos enrutadores, que puede incluir deshabilitar el protocolo de enrutamiento de copia desde CD - 2M. Para obtener más información, consulta la documentación proporcionada por el fabricante del enrutador.  
  
##  <a name="BKMK_ConnectorIssueAutologon"></a>Problema 9  
 **Problema**  
  
 Inicio de sesión automático dejó de funcionar después de que conectado el equipo al servidor  
  
 **Descripción**  
  
 Si el inicio de sesión automático se establece para la cuenta de usuario cuando se conecta el equipo a Windows Server Essentials, se sobrescribirá la configuración cuando el software del conector está instalado en el equipo.  
  
 **Solución** para resolver este problema, cuando se conecta el equipo al servidor, ten en cuenta la contraseña que se usa para la cuenta de usuario. Después de instala el software del conector, configurar el inicio de sesión automático para usar esa cuenta.  
  
> [!NOTE]
>  La cuenta de dominio de Windows Server Essentials requiere una contraseña que cumpla los requisitos de directivas de contraseña de forma predeterminada.  
  
##  <a name="BKMK_ConnectorIssueOldLogs"></a>Problema 10  
 **Problema**  
  
 Desinstalación de una versión preliminar del software del conector no quita los registros existentes  
  
 **Descripción**  
  
 Después de actualizar desde una versión de versión preliminar (Beta o RC) de Windows Server Essentials a la versión publicada, debes quitar el software del conector de cada equipo que se ha conectado al servidor y, a continuación, conecta el equipo para instalar la versión de software del conector.  
  
 Sin embargo, cuando se quita el software del conector desde un equipo de red, no se eliminan los archivos de registro existentes en la carpeta de Server\Logs\ %ProgramData%\Microsoft\Windows en ese equipo. Si no se elimina la carpeta de registros, pueden dañarse los archivos de registro cuando se conecta el equipo a la versión comercial de Windows Server Essentials.  
  
 **Solución** para evitar daños en los archivos de registro, eliminar manualmente la carpeta de registros antes de volver a conectar el equipo cliente al servidor actualizado.  
  
#### <a name="to-reconnect-a-computer-after-a-server-update-without-corrupting-the-log-files"></a>Para volver a conectar un equipo después de actualizar un servidor sin dañar el registro de archivos  
  
1.  Desinstalar el software del conector desde el equipo cliente.  
  
2.  Elimina la carpeta de registros de la carpeta de Server\ %ProgramData%\Microsoft\Windows.  
  
3.  Vuelva a conectar el equipo al servidor. Que instala la versión del software del conector, crear una nueva carpeta de registros y archivos de registro.  
  
##  <a name="BKMK_UpgradeClientOS"></a>Problema de 11  
 **Problema**  
  
 Quiero actualizar el sistema operativo en un equipo cliente  
  
 **Descripción**  
  
 Durante la instalación del software del conector, se realizan numerosas comprobaciones en el sistema operativo de cliente para garantizar que el cliente cumple todos los requisitos previos de conector. Si actualizas el sistema operativo cliente después de instalar el software del conector, algunos de los requisitos previos no estén presentes y puede producir un error en el conector de cliente.  
  
 **Solución**  
  
 Antes de actualizar el sistema operativo del cliente a una versión diferente (por ejemplo, actualizar Windows XP a Windows Vista o Windows Vista a Windows 7), debe desinstalar el software del conector. Usa **agregar o quitar programas** en el Panel de Control. Una vez completada la actualización de sistema operativo del cliente, puedes reinstalar el conector de cliente abriendo http://<*server*> / connect en un explorador Web, donde <*server*> es el nombre del servidor de Windows Server Essentials.  
  
 Si ya ha actualizado el cliente con el software del conector instalado, usa **agregar o quitar programas** o **programas y características** para desinstalar el software del conector. A continuación, vuelve a instalar el software del conector.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  
-   [Solución de problemas (Wiki de TechNet) de Windows 2012 Server Essentials http://%s/ConnectComputer](https://social.technet.microsoft.com/wiki/contents/articles/14370.windows-2012-server-essentials-connectcomputer-troubleshooting.aspx)
