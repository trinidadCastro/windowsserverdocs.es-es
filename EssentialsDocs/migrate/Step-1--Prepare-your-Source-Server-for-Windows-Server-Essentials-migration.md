---
title: 'Paso 1: Preparación del servidor de origen para la migración a Windows Server Essentials'
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 244c8a06-04c6-4863-8b52-974786455373
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6666a0f68863913c0c0a5a1b1e903eaebf5470a4
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87180491"
---
# <a name="step-1-prepare-your-source-server-for-windows-server-essentials-migration"></a>Paso 1: Preparación del servidor de origen para la migración a Windows Server Essentials

> Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials

En esta sección se explica cómo realizar copias de seguridad del servidor de origen, evaluar el estado del servidor de origen, instalar los Service Packs y las correcciones más recientes y comprobar la configuración de la red.

## <a name="to-prepare-for-migration"></a>Para preparar la migración
 Complete los siguientes pasos previos para asegurarse de que los datos y la configuración del servidor de origen se migran correctamente al servidor de destino.

1.  [Hacer una copia de seguridad del servidor de origen](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)

2.  [Instalar los Service Packs más recientes](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)

3.  [Eliminar la configuración de cuenta de inicio de sesión como servicio](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_DeleteSvcAcctSetting)

4.  [Evaluar el estado del servidor de origen](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_EvaluateHealth)

5.  [Planear la migración de las aplicaciones de línea de negocio](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MigrateLOB)

###  <a name="back-up-your-source-server"></a><a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Hacer una copia de seguridad del servidor de origen
 Haga una copia de seguridad del servidor de origen antes de iniciar el proceso de migración. Esto ayudará a proteger los datos de una pérdida accidental si se produce un error irrecuperable durante la migración.

##### <a name="to-back-up-the-source-server"></a>Para realizar copias de seguridad del servidor de origen

1. Use uno de los recursos que aparecen en la tabla siguiente para obtener ayuda a la hora de realizar una copia de seguridad completa del servidor de origen.

2. Compruebe que la copia de seguridad se ejecute correctamente. Para probar la integridad de la copia de seguridad, seleccione archivos aleatorios de la copia de seguridad, restáurelos en una ubicación alternativa y confirme que los archivos restaurados son los mismos que los archivos originales.

   |Product|Resource|
   |---|---|
   |Windows Small Business Server 2003|[Realizar una copia de seguridad y restaurar Windows Small Business Server 2003](https://msdn.microsoft.com/library/cc875809.aspx)
   |Windows Small Business Server 2008|[Realizar una copia de seguridad y restaurar los datos en Windows Small Business Server 2008](https://technet.microsoft.com/library/cc527505\(WS.10\).aspx)
   |Windows Server 2008 Foundation|[Copia de seguridad y recuperación](https://technet.microsoft.com/library/cc754097\(WS.10\).aspx)
   |Windows Small Business Server 2011 Essentials|[Más información acerca de cómo configurar la copia de seguridad del servidor](https://technet.microsoft.com/library/server-backup-support-1.aspx)
   |Windows Small Business Server 2011 Standard|[Administrar copias de seguridad del servidor](https://technet.microsoft.com/library/cc527488.aspx)
   |Windows Server Essentials|[Administrar copias de seguridad y restaurar en Windows Server Essentials](https://technet.microsoft.com/library/jj713536.aspx)

###  <a name="install-the-most-recent-service-packs"></a><a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Instalar los Service Packs más recientes
 Debe instalar las actualizaciones y Service Packs más recientes en el servidor de origen antes de la migración.

###  <a name="delete-the-log-on-as-a-service-account-setting"></a><a name="BKMK_DeleteSvcAcctSetting"></a>Eliminar la configuración de cuenta de inicio de sesión como servicio
 Si efectúa una migración desde Windows Small Business Server 2003 o Windows Server 2003, elimine la configuración de cuenta **Iniciar sesión como servicio** de la directiva de grupo.

##### <a name="to-delete-the-log-on-as-a-service-account-setting"></a>Para eliminar la configuración de cuenta de inicio de sesión como servicio

1.  Para abrir la herramienta **Administración de directivas de grupo**, haga clic en **Inicio**, en **Panel de Control**, en **Herramientas administrativas** y, a continuación, en **Administración de directivas de grupo**.

2.  Haga clic con el botón derecho en **Directiva predeterminada de controladores de dominio** y, a continuación, haga clic en **Editar**.

3.  Vaya a **Configuración de equipo\Configuración de Windows\Configuración de seguridad\Directivas locales\Asignación de derechos de usuario**.

4.  En el panel de detalles, haga doble clic en **Iniciar sesión como servicio**.

5.  Desactive la casilla **Definir esta configuración de directiva**.

6.  Elimine \\ \localhost\SYSVOL \\<domainname \>\scripts\SBS_LOGIN_SCRIPT.bat.

###  <a name="evaluate-the-health-of-the-source-server"></a><a name="BKMK_EvaluateHealth"></a>Evaluar el estado del servidor de origen
 Es importante evaluar el estado de su servidor de origen antes de empezar la migración. Use los siguientes procedimientos para asegurarse de que las actualizaciones están actualizadas, para generar un informe de mantenimiento del sistema y para ejecutar el Analizador de procedimientos recomendados (BPA) de Soluciones de Windows Server.

#### <a name="download-and-install-critical-and-security-updates"></a>Descargar e instalar actualizaciones críticas y de seguridad
 Las actualizaciones críticas y de seguridad en el servidor de origen garantizan que la migración se realice correctamente y ayudan a proteger su red durante el proceso.

###### <a name="to-check-for-the-latest-updates"></a>Para comprobar las actualizaciones más recientes

1.  En el servidor de origen, haga clic en **Inicio**, en **Todos los programas** y, a continuación, en **Windows Update**.

2.  Haga clic en **Buscar actualizaciones**.

3.  Si se encuentran actualizaciones, haga clic en **Instalar actualizaciones**.

#### <a name="run-the-best-practices-analyzer"></a>Ejecutar el analizador de procedimientos recomendados
 Puede ejecutar el analizador de procedimientos recomendados (BPA) para comprobar que no hay ningún problema en el servidor, red o dominio antes de iniciar el proceso de migración. El BPA recopila información de configuración de los siguientes orígenes:

-   Instrumental de administración de Windows (WMI) de Active Directory

-   El Registro

-   Internet Information Services (IIS)

###### <a name="to-use-the-bpa-to-analyze-your-source-server"></a>Para usar el BPA para analizar el servidor de origen

1. En la siguiente tabla se proporcionan vínculos al Centro de descarga de Microsoft, donde puede descargar e instalar el analizador de procedimientos recomendados (BPA) para el servidor de origen.


   |             Si el servidor de origen está ejecutando             |                                                     puede obtener las herramientas del BPA desde                                                      |
   |----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
   |                     Windows SBS 2003                     | [Sitio web del Analizador de procedimientos recomendados de Microsoft Windows Small Business Server 2003](https://www.microsoft.com/download/details.aspx?id=5334) |
   |                     Windows SBS 2008                     | [Sitio web del Analizador de procedimientos recomendados de Microsoft Windows Small Business Server 2008](https://www.microsoft.com/download/details.aspx?id=6231) |
   | Windows SBS 2011 Essentials o Windows SBS 2011 Standard |          [Sitio web del Analizador de procedimientos recomendados de Soluciones de Windows Server](https://www.microsoft.com/download/details.aspx?id=15556)           |
   |     Windows Server Essentials o Windows Server 2012     |                                                          El panel del servidor                                                           |


2. Una vez completada la descarga, haga clic en **Inicio**, seleccione **Todos los programas** y haga clic en **Herramienta Analizador de procedimientos recomendados de SBS**.

   > [!NOTE]
   >  Compruebe si hay actualizaciones antes de examinar el servidor.

3. En el panel de navegación, haga clic en **Iniciar un análisis**.

    Si el servidor de origen ejecuta Windows Server Essentials, haga lo siguiente:

   1.  Inicie sesión en el servidor de destino como administrador y abra el panel.

   2.  En el panel, haga clic en la pestaña **Dispositivos**.

   3.  En el panel tareas de **servidor**de < > **Tasks** , haga clic en **analizador de procedimientos recomendados**.

4. En el panel de detalles, escriba la etiqueta del análisis y, a continuación, haga clic en **Iniciar análisis**. La etiqueta del análisis es el nombre del informe de análisis, por ejemplo, **SBS BPA Scan 1Jul2013**.

5. Cuando termine el análisis, haga clic en **Ver un informe de este análisis de Best Practices**.

   Después de que la herramienta BPA recopile información sobre la configuración del servidor, comprueba que la información sea correcta y muestra a los administradores una lista con información y problemas ordenados por gravedad. En la lista se describe cada problema y se propone una recomendación o posible solución. Hay disponibles tres tipos de informes:

|Tipo de informe|Description
|-----------------|-----------------
|Informes de lista|Muestra los informes como una lista unidimensional.
|Informes de árbol|Muestra los informes como una lista jerárquica.

Para ver la descripción y las soluciones para un problema, haga clic en el problema en el informe. No todos los problemas notificados por la herramienta BPA afectan a la migración, pero debe resolver todos los problemas posibles para asegurarse de que la migración se realice correctamente.

####  <a name="synchronize-the-source-server-time-with-an-external-time-source"></a><a name="BKMK_SynchronizeTheSourceServerTimeWithAnExternalTimeSource"></a>Sincronizar la hora del servidor de origen con un origen de hora externo
 La hora del servidor de origen no debe tener una diferencia mayor a cinco minutos con la hora del servidor de destino, y la fecha y la zona horaria deben coincidir en los dos servidores. Si el servidor de origen se está ejecutando en una máquina virtual, la fecha, la hora y la zona horaria en el servidor host deben coincidir con las del servidor de origen y el servidor de destino. Para asegurarse de que Windows Server Essentials se instala correctamente, debe sincronizar la hora del servidor de origen con el servidor NTP (Protocolo de tiempo de red) en Internet.

###### <a name="to-synchronize-the-source-server-time-with-the-ntp-server"></a>Para sincronizar la hora del servidor de origen con el servidor NTP

1.  Inicie sesión en el servidor de origen con una cuenta y una contraseña de administrador de dominio.

2.  Haga clic en **Inicio** y en **Ejecutar**. Luego escriba **cmd** en el cuadro de texto y presione ENTRAR.

3.  En el símbolo del sistema, escriba w32tm /config /syncfromflags:domhier /reliable:no /update y presione ENTRAR.

4.  En el símbolo del sistema, escriba net stop w32time y, a continuación, presione ENTRAR.

5.  En el símbolo del sistema, escriba net start w32time y, a continuación, presione ENTRAR.

> [!IMPORTANT]
>  Durante la instalación de Windows Server Essentials, tiene la oportunidad de comprobar la hora en el servidor de destino y cambiarla si es necesario. Asegúrese de que la diferencia de hora es inferior a cinco minutos respecto a la hora establecida en el servidor de origen. Una vez terminada la instalación, el servidor de destino se sincroniza con el NTP. Todos los equipos unidos a dominio, incluido el servidor de origen, se sincronizan con el servidor de destino, que asume el rol de maestro emulador de controlador de dominio principal (PDC).

###  <a name="create-a-plan-to-migrate-line-of-business-applications"></a><a name="BKMK_MigrateLOB"></a>Crear un plan para migrar aplicaciones de línea de negocio
 Una aplicación de línea de negocio (LOB) es una aplicación informática crítica y fundamental para dirigir un negocio. Las aplicaciones LOB incluyen aplicaciones contables, de administración de la cadena de suministro y de planificación de recursos.

 Al planificar la migración de aplicaciones LOB, consulte a los proveedores de aplicaciones LOB para determinar el método adecuado para la migración de cada aplicación. También debe ubicar los medios que se usan para instalar las aplicaciones LOB en el servidor de destino.

> [!NOTE]
>  Si ha usado el SDK de Windows Small Business Server 2011 Essentials para desarrollar un complemento personalizado de alerta o de mantenimiento del sistema y desea seguir usando el complemento con Windows Server Essentials, también debe actualizar el complemento e implementarlo en el servidor de destino.


### <a name="create-a-plan-to-migrate-email-hosted-on-windows-sbs-2011-windows-sbs-2008-and-windows-sbs-2003"></a>Crear un plan para migrar el correo electrónico hospedado en Windows SBS 2011, Windows SBS 2008 y Windows SBS 2003
 En Windows SBS 2011, Windows SBS 2008 y Windows SBS 2003, el correo electrónico se proporciona a través de Microsoft Exchange Server. Sin embargo, Windows Server Essentials no proporciona un servicio de correo electrónico de bandeja de entrada. Si actualmente usa un servidor que ejecuta Windows SBS 2011, Windows SBS 2008 o Windows SBS 2003 para hospedar el correo electrónico de su empresa, debe migrar a una solución local u hospedada alternativa.

> [!NOTE]
>  Después de actualizar y preparar el servidor de origen para la migración, se recomienda crear una copia de seguridad del servidor actualizado antes de continuar el proceso de migración.

#### <a name="migrate-email-to-microsoft-office-365"></a>Migrar el correo electrónico a Microsoft Office 365
 Si eligió usar Microsoft Office 365 como solución de correo electrónico para su dominio, siga las instrucciones de [Migrar todos los buzones de correo a la nube con una migración total de Exchange](https://help.outlook.com/140/ms.exch.ecp.emailmigrationwizardexchangelearnmore.aspx) para comenzar la migración del correo electrónico a Office 365. Se recomienda completar la migración de correo electrónico antes de instalar Windows Server Essentials.

> [!NOTE]
>  El paso para quitar el servidor local de Exchange Server en el servidor de origen es obligatorio si tiene previsto integrar Windows Server Essentials con Office 365. Para obtener información acerca de cómo migrar carpetas públicas de Exchange Server a Office 365, consulte la entrada de blog sobre los [scripts de migración de carpetas públicas de Microsoft Exchange 2013 para Office 365](https://blogs.technet.com/b/fmustafa/archive/2013/04/11/microsoft-exchange-2013-public-folders-migration-scripts-for-office-365.aspx).
>
>  Después de completar la instalación, debe activar la característica de integración de Office 365 en Windows Server Essentials mediante la ejecución de la tarea **integrar con Microsoft Office 365** .

> [!IMPORTANT]
>  Para que la herramienta de migración de Office 365 pueda conectar con el servidor de Exchange Server que se ejecuta en el servidor de origen, debe habilitar RPC a través de HTTP en el servidor de origen. Para obtener más información sobre cómo habilitar RPC a través de HTTP, consulte [Cómo implementar RPC a través de HTTP por primera vez en Small Business Server 2003 (Standard o Premium)](https://technet.microsoft.com/library/bb123622%28EXCHG.65%29.aspx). Si no puede ejecutar correctamente la herramienta de migración de Office 365 después de habilitar RPC a través de HTTP, vea la opción **ValidPorts** del Registro en HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy y asegúrese de que aparezca el nombre completo (FQDN) del servidor de origen. Si no aparece el nombre completo, agréguelo como en el ejemplo siguiente:
>
>  remote. *contoso*.com:6001-6002;remote. *contoso*.com:6004 (reemplace *contoso* por el nombre de su dominio).

#### <a name="migrate-email-to-another-on-premises-exchange-server"></a>Migrar correo electrónico a otro servidor local de Exchange Server
 Para obtener información sobre cómo migrar el correo electrónico a otro servidor local de Exchange Server, consulte [integrar un servidor local de Exchange Server con Windows Server Essentials](https://technet.microsoft.com/library/jj200172.aspx). Se recomienda que configure el nuevo servidor local de Exchange después de instalar Windows Server Essentials y, a continuación, finalice la migración de correo electrónico antes de disminuir el nivel del servidor de origen.

> [!NOTE]
>  Exchange Server no incluye el conector POP3 de Windows Small Business Server. Después de migrar los datos de correo electrónico a otro servidor de Exchange Server, ya no podrá usar la característica Conector POP3.

> [!NOTE]
>  Después de actualizar y preparar el servidor de origen para la migración, debe crear una copia de seguridad del servidor actualizado antes de continuar el proceso de migración.

## <a name="next-steps"></a>Pasos siguientes
 Ha preparado el servidor de origen para la migración a Windows Server Essentials.  Ahora, vaya al [paso 2: instalación de Windows Server Essentials como nuevo controlador de dominio de réplica](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md).

Para ver todos los pasos, vea [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

