---
title: 'Paso 4: configurar las opciones de directiva de grupo para Actualizaciones automáticas'
description: 'Tema de Windows Server Update Service (WSUS): configurar los valores de directiva de grupo para Actualizaciones automáticas es el paso cuatro en un proceso de cuatro pasos para implementar WSUS'
ms.prod: windows-server
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
ms.openlocfilehash: a01d8881e8f0f7ca6feff691938f926a12460db0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361663"
---
# <a name="step-4-configure-group-policy-settings-for-automatic-updates"></a>Paso 4: configurar la directiva de grupo para Actualizaciones automáticas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En un entorno de Active Directory, puede usar directiva de grupo para definir la forma en que los equipos y los usuarios (a los que se hace referencia en este documento como clientes de WSUS) pueden interactuar con las actualizaciones de Windows para obtener actualizaciones automáticas de Windows Server Update Services (WSUS).

Este tema contiene dos secciones principales:

[Directiva de grupo configuración de las actualizaciones de cliente de WSUS](#group-policy-settings-for-wsus-client-updates), que proporciona instrucciones preceptivas y detalles de comportamiento sobre la configuración del Windows Update y del programador de mantenimiento de directiva de grupo que controlan cómo los clientes de WSUS pueden interactuar con Windows Update para obtener actualizaciones automáticas.

La [información complementaria](#supplemental-information) tiene las siguientes secciones:

-   [Obtener acceso a la configuración de Windows Update en Directiva de grupo](#accessing-the-windows-update-settings-in-group-policy), que proporciona instrucciones generales sobre el uso del editor de administración de directiva de grupo y la información sobre el acceso a la configuración de las extensiones de directiva y del programador de mantenimiento de los servicios de actualización en Directiva de grupo.

-   [Cambios en WSUS relevantes para esta guía](#changes-to-wsus-relevant-to-this-guide): para los administradores familiarizados con WSUS 3,2 y versiones anteriores, en esta sección se proporciona un breve resumen de las diferencias principales entre la versión actual y la anterior de WSUS relevante para esta guía.

-   [Términos y definiciones](#terms-and-definitions): definiciones de varios términos relacionados con WSUS y los servicios de actualización que se usan en esta guía.

## <a name="group-policy-settings-for-wsus-client-updates"></a>Configuración de directiva de grupo para las actualizaciones de cliente de WSUS
En esta sección se proporciona información sobre las tres extensiones de directiva de grupo. En estas extensiones encontrará la configuración que puede usar para configurar el modo en que los clientes de WSUS pueden interactuar con Windows Update para recibir actualizaciones automáticas.

-   [configuración del equipo &gt; Windows Update configuración de directivas](#computer-configuration--windows-update-policy-settings)

-   [configuración del equipo &gt; configuración de directiva del programador de mantenimiento](#computer-configuration--maintenance-scheduler-policy-settings)

-   [Configuración de usuario &gt; Windows Update configuración de directivas](#user-configuration--windows-update-policy-settings)

> [!NOTE]
> En este tema se da por supuesto que ya usa y está familiarizado con directiva de grupo. Si no está familiarizado con directiva de grupo, se recomienda que revise la información de la sección [información complementaria](#supplemental-information) de este documento antes de intentar configurar las opciones de directiva para WSUS.

### <a name="computer-configuration--windows-update-policy-settings"></a>Configuración del equipo > Windows Update configuración de directivas
En esta sección se proporcionan detalles acerca de las siguientes configuraciones de directiva basadas en equipos:

-   [Permitir la instalación inmediata de Actualizaciones automáticas](#allow-automatic-updates-immediate-installation)

-   [Permitir a los usuarios que no son administradores recibir notificaciones de actualización](#allow-non-administrators-to-receive-update-notifications)

-   [Permitir actualizaciones firmadas desde una ubicación del servicio Microsoft Update en la intranet](#allow-signed-updates-from-an-intranet-microsoft-update-service-location)

-   [Frecuencia de detección de Actualizaciones automáticas](#automatic-updates-detection-frequency)

-   [Configurar Actualizaciones automáticas](#configure-automatic-updates)

-   [Retraso de reinicio para instalaciones programadas](#delay-restart-for-scheduled-installations)

-   [No ajustar la opción predeterminada a "instalar actualizaciones y apagar" en el cuadro de diálogo cerrar Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [No mostrar la opción "instalar actualizaciones y apagar" en el cuadro de diálogo cerrar Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [Habilitar destinatarios del lado cliente](#enable-client-side-targeting)

-   [Habilitar Windows Update la administración de energía para reactivar automáticamente el equipo para instalar actualizaciones programadas](#enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates)

-   [No reiniciar automáticamente con usuarios que han iniciado sesión en las instalaciones de actualizaciones automáticas programadas](#no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations)

-   [Volver a solicitar el reinicio con instalaciones programadas](#re-prompt-for-restart-with-scheduled-installations)

-   [Volver a programar Actualizaciones automáticas instalaciones programadas](#reschedule-automatic-updates-scheduled-installations)

-   [Especificar la ubicación del servicio Microsoft Update en la intranet](#specify-intranet-microsoft-update-service-location)

-   [Activar las actualizaciones recomendadas a través de Actualizaciones automáticas](#turn-on-recommended-updates-via-automatic-updates)

-   [Activar las notificaciones de software](#turn-on-software-notifications)

En GPME, las directivas de Windows Update para la configuración basada en equipos se encuentran en la ruta de acceso: *PolicyName* > **configuración del equipo** > **directivas** > **plantillas administrativas** > **componentes de Windows** > **Windows Update.**

> [!NOTE]
> De forma predeterminada, estas opciones no están configuradas.

#### <a name="allow-automatic-updates-immediate-installation"></a>Permitir la instalación inmediata de Actualizaciones automáticas
Especifica si Actualizaciones automáticas instalará automáticamente las actualizaciones que no interrumpan los servicios de Windows o reinicien Windows.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Si la configuración de directiva "configurar Actualizaciones automáticas" está establecida en **deshabilitada**, esta Directiva no tiene ningún efecto.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica que las actualizaciones no se instalan inmediatamente. Los administradores locales pueden cambiar esta configuración mediante el editor de directiva de grupo local.|
|**Habilitado**|Especifica que Actualizaciones automáticas instala inmediatamente las actualizaciones una vez descargadas y listas para instalar.|
|**Deshabilitado**|Especifica que las actualizaciones no se instalan inmediatamente.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="allow-non-administrators-to-receive-update-notifications"></a>Permitir a los usuarios que no son administradores recibir notificaciones de actualización
Especifica si los usuarios no administrativos recibirán notificaciones de actualización en función de la configuración de la directiva configurar Actualizaciones automáticas.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Vea los detalles en la tabla siguiente.|

> [!NOTE]
> Si la configuración de directiva "configurar Actualizaciones automáticas" está deshabilitada o no está configurada, esta configuración de Directiva no tiene ningún efecto.

> [!IMPORTANT]
> a partir de Windows 8 y Windows RT, esta configuración de directiva está habilitada de forma predeterminada. En todas las versiones anteriores de Windows, está deshabilitada de forma predeterminada.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica que los usuarios verán siempre una ventana de control de cuentas y requieren permisos elevados para realizar estas tareas. Un administrador local puede cambiar esta configuración mediante el editor de directiva de grupo local.|
|**Habilitado**|Especifica que las actualizaciones automáticas de Windows y el Microsoft Update incluirán no administradores al determinar qué usuario con sesión iniciada recibirá notificaciones de actualización. Los usuarios no administrativos podrán instalar todo el contenido de actualización opcional, recomendado y importante para el que reciban una notificación. Los usuarios no verán una ventana de control de cuentas de usuario y no necesitan permisos elevados para instalar estas actualizaciones, excepto en el caso de las actualizaciones que contengan la interfaz de usuario, el contrato de licencia para el usuario final o los cambios de configuración de Windows Update.<br /><br />Hay dos situaciones en las que el efecto de esta configuración depende del equipo operativo:<br /><br />1. **ocultar** o **restaurar** actualizaciones<br />2. **Cancelar** una instalación de actualización<br /><br />En Windows Vista o Windows XP, si esta configuración de directiva está habilitada, los usuarios no verán una ventana de control de cuentas de usuario y no necesitan permisos elevados para ocultar, restaurar o cancelar actualizaciones.<br /><br />En Windows Vista, si esta configuración de directiva está habilitada, los usuarios no verán una ventana control de cuentas de usuario y no necesitan permisos elevados para ocultar, restaurar o cancelar actualizaciones. Si esta configuración de Directiva no está habilitada, los usuarios siempre verán una ventana de control de cuentas y requieren permisos elevados para ocultar, restaurar o cancelar actualizaciones.<br /><br />En Windows 7, esta configuración de Directiva no tiene ningún efecto. Los usuarios siempre verán una ventana de control de cuentas y requieren permisos elevados para realizar estas tareas.<br /><br />En Windows 8 y Windows RT, esta configuración de Directiva no tiene ningún efecto.|
|**Deshabilitado**|Especifica que solo los administradores que han iniciado sesión reciben notificaciones de actualización. **Nota:** En Windows 8 y Windows RT, esta configuración de directiva está habilitada de forma predeterminada. En todas las versiones anteriores de Windows, está deshabilitada de forma predeterminada.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="allow-signed-updates-from-an-intranet-microsoft-update-service-location"></a>Permitir las actualizaciones firmadas procedentes de una ubicación del servicio en Internet de Microsoft Update
Especifica si Actualizaciones automáticas acepta actualizaciones firmadas por entidades distintas de Microsoft cuando la actualización se encuentra en una ubicación del servicio Microsoft Update en la intranet.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Las actualizaciones de un servicio que no sea el servicio Microsoft Update de la intranet siempre deben estar firmadas por Microsoft y no se ven afectadas por esta configuración de directiva.

> [!NOTE]
> Esta Directiva no se admite en Windows RT. La habilitación de esta Directiva no tendrá ningún efecto en los equipos que ejecutan Windows RT.

**Opciones:** No hay opciones para esta configuración.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica que las actualizaciones de la ubicación del servicio Microsoft Update de la intranet deben estar firmadas por Microsoft.|
|**Habilitado**|Especifica que Actualizaciones automáticas acepta actualizaciones recibidas a través de una ubicación del servicio Microsoft Update en la intranet si están firmadas por un certificado encontrado en el almacén de certificados del equipo local "editores de confianza".|
|**Deshabilitado**|Especifica que las actualizaciones de la ubicación del servicio Microsoft Update de la intranet deben estar firmadas por Microsoft.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="always-automatically-restart-at-the-scheduled-time"></a>Reiniciar automáticamente siempre en la hora programada
Especifica si un temporizador de reinicio siempre se iniciará inmediatamente después de que Windows Update Instale actualizaciones importantes, en lugar de notificar primero a los usuarios de la pantalla de inicio de sesión durante al menos dos días.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Si está habilitada la configuración de directiva "no reiniciar automáticamente con usuarios que han iniciado sesión en instalaciones de actualizaciones automáticas programadas", esta Directiva no tiene ningún efecto.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica que Windows Update no modificará el comportamiento de reinicio del equipo.|
|**Habilitado**|Especifica que un temporizador de reinicio siempre se iniciará inmediatamente después de que Windows Update Instale actualizaciones importantes, en lugar de notificar primero a los usuarios de la pantalla de inicio de sesión durante al menos dos días.<br /><br />El temporizador de reinicio puede configurarse para que se inicie con cualquier valor entre 15 y 180 minutos. Cuando se agote el tiempo de espera del temporizador, el reinicio continuará incluso si el equipo tiene usuarios que han iniciado sesión.|
|**Deshabilitado**|Especifica que Windows Update no modificará el comportamiento de reinicio del equipo.|

**Opciones:** si esta opción está habilitada, puede especificar la cantidad de tiempo que transcurrirá después de que se instalen las actualizaciones antes de que se produzca un reinicio forzado del equipo.

#### <a name="automatic-updates-detection-frequency"></a>Frecuencia de detección de las actualizaciones automáticas
Especifica las horas que tardará Windows en determinar cuánto tiempo debe esperar antes de comprobar si hay actualizaciones disponibles. El tiempo de espera exacto se determina mediante el uso de las horas especificadas aquí menos cero hasta el veinte por ciento de las horas especificadas. Por ejemplo, si esta Directiva se usa para especificar una frecuencia de detección de 20 horas, todos los clientes a los que se aplica esta directiva comprobarán las actualizaciones en cualquier lugar entre 16 y 20 horas.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> La configuración "especificar la ubicación del servicio Microsoft Update en la intranet" debe estar habilitada para que esta directiva surta efecto.
>
> Si la configuración de directiva "configurar Actualizaciones automáticas" está deshabilitada, esta Directiva no tiene ningún efecto.

> [!NOTE]
> Esta Directiva no se admite en Windows RT. La habilitación de esta Directiva no tendrá ningún efecto en los equipos que ejecutan Windows RT.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica que Windows comprobará si hay actualizaciones disponibles en el intervalo predeterminado de 22 horas.|
|**Habilitado**|Especifica que Windows comprobará si hay actualizaciones disponibles en el intervalo especificado.|
|**Deshabilitado**|Especifica que Windows comprobará si hay actualizaciones disponibles en el intervalo predeterminado de 22 horas.|

**Opciones:** si esta opción está habilitada, puede especificar el intervalo de tiempo (en horas) que Windows Update espera antes de comprobar si hay actualizaciones.

#### <a name="configure-automatic-updates"></a>Configurar las actualizaciones automáticas
Especifica si las actualizaciones automáticas están habilitadas en este equipo.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

Si está habilitada, debe seleccionar una de las cuatro opciones proporcionadas en este directiva de grupo configuración.

Para usar esta opción, seleccione **habilitado**y, en **Opciones** , en **configurar actualizaciones automáticas**, seleccione una de las opciones (2, 3, 4 o 5).

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica que el uso de actualizaciones automáticas no se especifica en el nivel de directiva de grupo. Sin embargo, un administrador de equipo puede seguir configurando las actualizaciones automáticas en el panel de control.|
|**Habilitado**|Especifica que Windows reconoce cuando el equipo está en línea y utiliza su conexión a Internet para buscar las actualizaciones disponibles en Windows Update.<br /><br />Cuando está habilitada, los administradores locales podrán usar el Windows Update panel de control para seleccionar una opción de configuración de su elección. Sin embargo, no se permite a los administradores locales deshabilitar la configuración de Actualizaciones automáticas.<br /><br />-   **2: notificar la descarga y notificar la instalación**<br />    Cuando Windows Update encuentra actualizaciones que se aplican al equipo, se notificará a los usuarios que las actualizaciones están listas para su descarga. Los usuarios pueden ejecutar Windows Update para descargar e instalar las actualizaciones disponibles.<br />-   **3: descargar automáticamente y notificar la instalación** (configuración predeterminada)<br />    Windows Update encuentra las actualizaciones aplicables y las descarga en segundo plano; no se notifica al usuario ni se le interrumpe durante el proceso. Cuando se completen las descargas, se notificará a los usuarios que hay actualizaciones listas para instalar. A continuación, los usuarios pueden ejecutar Windows Update para instalar las actualizaciones descargadas.<br />-   **4: descargar automáticamente y programar la instalación**<br />    Puede especificar la programación mediante las opciones de este directiva de grupo configuración. Si no se especifica ninguna programación, la programación predeterminada de todas las instalaciones será todos los días a las 3:00 A.M. Si alguna actualización requiere un reinicio para completar la instalación, Windows reiniciará el equipo automáticamente. (si un usuario ha iniciado sesión en el equipo cuando Windows está listo para reiniciarse, se le notificará al usuario y se le dará la opción de retrasar el reinicio). **Nota:** a partir de Windows 8, puede establecer las actualizaciones que se instalarán durante el mantenimiento automático en lugar de usar una programación específica vinculada a Windows Update. El mantenimiento automático instalará las actualizaciones cuando el equipo no esté en uso y evitará la instalación de actualizaciones cuando el equipo esté funcionando con batería. Si el mantenimiento automático no puede instalar actualizaciones en un plazo de días, Windows Update instalará las actualizaciones inmediatamente. A continuación, se notificará a los usuarios acerca de un reinicio pendiente. Un reinicio pendiente solo tendrá lugar si no hay ninguna posibilidad de pérdida accidental de datos.    Puede especificar opciones de programación en la configuración del programador de mantenimiento de GPME, que se encuentra en la ruta de acceso, *PolicyName* > **configuración del equipo** > **directivas** > **plantillas administrativas** > **los componentes de Windows** > programador de **mantenimiento** > el límite de **activación de mantenimiento automático**. Vea la sección de esta referencia titulada: [configuración del programador de mantenimiento](#computer-configuration--maintenance-scheduler-policy-settings)para establecer detalles.    **5-permitir al administrador local elegir la configuración**<br />: Especifica si los administradores locales pueden usar el panel de control de Actualizaciones automáticas para seleccionar una opción de configuración de su elección, por ejemplo, si los administradores locales pueden elegir una hora de instalación programada.<br />    Los administradores locales no podrán deshabilitar la configuración de las actualizaciones automáticas.|
|**Deshabilitado**|Especifica que las actualizaciones de cliente que están disponibles desde el servicio de Windows Update público deben descargarse manualmente desde Internet e instalarse.|

#### <a name="delay-restart-for-scheduled-installations"></a>Retraso de reinicio para instalaciones programadas
Especifica la cantidad de tiempo que Actualizaciones automáticas esperará antes de continuar con un reinicio programado.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta directiva solo se aplica cuando Actualizaciones automáticas está configurado para realizar instalaciones de actualizaciones programadas. Si la configuración de directiva "configurar Actualizaciones automáticas" está deshabilitada, esta Directiva no tiene ningún efecto.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica que, una vez instaladas las actualizaciones, el tiempo de espera predeterminado de quince minutos pasará antes de que se produzca cualquier reinicio programado.|
|**Habilitado**|Especifica que, una vez finalizada la instalación, se producirá un reinicio programado después de haber expirado el número de minutos especificado.|
|**Deshabilitado**|Especifica que, una vez instaladas las actualizaciones, el tiempo de espera predeterminado de quince minutos pasará antes de que se produzca cualquier reinicio programado.|

**Opciones:** si esta opción está habilitada, puede usar esta opción para especificar la cantidad de tiempo (en minutos) que actualizaciones automáticas espera antes de continuar con un reinicio programado.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog"></a>No ajustar la opción predeterminada para instalar actualizaciones y apagar el cuadro de diálogo cerrar Windows
Esta configuración de directiva le permite especificar si se permite la opción **instalar actualizaciones y apagar** como opción predeterminada en el cuadro de diálogo **cerrar Windows** .

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta *configuración de directiva* no tiene ningún efecto si la opción de **configuración del equipo** >  > **directivas** > **plantillas administrativas** > **componentes de Windows** ** > Windows Update > no** **Mostrar la opción "instalar actualizaciones y apagar" en la configuración de la Directiva de diálogo cerrar Windows** está habilitada.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica que la opción **instalar actualizaciones y apagar** será la predeterminada en el cuadro de diálogo **cerrar Windows** si hay actualizaciones disponibles para la instalación en el momento en que el usuario seleccione la opción Apagar para apagar el equipo.|
|**Habilitado**|Si habilita esta configuración de Directiva, la opción del último apagado del usuario (por ejemplo, hibernar o reiniciar) es la opción predeterminada en el cuadro de diálogo **cerrar Windows** , independientemente de si la opción **instalar actualizaciones y apagar** está disponible en el menú **¿qué desea hacer?** .|
|**Deshabilitado**|Especifica que la opción **instalar actualizaciones y apagar** será la predeterminada en el cuadro de diálogo **cerrar Windows** si hay actualizaciones disponibles para la instalación en el momento en que el usuario seleccione la opción Apagar para apagar el equipo.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="do-not-connect-to-any-windows-update-internet-locations"></a>No conectarse a ninguna ubicación de Internet de Windows Update
Incluso cuando Windows Update está configurado para recibir actualizaciones de un servicio de actualización de la intranet, recuperará periódicamente información del servicio Windows Update público para permitir futuras conexiones con Windows Update y otros servicios, como Microsoft Update o Microsoft Store.

Al habilitar esta Directiva, se deshabilitará la funcionalidad para recuperar periódicamente información del servicio de actualización de Windows Server público, y puede provocar que la conexión a servicios públicos como Microsoft Store deje de funcionar.

|Compatible con:|EXCLUI|
|---------|-------|
|a partir de Windows Server 2012 R2, Windows 8.1 o Windows RT 8,1, los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta directiva solo se aplica cuando el equipo está configurado para conectarse a un servicio de actualización de la intranet mediante la configuración de directiva "especificar la ubicación del servicio Microsoft Update en la intranet".

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|El comportamiento predeterminado para recuperar información del servicio de actualización de Windows Server público persiste.|
|**Habilitado**|Especifica que los equipos no recuperarán información del servicio de actualización de Windows Server público.|
|**Deshabilitado**|El comportamiento predeterminado para recuperar información del servicio de actualización de Windows Server público persiste.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog"></a>No mostrar la opción instalar actualizaciones y apagar en el cuadro de diálogo cerrar Windows
Especifica si se muestra la opción **instalar actualizaciones y apagar** en el cuadro de diálogo **cerrar Windows** .

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica que la opción **instalar actualizaciones y apagar** está disponible en el cuadro de diálogo **cerrar Windows** si hay actualizaciones disponibles cuando el usuario selecciona la opción Apagar para apagar el equipo. Un administrador local puede cambiar esta configuración mediante la directiva local.|
|**Habilitado**|Especifica que las **actualizaciones de instalación y apagado** no aparecerán como una opción en el cuadro de diálogo **cerrar Windows** , incluso si las actualizaciones están disponibles para su instalación cuando el usuario selecciona la opción Apagar para apagar el equipo.|
|**Deshabilitado**|Especifica que la opción **instalar actualizaciones y apagar** será la predeterminada en el cuadro de diálogo **cerrar Windows** si hay actualizaciones disponibles para la instalación en el momento en que el usuario seleccione la opción Apagar para apagar el equipo.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="enable-client-side-targeting"></a>Habilitar la orientación de cliente
Especifica el nombre o los nombres de grupo de destino que están configurados en la consola de WSUS y que van a recibir actualizaciones de WSUS.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Esta directiva solo se aplica cuando este equipo está configurado para admitir los nombres de grupo de destino especificados en WSUS. Si el nombre del grupo de destino no existe en WSUS, se omitirá hasta que se cree. Si la configuración de directiva "especificar la ubicación del servicio Microsoft Update en la intranet" está deshabilitada o no está configurada, esta Directiva no tiene ningún efecto.

> [!NOTE]
> Esta Directiva no se admite en Windows RT. La habilitación de esta Directiva no tendrá ningún efecto en los equipos que ejecutan Windows RT.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica que no se envía información de grupo de destino a WSUS. Un administrador local puede cambiar esta configuración mediante una directiva local.|
|**Habilitado**|Especifica que la información del grupo de destino especificado se envía a WSUS, que la usa para determinar las actualizaciones que se deben implementar en este equipo. Si WSUS admite varios grupos de destino, puede usar esta directiva para especificar varios nombres de grupo, separados por punto y coma, si ha agregado los nombres de grupo de destino en la lista de grupos de equipos de WSUS. De lo contrario, se debe especificar un solo grupo.|
|**Deshabilitado**|Especifica que no se envía información de grupo de destino a WSUS.|

**Opciones:** Use este espacio para especificar uno o más nombres de grupos de destino.

#### <a name="enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates"></a>Habilitar Windows Update la administración de energía para reactivar automáticamente el equipo para instalar actualizaciones programadas
Especifica si Windows Update usará las características de administración de energía o opciones de energía de Windows para reactivar automáticamente el equipo contra la hibernación si hay actualizaciones programadas para la instalación.

El equipo se reactivará automáticamente solo si Windows Update está configurado para instalar las actualizaciones automáticamente. Si el equipo está en hibernación cuando se produce la hora de instalación programada y se aplican actualizaciones, Windows Update usará las características de administración de energía de Windows o opciones de energía para reactivar automáticamente el equipo para instalar las actualizaciones. Windows Update también reactivará el equipo e instalará una actualización en caso de que se produzca una fecha límite de instalación.

El equipo no se reactivará a menos que se instalen actualizaciones. Si el equipo funciona con batería, cuando Windows Update lo reactiva, no instalará actualizaciones y el equipo volverá automáticamente a hibernación en dos minutos.

|Compatible con:|EXCLUI|
|---------|-------|
|a partir de Windows Vista y Windows Server 2008 (Windows 7), los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Windows Update no reactiva el equipo contra la hibernación para instalar actualizaciones. Un administrador local puede cambiar esta configuración mediante una directiva local.|
|**Habilitado**|Windows Update activa el equipo contra la hibernación para instalar actualizaciones en las condiciones mencionadas anteriormente.|
|**Deshabilitado**|Windows Update no reactiva el equipo contra la hibernación para instalar actualizaciones.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations"></a>No reiniciar automáticamente para instalar actualizaciones automáticas programadas con usuarios que hayan iniciado sesión
Especifica que para completar una instalación programada, Actualizaciones automáticas esperará a que se reinicie el equipo por cualquier usuario que haya iniciado sesión, en lugar de que el equipo se reinicie automáticamente.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta directiva solo se aplica cuando Actualizaciones automáticas está configurado para realizar instalaciones de actualizaciones programadas. Si la configuración de directiva "configurar Actualizaciones automáticas" está deshabilitada, esta Directiva no tiene ningún efecto.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica que Actualizaciones automáticas notificará al usuario que el equipo se reiniciará automáticamente en cinco minutos para completar la instalación.|
|**Habilitado**|Algunas actualizaciones requieren que se reinicie el equipo para que las actualizaciones surtan efecto. Si el estado se establece en habilitado, Actualizaciones automáticas no reiniciará un equipo automáticamente durante una instalación programada Si un usuario ha iniciado sesión en el equipo. En su lugar, Actualizaciones automáticas notificará al usuario que reinicie el equipo.|
|**Deshabilitado**|Especifica que Actualizaciones automáticas notificará al usuario que el equipo se reiniciará automáticamente en cinco minutos para completar la instalación.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="re-prompt-for-restart-with-scheduled-installations"></a>Volver a pedir la intervención del usuario para reiniciar con instalaciones programadas
Especifica la cantidad de tiempo que Actualizaciones automáticas esperar antes de volver a preguntar con un reinicio programado.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!IMPORTANT]
> Esta directiva solo se aplica cuando Actualizaciones automáticas está configurado para realizar instalaciones de actualizaciones programadas. Si la configuración de directiva "configurar Actualizaciones automáticas" está deshabilitada, esta Directiva no tiene ningún efecto.

> [!NOTE]
> Esta Directiva no tiene ningún efecto en los equipos que ejecutan Windows RT.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Un reinicio programado se produce diez minutos después de que se descarte el mensaje de solicitud de reinicio. Un administrador local puede cambiar esta configuración mediante una directiva local.|
|**Habilitado**|Especifica que, una vez pospuesto la solicitud de reinicio anterior, se producirá un reinicio programado una vez transcurrido el número de minutos especificado.|
|**Deshabilitado**|Un reinicio programado se produce diez minutos después de que se descarte el mensaje de solicitud de reinicio.|

**Opciones:** Cuando está habilitada, puede usar esta opción de configuración para especificar (en minutos) la cantidad de tiempo que transcurrirá antes de que los usuarios vuelvan a solicitar un reinicio programado.

#### <a name="reschedule-automatic-updates-scheduled-installations"></a>Volver a programar las instalaciones programadas de actualizaciones automáticas
Especifica la cantidad de tiempo que Actualizaciones automáticas esperar después de un inicio del equipo antes de continuar con una instalación programada que se ha perdido previamente.

Si el estado se establece en **no configurado**, se producirá una instalación programada perdida un minuto después de que se inicie el equipo.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta directiva solo se aplica cuando Actualizaciones automáticas está configurado para realizar instalaciones de actualizaciones programadas. Si la configuración de directiva "configurar Actualizaciones automáticas" está deshabilitada, esta Directiva no tiene ningún efecto.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica que una instalación programada perdida se producirá un minuto después de que se inicie el equipo.|
|**Habilitado**|Especifica que una instalación programada que no se haya realizado antes se producirá el número especificado de minutos después de que se inicie el equipo.|
|**Deshabilitado**|Especifica que se producirá una instalación programada perdida con la siguiente instalación programada.|

**Opciones:** Cuando esta configuración de directiva está habilitada, puede usarla para especificar un número de minutos después de que se inicie el equipo, ya que se producirá una instalación programada que no se ha realizado con anterioridad.

#### <a name="specify-intranet-microsoft-update-service-location"></a>Especificar la ubicación del servicio Microsoft Update en la intranet
Especificar un servidor de intranet alternativo para las actualizaciones de host de Microsoft Update. Después, puede usar WSUS para actualizar automáticamente los equipos de la red.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT.|

Esta configuración permite especificar un servidor WSUS en la red que funcionará como un servicio de actualización interno. En lugar de usar las actualizaciones de Microsoft en Internet, los clientes de WSUS buscarán las actualizaciones que se apliquen en este servicio.

Para usar esta configuración, debe establecer dos valores de nombre de servidor: el servidor desde el que el cliente detecta y descarga las actualizaciones, y el servidor en el que las estaciones de trabajo actualizadas cargan las estadísticas. Los valores no deben ser diferentes si ambos servicios están configurados en el mismo servidor.

> [!NOTE]
> Si la configuración de directiva "configurar Actualizaciones automáticas" está deshabilitada, esta Directiva no tiene ningún efecto.

> [!NOTE]
> Esta Directiva no se admite en Windows RT. La habilitación de esta Directiva no tendrá ningún efecto en los equipos que ejecutan Windows RT.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Si Actualizaciones automáticas no está deshabilitado por la Directiva o por las preferencias del usuario, esta Directiva especifica que los clientes se conecten directamente al sitio de Windows Update en Internet.|
|**Habilitado**|Especifica que el cliente se conecta al servidor WSUS especificado, en lugar de Windows Update, para buscar y descargar actualizaciones. La habilitación de esta configuración significa que los usuarios finales de su organización no tienen que pasar por un firewall para obtener actualizaciones, y ofrece la oportunidad de probar las actualizaciones antes de implementarlas.|
|**Deshabilitado**|Si Actualizaciones automáticas no está deshabilitado por la Directiva o por las preferencias del usuario, esta Directiva especifica que los clientes se conecten directamente al sitio de Windows Update en Internet.|

**Opciones:** Cuando esta configuración de directiva está habilitada, debe especificar el servicio de actualización de la intranet que los clientes de WSUS usarán al detectar actualizaciones, y el servidor de estadísticas de Internet en el que los clientes WSUS actualizados cargarán las estadísticas. Valores de ejemplo:


|                    Opción de configuración:                    |    Valor de ejemplo:    |
|-------------------------------------------------------|----------------------|
| Establecer el servicio de actualización de la intranet para detectar actualizaciones |  http://wsus01:8530  |
|          Establecer el servidor de estadísticas de la intranet           | http://IntranetUpd01 |

#### <a name="turn-on-recommended-updates-via-automatic-updates"></a>Activar las actualizaciones recomendadas a través de Actualizaciones automáticas
Especifica si Actualizaciones automáticas proporcionará actualizaciones importantes y recomendadas de WSUS.

|Compatible con:|EXCLUI|
|---------|-------|
|a partir de Windows Vista, los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica que Actualizaciones automáticas continuará proporcionando actualizaciones importantes si ya está configurada para ello.|
|**Habilitado**|Especifica que Actualizaciones automáticas instalará las actualizaciones recomendadas y las actualizaciones importantes de WSUS.|
|**Deshabilitado**|Especifica que Actualizaciones automáticas continuará proporcionando actualizaciones importantes si ya está configurada para ello.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="turn-on-software-notifications"></a>Activar las notificaciones de software
Esta configuración de directiva le permite controlar si los usuarios ven mensajes de notificación mejorados sobre el software destacado del servicio Microsoft Update. Los mensajes de notificación mejorados transmiten el valor y promueven la instalación y el uso de software opcional. Esta configuración de directiva está pensada para usarse en entornos administrados de forma flexible en los que se permite el acceso del usuario final al servicio de Microsoft Update.

Si no usa el servicio Microsoft Update, la configuración de directiva "notificaciones de software" no tiene ningún efecto.

Si la configuración de directiva "configurar Actualizaciones automáticas" está deshabilitada o no está configurada, la configuración de directiva "notificaciones de software" no tiene ningún efecto.

|Compatible con:|EXCLUI|
|---------|-------|
|a partir de Windows Server 2008 (Windows Vista) y Windows 7, los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> De manera predeterminada, esta configuración de directiva está deshabilitada.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|No se ofrecen mensajes a los usuarios en equipos que ejecutan Windows 7 para aplicaciones opcionales. A los usuarios de equipos que ejecutan Windows Vista no se les ofrecen mensajes para las actualizaciones o aplicaciones opcionales. Un administrador local puede cambiar esta configuración mediante el panel de control o una directiva local.|
|**Habilitado**|Si habilita esta configuración de Directiva, aparecerá un mensaje de notificación en el equipo del usuario cuando esté disponible software destacado. El usuario puede hacer clic en la notificación para abrir Windows Update y obtener más información acerca del software o la instalación. El usuario también puede hacer clic en **cerrar este mensaje** o **mostrarme más tarde** para diferir la notificación según corresponda.<br /><br />En Windows 7, esta configuración de directiva solo controlará las notificaciones detalladas de las aplicaciones opcionales. En Windows Vista, esta configuración de directiva controla las notificaciones detalladas de las aplicaciones y actualizaciones opcionales.|
|**Deshabilitado**|Especifica que los usuarios que ejecutan Windows 7 no recibirán mensajes de notificación detallados para las aplicaciones opcionales, y los usuarios que ejecuten Windows Vista no recibirán mensajes de notificación detallados para las aplicaciones opcionales o las actualizaciones opcionales.|

**Opciones:** No hay opciones para esta configuración.

### <a name="computer-configuration--maintenance-scheduler-policy-settings"></a>Configuración del equipo > configuración de directiva del programador de mantenimiento
En la opción configurar Actualizaciones automáticas, seleccionó la opción **4: descargar automáticamente y programar la instalación**; puede especificar la configuración del programador de mantenimiento de Schedule en la GPMC para equipos que ejecutan Windows 8 y Windows RT. Si no seleccionó la opción 4 en el valor "configurar Actualizaciones automáticas", no es necesario que configure estas opciones para las actualizaciones automáticas. La configuración del programador de mantenimiento se encuentra en la ruta de acceso: *PolicyName* > **configuración del equipo** > **directivas** > **plantillas administrativas** > **programador de mantenimiento** > componentes de **Windows** . La extensión del programador de mantenimiento de directiva de grupo contiene las siguientes opciones:

-   [Límite de activación de mantenimiento automático](#automatic-maintenance-activation-boundary)

-   [Retraso aleatorio de mantenimiento automático](#automatic-maintenance-random-delay)

-   [Directiva de activación automática](#automatic-wakeup-policy)

#### <a name="automatic-maintenance-activation-boundary"></a>Límite de activación del Mantenimiento automático
Esta directiva le permite configurar la opción "límite de activación de mantenimiento automático".

El límite de activación de mantenimiento es la hora programada diaria a la que se inicia el mantenimiento automático.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Este valor está relacionado con la opción 4 en **configurar actualizaciones automáticas**. Si no seleccionó la opción 4 en **configurar actualizaciones automáticas**, no es necesario configurar esta opción.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Si esta configuración de Directiva no está configurada, se aplicará el tiempo programado diario especificado en los equipos cliente en el **centro de actividades** > panel de control de **mantenimiento automático** .|
|**Habilitado**|Al habilitar esta configuración de Directiva, se invalidan los valores predeterminados o modificados configurados en los equipos cliente en el **Panel de Control** > **centro de actividades** > **mantenimiento automático** (o en algunas versiones de cliente, **mantenimiento**).|
|**Deshabilitado**|Si establece esta configuración de directiva en **deshabilitado**, la hora programada diaria según se especifica en el **centro de actividades** > **mantenimiento automático**, se aplicará en el panel de control.|

#### <a name="automatic-maintenance-random-delay"></a>Retraso aleatorio de mantenimiento automático
Esta configuración de directiva le permite configurar el retraso aleatorio de activación de mantenimiento automático.

El retraso aleatorio de mantenimiento es la cantidad de tiempo hasta el que se retrasará el mantenimiento automático a partir de su límite de activación. Esta configuración es útil para las máquinas virtuales en las que el mantenimiento aleatorio puede ser un requisito de rendimiento.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Este valor está relacionado con la opción 4 en **configurar actualizaciones automáticas**. Si no seleccionó la opción 4 en **configurar actualizaciones automáticas**, no es necesario configurar esta opción.

De forma predeterminada, cuando está habilitado, el retraso aleatorio de mantenimiento normal se establece en **PT4H**.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Se aplica un retraso aleatorio de cuatro horas a **automático**.|
|**Habilitado**|El mantenimiento automático retrasará el inicio desde su límite de activación hasta la cantidad de tiempo especificada.|
|**Deshabilitado**|No se aplica ningún retraso aleatorio al mantenimiento automático.|

#### <a name="automatic-wakeup-policy"></a>Directiva de activación automática
Esta configuración de directiva le permite configurar la Directiva de reactivación de mantenimiento automático.

La Directiva de reactivación de mantenimiento especifica si el mantenimiento automático debe llevar a cabo una solicitud de reactivación al equipo operativo para el mantenimiento programado diario.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Si la Directiva de activación de energía del equipo operativo está deshabilitada explícitamente, esta configuración no tiene ningún efecto.

> [!NOTE]
> Este valor está relacionado con la opción 4 en **configurar actualizaciones automáticas**. Si no seleccionó la opción 4 en **configurar actualizaciones automáticas**, no es necesario configurar esta opción.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Si no establece esta configuración de Directiva, se aplicará el valor de reactivación tal como se especifica en el **centro de actividades** > panel de control de **mantenimiento automático** .|
|**Habilitado**|Si habilita esta configuración de Directiva, el mantenimiento automático intentará establecer una directiva de reactivación del sistema operativo y realizar una solicitud de reactivación para la hora programada diariamente, si es necesario.|
|**Deshabilitado**|Si deshabilita esta configuración de Directiva, se aplicará el valor de reactivación tal como se especifica en el **centro de actividades** > panel de control de **mantenimiento automático** .|

### <a name="user-configuration--windows-update-policy-settings"></a>Configuración de usuario > Windows Update configuración de directivas
En esta sección se proporcionan detalles sobre las siguientes configuraciones de directiva basadas en el usuario:

-   [No mostrar la opción "instalar actualizaciones y apagar" en el cuadro de diálogo cerrar Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [No ajustar la opción predeterminada a "instalar actualizaciones y apagar" en el cuadro de diálogo cerrar Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [quitar el acceso para usar todas las características de Windows Update](#remove-access-to-use-all-windows-update-features)

En la GPMC, la configuración de usuario para las actualizaciones automáticas del equipo se encuentra en la ruta de acceso: *PolicyName* > **configuración de usuario** > **directivas** > **plantillas administrativas** > **componentes de Windows** ** > Windows Update.** La configuración se muestra en el mismo orden en que aparecen en las extensiones configuración del equipo y configuración de usuario en directiva de grupo, cuando se selecciona la pestaña **configuración** de la directiva de Windows Update para ordenar la configuración alfabéticamente.

> [!NOTE]
> De forma predeterminada, a menos que se indique lo contrario, esta configuración no se configura.

> [!TIP]
> para cada una de estas opciones, puede usar los pasos siguientes para habilitar, deshabilitar o desplazarse por la configuración:

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog-box"></a>No mostrar la opción "instalar actualizaciones y apagar" en el cuadro de diálogo cerrar Windows
Especifica si se muestra la opción **instalar actualizaciones y apagar** en el cuadro de diálogo **cerrar Windows** .

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica que la opción **instalar actualizaciones y apagar** aparecerá en el cuadro de diálogo **cerrar Windows** si hay actualizaciones disponibles cuando el usuario seleccione la opción Apagar para apagar el equipo.|
|**Habilitado**|Si habilita esta configuración de Directiva, **las actualizaciones de instalación y apagado** no aparecerán como una opción en el cuadro de diálogo **cerrar Windows** , incluso si hay actualizaciones disponibles para la instalación cuando el usuario selecciona la opción Apagar para apagar el equipo.|
|**Deshabilitado**|Especifica que la opción **instalar actualizaciones y apagar** aparecerá en el cuadro de diálogo **cerrar Windows** si hay actualizaciones disponibles cuando el usuario seleccione la opción Apagar para apagar el equipo.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog-box"></a>No ajustar la opción predeterminada a "instalar actualizaciones y apagar" en el cuadro de diálogo cerrar Windows
Especifica si se permite la opción **instalar actualizaciones y apagar** como opción predeterminada en el cuadro de diálogo **cerrar Windows** .

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta *configuración de directiva* no tiene ningún efecto si la opción de > **directivas** de **configuración de usuario** de >  > **plantillas administrativas** > **componentes de Windows** ** > Windows Update > no** **Mostrar la opción "instalar actualizaciones y apagar" del cuadro de diálogo cerrar Windows** está habilitada.

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Especifica si la opción **instalar actualizaciones y apagar** será la predeterminada en el cuadro de diálogo **cerrar Windows** si hay actualizaciones disponibles para la instalación en el momento en que el usuario selecciona la opción Apagar para apagar el equipo.|
|**Habilitado**|Especifica si la opción del último apagado del usuario (por ejemplo, hibernar o reiniciar) es la opción predeterminada en el cuadro de diálogo **cerrar Windows** , independientemente de si la **opción instalar actualizaciones y apagar** está disponible en el menú **¿qué desea hacer?** .|
|**Deshabilitado**|Especifica si la opción **instalar actualizaciones y apagar** será la predeterminada en el cuadro de diálogo **cerrar Windows** si hay actualizaciones disponibles para la instalación en el momento en que el usuario selecciona la opción Apagar para apagar el equipo.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="remove-access-to-use-all-windows-update-features"></a>Desactivar el acceso al uso de todas las características de Windows Update
Esta configuración permite quitar el acceso del cliente de WSUS a Windows Update.

|Compatible con:|EXCLUI|
|---------|-------|
|Sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuración de Directiva**|**Conducta**|
|**No configurado**|Los usuarios pueden conectarse al sitio web de Windows Update.|
|**Habilitado**|**Importante:** si se habilita, se quitan todas las características de Windows Update. Esto incluye el bloqueo del acceso al sitio web de Windows Update en https://windowsupdate.microsoft.com, desde el hipervínculo Windows Update en el menú Inicio o la pantalla Inicio, y también en el menú **herramientas** de Internet Explorer. La actualización automática de Windows también está deshabilitada; no se notificará al usuario ni recibirá actualizaciones críticas de Windows Update. Esta configuración también impide que Device Manager instale automáticamente actualizaciones de controladores desde el sitio web de Windows Update.<br /><br />Cuando está habilitada, puede configurar una de las siguientes opciones de notificación:<br /><br />-   **0: no mostrar ninguna notificación**<br />    Esta configuración quitará todo el acceso a Windows Update características y no se mostrarán notificaciones.<br />-   **1: mostrar las notificaciones necesarias para el reinicio**<br />    Esta configuración mostrará notificaciones acerca de los reinicios necesarios para completar una instalación. **Nota:** En los equipos que ejecutan Windows 8 y Windows RT, si esta directiva está habilitada, solo se mostrarán las notificaciones relacionadas con los reinicios y la imposibilidad de detectar actualizaciones. No se admiten las opciones de notificación. Siempre se muestran las notificaciones de la pantalla de inicio de sesión.|
|**Deshabilitado**|Los usuarios pueden conectarse al sitio web de Windows Update.|

**Opciones:** Consulte **habilitado** en la tabla para esta configuración.

## <a name="supplemental-information"></a>Información complementaria
En esta sección se proporciona información adicional acerca del uso de la apertura y el guardado de la configuración de WSUS en directivas de grupo, así como definiciones de los términos que se usan en esta guía. Para los administradores familiarizados con las versiones anteriores de WSUS (WSUS 3,2 y versiones anteriores), hay una tabla que resume brevemente las diferencias entre las versiones de WSUS.

### <a name="accessing-the-windows-update-settings-in-group-policy"></a>Obtener acceso a la configuración de Windows Update en directiva de grupo
En el procedimiento siguiente se describe cómo abrir GPMC en el controlador de dominio. A continuación, el procedimiento describe cómo abrir un objeto de directiva de grupo de nivel de dominio (GPO) existente para editarlo, o crear un nuevo GPO de nivel de dominio y abrirlo para su edición.

> [!NOTE]
> Para llevar a cabo este procedimiento, debe ser miembro del grupo **Admins** . del dominio o equivalente.

##### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Para abrir o agregar y abrir un objeto directiva de grupo

1.  En el controlador de dominio, vaya a **Administrador del servidor**, > **herramientas**> **Directiva de grupo Management**. Se abre el Consola de administración de directivas de grupo.

2.  En el panel izquierdo, expanda el bosque. Por ejemplo, haga doble clic en **bosque: example.com**.

3.  En el panel izquierdo, haga doble clic en **dominios**y, a continuación, haga doble clic en el dominio para el que desea administrar un objeto de directiva de grupo. Por ejemplo, haga doble clic en **example.com**.

4.  Realiza una de las siguientes acciones:

    -  **Para abrir un GPO de nivel de dominio existente para editarlo**, haga doble clic en el dominio que contiene el Directiva de grupo objeto que desea administrar, haga clic con el botón secundario en la Directiva de dominio que desea administrar y, a continuación, haga clic en **Editar**. Se abre el editor de administración de directiva de grupo (GPME).

    -  **Para crear un objeto de directiva de grupo nuevo y abrirlo para su edición**:
        1.  Haga clic con el botón secundario en el dominio para el que desea crear un nuevo directiva de grupo objeto y, a continuación, haga clic en **crear un GPO en este dominio y vincularlo aquí**.

        2.  En **nuevo GPO**, en **nombre**, escriba un nombre para el nuevo objeto de directiva de grupo y, a continuación, haga clic en **Aceptar**.

        3.  Haga clic con el botón secundario en el nuevo objeto directiva de grupo y, a continuación, haga clic en **Editar**. Se abre GPME.

##### <a name="to-open-the-windows-update-or-maintenance-scheduler-extensions-of-group-policy"></a>Para abrir el Windows Update o las extensiones del programador de mantenimiento de directiva de grupo

1.  En directiva de grupo editor de administración, realice una de las acciones siguientes:

    -   **Abra la > configuración del equipo Windows Update extensión de directiva de grupo**. Vaya a: *PolicyName* > **configuración del equipo** > **directivas** / **plantillas administrativas** > **componentes de Windows** ** > Windows Update.**

    -   **Abra la > de configuración de usuario Windows Update extensión de directiva de grupo**. Vaya a: *PolicyName* > **configuración de usuario** > **directivas** > **plantillas administrativas** > **componentes de Windows** ** > Windows Update.**

    -   **Abra la extensión configuración del equipo > programador de mantenimiento de directiva de grupo**. En GPOE, vaya a *PolicyName* > **configuración del equipo** > **directivas** > **plantillas administrativas** > de **componentes de Windows** > programador de **mantenimiento**.

Para obtener más información sobre el directiva de grupo, consulte [información general sobre Directiva de grupo](https://technet.microsoft.com/library/hh831791.aspx(v=ws.12)).

> [!TIP]
> Después de abrir la extensión de directiva de grupo desea, puede usar los pasos siguientes para habilitar, deshabilitar o desplazarse por la configuración:

##### <a name="to-configure-group-policy-settings"></a>Para definir la configuración de directiva de grupo

1.  En *ExtensionOfGroupPolicy*, haga doble clic en el valor que desea ver o modificar.

2.  Para configurar la opción, realice una de las acciones siguientes:

    -   Para conservar el estado predeterminado no especificado de la configuración, seleccione **no configurado**.

    -   Para habilitar la configuración, seleccione **habilitado**.

    -   Para deshabilitar la configuración, seleccione **deshabilitado**.

3.  En **Opciones**, si se enumeran las opciones, conserve los valores predeterminados o modifíquelas según sea necesario.

4.  Realiza una de las siguientes acciones:

    -   Para guardar los cambios y continuar con la configuración siguiente, haga clic en **aplicar**y, a continuación, haga clic en **configuración siguiente**.

    -   Para guardar los cambios y cerrar el cuadro de diálogo, haga clic en **Aceptar**.

    -   Para descartar todos los cambios no guardados y cerrar el cuadro de diálogo, haga clic en **Cancelar**.

### <a name="changes-to-wsus-relevant-to-this-guide"></a>Cambios en WSUS relevantes para esta guía
En la tabla siguiente se resumen las diferencias principales entre las versiones actuales y anteriores de WSUS que son relevantes para esta guía.

|Windows Server y versiones de WSUS|Descripción|
|------------------|--------|
| Windows Server 2012 R2 con WSUS 6,0 y versiones posteriores|a partir de Windows Server 2012, el rol de servidor de WSUS está integrado con el sistema operativo y la configuración de directiva de grupo asociada de los clientes WSUS se incluye, de forma predeterminada, en directiva de grupo.|
| Windows Server 2008 (y versiones anteriores de Windows Server) con WSUS 3,2 y versiones anteriores|En Windows Server 2008 (y versiones anteriores de Windows Server) con las versiones de WSUS 3,2 (y versiones anteriores), la configuración de directiva de grupo que rige los clientes de WSUS no se incluye en estos sistemas operativos de Windows Server. La configuración de directiva está en la plantilla administrativa de WSUS, **wuau. adm**. En estas versiones de servidor, primero se debe agregar la plantilla administrativa de WSUS en el Consola de administración de directivas de grupo (GPMC) para poder configurar la configuración del cliente WSUS.|

### <a name="terms-and-definitions"></a>Términos y definiciones
A continuación se muestra una lista de los términos que se usan en esta guía.

|Término|Definición|
|----|-------|
|Actualizaciones automáticas|**Un servicio que se ejecuta en equipos Windows** (actualizaciones automáticas): hace referencia al componente de equipo cliente integrado en los sistemas operativos Microsoft Windows Vista, windows Server 2003, Windows XP y Windows 2000 con SP3 para obtener actualizaciones de Microsoft Update o Windows Update.<br /><br />**Referencia casual** (actualizaciones automáticas): el término que se usa para describir Cuándo el agente de Windows Update programa y descarga automáticamente las actualizaciones.|
|servidor autónomo|Use para hacer referencia a un servidor de Windows Server Update Services de bajada (WSUS) en el que los administradores pueden administrar componentes de WSUS.|
|servidor de bajada|Use para hacer referencia a un servidor Windows Server Update Services (WSUS) que obtiene actualizaciones de otro servidor WSUS en lugar de Microsoft Update o Windows Update.|
|Directiva de grupo extensión (y: extensión de directiva de grupo|Colección de valores de directiva de grupo que se usan para controlar el modo en que los usuarios y equipos (a los que se aplican las directivas) pueden configurar y usar diversos servicios y características de Windows. Los administradores pueden usar WSUS con directiva de grupo para la configuración del cliente del Actualizaciones automáticas cliente, para ayudar a garantizar que los usuarios finales no puedan deshabilitar u eludir las directivas de actualización corporativas.<br /><br />WSUS no requiere el uso de Active Directory o directiva de grupo. La configuración del cliente también se puede aplicar mediante la Directiva de grupo local o modificando el registro de Windows.|
|servicio de actualización interno|Referencia casual a una infraestructura de red que usa uno o más servidores WSUS para distribuir actualizaciones.|
|servidor réplica|Use para hacer referencia a un servidor de Windows Server Update Services de bajada (WSUS) que refleje las aprobaciones y la configuración del servidor que precede en la cadena a la que está conectado. No se puede administrar WSUS en un servidor réplica.|
|Microsoft Update|**Un sitio de descarga de Microsoft basado en Internet:** Un sitio de Microsoft Internet que almacena y distribuye actualizaciones para equipos Windows (controladores de dispositivos), sistemas operativos Windows y otros productos de software de Microsoft.|
|Servicios de actualización de software (SUS)|SUS era el producto predecesor para Windows Server Update Services (WSUS).|
|actualizaciones|Cualquiera de las recopilaciones de software, revisiones, Service Packs, paquetes de características y controladores de dispositivos que se pueden instalar en un equipo para ampliar la funcionalidad, o mejorar el rendimiento y la seguridad.|
|archivos de actualización|Archivos necesarios para instalar una actualización en un equipo.|
|Actualizar información (también conocida como metadatos de actualización)|Información acerca de una actualización, en lugar de los archivos binarios de actualización de un paquete de actualización. Por ejemplo, los metadatos proporcionan información para las propiedades de una actualización, lo que le permite averiguar cuál es la utilidad de actualización. Los metadatos también incluyen términos de licencia del software de Microsoft. El paquete de metadatos descargado para una actualización suele ser mucho menor que el paquete del archivo de actualización real.|
|Actualizar origen|Ubicación en la que se sincroniza un servidor Windows Server Update Services (WSUS) para obtener los archivos de actualización. Esta ubicación puede ser Microsoft Update o un servidor WSUS que precede en la cadena.|
|servidor que precede en la cadena|Un servidor Windows Server Update Services (WSUS) que proporciona archivos de actualización a otro servidor WSUS, que a su vez se conoce como servidor de nivel inferior.|
|Windows Server Update Services (WSUS)|Programa de rol de servidor que se ejecuta en uno o más equipos con Windows Server en una red corporativa. Una infraestructura de WSUS permite administrar las actualizaciones de los equipos de la red que se van a instalar.<br /><br />Puede usar WSUS para aprobar o rechazar actualizaciones antes de la versión, para forzar la instalación de actualizaciones en una fecha determinada y para obtener informes completos sobre qué actualizaciones requiere cada equipo de la red. Puede configurar WSUS para que apruebe ciertas clases de actualizaciones automáticamente (actualizaciones críticas, actualizaciones de seguridad, Service Packs, controladores, etc.). WSUS también le permite aprobar actualizaciones solo para "detección", de modo que pueda ver qué equipos requerirán una actualización determinada sin tener que instalar las actualizaciones.<br /><br />En una implementación de WSUS, al menos un servidor WSUS de la red debe ser capaz de conectarse a Microsoft Update para obtener las actualizaciones disponibles. En función de la seguridad y la configuración de la red, el administrador puede determinar el número de servidores que se conectan directamente a Microsoft Update.<br /><br />Puede configurar un servidor WSUS para obtener actualizaciones a través de Internet desde lugares como:<br /><br />-el Microsoft Update público<br />-el Windows Update público<br />-Microsoft Store|
|Windows Update|**Un sitio de descarga de Microsoft basado en Internet:** Un sitio de Microsoft Internet que almacena y distribuye actualizaciones para equipos Windows (controladores de dispositivos) y sistemas operativos Windows.<br /><br />**servicio de equipo:** El nombre del servicio Windows Update que se ejecuta en los equipos. Windows Update detecta, descarga e instala actualizaciones en equipos Windows.<br /><br />En función de las configuraciones de equipo y Directiva, el agente de Windows Update puede descargar actualizaciones desde:<br /><br />-Microsoft Update<br />-Windows Update<br />-Microsoft Store<br />-Un servicio de actualización de Internet (red) (WSUS)<br /><br />los equipos que no están administrados en un entorno basado en WSUS normalmente usarán Windows Update para conectarse directamente a través de Internet-a Windows Update, Microsoft Update o Microsoft Store para obtener actualizaciones.|
|Cliente WSUS|Un equipo que recibe actualizaciones de un servicio de actualización de la intranet de WSUS.<br /><br />En el caso de directiva de grupo configuración que controla la interacción del usuario final con Actualizaciones automáticas: un usuario de un equipo en un entorno de WSUS.|
