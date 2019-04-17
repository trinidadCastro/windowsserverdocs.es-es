---
title: Solucionar problemas de copia de seguridad del equipo y restaurar errores en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 06/25/2013
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc73aff-d2c0-4cf9-a23d-ef928ae5ddc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 37e79661442ba9f66a564b6c6c8fb57db1978454
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-computer-backup-and-restore-errors-in-windows-server-essentials"></a>Solucionar problemas de copia de seguridad del equipo y restaurar errores en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Usa estos procedimientos para solucionar problemas de las copias de seguridad del equipo en Windows Server Essentials, incluidos los problemas de configuración de copia de seguridad, las copias de seguridad incompletas o incorrecta, alertas de estado de copia de seguridad y problemas con el archivo, carpeta o todo el sistema restaura.  
  
> [!NOTE]
>  Para obtener la información de solución de problemas más reciente de la Comunidad de Windows Server Essentials, visita el [foro de Windows Server Essentials](https://social.technet.microsoft.com/Forums//winserveressentials/threads).  
  
##  <a name="BKMK_TroubleshootBackupConfigurationIssues"></a>Solucionar problemas de configuración de copia de seguridad para un equipo conectado  
 Usa estos procedimientos para solucionar problemas con las configuraciones de copia de seguridad para los equipos que se copian en el servidor de Windows Server Essentials.  
  
### <a name="errors"></a>Errores  
  
-   No se completó correctamente la configuración de copia de seguridad  
  
-   Error recopilar información de PC  
  
-   Quitar equipo de error de copia de seguridad  
  
### <a name="resolutions"></a>Resoluciones  
  
##### <a name="to-troubleshoot-errors-that-occur-while-you-configure-backups-for-a-connected-computer"></a>Para solucionar los errores que ocurren mientras configuras copias de seguridad de un equipo conectado  
  
1.  Asegúrate de que el equipo está conectado a la red a través de un dispositivo de red.  
  
2.  Asegúrese de que el dispositivo de red que el equipo está conectado a también está conectado a la red, encendida y funciona correctamente.  
  
3.  Asegúrate de que el servicio de copia de seguridad de cliente de Windows Server y el servicio de proveedor de copia de seguridad de Windows Server cliente equipo están ejecutando en el servidor.  
  
    ###### <a name="to-start-computer-backup-services-on-the-server"></a>Para iniciar servicios de copia de seguridad del equipo en el servidor  
  
    1.  En el servidor, haz clic en **inicio**, haz clic en **herramientas administrativas**y, a continuación, haz clic en **servicios**.  
  
    2.  Desplázate hacia abajo hasta y haz clic en **servicio de proveedor de copia de seguridad de Windows Server cliente equipo**. Si el estado del servicio no es **iniciado**, haz clic en el servicio y, a continuación, haz clic en **inicio**.  
  
    3.  Haz clic en **servicio copia de seguridad del equipo de cliente de Windows Server**. Si el estado del servicio no es **iniciado**, haz clic en el servicio y, a continuación, haz clic en **inicio**.  
  
    4.  Cerrar **servicios**.  
  
4.  Asegúrate de que el servicio de proveedor de copia de seguridad de Windows Server cliente equipo se ejecuta en el equipo cliente.  
  
    ###### <a name="to-start-the-computer-backup-service-on-the-client-computer"></a>Para iniciar el servicio de copia de seguridad del equipo en el equipo cliente  
  
    1.  En el equipo cliente, haz clic en **inicio**, tipo **servicios** en la **buscar programas y archivos** cuadro de texto y, a continuación, presiona ENTRAR.  
  
    2.  Desplázate hacia abajo hasta y haz clic en **servicio de proveedor de copia de seguridad de Windows Server cliente equipo**. Si el estado del servicio no es **iniciado**, haz clic en el servicio y, a continuación, haz clic en **inicio**.  
  
    3.  Cerrar **servicios**.  
  
5.  Active alertas para determinar si hay errores en la base de datos de copia de seguridad. Si hay errores, sigue las instrucciones en la alerta para reparar la copia de seguridad.  
  
6.  Desinstalar el software del conector de Windows Server Essentials desde el equipo y, a continuación, volver a instalarlo. Para obtener más información, consulta los temas [desinstalar el software del conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) y [instalar el software del conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).  
  
##  <a name="BKMK_TroubleshootaBackupThatDidNotCompleteProperly"></a>Solucionar problemas de una copia de seguridad que no se completó correctamente  
 Cuando una copia de seguridad tiene estado incorrecto, ninguna parte de la copia de seguridad se realizó correctamente y no hay datos están disponibles para restaurar. Sin embargo, cuando una copia de seguridad tiene estado incompleta, no todos los elementos a los que se especifican en la copia de seguridad de configuración copia de seguridad, pero algunos datos pueden estar disponibles para restaurar.  
  
### <a name="errors"></a>Errores  
  
-   Copia de seguridad está incompleta  
  
-   Copia de seguridad incorrecta  
  
### <a name="resolutions"></a>Resoluciones  
  
##### <a name="to-identify-volumes-that-were-not-backed-up-successfully"></a>Para identificar los volúmenes que no se copia correctamente  
  
1.  Abre el panel de Windows Server Essentials y, a continuación, haz clic en **copia de seguridad y equipos**.  
  
2.  Haz clic en el nombre del equipo donde copia de seguridad no se completa correctamente y, a continuación, haz clic en **ver las propiedades** en la **tareas** panel.  
  
3.  Haz clic en la copia de seguridad que no se completa correctamente y, a continuación, haz clic en **ver detalles**.  
  
4.  En la **detalles de la copia de seguridad** se muestra el cuadro de diálogo, una marca de verificación verde en el estado de cada volumen que se copia correctamente.  
  
##### <a name="to-troubleshoot-an-unsuccessful-backup-of-a-volume"></a>Solucionar problemas de una copia de seguridad incorrecta de un volumen  
  
1.  Asegúrese de que el disco duro está conectado al equipo, activado y funciona correctamente.  
  
2.  Ejecutar **chkdsk /f /r** de corregir los errores en el disco duro (**/f**) y recuperar la información legible desde todos los sectores defectuosos (**/r**). Para obtener más información sobre cómo ejecutar **chkdsk**, consulta [CHKDSK](https://go.microsoft.com/fwlink/?LinkId=206562).  
  
3.  Asegúrate de que el equipo no se apaga o se desconecta de la red mientras se estaba ejecutando copia de seguridad.  
  
4.  Asegúrate de que hay suficiente espacio libre en cada volumen para ejecutar copia de seguridad. Copia de seguridad requiere espacio adicional en el disco en el equipo cliente para crear una instantánea VSS. En cualquier otro volumen que no está reservada del sistema, al menos 10 por ciento de espacio en disco disponible deben ser gratuitas. En un volumen del sistema reservado, si el tamaño del volumen es inferior a 500 MB, VSS requiere 32 MB de espacio libre para crear una instantánea; Si el volumen es de 500 MB o más, VSS requiere 320 MB de espacio libre.  
  
     Si hay suficiente espacio libre en un volumen, prueba una de estas soluciones:  
  
    -   Extender el volumen. Puedes ampliar cualquier volumen básico o dinámico excepto el volumen del sistema reservado.  
  
        ###### <a name="to-extend-a-volume"></a>Para ampliar un volumen  
  
        1.  En el Panel de Control, haz clic en **sistema y seguridad**.  
  
        2.  En **herramientas administrativas**, haz clic en **crear y formatear particiones de disco duro**.  
  
        3.  Haz clic en el volumen que quieres ampliar. Si **Extender volumen** está habilitado, selecciona esa opción. Si la opción no está habilitada, no se puede extender el volumen.  
  
        4.  Sigue los pasos del Asistente de volumen extender para extender el volumen.  
  
    -   Eliminar el contenido del volumen para hacer más espacio disponible.  
  
        > [!NOTE]
        >  Si necesitas liberar espacio en el volumen del sistema reservado, puedes mover la imagen de recuperación del sistema a un volumen diferente. Para obtener instrucciones, consulta [implementar una imagen de recuperación del sistema](https://technet.microsoft.com/library/dd744280\(v=ws.10\).aspx).  
  
    -   Excluir el volumen de la copia de seguridad de cliente. Hazlo solamente si no es importante mantener una copia de seguridad de los datos en el volumen.  
  
        > [!WARNING]
        >  Si se excluye el volumen del sistema reservado de una copia de seguridad de cliente, no se copia el sistema de cliente, y no podrá realizar una restauración completa del sistema en el equipo.  
  
5.  Buscar otras alertas en el servidor que puede indicar que no hay suficiente espacio en disco en el servidor para completar correctamente la copia de seguridad. Sigue las instrucciones en la alerta para corregir el problema.  
  
6.  Ejecutar **vssadmin** en un símbolo del sistema para solucionar problemas de servicio de copia sombra de volumen (VSS) emite. Para obtener información sobre **vssadmin**, consulta [VSSADMIN](https://go.microsoft.com/fwlink/?LinkID=94332).  
  
##  <a name="BKMK_TroubleshootBackupHealthAlertIssues"></a>Solucionar problemas de alertas de estado de copia de seguridad  
  
### <a name="errors"></a>Errores  
  
-   El servicio de proveedor de soluciones de equipo copia de seguridad de Windows Server dejó de funcionar  
  
-   El servicio de proveedor del cliente equipo copia de seguridad de Windows Server soluciones dejó de funcionar  
  
### <a name="resolutions"></a>Resoluciones  
  
##### <a name="to-troubleshoot-a-backup-health-alert"></a>Solucionar problemas de una alerta de estado de copia de seguridad  
  
1.  Si una alerta indica que la copia de seguridad tiene problemas, sigue las instrucciones en la alerta para corregir el problema.  
  
2.  Si una alerta indica que no se está ejecutando un servicio de copia de seguridad, intente iniciar el servicio en el servidor o el equipo cliente en la que recibiste el mensaje de error.  
  
    ###### <a name="to-start-backup-services-on-the-server"></a>Para iniciar servicios de copia de seguridad en el servidor  
  
    1.  En el servidor, haz clic en **inicio**y haz clic en **herramientas administrativas**y, a continuación, haz clic en **servicios**.  
  
        > [!NOTE]
        >  Si va a administrar el servidor de forma remota, debes usar conexión a Escritorio remoto para acceder al escritorio del servidor. Para obtener información sobre el uso de conexión a Escritorio remoto, vea [conectarse a otro equipo mediante conexión a Escritorio remoto](https://windows.microsoft.com/windows-vista/Connect-to-another-computer-using-Remote-Desktop-Connection).  
  
    2.  Desplázate hacia abajo hasta y haz clic en **servicio de proveedor de copia de seguridad de Windows Server cliente equipo**. Si el estado del servicio no es **iniciado**, haz clic en el servicio y, a continuación, haz clic en **inicio**.  
  
    3.  Haz clic en **servicio copia de seguridad del equipo de cliente de Windows Server**. Si el estado del servicio no es **iniciado**, haz clic en el servicio y, a continuación, haz clic en **inicio**.  
  
    4.  Cerrar **servicios**.  
  
    ###### <a name="to-start-backup-services-on-a-client-computer"></a>Para iniciar servicios de copia de seguridad en un equipo cliente  
  
    1.  En el equipo cliente, haz clic en **inicio**, tipo **servicios** en la **buscar programas y archivos** cuadro de texto y, a continuación, haz clic en ENTRAR.  
  
    2.  Haz clic en **servicio de proveedor de copia de seguridad de Windows Server cliente equipo**y, a continuación, haz clic en **inicio**.  
  
    3.  Cerrar la **servicios**.  
  
3.  Comprueba los registros de eventos en el equipo cliente o servidor para problemas relacionados con los servicios de copia de seguridad o controladores.  
  
4.  Reinicia el servidor o el equipo cliente en la que recibiste el mensaje de error.  
  
5.  Comprueba las alertas de estado para otros problemas que pueden tener un efecto en la copia de seguridad de cliente.  
  
##  <a name="BKMK_FileAndFolder"></a>Solucionar problemas de restauración de un archivo o carpeta  
  
### <a name="errors"></a>Errores  
  
-   Restauración de archivos o carpetas no se completó correctamente.  
  
### <a name="resolutions"></a>Resoluciones  
  
##### <a name="to-troubleshoot-an-unsuccessful-file-or-folder-restore"></a>Para solucionar una error al restauración de archivo o carpeta  
  
1.  Asegúrate de que el equipo está conectado a la red a través de un dispositivo de red.  
  
2.  Asegúrese de que el dispositivo de red que el equipo está conectado a también está conectado a la red, encendida y funciona correctamente.  
  
3.  Active alertas para determinar si hay errores en la base de datos de copia de seguridad. Si hay errores, sigue las instrucciones en la alerta para reparar la copia de seguridad.  
  
4.  Intentar restaurar los archivos o carpetas desde otra copia de seguridad.  
  
5.  Asegúrate de que la **controlador Restaurar equipo de Windows Server solución** está instalada y funciona correctamente.  
  
    ###### <a name="to-check-the-status-of-the-windows-server-solution-computer-restore-driver"></a>Para comprobar el estado del controlador para restaurar del equipo de Windows Server solución  
  
    1.  Haz clic en **inicio**, tipo **Administrador de dispositivos** en la **buscar programas y archivos** cuadro de texto y, a continuación, haz clic en ENTRAR.  
  
    2.  En el Administrador de dispositivos, haga clic en **dispositivos del sistema**, desplázate hasta **controlador Restaurar equipo de Windows Server soluciones**.  
  
    3.  Si no se muestra el controlador:  
  
        1.  Abre un símbolo del sistema con privilegios de administrador y ejecuta el siguiente comando:  
  
             **¿%ProgramFiles%\Windows Server\Bin\BackupDriverInstaller.exe?  puedo**  
  
        2.  Actualizar al administrador de dispositivos. Debería aparecer el controlador.  
  
    4.  Si el icono que se muestra es un monitor de PC, el controlador está instalado y se ejecuta correctamente. Cierra el Administrador de dispositivos.  
  
    5.  Si el icono muestra no es un monitor de PC  
  
        1.  Haz clic en **controlador Restaurar equipo de Windows Server soluciones**y, a continuación, haz clic en **propiedades**.  
  
        2.  Haz clic en el **controlador** pestaña y, a continuación, haz clic en **Actualizar controlador**.  
  
        3.  Haz clic en **buscar software de controlador actualizado automáticamente**y, a continuación, sigue las instrucciones para actualizar el controlador.  
  
    6.  Cierra el Administrador de dispositivos.  
  
6.  Desinstalar el software del conector de Windows Server Essentials desde el equipo y, a continuación, volver a instalarlo. Para obtener más información, consulta los temas [desinstalar el software del conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) y [instalar el software del conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).  
  
##  <a name="BKMK_Troubleshootfullsystemrestoreissues"></a>Solucionar problemas de una restauración completa del sistema  
  
### <a name="errors"></a>Errores  
  
-   No se puede iniciar sesión en el equipo cliente tras una restauración completa del sistema.  
  
### <a name="resolutions"></a>Resoluciones  
 Si cambias el nombre de un equipo y necesitas más adelante restaurar una copia de seguridad que se guardó antes de que el nombre del equipo cambiado, después de la restauración, cuando intentas iniciar sesión en tu cuenta de dominio, recibirás este error: "la base de datos de seguridad en el servidor no tiene una cuenta de equipo para esta relación de confianza de la estación de trabajo". Para obtener acceso a la red en el equipo nuevo, quitar el software del conector, quita el dominio de Windows en el equipo y vuelva a conectar al servidor.  
  
##### <a name="to-regain-network-access-to-a-restored-computer-after-a-computer-name-change"></a>Para recuperar el acceso de red a un equipo restaurado después de un cambio de nombre de equipo  
  
1.  Inicia sesión como administrador local el equipo.  
  
2.  Desinstalar el software del conector. Para obtener más información, consulta [desinstalar el software del conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).  
  
3.  Quitar el equipo del dominio. Para obtener más información, consulta [quitar un equipo de un dominio de Windows](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).  
  
4.  Vuelva a conectar el equipo al servidor. Para obtener más información, consulta [conectar equipos con el servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
## <a name="see-also"></a>Consulta también  
  
-   [Compatibilidad con Windows Server Essentials](Support-Windows-Server-Essentials.md)