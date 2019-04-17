---
title: Preparar el servidor de origen para Windows Server Essentials migration1
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5861ae9-77cb-4d37-b4c5-8f0757213385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 929c7506c78667646e429c4f28df7e5642c575ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="prepare-your-source-server-for-windows-server-essentials-migration1"></a>Preparar el servidor de origen para Windows Server Essentials migration1

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Completa los siguientes pasos preliminares para asegurarte de que las configuraciones y datos en el servidor de origen de migran correctamente al servidor de destino.  
  
#### <a name="to-prepare-for-migration"></a>Prepararse para la migración  
  

1.  [Hacer copia de seguridad de tu servidor de origen](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Instalar el service Pack más recientes](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Evaluar el estado del servidor de origen](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [Ejecutar la herramienta de preparación de la migración en el servidor de origen](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [Crear un plan para migrar aplicaciones de línea de negocio](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

1.  [Hacer copia de seguridad de tu servidor de origen](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Instalar el service Pack más recientes](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Evaluar el estado del servidor de origen](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [Ejecutar la herramienta de preparación de la migración en el servidor de origen](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [Crear un plan para migrar aplicaciones de línea de negocio](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

  
###  <a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Hacer copia de seguridad de tu servidor de origen  
 Hacer una copia de tu servidor de origen antes de comenzar el proceso de migración. Realizar una copia de seguridad ayuda a proteger los datos contra la pérdida accidental si se produce un error irrecuperable durante la migración.  
  
##### <a name="to-back-up-the-source-server"></a>Hacer copia de seguridad del servidor de origen  
  
1.  Realizar una copia de seguridad completa del servidor de origen. Para obtener más información sobre cómo hacer copias de seguridad de Windows Small Business Server 2011 Essentials, consulte [más información sobre la configuración de copia de seguridad del servidor](https://technet.microsoft.com/library/server-backup-support-1.aspx).  
  
2.  Comprueba que la copia de seguridad se ejecutó correctamente. Para probar la integridad de la copia de seguridad, seleccionar archivos aleatorios de la copia de seguridad, restaurarlos en una ubicación alternativa y, a continuación, confirma que los archivos restaurados son los mismos que los archivos originales.  
  
###  <a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Instalar el service Pack más recientes  
 Debes instalar los service Pack y actualizaciones más recientes en el servidor de origen antes de la migración.  
  
###  <a name="BKMK_UseWindowsBestPracticeAnalyzer"></a>Evaluar el estado del servidor de origen  
 Es importante evaluar el estado de tu servidor de origen antes de empezar la migración. Usa los siguientes procedimientos para garantizar que las actualizaciones están actuales, para generar un informe de estado del sistema y ejecutar el Windows Server soluciones mejor práctica Analyzer (BPA).  
  
#### <a name="download-and-install-critical-and-security-updates"></a>Seguridad y críticas de la instalación y descarga las actualizaciones  
 Instalar críticas y seguridad se actualiza en el servidor de origen que ayuda a garantiza que la migración se realizará correctamente y ayuda a proteger la red durante el proceso de migración.  
  
###### <a name="to-check-for-the-latest-updates"></a>Para comprobar las últimas actualizaciones  
  
1.  El servidor de origen, haz clic en **inicio**, haz clic en **todos los programas**y, a continuación, haz clic en **Windows Update**.  
  
2.  Haz clic en **buscar actualizaciones**.  
  
3.  Si se encuentran actualizaciones, haz clic en **instalar actualizaciones**.  
  
#### <a name="check-the-alert-viewer-for-critical-errors"></a>Comprobar el Visor de alertas para errores graves  
 Puedes comprobar el Visor de alertas en el panel para todos los errores críticos.  
  
#### <a name="run-the-windows-server-solutions-best-practices-analyzer"></a>Ejecuta el analizador de procedimientos recomendados de Windows Server soluciones  
 Puedes ejecutar el Windows Server soluciones Best Practices Analyzer (BPA) para comprobar que no hay ningún problema en el servidor, red o dominio antes de comenzar el proceso de migración. El BPA recopila información de configuración de los siguientes orígenes:  
  
-   Active Directory® Instrumental de administración de Windows (WMI)  
  
-   El registro  
  
-   La metabase de Internet Information Services (IIS)  
  
###### <a name="to-use-the-windows-server-solutions-bpa-to-analyze-your-source-server"></a>Para usar el BPA de soluciones de Windows Server para analizar tu servidor de origen  
  
1.  Descargar e instalar el [analizador de procedimientos recomendados de Windows Server soluciones](https://www.microsoft.com/en-us/download/details.aspx?id=15556) en Microsoft Download Center.  
  
2.  Una vez completada la descarga, haz clic en **inicio**, elija **todos los programas**y, a continuación, haz clic en **SBS herramienta**.  
  
    > [!NOTE]
    >  Buscar actualizaciones antes de analizar el servidor.  
  
3.  En el panel de navegación, haz clic en **iniciar una exploración**.  
  
4.  En el panel de detalles, escribe la etiqueta de análisis y, a continuación, haz clic en **iniciar la digitalización**. La etiqueta de análisis es el nombre del informe de análisis, por ejemplo, **SBS BPA digitalizar 1 de julio de 2012**.  
  
5.  Una vez completada la búsqueda, haz clic en **ver un informe de este análisis de los procedimientos recomendados**.  
  
 Después de obtener información sobre la configuración del servidor, el BPA de soluciones de Windows Server comprueba que la información es correcta y, a continuación, presenta los administradores con una lista de información y los problemas que se ordenan por gravedad. La lista describen cada problema y proporciona una solución de recomendación o posibles. Existen tres tipos de informe:  
  
|Tipo de informe|Descripción|  
|-----------------|-----------------|  
|Informes de lista|Informes de muestra una lista de una dimensión.|  
|Informes de árbol|Informes de muestra una lista jerárquica.|  
|Otros informes|Informes de muestra como un registro de tiempo de ejecución.|  
  
 Para ver la descripción y las soluciones para un problema, haz clic en el problema en el informe. No todos los problemas notificados por la BPA de Windows SBS 2011 Essentials afecta a la migración, pero debería solucionar como muchos de los problemas como sea posible para garantizar que la migración se realiza correctamente.  
  
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
  
###  <a name="BKMK_MPT"></a>Ejecutar la herramienta de preparación de la migración en el servidor de origen  
 No puedes realizar una instalación de modo de migración sin primera ejecutar la herramienta de preparación de la migración en el servidor de origen. Esta herramienta está diseñada para preparar el servidor de origen y el dominio migrar a Windows Server Essentials.  
  
> [!IMPORTANT]
>  Hacer una copia de tu servidor de origen antes de ejecutar la herramienta de preparación de la migración. Todos los cambios que la herramienta de preparación de la migración se realiza en el esquema sean irreversibles. Si tienes problemas durante la migración, es la única forma devolver el servidor de origen al estado que tenía antes de ejecutar la herramienta Restaurar el servidor de una copia de seguridad del sistema.  
  
 Para ejecutar la herramienta de preparación de la migración, debe ser miembro del grupo Administradores de empresa, el grupo de administradores de esquema y el grupo de administradores de dominio.  
  
##### <a name="to-verify-that-you-have-the-appropriate-permissions-to-run-the-tool-on-the-source-server"></a>Para comprobar que tienes los permisos adecuados para ejecutar la herramienta en el servidor de origen  
  
1.  En el servidor de origen, haz clic en **inicio**, haz clic en **herramientas administrativas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**.  
  
2.  En el árbol de consola, expande el dominio y, a continuación, haga clic en **usuarios**.  
  
3.  Haz clic en la cuenta de administrador que está usando para la migración y, a continuación, haz clic en **propiedades**.  
  
4.  Haz clic en el **miembro de** pestaña y, a continuación, comprueba que los administradores de empresa, los administradores de esquema y los administradores de dominio se muestran en la **miembro de** cuadro de texto.  
  
5.  Si no se muestran los grupos, haz clic en **agregar**, y, a continuación, agrega cada grupo que no aparece.  
  
    > [!NOTE]
    >  -   Puedes recibir un error de permiso si no se inicia el servicio de Net Logon.  
    > -   Debe cerrar sesión e iniciarla en el servidor para que los cambios surtan efecto.  
  
     Puedes usar la versión más reciente del agente de Windows Update para asegurarse de que el proceso de actualización de servidor funciona correctamente.  
  
 Puedes usar la versión más reciente del agente de Windows Update para asegurarse de que el proceso de actualización de servidor funciona correctamente.  
  
 Antes de instalar a agente de Windows Update en el servidor de origen, primero debes instalar Windows PowerShell 2.0 y Microsoft Baseline configuración Analyzer 2.0.  
  
-   Para descargar e instalar Windows PowerShell 2.0, consulta [artículo 968929](https://go.microsoft.com/fwlink/p/?LinkId=241483) en Microsoft Knowledge Base.  
  
-   Para descargar e instalar Microsoft Baseline configuración Analyzer 2.0, consulta el [Microsoft Baseline configuración Analyzer 2.0](https://go.microsoft.com/fwlink/p/?LinkId=241484) en Microsoft Download Center.  
  
-   Para descargar e instalar la versión más reciente de agente de Windows Update, vea [artículo 949104](https://go.microsoft.com/fwlink/p/?LinkId=237493) en Microsoft Knowledge Base.  
  
##### <a name="to-install-and-run-the-migration-preparation-tool-on-the-source-server"></a>Para instalar y ejecutar la herramienta de preparación de la migración en el servidor de origen  
  
1.  Windows Server Essentials DVD1 se inserta en la unidad de DVD en el servidor de origen.  
  
2.  Abre el Explorador de Windows, busque la **\support\tools** carpeta del DVD y, a continuación, haz doble clic en el **sourcetool.msi** archivo.  
  
    > [!NOTE]
    >  -   Si la herramienta de preparación de migración ya está instalada en el servidor, ejecuta la herramienta de la **inicio** menú.  
    > -   Para asegurarte de que está preparado para la mejor experiencia posible migración, se recomienda que siempre que elijas instalar la actualización más reciente.  
  
     El asistente instala la herramienta de preparación de la migración en el servidor de origen. Una vez completada la instalación, la herramienta de preparación de la migración se ejecuta automáticamente e instala las actualizaciones más recientes.  
  
3.  En la herramienta de preparación de la migración, selecciona **tengo una copia de seguridad y estoy listo para continuar**y, a continuación, haz clic en **siguiente**.  
  
    > [!WARNING]
    >  Si recibes un mensaje de error relacionados con una instalación de revisiones, consulta el método 2: cambiar el nombre de la carpeta Catroot2 en [artículo 822798](https://go.microsoft.com/FWLink/p/?LinkID=118672) en Microsoft Knowledge Base.  
  
     La herramienta de preparación de la migración se prepara el dominio de origen para la migración mediante la extensión de esquema de Active Directory. Después de completa la tarea, haz clic en **siguiente** para continuar.  
  
4.  Después de preparar el dominio de origen, la herramienta de preparación de la migración analiza el servidor de origen para identificar los dos tipos de posibles problemas.  
  
    -   **Errores** problemas encontraron en el servidor de origen que puede bloquear la migración o hacer que la migración genere un error. Sigue las instrucciones en el mensaje de error para solucionar los problemas y, a continuación, haz clic en **digitalizar nuevo**.  
  
    -   **Advertencias** problemas encontraron en el servidor de origen que pueden causar problemas funcionales durante la migración. Se recomienda encarecidamente que sigas las instrucciones en el mensaje de error para corregir problemas antes de continuar con la migración.  
  
     Después de solucionar o reconocer a todos los problemas, haz clic en **siguiente**.  
  
5.  En la herramienta de preparación de la migración, haz clic en **finalizar**.  
  
6.  Cuando finalice la herramienta de preparación de la migración, se te pedirá que reiniciar el servidor de origen antes de empezar a migrar a Windows Server Essentials.  
  
> [!NOTE]
>  Debes completar una ejecución correcta de la herramienta de preparación de la migración en el servidor de origen en dos semanas de instalación de Windows Server Essentials en el servidor de destino. De lo contrario, se bloqueará la instalación de Windows Server Essentials en el servidor de destino. Si esto sucede, vuelve a debe ejecutar la herramienta de preparación de la migración en el servidor de origen.  
  
###  <a name="BKMK_PlanToMigrateLineOfBusinessApplications"></a>Crear un plan para migrar aplicaciones de línea de negocio  
 Una aplicación de línea de negocio (LOB) es una aplicación de equipo crítica que es fundamental a la ejecución de un negocio. Las aplicaciones de LOB incluyen la administración de cuentas, la cadena de suministro y las aplicaciones de la planificación de recursos.  
  
 Al que vas a migrar las aplicaciones de LOB, consultar con los proveedores de aplicaciones de LOB para determinar el método adecuado para la migración de cada aplicación. También deben localizar los medios que se usan para instalar las aplicaciones de línea de negocio en el servidor de destino.  
  
> [!NOTE]
>  Si has usado el 2011 Essentials SDK de Windows Small Business Server para desarrollar un mantenimiento del sistema personalizada o agregar en la alerta y quieres seguir usando el complemento con Windows Server Essentials, también debes actualizar el complemento e implementarla en el servidor de destino.  
  
