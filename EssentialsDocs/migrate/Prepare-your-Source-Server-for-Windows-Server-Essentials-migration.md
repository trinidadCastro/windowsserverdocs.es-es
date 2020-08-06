---
title: Preparar el servidor de origen para Windows Server Essentials migration1
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: f5861ae9-77cb-4d37-b4c5-8f0757213385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bc672a3f2d9b74ff4ed283e826312f6661ba63f6
ms.sourcegitcommit: 04637054de2bfbac66b9c78bad7bf3e7bae5ffb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87838404"
---
# <a name="prepare-your-source-server-for-windows-server-essentials-migration1"></a>Preparar el servidor de origen para Windows Server Essentials migration1

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Complete los siguientes pasos previos para asegurarse de que los datos y la configuración del servidor de origen se migran correctamente al servidor de destino.

#### <a name="to-prepare-for-migration"></a>Para preparar la migración


1.  [Hacer una copia de seguridad del servidor de origen](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)

2.  [Instalar los Service Packs más recientes](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)

3.  [Evaluar el estado del servidor de origen](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)

4.  [Ejecute la herramienta de preparación de la migración en el servidor de origen](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)

5.  [Planear la migración de las aplicaciones de línea de negocio](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)

###  <a name="back-up-your-source-server"></a><a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Hacer una copia de seguridad del servidor de origen
 Haga una copia de seguridad del servidor de origen antes de iniciar el proceso de migración. Esto ayudará a proteger los datos de una pérdida accidental si se produce un error irrecuperable durante la migración.


##### <a name="to-back-up-the-source-server"></a>Para realizar copias de seguridad del servidor de origen

1.  Realice una copia de seguridad completa del servidor de origen. Para obtener más información acerca de las copias de seguridad de Windows Small Business Server 2011 Essentials, vea el tema [Learn more about setting up server backup (Más información acerca de cómo configurar copias de seguridad de servidor)](/previous-versions/windows/it-pro/windows-server-essentials-sbs/ff402413(v=ws.11)).

2.  Compruebe que la copia de seguridad se ejecute correctamente. Para probar la integridad de la copia de seguridad, seleccione archivos aleatorios de la copia de seguridad, restáurelos en una ubicación alternativa y confirme que los archivos restaurados son los mismos que los archivos originales.

###  <a name="install-the-most-recent-service-packs"></a><a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Instalar los Service Packs más recientes
 Debe instalar las actualizaciones y Service Packs más recientes en el servidor de origen antes de la migración.

###  <a name="evaluate-the-health-of-the-source-server"></a><a name="BKMK_UseWindowsBestPracticeAnalyzer"></a>Evaluar el estado del servidor de origen
 Es importante evaluar el estado de su servidor de origen antes de empezar la migración. Use los siguientes procedimientos para asegurarse de que las actualizaciones están actualizadas, para generar un informe de mantenimiento del sistema y para ejecutar el Analizador de procedimientos recomendados (BPA) de Soluciones de Windows Server.

#### <a name="download-and-install-critical-and-security-updates"></a>Descargar e instalar actualizaciones críticas y de seguridad
 Las actualizaciones críticas y de seguridad en el servidor de origen garantizan que la migración se realice correctamente y ayudan a proteger su red durante el proceso.

###### <a name="to-check-for-the-latest-updates"></a>Para comprobar las actualizaciones más recientes

1.  En el servidor de origen, haga clic en **Inicio**, en **Todos los programas** y, a continuación, en **Windows Update**.

2.  Haga clic en **Buscar actualizaciones**.

3.  Si se encuentran actualizaciones, haga clic en **Instalar actualizaciones**.

#### <a name="check-the-alert-viewer-for-critical-errors"></a>Buscar errores críticos en el visor de alertas.
 En el panel, puede buscar errores críticos en el visor de alertas.

#### <a name="run-the-windows-server-solutions-best-practices-analyzer"></a>Ejecutar en Analizador de procedimientos recomendados de Soluciones de Windows Server
 Puede ejecutar el Analizador de procedimientos recomendados (BPA) de Soluciones de Windows Server para comprobar que no haya problemas en el servidor, red o dominio antes de comenzar el proceso de migración. El BPA recopila información de configuración de los siguientes orígenes:

-   Instrumental de administración de Windows de Active Directory &reg; (WMI)

-   El Registro

-   La metabase de Internet Information Services (IIS)

###### <a name="to-use-the-windows-server-solutions-bpa-to-analyze-your-source-server"></a>Para usar el BPA de Soluciones de Windows Server para analizar el servidor de origen

1. Descargue e instale el [analizador de procedimientos recomendados de Soluciones de Windows Server](https://www.microsoft.com/download/details.aspx?id=15556) en el Centro de descarga de Microsoft.

2. Una vez completada la descarga, haga clic en **Inicio**, seleccione **Todos los programas** y haga clic en **Herramienta Analizador de procedimientos recomendados de SBS**.

   > [!NOTE]
   >  Compruebe si hay actualizaciones antes de examinar el servidor.

3. En el panel de navegación, haga clic en **Iniciar un análisis**.

4. En el panel de detalles, escriba la etiqueta del análisis y, a continuación, haga clic en **Iniciar análisis**. La etiqueta del análisis es el nombre del informe de análisis, por ejemplo **SBS BPA Scan 1Jul2012**.

5. Cuando termine el análisis, haga clic en **Ver un informe de este análisis de Best Practices**.

   Después de recopilar información sobre la configuración del servidor, el BPA de Soluciones de Windows Server comprueba que la información sea correcta y, a continuación, presenta a los administradores una lista de información y problemas ordenados por gravedad. En la lista se describe cada problema y se propone una recomendación o posible solución. Hay disponibles tres tipos de informes:

|Tipo de informe|Descripción|
|-----------------|-----------------|
|Informes de lista|Muestra los informes como una lista unidimensional.|
|Informes de árbol|Muestra los informes como una lista jerárquica.|
|Otros informes|Muestra los informes como un registro de tiempo de ejecución.|

 Para ver la descripción y las soluciones para un problema, haga clic en el problema en el informe. No todos los problemas notificados por el BPA de Windows SBS 2011 Essentials afectan a la migración, pero debe resolver todos los problemas posibles para asegurarse de que la migración se realice correctamente.

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

###  <a name="run-the-migration-preparation-tool-on-the-source-server"></a><a name="BKMK_MPT"></a>Ejecutar la herramienta de preparación de la migración en el servidor de origen
 No se puede realizar una instalación en el modo de migración sin haber ejecutado antes la herramienta de preparación de la migración en el servidor de origen. Esta herramienta está diseñada para preparar el servidor de origen y el dominio que se van a migrar a Windows Server Essentials.

> [!IMPORTANT]
>  Haga una copia de seguridad del servidor de origen antes de ejecutar la herramienta de preparación de la migración. Todos los cambios que la herramienta de preparación de la migración efectúa en el esquema son irreversibles. Si tiene problemas durante la migración, la única manera de restablecer el servidor de origen al estado previo es restaurar el servidor desde la copia de seguridad del sistema.

 Para ejecutar la herramienta de preparación de la migración, debe ser miembro de los grupos Administradores de empresa, Administradores de esquema y Administradores de dominio.

##### <a name="to-verify-that-you-have-the-appropriate-permissions-to-run-the-tool-on-the-source-server"></a>Comprobar que tiene los permisos adecuados para ejecutar la herramienta en el servidor de origen

1. En el servidor de origen, haga clic en **Inicio**, **Herramientas administrativas** y, a continuación, haga clic en **Usuarios y equipos de Active Directory**.

2. En el árbol de consola, expanda el dominio y, a continuación, haga clic en **Usuarios**.

3. Haga clic con el botón secundario en la cuenta de administrador que está usando para la migración y, a continuación, en **Propiedades**.

4. Haga clic en la ficha **Miembro de** y, a continuación, compruebe que se muestren los grupos Administradores de empresas, Administradores de esquema y Administradores de dominio en el cuadro de texto **Miembro de**.

5. Si no se muestran, haga clic en **Agregar** y agregue cada grupo que no se encuentre en la lista.

   > [!NOTE]
   > - Es posible que reciba un error de permiso si no se ha iniciado el servicio Netlogon.
   >   -   Debe cerrar la sesión y volver a iniciar sesión en el servidor para que se apliquen los cambios.

    Puede utilizar la versión más reciente del Agente de Windows Update para asegurarse de que el proceso de actualización de servidor funciona correctamente.

   Puede utilizar la versión más reciente del Agente de Windows Update para asegurarse de que el proceso de actualización de servidor funciona correctamente.

   Para instalar el Agente de Windows Update en el servidor de origen, deberá instalar primero Microsoft PowerShell 2.0 y Microsoft Baseline Configuration Analyzer 2.0.

-   Para descargar e instalar PowerShell 2.0, consulte el [artículo 968929](https://go.microsoft.com/fwlink/p/?LinkId=241483) en Microsoft Knowledge Base.

-   Para descargar e instalar Microsoft Baseline Configuration Analyzer 2.0, consulte [Microsoft Baseline Configuration Analyzer 2.0](https://go.microsoft.com/fwlink/p/?LinkId=241484) en el Centro de descarga de Microsoft.

-   Para descargar e instalar la versión más reciente del agente de Windows Update, consulte el [artículo 949104](https://go.microsoft.com/fwlink/p/?LinkId=237493) en Microsoft Knowledge Base.

##### <a name="to-install-and-run-the-migration-preparation-tool-on-the-source-server"></a>Para instalar la herramienta de preparación de la migración en el servidor de origen

1. Inserte Windows Server Essentials DVD1 en la unidad de DVD del servidor de origen.

2. Abra el Explorador de Windows, vaya a la carpeta **\support\tools** del DVD y, a continuación, haga doble clic en el archivo **sourcetool.msi**.

   > [!NOTE]
   > - Si la herramienta de preparación de la migración ya está instalada en el servidor, ejecute la herramienta desde el menú **Inicio**.
   >   -   Para asegurarse de estar preparado para la mejor experiencia de migración posible, se recomienda siempre la instalación de la actualización más reciente.

    El asistente instala la herramienta de preparación de la migración en el servidor de origen. Una vez finalizada la instalación, la herramienta de preparación de la migración se ejecuta automáticamente e instala las actualizaciones más recientes.

3. En la Herramienta de preparación de la migración, haga clic en **Tengo una copia de seguridad y estoy listo para continuar** y luego en **Siguiente**.

   > [!WARNING]
   >  Si recibe un mensaje de error relacionado con la instalación de una revisión, consulte el método 2: cambiar el nombre de la carpeta Catroot2 en el [artículo 822798](https://go.microsoft.com/FWLink/p/?LinkID=118672) de Microsoft Knowledge base.

    La herramienta de preparación de la migración prepara el dominio de origen para la migración ampliando el esquema de Active Directory. Una vez finalizada la tarea, haga clic en **Siguiente** para continuar.

4. Después de preparar el dominio de origen, la herramienta de preparación de la migración analiza el servidor de origen para identificar dos tipos de problemas potenciales.

   - **Errores** de Problemas encontrados en el servidor de origen que pueden bloquear la migración o provocar un error en la migración. Siga las instrucciones que aparecen en el mensaje de error para solucionar los problemas y, a continuación, haga clic en **Detectar de nuevo**.

   - **Advertencias** Problemas encontrados en el servidor de origen que pueden causar problemas funcionales durante la migración. Siga las instrucciones que aparecen en el mensaje de error para solucionar los problemas antes de continuar la migración.

     Después de corregir o reconocer todos los problemas, haga clic en **Siguiente**.

5. En la herramienta de preparación de la migración, haga clic en **Finalizar**.

6. Cuando finalice la herramienta de preparación de la migración, es posible que se le pida que reinicie el servidor de origen para poder empezar a migrar a Windows Server Essentials.

> [!NOTE]
>  Debe completar una ejecución correcta de la herramienta de preparación de la migración en el servidor de origen en un plazo de dos semanas a partir de la instalación de Windows Server Essentials en el servidor de destino. De lo contrario, se bloqueará la instalación de Windows Server Essentials en el servidor de destino. Si esto ocurre, vuelva a instalar la herramienta de preparación de la migración en el servidor de origen.

###  <a name="create-a-plan-to-migrate-line-of-business-applications"></a><a name="BKMK_PlanToMigrateLineOfBusinessApplications"></a>Crear un plan para migrar aplicaciones de línea de negocio
 Una aplicación de línea de negocio (LOB) es una aplicación informática crítica y fundamental para dirigir un negocio. Las aplicaciones LOB incluyen aplicaciones contables, de administración de la cadena de suministro y de planificación de recursos.

 Al planificar la migración de aplicaciones LOB, consulte a los proveedores de aplicaciones LOB para determinar el método adecuado para la migración de cada aplicación. También debe ubicar los medios que se usan para instalar las aplicaciones LOB en el servidor de destino.

> [!NOTE]
>  Si ha usado el SDK de Windows Small Business Server 2011 Essentials para desarrollar un complemento personalizado de alerta o de mantenimiento del sistema y desea seguir usando el complemento con Windows Server Essentials, también debe actualizar el complemento e implementarlo en el servidor de destino.