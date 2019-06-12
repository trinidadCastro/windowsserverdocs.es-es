---
title: 'Paso 4: configuración de directivas de grupo para actualizaciones automáticas'
description: 'Tema de Windows Server Update Service (WSUS): configurar la configuración de directiva de grupo para actualizaciones automáticas es el paso cuatro en un proceso de cuatro pasos para la implementación de WSUS'
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62177d05-d832-4ea8-bca4-47a8cd34a19c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9205565486b75edcd550174fc89990a5aa2d69b7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439851"
---
# <a name="step-4-configure-group-policy-settings-for-automatic-updates"></a>Paso 4: Configuración de directivas de grupo para actualizaciones automáticas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En un entorno de Active Directory, puede usar la directiva de grupo para definir cómo los equipos y usuarios (denominados en este documento los clientes WSUS) pueden interactuar con las actualizaciones de Windows para obtener las actualizaciones automáticas de Windows Server Update Services (WSUS).

Este tema contiene dos secciones principales:

[Configuración de directiva para las actualizaciones de cliente WSUS de grupo](#group-policy-settings-for-wsus-client-updates), que proporciona una guía preceptiva y comportamiento detalles sobre la configuración de Windows Update y programador de mantenimiento de directiva de grupo que controlan cómo los clientes WSUS pueden interactuar con Windows Update Para obtener las actualizaciones automáticas.

[Información complementaria](#supplemental-information) tiene las siguientes secciones:

-   [Obtener acceso a la configuración de Windows Update en la directiva de grupo](#accessing-the-windows-update-settings-in-group-policy), que proporcionan instrucciones generales sobre cómo usar el editor de administración de directivas de grupo e información sobre el acceso a las extensiones de directiva de servicios de actualización y configuración del programador de mantenimiento en Directiva de grupo.

-   [Cambia a WSUS relevantes para esta guía](#changes-to-wsus-relevant-to-this-guide): administradores familiarizados con WSUS 3.2 y versiones anteriores, esta sección ofrece un breve resumen de las diferencias clave entre la versión actual y pasada de WSUS relevantes para esta guía.

-   [Términos y definiciones](#terms-and-definitions): definiciones para varios términos relacionados con WSUS y actualización de los servicios que se usan en esta guía.

## <a name="group-policy-settings-for-wsus-client-updates"></a>Configuración de directiva de grupo para actualizaciones de cliente WSUS
Esta sección proporciona información acerca de tres extensiones de directiva de grupo. En estas extensiones puede encontrar la configuración que puede usar para configurar cómo los clientes WSUS pueden interactuar con Windows Update para recibir actualizaciones automáticas.

-   [equipo configuración &gt; configuración de directiva de actualización de Windows](#computer-configuration--windows-update-policy-settings)

-   [equipo configuración &gt; configuración de directiva de programador de mantenimiento](#computer-configuration--maintenance-scheduler-policy-settings)

-   [Configuración de usuario &gt; configuración de directiva de actualización de Windows](#user-configuration--windows-update-policy-settings)

> [!NOTE]
> En este tema se da por supuesto que ya usa y está familiarizado con la directiva de grupo. Si no está familiarizado con la directiva de grupo, se recomienda que revise la información de la [información complementaria](#supplemental-information) sección de este documento antes de intentar configurar la directiva de WSUS.

### <a name="computer-configuration--windows-update-policy-settings"></a>Configuración del equipo > configuración de directiva de actualización de Windows
En esta sección se proporciona detalles sobre la configuración de directiva de equipo siguiente:

-   [Permitir la instalación inmediata de actualizaciones automáticas](#allow-automatic-updates-immediate-installation)

-   [Permitir que los usuarios reciben notificaciones de actualización](#allow-non-administrators-to-receive-update-notifications)

-   [Permitir actualizaciones firmadas desde una ubicación del servicio Microsoft update de la intranet](#allow-signed-updates-from-an-intranet-microsoft-update-service-location)

-   [Frecuencia de detección de actualizaciones automática](#automatic-updates-detection-frequency)

-   [Configurar actualizaciones automáticas](#configure-automatic-updates)

-   [Retrasar el reinicio para las instalaciones programadas](#delay-restart-for-scheduled-installations)

-   [No ajustar la opción predeterminada para "Instalar actualizaciones y apagar" en el cuadro de diálogo de cierre abajo Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [No mostrar la opción "Instalar actualizaciones y apagar" en el cuadro de diálogo de cierre abajo Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [Habilitar destinatarios del lado cliente](#enable-client-side-targeting)

-   [Habilitar la administración de energía de Windows Update reactivar automáticamente el equipo para instalar las actualizaciones programadas](#enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates)

-   [Instalaciones de actualizaciones no reiniciar automáticamente con usuarios con sesión iniciada para automáticas programadas](#no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations)

-   [Volver a preguntar para reiniciar con instalaciones programadas](#re-prompt-for-restart-with-scheduled-installations)

-   [Volver a programar la instalación programada de actualizaciones automáticas](#reschedule-automatic-updates-scheduled-installations)

-   [Especifique la ubicación del servicio Microsoft update de la intranet](#specify-intranet-microsoft-update-service-location)

-   [Activar las actualizaciones recomendadas mediante actualizaciones automáticas](#turn-on-recommended-updates-via-automatic-updates)

-   [Activar las notificaciones de Software](#turn-on-software-notifications)

En GPME, directivas de actualización de Windows para la configuración basada en el equipo se encuentran en la ruta de acceso: *PolicyName* > **configuración del equipo** > **directivas** > **plantillas administrativas**  >  **Componentes de Windows** > **Windows Update**.

> [!NOTE]
> De forma predeterminada, estas opciones no están configuradas.

#### <a name="allow-automatic-updates-immediate-installation"></a>Permitir la instalación inmediata de actualizaciones automáticas
Especifica si las actualizaciones automáticas se instalará automáticamente las actualizaciones que no interrumpe los servicios de Windows o reiniciar Windows.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Si se establece la configuración de directiva "Configurar actualizaciones automáticas" en **deshabilitado**, esta directiva no tiene ningún efecto.

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica que no se instalan inmediatamente las actualizaciones. Los administradores locales pueden cambiar esta configuración mediante el editor de directivas de grupo Local.|
|**Enabled**|Especifica que las actualizaciones automáticas instala inmediatamente las actualizaciones una vez que se descargaron y están listos instalar.|
|**Deshabilitado**|Especifica que no se instalan inmediatamente las actualizaciones.|

**Opciones:** No hay ninguna opción para esta configuración.

#### <a name="allow-non-administrators-to-receive-update-notifications"></a>Permitir que los usuarios reciben notificaciones de actualización
Especifica si los usuarios no administrativos recibirán notificaciones de actualización según la configuración de directiva Configurar actualizaciones automáticas.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Vea los detalles en la tabla siguiente.|

> [!NOTE]
> Si la configuración de directiva "Configurar actualizaciones automáticas" está deshabilitada o no está configurada, esta configuración de directiva tiene ningún efecto.

> [!IMPORTANT]
> a partir de Windows 8 y Windows RT, esta configuración de directiva está habilitada de forma predeterminada. En todas las versiones anteriores de Windows está deshabilitado de forma predeterminada.

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica que los usuarios siempre se vea una ventana de Control de cuentas y exigen permisos elevados para realizar estas tareas. Un administrador local puede cambiar esta configuración mediante el editor de directivas de grupo Local.|
|**Enabled**|Especifica que la actualización automática de Windows y Microsoft Update incluirá no sean administradores al determinar qué usuario ha iniciado sesión recibirá notificaciones de actualización. Los usuarios no sean administradores podrán instalar todo el contenido de actualización opcional, importantes y recomendadas para la que recibió una notificación. Los usuarios no verán una ventana de Control de cuentas de usuario y no necesitan permisos elevados para instalar estas actualizaciones, excepto en el caso de las actualizaciones que contienen cambios de configuración de la interfaz de usuario, contrato de licencia de usuario final o Windows Update.<br /><br />Existen dos situaciones donde el efecto de esta configuración depende del equipo operativo:<br /><br />1.  **Ocultar** o **restaurar** actualizaciones<br />2.  **Cancelar** una instalación de actualización<br /><br />En Windows Vista o Windows XP, si está habilitada esta configuración de directiva, los usuarios no verán una ventana de Control de cuentas de usuario y no necesitan permisos elevados para ocultar, restaurar o cancelar las actualizaciones.<br /><br />En Windows Vista, si está habilitada esta configuración de directiva, los usuarios no verán una ventana de Control de cuentas de usuario y no necesitan permisos elevados para ocultar, restaurar o cancelar las actualizaciones. Si no está habilitada esta configuración de directiva, los usuarios siempre verán una ventana de Control de cuentas y requieren permisos elevados para ocultar, restaurar o cancelar las actualizaciones.<br /><br />En Windows 7, esta configuración de directiva tiene ningún efecto. Los usuarios siempre verán una ventana de Control de cuentas y requieren permisos elevados para realizar estas tareas.<br /><br />En Windows 8 y Windows RT, esta configuración de directiva tiene ningún efecto.|
|**Deshabilitado**|Especifica que sólo ha iniciado la sesión de los administradores reciban notificaciones de actualización. **Nota:** En Windows 8 y Windows RT, esta configuración de directiva está habilitada de forma predeterminada. En todas las versiones anteriores de Windows está deshabilitado de forma predeterminada.|

**Opciones:** No hay ninguna opción para esta configuración.

#### <a name="allow-signed-updates-from-an-intranet-microsoft-update-service-location"></a>Permitir las actualizaciones firmadas procedentes de una ubicación del servicio en Internet de Microsoft Update
Especifica si las actualizaciones automáticas acepta las actualizaciones que están firmadas por las entidades que no sean de Microsoft cuando la actualización se encuentra en una ubicación del servicio de intranet Microsoft update.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Las actualizaciones de un servicio que no sea un servicio de intranet Microsoft update siempre deben estar firmadas por Microsoft y no se ven afectadas por esta configuración de directiva.

> [!NOTE]
> Esta directiva no se admite en Windows RT Si habilita esta directiva no tendrá ningún efecto en los equipos que ejecutan Windows RT

**Opciones:** No hay ninguna opción para esta configuración.

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica que las actualizaciones desde una ubicación del servicio Microsoft update de la intranet deben estar firmadas por Microsoft.|
|**Enabled**|Especifica que las actualizaciones automáticas acepta actualizaciones recibidas a través de una ubicación del servicio Microsoft update de la intranet si están firmados por un certificado que se encuentra en el almacén de certificados del equipo local "Editores de confianza".|
|**Deshabilitado**|Especifica que las actualizaciones desde una ubicación del servicio Microsoft update de la intranet deben estar firmadas por Microsoft.|

**Opciones:** No hay ninguna opción para esta configuración.

#### <a name="always-automatically-restart-at-the-scheduled-time"></a>Reiniciar automáticamente siempre en la hora programada
Especifica si un temporizador de reinicio se iniciará siempre después de que Windows Update instala las actualizaciones importantes, en lugar de la primera notificación a que los usuarios en la pantalla de inicio de sesión para al menos dos días.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Si está habilitada la configuración de directiva "instalaciones de actualizaciones No reiniciar automáticamente con usuarios con sesión iniciada para automáticas programadas", esta directiva no tiene ningún efecto.

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica que Windows Update no alterará el comportamiento de reinicio del equipo.|
|**Enabled**|Especifica que siempre se iniciará un temporizador de reinicio después de que Windows Update instala las actualizaciones importantes, en lugar de la primera notificación a que los usuarios en la pantalla de inicio de sesión para al menos dos días.<br /><br />El temporizador de reinicio puede estar configurado para iniciarse con cualquier valor desde 15 a 180 minutos. Cuando el temporizador expira, el reinicio continuará incluso si el equipo tiene usuarios con sesión iniciada.|
|**Deshabilitado**|Especifica que Windows Update no alterará el comportamiento de reinicio del equipo.|

**Opciones:** Si habilita esta configuración, puede especificar la cantidad de tiempo que debe transcurrir después de instalar actualizaciones antes de que se produce un reinicio forzado.

#### <a name="automatic-updates-detection-frequency"></a>Frecuencia de detección de las actualizaciones automáticas
Especifica las horas que tardará Windows en determinar cuánto tiempo debe esperar antes de comprobar si hay actualizaciones disponibles. El tiempo de espera exacto se determina mediante el uso de las horas especificadas aquí menos cero hasta el veinte por ciento de las horas especificadas. Por ejemplo, si esta directiva se usa para especificar una frecuencia de detección de 20 horas, todos los clientes a la que se aplica esta directiva buscará actualizaciones en cualquier lugar entre 16 y 20 horas.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> La opción "Especificar la ubicación del servicio Microsoft update la intranet" debe habilitarse para que esta directiva surta efecto.
>
> Si se deshabilita la configuración de directiva "Configurar actualizaciones automáticas", esta directiva no tiene ningún efecto.

> [!NOTE]
> Esta directiva no se admite en Windows RT Si habilita esta directiva no tendrá ningún efecto en los equipos que ejecutan Windows RT

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica que Windows buscará actualizaciones disponibles en el intervalo predeterminado de 22 horas.|
|**Enabled**|Especifica que Windows buscará actualizaciones disponibles en el intervalo especificado.|
|**Deshabilitado**|Especifica que Windows buscará actualizaciones disponibles en el intervalo predeterminado de 22 horas.|

**Opciones:** Si habilita esta configuración, puede especificar el intervalo de tiempo (en horas) que espera a que Windows Update antes de comprobar las actualizaciones.

#### <a name="configure-automatic-updates"></a>Configurar las actualizaciones automáticas
Especifica especificar si las actualizaciones automáticas están habilitadas en este equipo.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

Si está habilitada, debe seleccionar una de las cuatro opciones proporcionadas en esta configuración de directiva de grupo.

Para usar esta configuración, seleccione **habilitado**y, a continuación, en **opciones** en **configurar actualizaciones automáticas**, seleccione una de las opciones (2, 3, 4 o 5).

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica que el uso de las actualizaciones automáticas no se especifica en el nivel de directiva de grupo. Sin embargo, un administrador del equipo todavía puede configurar actualizaciones automáticas en el Panel de Control.|
|**Enabled**|Especifica que Windows reconoce cuando el equipo está en línea y usa su conexión a Internet para buscar en Windows Update para las actualizaciones disponibles.<br /><br />Cuando se habilita, los administradores locales se podrá utilizar el panel de control de Windows Update para seleccionar una opción de configuración de su elección. Sin embargo, los administradores locales no se podrá deshabilitar la configuración para actualizaciones automáticas.<br /><br />-   **2 - notificar descarga y notificar instalación**<br />    Cuando Windows Update busca las actualizaciones que se aplican al equipo, se notificará a los usuarios que las actualizaciones están listas para su descarga. Los usuarios, a continuación, pueden ejecutar Windows Update para descargar e instalar las actualizaciones disponibles.<br />-   **3 - Descargar automáticamente y notificar instalación** (valor predeterminado)<br />    Windows Update busca las actualizaciones aplicables y los descarga en segundo plano; el usuario no es una notificación o se interrumpió durante el proceso. Cuando finalizan las descargas, los usuarios reciben notificaciones de que hay actualizaciones listas para instalar. Los usuarios, a continuación, pueden ejecutar Windows Update para instalar las actualizaciones descargadas.<br />-   **4 - Descargar automáticamente y programar la instalación**<br />    Puede especificar la programación con las opciones en esta configuración de directiva de grupo. Si no se especifica ninguna programación, la programación predeterminada para todas las instalaciones será cada día a las 3:00 A.M. Si alguna actualización requiere un reinicio para completar la instalación, Windows reiniciará automáticamente el equipo. (si un usuario ha iniciado sesión en el equipo cuando esté listo para reiniciar Windows, el usuario se notifica y ofrece la opción de retrasar el reinicio.) **Nota:** a partir de Windows 8, puede establecer las actualizaciones para instalar durante el mantenimiento automático en lugar de usar una programación específica asociada a Windows Update. Mantenimiento automático instalará las actualizaciones cuando el equipo no está en uso y evite la instalación de actualizaciones cuando el equipo se está ejecutando con batería. Si no puede instalar actualizaciones en días mantenimiento automático, Windows Update instalarán las actualizaciones inmediatamente. A continuación, se notificará a los usuarios acerca de un reinicio pendiente. Solo un reinicio pendiente llevará a cabo si no hay ninguna posibilidad de pérdida accidental de datos.    Puede especificar las opciones de programación en la configuración del programador de mantenimiento GPME, que se encuentra en la ruta de acceso, *PolicyName* > **equipo configuración**  >  **Directivas** > **plantillas administrativas** > **componentes de Windows** > **mantenimiento Programador** > **límite de activación de mantenimiento automático**. Consulte la sección de esta referencia titulada: [Configuración del programador de mantenimiento](#computer-configuration--maintenance-scheduler-policy-settings), para establecer los detalles.    **5 - Permitir que el administrador local elija la opción**<br />: Especifica si los administradores locales pueden usar el panel de control de las actualizaciones automáticas para seleccionar una opción de configuración de su elección, por ejemplo, si los administradores locales pueden elegir una hora de instalación programada.<br />    Los administradores locales no podrán deshabilitar la configuración de las actualizaciones automáticas.|
|**Deshabilitado**|Especifica que las actualizaciones de cliente que están disponibles en el servicio de actualización de Windows público deben ser descargadas de Internet manualmente y se instalado.|

#### <a name="delay-restart-for-scheduled-installations"></a>Retrasar el reinicio para las instalaciones programadas
Especifica la cantidad de tiempo que las actualizaciones automáticas esperará antes de continuar con un reinicio programado.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta directiva se aplica solo cuando actualizaciones automáticas está configurada para realizar instalaciones o actualizaciones programadas. Si se deshabilita la configuración de directiva "Configurar actualizaciones automáticas", esta directiva no tiene ningún efecto.

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica que después de instalar actualizaciones, el tiempo de espera predeterminado de quince minutos ha de transcurrir antes de que se produce cualquier reinicio programado.|
|**Enabled**|Especifica que cuando finalice la instalación, se producirá un reinicio programado una vez transcurrido el número especificado de minutos.|
|**Deshabilitado**|Especifica que después de instalar actualizaciones, el tiempo de espera predeterminado de quince minutos ha de transcurrir antes de que se produce cualquier reinicio programado.|

**Opciones:** Si habilita esta configuración, puede usar esta opción para especificar la cantidad de tiempo (en minutos) de actualizaciones automáticas espera antes de continuar con un reinicio programado.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog"></a>No ajustar la opción predeterminada para instalar actualizaciones y apagar en el cuadro de diálogo de cierre abajo Windows
Esta configuración de directiva le permite especificar si el **instalar actualizaciones y apagar** opción se permite como la opción predeterminada en el **apagar abajo Windows** cuadro de diálogo.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta configuración de directiva no tendrá ningún efecto si la *PolicyName* > **equipo configuración** > **directivas**  >  **Plantillas administrativas** > **componentes de Windows** > **Windows Update** > **no mostrar " Opción de instalar actualizaciones y apagar"en el cuadro de diálogo de cierre abajo Windows** está habilitada.

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica que **instalar actualizaciones y apagar** será la opción predeterminada en el **apagar abajo Windows** cuadro de diálogo si hay actualizaciones disponibles para la instalación en el momento en que el usuario selecciona la opción Apagar en apagar el equipo.|
|**Enabled**|Si habilita esta configuración de directiva, última opción de cierre del usuario en la (por ejemplo, hibernación o reinicio), es la opción predeterminada en el **apagar abajo Windows** cuadro de diálogo, independientemente de si la **instalar actualizaciones y apagar**  opción está disponible en el **¿qué desea hacer?** menú.|
|**Deshabilitado**|Especifica que **instalar actualizaciones y apagar** será la opción predeterminada en el **apagar abajo Windows** cuadro de diálogo si hay actualizaciones disponibles para la instalación en el momento en que el usuario selecciona la opción Apagar en apagar el equipo.|

**Opciones:** No hay ninguna opción para esta configuración.

#### <a name="do-not-connect-to-any-windows-update-internet-locations"></a>No conectarse a ninguna ubicación de Internet de Windows Update
Incluso cuando la actualización de Windows está configurado para recibir actualizaciones de un servicio de actualización de la intranet, se recuperarán periódicamente información desde el servicio de actualización de Windows público para habilitar las conexiones futuras para actualizar de Windows y otros servicios, como Microsoft Update o Microsoft Store.

Si habilita esta directiva se deshabilitará la funcionalidad para recuperar periódicamente la información de la versión pública de Windows Server Update Services y puede hacer que la conexión a servicios públicos, como Microsoft Store para dejar de funcionar.

|Compatible con:|Excluir:|
|---------|-------|
|a partir de Windows Server 2012 R2, Windows 8.1 o Windows RT 8.1, sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta directiva aplica solo cuando el equipo está configurado para conectarse a un servicio de actualización de la intranet mediante el uso de la configuración de directiva "Especificar intranet Microsoft update ubicación del servicio".

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Conserva el comportamiento predeterminado para recuperar información de la versión pública de Windows Server Update Services.|
|**Enabled**|Especifica que los equipos no recuperar información de la versión pública de Windows Server Update Services.|
|**Deshabilitado**|Conserva el comportamiento predeterminado para recuperar información de la versión pública de Windows Server Update Services.|

**Opciones:** No hay ninguna opción para esta configuración.

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog"></a>No mostrar instalar actualizaciones y la opción Apagar en el cuadro de diálogo de cierre abajo Windows
Especifica si el **instalar actualizaciones y apagar** opción se muestra en el **apagar abajo Windows** cuadro de diálogo.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica que el **instalar actualizaciones y apagar** opción está disponible en el **apagar abajo Windows** cuadro de diálogo si hay actualizaciones disponibles cuando el usuario selecciona la opción Apagar para apagar el equipo. Un administrador local puede cambiar esta configuración mediante la directiva local.|
|**Enabled**|Especifica que **instalar actualizaciones y apagar** no aparecerá como una opción en el **apagar abajo Windows** cuadro de diálogo, incluso si hay actualizaciones disponibles para la instalación cuando el usuario selecciona la opción Apagar en apagar el equipo.|
|**Deshabilitado**|Especifica que el **instalar actualizaciones y apagar** opción será la opción predeterminada en el **apagar abajo Windows** cuadro de diálogo si hay actualizaciones disponibles para la instalación en el momento en que el usuario selecciona el apagar opción para apagar el equipo.|

**Opciones:** No hay ninguna opción para esta configuración.

#### <a name="enable-client-side-targeting"></a>Habilitar la orientación de cliente
Especifica el nombre del grupo de destino o nombres que se configuran en la consola de WSUS que van a recibir actualizaciones de WSUS.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Esta directiva se aplica solo cuando el equipo está configurado para admitir los nombres de grupo de destino especificado en WSUS. Si el nombre del grupo de destino no existe en WSUS, se omitirá hasta que se crea. Si la configuración de directiva "Especificar la ubicación del servicio Microsoft update la intranet" está deshabilitada o no configurada, esta directiva no tiene ningún efecto.

> [!NOTE]
> Esta directiva no se admite en Windows RT Si habilita esta directiva no tendrá ningún efecto en los equipos que ejecutan Windows RT

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica que no se envía ninguna información de grupo de destino a WSUS. Un administrador local puede cambiar esta configuración mediante una directiva local.|
|**Enabled**|Especifica que se envía la información del grupo de destino especificado a WSUS, que se usa para determinar qué actualizaciones deben implementarse en este equipo. Si WSUS es compatible con varios grupos de destino, puede usar esta directiva para especificar varios nombres de grupo, separados por punto y coma, si ha agregado los nombres de grupo de destino en la lista de grupos de equipos en WSUS. De lo contrario, se debe especificar un solo grupo.|
|**Deshabilitado**|Especifica que no se envía ninguna información de grupo de destino a WSUS.|

**Opciones:** Use este espacio para especificar uno o más nombres de grupo de destino.

#### <a name="enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates"></a>Habilitar la administración de energía de Windows Update reactivar automáticamente el equipo para instalar las actualizaciones programadas
Especifica si Windows Update usará las características de administración de energía de Windows o las opciones de energía para reactivar el equipo desde la hibernación automáticamente si hay actualizaciones programadas para su instalación.

El equipo se activará automáticamente solo si Windows Update está configurado para instalar actualizaciones automáticamente. Si el equipo está en hibernación cuando se produce la hora de instalación programada y hay aplicar actualizaciones, Windows Update usará las características de administración de energía de Windows o las opciones de energía para reactivar automáticamente el equipo para instalar las actualizaciones. Actualizar Windows también se reactivar el equipo e instalar una actualización si se produce una fecha límite de instalación.

El equipo no se reactivará a menos que haya actualizaciones para instalarse. Si el equipo está en alimentación por batería, cuando Windows Update se reactiva, no instalará las actualizaciones y el equipo volverá automáticamente a modo de hibernación en dos minutos.

|Compatible con:|Excluir:|
|---------|-------|
|a partir de Windows Vista y Windows Server 2008 (Windows 7), sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Windows Update no se reactiva el equipo desde la hibernación para instalar las actualizaciones. Un administrador local puede cambiar esta configuración mediante una directiva local.|
|**Enabled**|Actualización de Windows se reactiva el equipo en hibernación para instalar las actualizaciones en las condiciones enumeradas anteriormente.|
|**Deshabilitado**|Windows Update no se reactiva el equipo desde la hibernación para instalar las actualizaciones.|

**Opciones:** No hay ninguna opción para esta configuración.

#### <a name="no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations"></a>No reiniciar automáticamente para instalar actualizaciones automáticas programadas con usuarios que hayan iniciado sesión
Especifica que, para completar una instalación programada, actualizaciones automáticas esperará el equipo se reinicie por cualquier usuario que ha iniciado sesión, en lugar de producir el equipo se reinicie automáticamente.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta directiva se aplica solo cuando actualizaciones automáticas está configurada para realizar instalaciones o actualizaciones programadas. Si se deshabilita la configuración de directiva "Configurar actualizaciones automáticas", esta directiva no tiene ningún efecto.

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica que las actualizaciones automáticas notificará al usuario que el equipo se reiniciará automáticamente en cinco minutos para completar la instalación.|
|**Enabled**|Algunas actualizaciones requieren que el equipo se reinicie antes de que las actualizaciones surtan efecto. Si el estado se establece en habilitado, las actualizaciones automáticas no se reiniciará un equipo automáticamente durante una instalación programada si un usuario ha iniciado sesión en el equipo. En su lugar, las actualizaciones automáticas notificará al usuario que reinicie el equipo.|
|**Deshabilitado**|Especifica que las actualizaciones automáticas notificará al usuario que el equipo se reiniciará automáticamente en cinco minutos para completar la instalación.|

**Opciones:** No hay ninguna opción para esta configuración.

#### <a name="re-prompt-for-restart-with-scheduled-installations"></a>Volver a pedir la intervención del usuario para reiniciar con instalaciones programadas
Especifica la cantidad de tiempo para las actualizaciones automáticas debe esperar antes de volver a preguntar con un reinicio programado.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!IMPORTANT]
> Esta directiva se aplica solo cuando actualizaciones automáticas está configurada para realizar instalaciones o actualizaciones programadas. Si se deshabilita la configuración de directiva "Configurar actualizaciones automáticas", esta directiva no tiene ningún efecto.

> [!NOTE]
> Esta directiva no tiene ningún efecto en los equipos que ejecutan Windows RT

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Ocurre un reinicio programado diez minutos después de reiniciar el símbolo del sistema de mensaje se descarta. Un administrador local puede cambiar esta configuración mediante una directiva local.|
|**Enabled**|Especifica que después se pospuso el mensaje anterior para el reinicio, se producirá un reinicio programado después del número especificado de transcurre minutos.|
|**Deshabilitado**|Ocurre un reinicio programado diez minutos después de reiniciar el símbolo del sistema de mensaje se descarta.|

**Opciones:** Cuando se habilita, puede usar esta opción de configuración para especificar (en minutos) de la duración de tiempo que transcurrirá antes de que los usuarios se pregunte si desea volver a un reinicio programado.

#### <a name="reschedule-automatic-updates-scheduled-installations"></a>Volver a programar las instalaciones programadas de actualizaciones automáticas
Especifica la cantidad de tiempo para las actualizaciones automáticas debe esperar después de un inicio del equipo, antes de continuar con una instalación programada que previamente se ha perdido.

Si el estado se establece en **no configurado**, se producirá una instalación programada no ejecutada inició un minuto después de que el equipo es el siguiente.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta directiva se aplica solo cuando actualizaciones automáticas está configurada para realizar instalaciones o actualizaciones programadas. Si se deshabilita la configuración de directiva "Configurar actualizaciones automáticas", esta directiva no tiene ningún efecto.

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica que las instalaciones programadas que se producen inició un minuto después de que el equipo es el siguiente paso.|
|**Enabled**|Especifica que una instalación programada que no tuvo lugar anteriormente producirá el número especificado de minutos después de iniciarse el equipo.|
|**Deshabilitado**|Especifica que las instalaciones programadas que se producen con la siguiente instalación programada.|

**Opciones:** Cuando se habilita esta configuración de directiva, puede usarlo para especificar un número de minutos después de que el equipo se inicia a continuación, que una instalación programada que no tuvo lugar anteriormente, se producirá.

#### <a name="specify-intranet-microsoft-update-service-location"></a>Especifique la ubicación del servicio Microsoft update de la intranet
Especificar un servidor de intranet alternativo para las actualizaciones de host de Microsoft Update. A continuación, puede utilizar WSUS para actualizar automáticamente los equipos de la red.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT.|

Esta configuración le permite especificar un servidor WSUS en la red que funcionará como un servicio de actualización interno. En lugar de usar Microsoft Updates en Internet, los clientes WSUS buscará en este servicio para las actualizaciones aplicables.

Para usar esta configuración, debe establecer dos valores de nombre de servidor: el servidor desde el que el cliente detecta y descarga las actualizaciones y el servidor al que las estaciones de trabajo actualizadas cargan las estadísticas. Los valores no deben ser diferentes si ambos servicios están configurados en el mismo servidor.

> [!NOTE]
> Si se deshabilita la configuración de directiva "Configurar actualizaciones automáticas", esta directiva no tiene ningún efecto.

> [!NOTE]
> Esta directiva no se admite en Windows RT Si habilita esta directiva no tendrá ningún efecto en los equipos que ejecutan Windows RT

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Si actualizaciones automáticas no están deshabilitadas por preferencias de usuario o de directiva, esta directiva especifica que los clientes se conectan directamente al sitio de Windows Update en Internet.|
|**Enabled**|Especifica que el cliente se conecta al servidor WSUS especificado, en lugar de Windows Update para buscar y descargar actualizaciones. Si habilita a esta configuración significa que los usuarios finales de su organización no tiene que pasar por un firewall para obtener las actualizaciones, y ofrece la oportunidad de probar las actualizaciones antes de implementarlas.|
|**Deshabilitado**|Si actualizaciones automáticas no están deshabilitadas por preferencias de usuario o de directiva, esta directiva especifica que los clientes se conectan directamente al sitio de Windows Update en Internet.|

**Opciones:** Cuando se habilita esta configuración de directiva, debe especificar el servicio de actualización de la intranet que va a usar los clientes WSUS al detectar las actualizaciones y el servidor de estadísticas de Internet a la que actualiza a los clientes WSUS cargará las estadísticas. Valores de ejemplo:


|                    Opción de configuración:                    |    Valor de ejemplo:    |
|-------------------------------------------------------|----------------------|
| Establecer el servicio de actualización de la intranet para detectar actualizaciones |  http://wsus01:8530  |
|          Establecer el servidor de estadísticas de la intranet           | http://IntranetUpd01 |

#### <a name="turn-on-recommended-updates-via-automatic-updates"></a>Activar las actualizaciones recomendadas mediante actualizaciones automáticas
Especifica si las actualizaciones automáticas ofrecerá importante y recomienda actualizaciones de WSUS.

|Compatible con:|Excluir:|
|---------|-------|
|a partir de Windows Vista, sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica que las actualizaciones automáticas continuará entregar las actualizaciones importantes si ya está configurado para ello.|
|**Enabled**|Especifica que las actualizaciones automáticas se instalará las actualizaciones recomendadas y las actualizaciones importantes de WSUS.|
|**Deshabilitado**|Especifica que las actualizaciones automáticas continuará entregar las actualizaciones importantes si ya está configurado para ello.|

**Opciones:** No hay ninguna opción para esta configuración.

#### <a name="turn-on-software-notifications"></a>Activar las notificaciones de Software
Esta configuración de directiva le permite controlar si los usuarios ven mensajes de notificación mejorada detallada sobre el software desde el servicio Microsoft Update. Los mensajes de notificación mejorada transmiten el valor y promoción la instalación y uso de software opcionales. Esta configuración de directiva está pensada para su uso en entornos escasamente administrados en los que el acceso del usuario final para el servicio Microsoft Update.

Si no usa el servicio Microsoft Update, la configuración de directiva "Notificaciones de Software" tiene ningún efecto.

Si la configuración de directiva "Configurar actualizaciones automáticas" está deshabilitada o no está configurada, la configuración de directiva "Notificaciones de Software" tiene ningún efecto.

|Compatible con:|Excluir:|
|---------|-------|
|a partir de Windows Server 2008 (Windows Vista) y Windows 7, sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> De manera predeterminada, esta configuración de directiva está deshabilitada.

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Los usuarios de equipos que ejecutan Windows 7 no se ofrecen los mensajes para las aplicaciones opcionales. Los usuarios de equipos que ejecutan Windows Vista no se ofrecen los mensajes para las aplicaciones opcionales o actualizaciones. Un administrador local puede cambiar esta configuración mediante el Panel de Control o una directiva local.|
|**Enabled**|Si habilita a esta configuración de directiva, un mensaje de notificación aparecerá en el equipo del usuario cuando destacados de software está disponible. El usuario puede pulsar la notificación para abrir Windows Update y obtener más información acerca del software o instalarlo. El usuario también puede hacer clic **cierre este mensaje** o **Mostrarme más adelante** para aplazar la notificación según corresponda.<br /><br />En Windows 7, esta configuración de directiva solo controlará las notificaciones detalladas para las aplicaciones opcionales. En Windows Vista, esta configuración de directiva controla notificaciones detalladas para las aplicaciones opcionales y las actualizaciones.|
|**Deshabilitado**|Especifica que los usuarios que ejecutan Windows 7 no se ofrecerán los mensajes de notificación detallada para las aplicaciones opcionales y los usuarios que ejecutan Windows Vista no se ofrecerán mensajes de notificación detallada para las aplicaciones opcionales o actualizaciones opcionales.|

**Opciones:** No hay ninguna opción para esta configuración.

### <a name="computer-configuration--maintenance-scheduler-policy-settings"></a>Configuración del equipo > configuración de directiva de programador de mantenimiento
En la configuración de configurar actualizaciones automáticas, seleccionó la opción **4 - Descargar automáticamente y programar la instalación**, puede especificar programar la configuración del programador de mantenimiento en la GPMC de equipos que ejecutan Windows 8 y Windows RT Si no ha seleccionado la opción 4 en la configuración "Configurar actualizaciones automáticas", no es necesario configurar estas opciones con el fin de las actualizaciones automáticas. Configuración del programador de mantenimiento se encuentra en la ruta de acceso: *PolicyName* > **equipo configuración** > **directivas** > **plantillas administrativas**  >  **Componentes de Windows** > **mantenimiento programador**. La extensión del programador de mantenimiento de directiva de grupo contiene las siguientes opciones:

-   [Límite de activación de mantenimiento automático](#automatic-maintenance-activation-boundary)

-   [Retraso aleatorio de mantenimiento automático](#automatic-maintenance-random-delay)

-   [Directiva de activación automática](#automatic-wakeup-policy)

#### <a name="automatic-maintenance-activation-boundary"></a>Límite de activación del Mantenimiento automático
Esta directiva permite configurar la opción "Límite de activación de mantenimiento automático".

El límite de activación de mantenimiento es la hora programada diariamente a la que se inicia el mantenimiento automático.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta configuración está relacionada con la opción 4 en **configurar actualizaciones automáticas**. Si no ha seleccionado la opción 4 en **configurar actualizaciones automáticas**, no es necesario configurar esta opción.

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Si no se configura esta configuración de directiva, el diario programado tiempo tal como se especifica en los equipos cliente en el **centro de actividades** > **mantenimiento automático** Panel de Control se aplicará.|
|**Enabled**|Si habilita esta configuración de directiva invalida cualquier valor predeterminado o modificar las opciones configuradas en los equipos cliente de **Panel de Control** > **centro de actividades**  >   **Mantenimiento automático** (o, en algunas versiones de cliente, **mantenimiento**).|
|**Deshabilitado**|Si establece esta configuración de directiva en **deshabilitado**, la hora diaria programada como se especifica en el **centro de actividades** > **mantenimiento automático**, en el Control Se aplicará el panel.|

#### <a name="automatic-maintenance-random-delay"></a>Retraso aleatorio de mantenimiento automático
Esta configuración de directiva permite configurar el retraso aleatorio de mantenimiento automático activación.

El retraso aleatorio de mantenimiento es la cantidad de tiempo hasta que el mantenimiento automático, retrasará a partir de su límite de activación. Esta configuración es útil para las máquinas virtuales donde mantenimiento aleatorio puede ser un requisito de rendimiento.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta configuración está relacionada con la opción 4 en **configurar actualizaciones automáticas**. Si no ha seleccionado la opción 4 en **configurar actualizaciones automáticas**, no es necesario configurar esta opción.

De forma predeterminada, cuando se habilita, se establece el retraso aleatorio de mantenimiento regular en **PT4H**.

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Se aplica un retraso aleatorio de cuatro horas a **automática**.|
|**Enabled**|Mantenimiento automático, retrasará a partir de su límite de activación por hasta el período de tiempo especificado.|
|**Deshabilitado**|Mantenimiento automático no se aplica ningún retraso aleatorio.|

#### <a name="automatic-wakeup-policy"></a>Directiva de activación automática
Esta configuración de directiva permite configurar la directiva de mantenimiento automático reactivación.

La directiva de mantenimiento de reactivación especifica si el mantenimiento automático debe realizar una solicitud de reactivación al equipo funcionamiento diario mantenimiento programado.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Si el equipo de funcionamiento del power wake directiva está deshabilitada de forma explícita, esta configuración no tiene ningún efecto.

> [!NOTE]
> Esta configuración está relacionada con la opción 4 en **configurar actualizaciones automáticas**. Si no ha seleccionado la opción 4 en **configurar actualizaciones automáticas**, no es necesario configurar esta opción.

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Si no configura esta configuración de directiva, la configuración como reactivación especificado en el **centro de actividades** > **mantenimiento automático** Panel de Control se aplicará.|
|**Enabled**|Si habilita a esta configuración de directiva, mantenimiento automático intentará establecer una directiva de reactivación del sistema operativo y realiza una solicitud de reactivación para el momento programado diario, si es necesario.|
|**Deshabilitado**|Si deshabilita esta configuración de directiva, la configuración como reactivación especificado en el **centro de actividades** > **mantenimiento automático** Panel de Control se aplicará.|

### <a name="user-configuration--windows-update-policy-settings"></a>Configuración de usuario > configuración de directiva de actualización de Windows
En esta sección se proporciona detalles sobre la configuración de directiva basada en usuario siguientes:

-   [No mostrar la opción "Instalar actualizaciones y apagar" en el cuadro de diálogo Cerrar abajo Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [No ajustar la opción predeterminada para "Instalar actualizaciones y apagar" en el cuadro de diálogo Cerrar abajo Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [quitar el acceso para usar todas las características de Windows Update](#remove-access-to-use-all-windows-update-features)

En GPMC, la configuración del usuario para las actualizaciones automáticas del equipo se encuentra en la ruta de acceso: *PolicyName* > **configuración de usuario** > **directivas** > **plantillas administrativas**  >  **Componentes de Windows** > **Windows Update**. La configuración se muestra en el mismo orden en que aparecen en la configuración del equipo y las extensiones de configuración de usuario en la directiva de grupo, cuando el **configuración** está seleccionada la pestaña de la directiva de actualización de Windows para ordenar la configuración orden alfabético.

> [!NOTE]
> De forma predeterminada, a menos que se indique lo contrario, estas opciones no están configuradas.

> [!TIP]
> para cada una de estas opciones, puede usar los pasos siguientes para habilitar, deshabilitar o navegar entre la configuración:

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog-box"></a>No mostrar la opción "Instalar actualizaciones y apagar" en el cuadro de diálogo Cerrar abajo Windows
Especifica si el **instalar actualizaciones y apagar** opción se muestra en el **apagar abajo Windows** cuadro de diálogo.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica que el **instalar actualizaciones y apagar** opción volverá a aparecer en el **apagar abajo Windows** cuadro de diálogo si hay actualizaciones disponibles cuando el usuario selecciona la opción Apagar para apagar el equipo.|
|**Enabled**|Si habilita esta configuración de directiva, **instalar actualizaciones y apagar** no aparecerá como una opción en el **apagar abajo Windows** cuadro de diálogo, incluso si hay actualizaciones disponibles para la instalación cuando el usuario selecciona el Opción Apagar para apagar el equipo.|
|**Deshabilitado**|Especifica que el **instalar actualizaciones y apagar** opción volverá a aparecer en el **apagar abajo Windows** cuadro de diálogo si hay actualizaciones disponibles cuando el usuario selecciona la opción Apagar para apagar el equipo.|

**Opciones:** No hay ninguna opción para esta configuración.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog-box"></a>No ajustar la opción predeterminada para "Instalar actualizaciones y apagar" en el cuadro de diálogo Cerrar abajo Windows
Especifica si el **instalar actualizaciones y apagar** se permite la opción como la opción predeterminada en el **apagar abajo Windows** cuadro de diálogo.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta configuración de directiva no tendrá ningún efecto si la *PolicyName* > **configuración de usuario** > **directivas**  >   **Plantillas administrativas** > **componentes de Windows** > **Windows Update** > **no mostrar " Opción de instalar actualizaciones y apagar"en el cuadro de diálogo Cerrar abajo Windows** está habilitado.

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Especifica si el **instalar actualizaciones y apagar** opción será la opción predeterminada en el **apagar abajo Windows** cuadro de diálogo si hay actualizaciones disponibles para la instalación en el momento en que el usuario selecciona el apagado Opción para apagar el equipo.|
|**Enabled**|Especifica si última opción de cierre del usuario en la (por ejemplo, hibernación o reiniciar) es la opción predeterminada en el **apagar abajo Windows** cuadro de diálogo, independientemente de si el **opción instalar actualizaciones y apagar** está disponible en el **¿qué desea hacer?** menú.|
|**Deshabilitado**|Especifica si el **instalar actualizaciones y apagar** opción será la opción predeterminada en el **apagar abajo Windows** cuadro de diálogo si hay actualizaciones disponibles para la instalación en el momento en que el usuario selecciona el apagado Opción para apagar el equipo.|

**Opciones:** No hay ninguna opción para esta configuración.

#### <a name="remove-access-to-use-all-windows-update-features"></a>Desactivar el acceso al uso de todas las características de Windows Update
Esta configuración le permite quitar el acceso de cliente WSUS en Windows Update.

|Compatible con:|Excluir:|
|---------|-------|
|Sistemas operativos de Windows que están aún dentro su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de la configuración de directiva**|**Comportamiento**|
|**No configurado**|Los usuarios pueden conectarse al sitio Web de Windows Update.|
|**Enabled**|**Importante:** si habilitar, todas las características de Windows Update están quitado. Esto incluye el acceso al sitio Web de Windows Update en https://windowsupdate.microsoft.com, desde el hipervínculo de Windows Update en la pantalla de inicio o el menú Inicio y también en el **herramientas** menús en Internet Explorer. También se deshabilitan actualizaciones automáticas de Windows; el usuario no le notificará sobre ni recibir actualizaciones críticas de Windows Update. Esta configuración también evita que el Administrador de dispositivos de la instalación automática de actualizaciones de controladores desde el sitio Web de Windows Update.<br /><br />Cuando se habilita, puede configurar una de las siguientes opciones de notificación:<br /><br />-   **0 - no se muestran las notificaciones**<br />    Esta configuración se quitará todo el acceso a características de Windows Update y no se mostrará ninguna notificación.<br />-   **1 - Mostrar reiniciar las notificaciones necesarias**<br />    Esta opción mostrará notificaciones acerca de los reinicios necesarios para completar la instalación. **Nota:** En los equipos que ejecutan Windows 8 y Windows RT, si esta directiva está habilitada, solo las notificaciones relacionadas con reinicios y a la incapacidad para detectar actualizaciones aparecerá. No se admiten las opciones de notificación. Siempre se muestran las notificaciones en la pantalla de inicio de sesión.|
|**Deshabilitado**|Los usuarios pueden conectarse al sitio Web de Windows Update.|

**Opciones:** Consulte **habilitado** en la tabla para esta configuración.

## <a name="supplemental-information"></a>Información complementaria
Esta sección proporciona información adicional sobre el uso de abrir y guardar la configuración de WSUS en las directivas de grupo y las definiciones de términos usados en esta guía. Para los administradores familiarizados con las versiones anteriores de WSUS (WSUS 3.2 y versiones anteriores), hay una tabla que resume brevemente las diferencias entre las versiones WSUS.

### <a name="accessing-the-windows-update-settings-in-group-policy"></a>Obtener acceso a la configuración de Windows Update en la directiva de grupo
El siguiente procedimiento describe cómo abrir la consola de GPMC en el controlador de dominio. El procedimiento a continuación, describe cómo abrir un existente objeto de directiva de grupo (GPO) nivel de dominio para la edición, o crear un nuevo GPO de nivel de dominio y abrirlo para su edición.

> [!NOTE]
> Debe ser un miembro de la **Admins. del dominio** o un grupo equivalente, llevar a cabo este procedimiento.

##### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Para abrir o agregar y abrir un objeto de directiva de grupo

1.  En el controlador de dominio, vaya a **administrador del servidor**, > **herramientas**, > **Group Policy Management**. Se abre la consola de administración de directivas de grupo.

2.  En el panel izquierdo, expanda el bosque. Por ejemplo, haga doble clic en **bosque: ejemplo.com**.

3.  En el panel izquierdo, haga doble clic en **dominios**y, a continuación, haga doble clic en el dominio para el que desea administrar un objeto de directiva de grupo. Por ejemplo, haga doble clic en **ejemplo.com**.

4.  Realiza una de las siguientes acciones:

    -  **Para abrir un GPO de nivel de dominio existente para editar**, haga doble clic en el dominio que contiene el objeto de directiva de grupo que desea administrar, haga clic en la directiva de dominio que desea administrar y, a continuación, haga clic en **editar**. Se abre el editor de administración de directivas de grupo (GPME).

    -  **Para crear un nuevo objeto de directiva de grupo y abrir para editar**:
        1.  Haga clic en el dominio para el que desea crear un nuevo objeto de directiva de grupo y, a continuación, haga clic en **crear un GPO en este dominio y vincularlo aquí**.

        2.  En **nuevo GPO**, en **nombre**, escriba un nombre para el nuevo objeto de directiva de grupo y, a continuación, haga clic en **Aceptar**.

        3.  Haga clic en el nuevo objeto de directiva de grupo y, a continuación, haga clic en **editar**. Se abre GPME.

##### <a name="to-open-the-windows-update-or-maintenance-scheduler-extensions-of-group-policy"></a>Para abrir las extensiones de Windows Update o el programador de mantenimiento de directiva de grupo

1.  En el editor de administración de directivas de grupo, realice una de las siguientes acciones:

    -   **Abra la configuración del equipo > extensión de Windows Update de la directiva de grupo**. Vaya a: *PolicyName* > **equipo configuración** > **directivas** / **plantillas administrativas**  >  **Componentes de Windows** > **Windows Update**.

    -   **Abra la configuración de usuario > extensión de Windows Update de la directiva de grupo**. Vaya a: *PolicyName* > **configuración de usuario** > **directivas** > **plantillas administrativas**  >  **Componentes de Windows** > **Windows Update**.

    -   **Abra la configuración del equipo > extensión de programador de mantenimiento de directiva de grupo**. En el GPOE, vaya a *PolicyName* > **equipo configuración** > **directivas** > **administrativo Plantillas de** > **componentes de Windows** > **mantenimiento programador**.

Para obtener más información acerca de la directiva de grupo, consulte [Group Policy Overview](https://technet.microsoft.com/library/hh831791.aspx(v=ws.12)).

> [!TIP]
> Después de haber abierto la extensión de directiva de grupo que desea, puede usar los pasos siguientes para habilitar, deshabilitar o navegar entre la configuración:

##### <a name="to-configure-group-policy-settings"></a>Para definir la configuración de directiva de grupo

1.  En *ExtensionOfGroupPolicy*, haga doble clic en la configuración que desea ver o modificar.

2.  Para definir la configuración, realice una de las siguientes acciones:

    -   Para conservar el valor predeterminado no especifica el estado de la configuración, seleccione **no configurado**.

    -   Para habilitar la configuración, seleccione **habilitado**.

    -   Para deshabilitar la configuración, seleccione **deshabilitado**.

3.  En **opciones**, si se muestran las opciones, conserve los valores predeterminados o modificarlas según sea necesario.

4.  Realiza una de las siguientes acciones:

    -   Para guardar los cambios y continúe con la siguiente opción de configuración, haga clic en **aplicar**y, a continuación, haga clic en **la siguiente configuración**.

    -   Para guardar los cambios y cerrar el cuadro de diálogo, haga clic en **Aceptar**.

    -   Para descartar todos los cambios sin guardar y cerrar el cuadro de diálogo, haga clic en **cancelar**.

### <a name="changes-to-wsus-relevant-to-this-guide"></a>Cambios realizados en WSUS relevantes para esta guía
En la tabla siguiente se resume las diferencias clave entre las versiones actuales y pasadas de WSUS que son relevantes para esta guía.

|Versiones de Windows Server y WSUS|Descripción|
|------------------|--------|
| Windows Server 2012 R2 con WSUS 6.0 y versiones posteriores|a partir de Windows Server 2012, el rol de servidor WSUS está integrado con el sistema operativo y la configuración de directiva de grupo asociada para los clientes WSUS es, de forma predeterminada, incluido en la directiva de grupo.|
| Windows Server 2008 (y versiones anteriores de Windows Server) con WSUS 3.2 y versiones anteriores|En Windows Server 2008 (y versiones anteriores de Windows Server) con WSUS 3.2 (y versiones anteriores), la configuración de directiva de grupo que rigen a los clientes WSUS no se incluye en estos sistemas operativos de Windows Server. La configuración de directiva se encuentran en la plantilla administrativa de WSUS, **wuau.adm**. En estas versiones de servidor, la plantilla administrativa de WSUS debe agregar primero en la consola de administración de directivas de grupo (GPMC) para poder configurar las opciones de cliente WSUS.|

### <a name="terms-and-definitions"></a>Términos y definiciones
Siguiente es una lista de los términos usados en esta guía.

|Término|Definición|
|----|-------|
|Actualizaciones automáticas|**Un servicio que se ejecuta en equipos Windows** (actualizaciones automáticas): Hace referencia al componente de equipo cliente creado en el Microsoft Windows Vista, Windows Server 2003, Windows XP y Windows 2000 con los sistemas operativos de SP3 para obtener actualizaciones desde Microsoft Update o Windows Update.<br /><br />**Referencia casual** (actualizaciones automáticas): El término que se usa para describir cuándo el agente de Windows Update automáticamente programaciones y descarga las actualizaciones.|
|servidores autónomos|Utilice esta opción para hacer referencia a un servidor de Windows Server Update Services (WSUS) de nivel inferior en el que los administradores pueden administrar los componentes de WSUS.|
|servidor de nivel inferior|Usar para hacer referencia a un servidor de Windows Server Update Services (WSUS) que obtiene actualizaciones desde otro servidor WSUS en lugar de desde Microsoft Update o Windows Update.|
|Extensión de directiva de grupo (y: extensión de directiva de grupo|Una colección de configuraciones de directiva de grupo que se utilizan para controlar cómo los usuarios y equipos (a los que se aplican las directivas) pueden configurar y usar diversos servicios de Windows y características. Los administradores pueden usar WSUS con directivas de grupo para configuración de cliente del cliente actualizaciones automáticas, para ayudar a garantizar que los usuarios finales no se pueden deshabilitar o burlar las directivas de actualización corporativa.<br /><br />WSUS no requiere el uso de active directory o la directiva de grupo. También se puede aplicar la configuración del cliente mediante la directiva de grupo local o modificando el registro de Windows.|
|servicio de actualización interno|Una referencia ocasional a una infraestructura de red que usa uno o varios servidores WSUS para distribuir actualizaciones.|
|servidor de réplica|Utilice esta opción para hacer referencia a un servidor de Windows Server Update Services (WSUS) de nivel inferior que refleja las aprobaciones y configuración en el servidor que precede a la que está conectado. No puede administrar WSUS en un servidor de réplica.|
|Microsoft Update|**Un sitio de descarga de Microsoft basado en Internet:** Un sitio de Internet de Microsoft que almacena y distribuye actualizaciones para los equipos de Windows (controladores de dispositivos), los sistemas operativos de Windows y otros productos de software de Microsoft.|
|Software Update Services (SUS)|SUS fue el producto predecesor para Windows Server Update Services (WSUS).|
|actualizaciones|Cualquiera de una colección de revisiones de software, las revisiones, service packs, feature packs de y controladores de dispositivo que se pueden instalar en un equipo para ampliar la funcionalidad o mejoran el rendimiento y seguridad.|
|archivos de actualización|Los archivos necesarios para instalar una actualización en un equipo.|
|actualizar la información (también conocido como metadatos de actualización)|La información sobre una actualización, en lugar de los archivos binarios de actualización en un paquete de actualización. Por ejemplo, los metadatos suministran información para las propiedades de una actualización, lo que permite averiguar para qué es útil la actualización. Los metadatos incluyen también los términos de licencia del Software de Microsoft. El paquete de metadatos descargado para una actualización es normalmente mucho menor que el paquete de archivos de actualización real.|
|origen de actualización|La ubicación a la que se sincroniza un servidor de Windows Server Update Services (WSUS) para obtener los archivos de actualización. Esta ubicación puede ser Microsoft Update o un servidor WSUS ascendente.|
|servidor que precede en|Un servidor de Windows Server Update Services (WSUS) que proporciona archivos de actualización a otro servidor WSUS, que a su vez se conoce como un servidor de nivel inferior.|
|Windows Server Update Services (WSUS)|Un programa de rol de servidor que se ejecuta en uno o más equipos de Windows Server en una red corporativa. Una infraestructura WSUS le permite administrar las actualizaciones de equipos de la red para instalar.<br /><br />Puede utilizar WSUS para aprobar o rechazar actualizaciones antes del lanzamiento, para forzar actualizaciones para instalar una fecha determinada, y para obtener informes amplios en lo que actualiza cada equipo de la red necesita. Puede configurar WSUS para aprobar ciertas clases de las actualizaciones automáticamente (actualizaciones críticas, actualizaciones de seguridad, los service packs, controladores, etcetera). WSUS también le permite aprobar las actualizaciones para sólo, "detección" para que puedan ver qué equipos requerirán una actualización determinada sin tener que instalar las actualizaciones.<br /><br />En una implementación de WSUS, al menos un servidor WSUS en la red debe ser capaz de conectarse a Microsoft Update para obtener las actualizaciones disponibles. Según la configuración y seguridad de red, el administrador puede determinar cuántos servidores más se conectan directamente a Microsoft Update.<br /><br />Puede configurar un servidor WSUS para obtener actualizaciones a través de Internet de estos lugares como:<br /><br />-pública de Microsoft Update<br />-la actualización pública de Windows<br />-Microsoft Store|
|Windows Update|**Un sitio de descarga de Microsoft basado en Internet:** Un sitio de Internet de Microsoft que almacena y distribuye las actualizaciones de equipos de Windows (controladores de dispositivos) y los sistemas operativos de Windows.<br /><br />**servicio de equipo:** El nombre del servicio Windows Update que se ejecuta en los equipos. Actualización de Windows detecta, descarga e instala las actualizaciones en los equipos de Windows.<br /><br />Función de equipo y las configuraciones de directiva, el agente de Windows Update puede descargar las actualizaciones desde:<br /><br />-   Microsoft Update<br />-Windows Update<br />-Microsoft Store<br />-Un servicio de actualización (WSUS) de Internet (red)<br /><br />los equipos que no se administran en un entorno basado en WSUS normalmente usará Windows Update para conectarse directamente, a través de Internet - Windows Update, Microsoft Update o Microsoft Store para obtener las actualizaciones.|
|Cliente WSUS|Un equipo que recibe las actualizaciones de un servicio de actualización de la intranet WSUS.<br /><br />En el caso de la configuración de directiva de grupo que controlan la interacción del usuario final con las actualizaciones automáticas: un usuario de un equipo en un entorno de WSUS.|
