---
title: "Paso 1: Preparar la migración de servidor de origen para Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 244c8a06-04c6-4863-8b52-974786455373
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2efb1badde6d0ca11bc3b7526fdfb377d9f95d3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="step-1-prepare-your-source-server-for-windows-server-essentials-migration"></a>Paso 1: Preparar la migración de servidor de origen para Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

En este tema se explica cómo hacer copias de seguridad del servidor de origen, evaluar el estado del sistema de servidor de origen, instalar el service Pack y correcciones más recientes y comprueba la configuración de red.  
  
## <a name="to-prepare-for-migration"></a>Prepararse para la migración  
 Completa los siguientes pasos preliminares para asegurarte de que las configuraciones y datos en el servidor de origen de migran correctamente al servidor de destino.  
  
1.  [Hacer copia de seguridad de tu servidor de origen](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Instalar el service Pack más recientes](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Eliminar el registro en como una configuración de cuenta de servicio](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_DeleteSvcAcctSetting)  
  
4.  [Evaluar el estado del servidor de origen](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_EvaluateHealth)  
  
5.  [Crear un plan para migrar aplicaciones de línea de negocio](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MigrateLOB)  
  
###  <a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Hacer copia de seguridad de tu servidor de origen  
 Hacer una copia de tu servidor de origen antes de comenzar el proceso de migración. Realizar una copia de seguridad ayuda a proteger los datos contra la pérdida accidental si se produce un error irrecuperable durante la migración.  
  
##### <a name="to-back-up-the-source-server"></a>Hacer copia de seguridad del servidor de origen  
  
1.  Usa uno de los recursos en la tabla siguiente para dirigirte a realizar una copia de seguridad completa del servidor de origen.  
  
2.  Comprueba que la copia de seguridad se ejecutó correctamente. Para probar la integridad de la copia de seguridad, seleccionar archivos aleatorios de la copia de seguridad, restaurarlos en una ubicación alternativa y, a continuación, confirma que los archivos restaurados son los mismos que los archivos originales.  
  
   |Producto|Recursos|
   |---|---|
   |Windows Small Business Server 2003|[Realizar copias de seguridad y restauración de Windows Small Business Server 2003](https://msdn.microsoft.com/library/cc875809.aspx) 
   |Windows Small Business Server 2008|[Realizar copias de seguridad y restauración de datos en Windows Small Business Server 2008](https://technet.microsoft.com/library/cc527505\(WS.10\).aspx)
   |Windows Server 2008 Foundation|[Copia de seguridad y recuperación](https://technet.microsoft.com/library/cc754097\(WS.10\).aspx)  
   |Windows Small Business Server 2011 Essentials|[Más información sobre la configuración de copia de seguridad del servidor](https://technet.microsoft.com/library/server-backup-support-1.aspx)
   |Windows Small Business Server 2011 Standard|[Administrar la copia de seguridad del servidor](https://technet.microsoft.com/library/cc527488.aspx)  
   |Windows Server Essentials|[Administrar la copia de seguridad y restauración en Windows Server Essentials](https://technet.microsoft.com/library/jj713536.aspx)

###  <a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Instalar el service Pack más recientes  
 Debes instalar los service Pack y actualizaciones más recientes en el servidor de origen antes de la migración.  
  
###  <a name="BKMK_DeleteSvcAcctSetting"></a>Eliminar el registro en como una configuración de cuenta de servicio  
 Si vas a migrar de Windows Small Business Server 2003 o Windows Server 2003, elimina el **iniciar sesión como servicio** configuración de directiva de grupo de la cuenta.  
  
##### <a name="to-delete-the-log-on-as-a-service-account-setting"></a>Para eliminar el registro en como una configuración de cuenta de servicio  
  
1.  Para abrir el **Group Policy Management** herramienta, haz clic en **inicio**, haz clic en **Panel de Control**, haz clic en **herramientas administrativas**y, a continuación, haz clic en **Group Policy Management**.  
  
2.  Haz clic en **directiva predeterminada de controladores de dominio**y, a continuación, haz clic en **editar**.  
  
3.  Navegar a **equipo equipo\Configuración Windows\Configuración seguridad\Directivas locales\Asignación de derechos**.  
  
4.  En el panel de detalles, haz doble clic en **iniciar sesión como servicio**.  
  
5.  Borrar la **definir esta configuración de directiva** casilla de verificación.  
  
6.  Eliminar \scripts\SBS_LOGIN_SCRIPT.bat \\\localhost\SYSVOL\\ < domainname\ >.  
  
###  <a name="BKMK_EvaluateHealth"></a>Evaluar el estado del servidor de origen  
 Es importante evaluar el estado de tu servidor de origen antes de empezar la migración. Usa los siguientes procedimientos para garantizar que las actualizaciones están actuales, para generar un informe de estado del sistema y ejecutar el Windows Server soluciones mejor práctica Analyzer (BPA).  
  
#### <a name="download-and-install-critical-and-security-updates"></a>Seguridad y críticas de la instalación y descarga las actualizaciones  
 Instalar críticas y seguridad se actualiza en el servidor de origen que ayuda a garantiza que la migración se realizará correctamente y ayuda a proteger la red durante el proceso de migración.  
  
###### <a name="to-check-for-the-latest-updates"></a>Para comprobar las últimas actualizaciones  
  
1.  El servidor de origen, haz clic en **inicio**, haz clic en **todos los programas**y, a continuación, haz clic en **Windows Update**.  
  
2.  Haz clic en **buscar actualizaciones**.  
  
3.  Si se encuentran actualizaciones, haz clic en **instalar actualizaciones**.  
  
#### <a name="run-the-best-practices-analyzer"></a>Ejecuta el analizador de procedimientos recomendados  
 Puedes ejecutar el analizador de procedimientos recomendados (BPA) para comprobar que no hay ningún problema en el servidor, red o dominio antes de comenzar el proceso de migración. El BPA recopila información de configuración de los siguientes orígenes:  
  
-   Instrumental de administración de Windows (WMI) de Active Directory  
  
-   El registro  
  
-   Internet Information Services (IIS)  
  
###### <a name="to-use-the-bpa-to-analyze-your-source-server"></a>Para usar el BPA para analizar tu servidor de origen  
  
1.  La siguiente tabla proporciona vínculos a Microsoft Download Center donde puede descargar e instalar el analizador de procedimientos recomendados (BPA) para el servidor de origen desde.  
  
   |Si el servidor de origen está ejecutando|Puedes obtener las herramientas BPA desde|
   |---|---|
   |Windows SBS 2003|[Sitio Web de Microsoft Windows Small Business Server 2003 analizador de procedimientos recomendados](https://www.microsoft.com/download/details.aspx?id=5334)
   |Windows SBS 2008|[Sitio Web de Microsoft Windows Small Business Server 2008 analizador de procedimientos recomendados](https://www.microsoft.com/download/details.aspx?id=6231)  
   |Windows SBS 2011 Essentials o estándar de Windows SBS 2011|[Sitio Web del analizador de procedimientos recomendados de soluciones de servidor de Windows](https://www.microsoft.com/download/details.aspx?id=15556) 
   |Windows Server Essentials o Windows Server 2012|El panel de servidor  
  
2.  Una vez completada la descarga, haz clic en **inicio**, elija **todos los programas**y, a continuación, haz clic en **SBS herramienta**.  
  
    > [!NOTE]
    >  Buscar actualizaciones antes de analizar el servidor.  
  
3.  En el panel de navegación, haz clic en **iniciar una exploración**.  
  
     Si tu servidor de origen ejecuta Windows Server Essentials, haz lo siguiente:  
  
    1.  Iniciar sesión en el servidor de destino como administrador y, a continuación, abra el panel.  
  
    2.  En el panel, haz clic en el **dispositivos** pestaña.  
  
    3.  En el <**Server** >**tareas** panel, haz clic en **analizador de procedimientos recomendados**.  
  
4.  En el panel de detalles, escribe la etiqueta de análisis y, a continuación, haz clic en **iniciar la digitalización**. La etiqueta de análisis es el nombre del informe de análisis, por ejemplo, **SBS BPA digitalizar 1 de julio de 2013**.  
  
5.  Una vez completada la búsqueda, haz clic en **ver un informe de este análisis de los procedimientos recomendados**.  
  
 Una vez la herramienta BPA recopila información sobre la configuración del servidor, comprueba que la información es correcta y, a continuación, presenta los administradores con una lista de información y los problemas que se ordenan por gravedad. La lista describen cada problema y proporciona una solución de recomendación o posibles. Existen tres tipos de informe:  
  
|Tipo de informe|Descripción
|-----------------|----------------- 
|Informes de lista|Informes de muestra una lista de una dimensión. 
|Informes de árbol|Informes de muestra una lista jerárquica.

Para ver la descripción y las soluciones para un problema, haz clic en el problema en el informe. No todos los problemas notificados por la herramienta BPA afecta a la migración, pero debería solucionar como muchos de los problemas como sea posible para garantizar que la migración se realiza correctamente.  
  
####  <a name="BKMK_SynchronizeTheSourceServerTimeWithAnExternalTimeSource"></a>Sincronizar la hora de servidor de origen con una fuente externa  
 El tiempo en el servidor de origen debe establecerse en cinco minutos de la hora en el servidor de destino y la fecha y la zona horaria deben ser el mismo en ambos servidores. Si el servidor de origen está ejecutando en una máquina virtual, la fecha, hora y la zona horaria en el servidor host deben coincidir con el servidor de origen y el servidor de destino. Para ayudar a garantizar que Windows Server Essentials se instala correctamente, debe sincronizar la hora de servidor de origen al servidor de protocolo de tiempo de red (NTP) en Internet.  
  
###### <a name="to-synchronize-the-source-server-time-with-the-ntp-server"></a>Para sincronizar la hora del servidor de origen con el servidor NTP  
  
1.  Inicia sesión el servidor de origen con una cuenta de administrador de dominio y la contraseña.  
  
2.  Haz clic en **inicio**, haz clic en **ejecutar**, tipo **cmd** en el cuadro de texto y, a continuación, presiona ENTRAR.  
  
3.  En el símbolo del sistema, escribe w32tm /config /syncfromflags:domhier / confiable: no/Update y, a continuación, presione ENTRAR.  
  
4.  En el símbolo del sistema, escriba net stop w32time y, a continuación, presione ENTRAR.  
  
5.  En el símbolo del sistema, escribe el comando net start w32time y, a continuación, presione ENTRAR.  
  
> [!IMPORTANT]
>  Durante la instalación de Windows Server Essentials, tienes la oportunidad de comprobar el tiempo en el servidor de destino y cambiarlo, si es necesario. Asegúrate de que la hora está en cinco minutos del tiempo que se establece en el servidor de origen. Cuando finaliza la instalación, el servidor de destino se sincroniza con el NTP. Todos los equipos unidos a un dominio, incluido el servidor de origen, se sincronizarán con el servidor de destino, que asume el rol del dominio principal maestro emulador del controlador (PDC).  
  
###  <a name="BKMK_MigrateLOB"></a>Crear un plan para migrar aplicaciones de línea de negocio  
 Una aplicación de línea de negocio (LOB) es una aplicación de equipo crítica que es fundamental a la ejecución de un negocio. Las aplicaciones de LOB incluyen la administración de cuentas, la cadena de suministro y las aplicaciones de la planificación de recursos.  
  
 Al que vas a migrar las aplicaciones de LOB, consultar con los proveedores de aplicaciones de LOB para determinar el método adecuado para la migración de cada aplicación. También deben localizar los medios que se usan para instalar las aplicaciones de línea de negocio en el servidor de destino.  
  
> [!NOTE]
>  Si has usado el 2011 Essentials SDK de Windows Small Business Server para desarrollar un mantenimiento del sistema personalizada o agregar en la alerta y quieres seguir usando el complemento con Windows Server Essentials, también debes actualizar el complemento e implementarla en el servidor de destino.  
  
  
### <a name="create-a-plan-to-migrate-email-hosted-on-windows-sbs-2011-windows-sbs-2008-and-windows-sbs-2003"></a>Crear un plan para migrar el correo electrónico hospedada en Windows SBS 2003, Windows SBS 2008 y Windows SBS 2011  
 En Windows SBS 2011, Windows SBS 2008 y Windows SBS 2003, correo electrónico se proporciona a través de Microsoft Exchange Server. Sin embargo, Windows Server Essentials no proporciona un servicio de correo electrónico de la Bandeja de entrada. Si estás usando actualmente un servidor que ejecuta Windows SBS 2011, Windows SBS 2008 o Windows SBS 2003 para hospedar el correo electrónico de la compañía s, necesita para migrar a una local alternativos u hospedado solución.  
  
> [!NOTE]
>  Después de actualizar y preparar el servidor de origen para la migración, te recomendamos que crees una copia de seguridad del servidor actualizado antes de continuar con el proceso de migración.  
  
#### <a name="migrate-email-to-microsoft-office-365"></a>Migrar de correo electrónico a Microsoft Office 365  
 Si decide usar Microsoft Office 365 como la solución de correo electrónico de tu dominio, sigue las instrucciones en [migrar todos los buzones en la nube con una migración de Exchange traslado](http://help.outlook.com/140/ms.exch.ecp.emailmigrationwizardexchangelearnmore.aspx) para iniciar la migración de correo electrónico a Office 365. Te recomendamos que haya finalizado la migración de correo electrónico antes de instalar Windows Server Essentials.  
  
> [!NOTE]
>  El paso para quitar el servidor de Exchange local en el servidor de origen es obligatorio si tienes intención de integrar Windows Server Essentials con Office 365. Para obtener información acerca de cómo migrar carpetas públicas de Exchange Server a Office 365, consulta la entrada de blog [Scripts de migración de carpetas públicas de 2013 de Exchange Microsoft para Office 365](http://blogs.technet.com/b/fmustafa/archive/2013/04/11/microsoft-exchange-2013-public-folders-migration-scripts-for-office-365.aspx).  
>   
>  Después de completar la instalación, debe activar la característica de integración con Office 365 en Windows Server Essentials ejecutando el **integración con Microsoft Office 365** tarea.  
  
> [!IMPORTANT]
>  Para permitir que la herramienta de migración de Office 365 conectar con el servidor Exchange que se ejecuta en el servidor de origen, debes habilitar RPC sobre HTTP del servidor de origen. Para obtener información acerca de cómo habilitar RPC a través de HTTP, consulta [cómo implementar RPC a través de HTTP por primera vez en Small Business Server 2003 (estándar o Premium)](https://technet.microsoft.com/library/bb123622%28EXCHG.65%29.aspx). Si no puedes ejecutar la herramienta de migración de Office 365 correctamente después de habilitar RPC a través de HTTP, ver la **ValidPorts** configuración en el registro en HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy y asegúrate de que se muestra el nombre de dominio completo (FQDN) del servidor de origen. Si no aparece el FQDN, agregarlo manualmente mediante el siguiente ejemplo:  
>   
>  remoto. *Contoso*.com:6001-6002; remoto. *Contoso*.com:6004 (reemplaza *contoso* con el nombre del dominio).  
  
#### <a name="migrate-email-to-another-on-premises-exchange-server"></a>Migrar de correo electrónico a otro servidor de Exchange local  
 Para obtener información acerca de cómo migrar el correo electrónico a otra local Exchange Server, consulta [integrar un servidor de Exchange local con Windows Server Essentials](https://technet.microsoft.com/library/jj200172.aspx). Te recomendamos configurar el servidor de Exchange local después de instalar Windows Server Essentials y, a continuación, finalizar la migración de correo electrónico antes de degradar al servidor de origen.  
  
> [!NOTE]
>  Conector de POP3 Windows Small Business Server no se incluye con el servidor Exchange. Después de migrar los datos de correo electrónico a otro servidor Exchange, ya no se puede utilizar la característica de conector POP3.  
  
> [!NOTE]
>  Después de actualizar y preparar el servidor de origen para la migración, debes crear una copia de seguridad del servidor actualizado antes de continuar con el proceso de migración.  
  
## <a name="next-steps"></a>Pasos siguientes  
 Ha preparado el servidor de origen para la migración a Windows Server Essentials.  Ahora ve a [paso 2: instalar Windows Server Essentials como un nuevo controlador de dominio de réplica](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md).  

Para ver todos los pasos, consulta [migrar a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

