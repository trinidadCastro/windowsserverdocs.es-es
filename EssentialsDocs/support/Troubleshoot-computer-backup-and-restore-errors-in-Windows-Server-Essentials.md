---
title: Solucionar errores de copias de seguridad del equipo y restauración en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 06/25/2013
ms.prod: windows-server
ms.topic: article
ms.assetid: 5cc73aff-d2c0-4cf9-a23d-ef928ae5ddc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 542c0a583aca56ce72ef0e2e55f2b1543558dfa8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852188"
---
# <a name="troubleshoot-computer-backup-and-restore-errors-in-windows-server-essentials"></a>Solucionar errores de copias de seguridad del equipo y restauración en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Use estos procedimientos para solucionar problemas de copias de seguridad del equipo en Windows Server Essentials, como por ejemplo los problemas de configuración de copia de seguridad, las copias de seguridad incompletas o erróneas, alertas de estado de copia de seguridad y problemas relacionados con la restauración de archivos, carpetas o la restauración completa del sistema.  
  
> [!NOTE]
>  Para obtener la información más reciente sobre la solución de problemas de la comunidad de Windows Server Essentials, visite el [Foro de Windows Server Essentials](https://social.technet.microsoft.com/Forums//winserveressentials/threads).  
  
##  <a name="troubleshoot-backup-configuration-issues-for-a-connected-computer"></a><a name="BKMK_TroubleshootBackupConfigurationIssues"></a>Solucionar problemas de configuración de copia de seguridad de un equipo conectado  
 Use estos procedimientos para solucionar problemas relacionados con las configuraciones de copia de seguridad de los equipos en los que se efectúan copias de seguridad en el servidor de Windows Server Essentials.  
  
### <a name="errors"></a>Errores  
  
-   La configuración de copia de seguridad no se ha completado correctamente  
  
-   Error al recopilar la información del equipo  
  
-   Error al quitar el equipo de la copia de seguridad  
  
### <a name="resolutions"></a>Soluciones  
  
##### <a name="to-troubleshoot-errors-that-occur-while-you-configure-backups-for-a-connected-computer"></a>Para solucionar los errores que se producen al configurar las copias de seguridad en un equipo conectado  
  
1.  Asegúrese de que el equipo esté conectado a la red a través de un dispositivo de red.  
  
2.  Asegúrese de que el dispositivo de red al que está conectado el equipo también esté conectado a la red, encendido y funcione correctamente.  
  
3.  Asegúrese de que el servicio de copia de seguridad de cliente de Windows Server y el servicio de proveedor de copias de seguridad de equipos cliente de Windows Server se estén ejecutando en el servidor.  
  
    ###### <a name="to-start-computer-backup-services-on-the-server"></a>Para iniciar los servicios de copia de seguridad del equipo en el servidor  
  
    1.  En el servidor, haga clic en **Inicio**, haga clic en **Herramientas administrativas** y, después, haga clic en **Servicios**.  
  
    2.  Desplácese hacia abajo y haga clic en **Servicio Proveedor de copias de seguridad de equipos cliente de Windows Server**. Si el estado del servicio no es **Iniciado**, haga clic con el botón derecho en el servicio y, a continuación, haga clic en **Iniciar**.  
  
    3.  Haga clic en **Servicio de copias de seguridad de equipos cliente de Windows Server**. Si el estado del servicio no es **Iniciado**, haga clic con el botón derecho en el servicio y, a continuación, haga clic en **Iniciar**.  
  
    4.  Cierre **Servicios**.  
  
4.  Asegúrese de que el servicio de proveedor de copias de seguridad de equipos cliente de Windows Server se esté ejecutando en el equipo cliente.  
  
    ###### <a name="to-start-the-computer-backup-service-on-the-client-computer"></a>Para iniciar el servicio de copias de seguridad del equipo en el equipo cliente  
  
    1.  En el equipo cliente, haga clic en **Inicio**, escriba **Servicios** en el cuadro de texto **Buscar programas y archivos** y, a continuación, presione ENTRAR.  
  
    2.  Desplácese hacia abajo y haga clic en **Servicio Proveedor de copias de seguridad de equipos cliente de Windows Server**. Si el estado del servicio no es **Iniciado**, haga clic con el botón derecho en el servicio y, a continuación, haga clic en **Iniciar**.  
  
    3.  Cierre **Servicios**.  
  
5.  Compruebe las alertas para determinar si hay algún error en la base de datos de copia de seguridad. Si existe algún error, siga las instrucciones que aparecen en la alerta para reparar la base de datos de copia de seguridad.  
  
6.  Desinstale el software del conector de Windows Server Essentials del equipo y, a continuación, vuelva a instalarlo. Para obtener más información, vea los temas [Desinstalar el software del conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) e [Instalar el software del conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).  
  
##  <a name="troubleshoot-a-backup-that-did-not-complete-properly"></a><a name="BKMK_TroubleshootaBackupThatDidNotCompleteProperly"></a>Solucionar problemas de una copia de seguridad que no se completó correctamente  
 Cuando una copia de seguridad tiene el estado Incorrecto, la copia de seguridad no se ha hecho correctamente y no hay datos disponibles para ser restaurados. Sin embargo, cuando una copia de seguridad tiene el estado Incompleto no se hace una copia de seguridad de todos los elementos especificados en la configuración de copia de seguridad, pero es posible que haya algunos datos disponibles para ser restaurados.  
  
### <a name="errors"></a>Errores  
  
-   La copia de seguridad es incompleta  
  
-   Error de copia de seguridad  
  
### <a name="resolutions"></a>Soluciones  
  
##### <a name="to-identify-volumes-that-were-not-backed-up-successfully"></a>Para identificar los volúmenes de los que no se ha hecho ninguna copia de seguridad correcta  
  
1.  Abra el panel de Windows Server Essentials y, a continuación, haga clic en **Equipos y copia de seguridad**.  
  
2.  Haga clic en el nombre del equipo en el que no se ha hecho la copia de seguridad correcta y, a continuación, haga clic en **Ver las propiedades del equipo** en el panel **Tareas**.  
  
3.  Haga clic en la copia de seguridad que no se ha completado correctamente y, a continuación, haga clic en **Ver detalles**.  
  
4.  En el cuadro de diálogo **Detalles de la copia de seguridad**, aparece una marca de comprobación verde en el estado de todos los volúmenes en los que se ha hecho una copia de seguridad correctamente.  
  
##### <a name="to-troubleshoot-an-unsuccessful-backup-of-a-volume"></a>Para solucionar problemas de una copia de seguridad incorrecta de un volumen  
  
1.  Asegúrese de que el disco duro esté conectado al equipo, esté activado y funcione correctamente.  
  
2.  Ejecute **chkdsk /f /r** para corregir los errores del disco duro ( **/f**) y recuperar información legible de cualquier sector erróneo ( **/r**). Para obtener más información sobre la ejecución de **chkdsk**, consulte [CHKDSK](https://go.microsoft.com/fwlink/?LinkId=206562).  
  
3.  Asegúrese de que el equipo no se había apagado o desconectado de la red mientras se ejecutaba la copia de seguridad.  
  
4.  Asegúrese de que haya suficiente espacio disponible en cada volumen para que se ejecute la copia de seguridad. La copia de seguridad necesita espacio en disco adicional en el equipo cliente para crear una instantánea VSS. En cualquier volumen que no esté reservado para el sistema debe haber al menos un 10 por ciento de espacio en disco disponible. En un volumen reservado para el sistema, si el tamaño del volumen es inferior a 500 MB, VSS necesitará 32 MB de espacio disponible para crear una instantánea; si el volumen es de 500 MB o superior, VSS necesitará 320 MB de espacio disponible.  
  
     Si no hay espacio disponible suficiente en un volumen, intente llevar a cabo alguna de estas soluciones:  
  
    -   Extienda el volumen. Puede extender cualquier volumen básico o dinámico excepto el volumen reservado para el sistema.  
  
        ###### <a name="to-extend-a-volume"></a>Para extender un volumen  
  
        1.  En el Panel de Control, haga clic en **Sistema y seguridad**.  
  
        2.  En **Herramientas administrativas**, haga clic en **Crear y formatear particiones del disco duro**.  
  
        3.  Haga clic con el botón derecho en el volumen que quiere extender. Si **Extender volumen** está habilitado, seleccione esta opción. Si la opción no está habilitada, no puede extender el volumen.  
  
        4.  Siga los pasos del asistente Extender volumen para extender el volumen.  
  
    -   Elimine el contenido del volumen para crear más espacio disponible.  
  
        > [!NOTE]
        >  Si necesita liberar espacio en el volumen reservado para el sistema puede mover la imagen de recuperación del sistema a otro volumen. Para obtener instrucciones, consulte [Implementar una imagen de recuperación del sistema](https://technet.microsoft.com/library/dd744280\(v=ws.10\).aspx).  
  
    -   Excluya el volumen de la copia de seguridad del cliente. Siga estos pasos únicamente si no le resulta imprescindible mantener una copia de seguridad de los datos en el volumen.  
  
        > [!WARNING]
        >  Si excluye el volumen reservado para el sistema desde una copia de seguridad de cliente, no se hará ninguna copia de seguridad del sistema cliente y no podrá realizar una restauración completa del sistema en el equipo.  
  
5.  Compruebe otras alertas del servidor que puedan indicar que no hay suficiente espacio en disco en el servidor para que la copia de seguridad se complete correctamente. Siga las instrucciones de la alerta para corregir el problema.  
  
6.  Ejecute **vssadmin** en un símbolo del sistema para solucionar problemas del Servicio de instantáneas de volumen (VSS). Para obtener información sobre **vssadmin**, consulte [VSSADMIN](https://go.microsoft.com/fwlink/?LinkID=94332).  
  
##  <a name="troubleshoot-backup-health-alert-issues"></a><a name="BKMK_TroubleshootBackupHealthAlertIssues"></a>Solucionar problemas de alertas de estado de copia de seguridad  
  
### <a name="errors"></a>Errores  
  
-   El servicio de proveedor de copias de seguridad del equipo de Soluciones de Windows Server ha dejado de funcionar  
  
-   El servicio de proveedor de copias de seguridad de equipos cliente de Windows Server ha dejado de funcionar  
  
### <a name="resolutions"></a>Soluciones  
  
##### <a name="to-troubleshoot-a-backup-health-alert"></a>Para solucionar problemas de una alerta de mantenimiento de la copia de seguridad  
  
1.  Si una alerta le indica que la base de datos de la copia de seguridad tiene problemas, siga las instrucciones de la alerta para corregir el problema.  
  
2.  Si una alerta le indica que no se está ejecutando un servicio de copia de seguridad, intente iniciar el servicio en el servidor o en el equipo cliente en el que ha recibido el mensaje de error.  
  
    ###### <a name="to-start-backup-services-on-the-server"></a>Para iniciar los servicios de copia de seguridad en el servidor  
  
    1.  En el servidor, haga clic en **Inicio**, haga clic en **Herramientas administrativas** y, después, haga clic en **Servicios**.  
  
        > [!NOTE]
        >  Si administra el servidor de forma remota debe usar Conexión a Escritorio remoto para tener acceso al escritorio del servidor. Para obtener información sobre el uso de Conexión a Escritorio remoto, consulte [Conectarse a otro equipo mediante Conexión a Escritorio remoto](https://windows.microsoft.com/windows-vista/Connect-to-another-computer-using-Remote-Desktop-Connection).  
  
    2.  Desplácese hacia abajo y haga clic en **Servicio Proveedor de copias de seguridad de equipos cliente de Windows Server**. Si el estado del servicio no es **Iniciado**, haga clic con el botón derecho en el servicio y, a continuación, haga clic en **Iniciar**.  
  
    3.  Haga clic en **Servicio de copias de seguridad de equipos cliente de Windows Server**. Si el estado del servicio no es **Iniciado**, haga clic con el botón derecho en el servicio y, a continuación, haga clic en **Iniciar**.  
  
    4.  Cierre **Servicios**.  
  
    ###### <a name="to-start-backup-services-on-a-client-computer"></a>Para iniciar los servicios de copia de seguridad en un equipo cliente  
  
    1.  En el equipo cliente, haga clic en **Inicio**, escriba **Servicios** en el cuadro de texto **Buscar programas y archivos** y, a continuación, haga clic en ENTRAR.  
  
    2.  Haga clic con el botón derecho en **Servicio Proveedor de copias de seguridad de equipos cliente de Windows Server**y, a continuación, haga clic en **Iniciar**.  
  
    3.  Cierre los **Servicios**.  
  
3.  Busque problemas relacionados con los servicios o los controladores de la copia de seguridad en los registros de eventos del equipo cliente o del servidor.  
  
4.  Reinicie el servidor o el equipo cliente en el que ha recibido el mensaje de error.  
  
5.  Busque otros problemas que puedan tener efectos en la copia de seguridad del cliente en las alertas de mantenimiento.  
  
##  <a name="troubleshoot-a-file-or-folder-restore"></a><a name="BKMK_FileAndFolder"></a>Solucionar problemas de una restauración de archivos o carpetas  
  
### <a name="errors"></a>Errores  
  
-   La restauración de archivos o carpetas no se ha completado correctamente.  
  
### <a name="resolutions"></a>Soluciones  
  
##### <a name="to-troubleshoot-an-unsuccessful-file-or-folder-restore"></a>Para solucionar problemas de una restauración incorrecta de archivos o carpetas  
  
1.  Asegúrese de que el equipo esté conectado a la red a través de un dispositivo de red.  
  
2.  Asegúrese de que el dispositivo de red al que está conectado el equipo también esté conectado a la red, encendido y funcione correctamente.  
  
3.  Compruebe las alertas para determinar si hay algún error en la base de datos de copia de seguridad. Si existe algún error, siga las instrucciones que aparecen en la alerta para reparar la base de datos de copia de seguridad.  
  
4.  Intente restaurar los archivos o las carpetas desde otra copia de seguridad.  
  
5.  Asegúrese de que el **Controlador de la restauración de equipos con las soluciones de Windows Server** esté instalado y funcione correctamente.  
  
    ###### <a name="to-check-the-status-of-the-windows-server-solution-computer-restore-driver"></a>Para comprobar el estado del Controlador de la restauración de equipos con las soluciones de Windows Server  
  
    1.  Haga clic en **Inicio**, escriba **Administrador de dispositivos** en el cuadro de texto **Buscar programas y archivos** y a continuación, haga clic en ENTRAR.  
  
    2.  En el Administrador de dispositivos, haga clic en **Dispositivos del sistema** y desplácese hasta **Controlador de la restauración de equipos con las soluciones de Windows Server**.  
  
    3.  Si no aparece el controlador:  
  
        1.  Abra un símbolo del sistema con privilegios de administrador y ejecute el siguiente comando:  
  
             **%ProgramFiles%\Windows Server\Bin\BackupDriverInstaller.exe? -i**  
  
        2.  Refresh Device Manager. Debe aparecer el controlador.  
  
    4.  Si el icono que aparece es un monitor de equipo, el controlador está instalado y se ejecuta correctamente. Cierre el Administrador de dispositivos.  
  
    5.  Si el icono que aparece no es un monitor de equipo  
  
        1.  Haga clic con el botón derecho en **Controlador de la restauración de equipos con las soluciones de Windows Server**y, a continuación, haga clic en **Propiedades**.  
  
        2.  Haga clic en la pestaña **Controlador** y, a continuación, haga clic en **Actualizar controlador**.  
  
        3.  Haga clic en **Buscar software de controlador actualizado automáticamente** y, a continuación, siga las instrucciones para actualizar el controlador.  
  
    6.  Cierre el Administrador de dispositivos.  
  
6.  Desinstale el software del conector de Windows Server Essentials del equipo y, a continuación, vuelva a instalarlo. Para obtener más información, vea los temas [Desinstalar el software del conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) e [Instalar el software del conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).  
  
##  <a name="troubleshoot-a-full-system-restore"></a><a name="BKMK_Troubleshootfullsystemrestoreissues"></a>Solucionar problemas de una restauración completa del sistema  
  
### <a name="errors"></a>Errores  
  
-   No se puede iniciar sesión en el equipo cliente después de una restauración completa del sistema.  
  
### <a name="resolutions"></a>Soluciones  
 Si cambia el nombre de un equipo y más adelante necesita restaurar una copia de seguridad guardada antes del cambio del nombre, después de la restauración, cuando intente iniciar sesión en la cuenta de dominio, recibirá este error: "La base de datos de seguridad en el servidor no tiene una cuenta de equipo para la relación de confianza de esta estación de trabajo." Para volver a tener acceso de red en el equipo, quite el software del conector, quite el equipo del dominio de Windows y, a continuación, vuelva a conectarlo al servidor.  
  
##### <a name="to-regain-network-access-to-a-restored-computer-after-a-computer-name-change"></a>Para recuperar el acceso de red a un equipo restaurado después de un cambio de nombre de equipo  
  
1.  Inicie sesión como administrador local en el equipo.  
  
2.  Desinstale el software del conector. Para obtener más información, vea el tema sobre cómo [desinstalar el software del conector](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).  
  
3.  Quite el equipo del dominio. Para obtener más información, vea el tema sobre cómo [quitar un equipo de un dominio de Windows](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).  
  
4.  Vuelva a conectar el equipo al servidor. Para obtener más información, consulte [Conectar los equipos al servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  
  
## <a name="see-also"></a>Vea también  
  
-   [Soporte técnico de Windows Server Essentials](Support-Windows-Server-Essentials.md)