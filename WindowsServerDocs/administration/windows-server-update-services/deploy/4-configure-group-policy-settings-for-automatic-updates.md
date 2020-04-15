---
title: 'Paso 4: configurar la directiva de grupo para Actualizaciones automáticas'
description: 'Tema de Windows Server Update Service (WSUS): configurar la directiva de grupo para Actualizaciones automáticas es el cuarto paso en un proceso de cuatro pasos para implementar WSUS'
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 62177d05-d832-4ea8-bca4-47a8cd34a19c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d678c139ae2327eeecdff2731f1edb57d358a28a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828848"
---
# <a name="step-4-configure-group-policy-settings-for-automatic-updates"></a>Paso 4: Configurar la directiva de grupo para las actualizaciones automáticas

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En un entorno de Active Directory, puedes usar la directiva de grupo para definir la forma en que los equipos y los usuarios (que en este documento se denominan clientes de WSUS) pueden interactuar con las actualizaciones de Windows para obtener actualizaciones automáticas de Windows Server Update Services (WSUS).

Este tema contiene dos secciones principales:

[La configuración de la directiva de grupo para las actualizaciones de cliente de WSUS](#group-policy-settings-for-wsus-client-updates), que proporciona guías prescriptivas y detalles de comportamiento sobre la configuración del programador de mantenimiento y Windows Update de la directiva de grupo que controlan la forma en que los clientes de WSUS pueden interactuar con Windows Update para obtener actualizaciones automáticas.

[La información complementaria](#supplemental-information) tiene las siguientes secciones:

-   [Acceso a la configuración de Windows Update en una directiva de grupo](#accessing-the-windows-update-settings-in-group-policy), que proporciona instrucciones generales sobre el uso del editor de administración de la directiva de grupo, así como información sobre el acceso a la configuración del programador de mantenimiento y las extensiones de la directiva de los servicios de actualización en la directiva de grupo.

-   [Cambios en WSUS relevantes para esta guía](#changes-to-wsus-relevant-to-this-guide): para los administradores familiarizados con WSUS 3.2 y las versiones anteriores, en esta sección se proporciona un breve resumen de las diferencias principales entre la versión actual y la anterior de WSUS relevante para esta guía.

-   [Términos y definiciones](#terms-and-definitions): definiciones de varios términos relacionados con WSUS y los servicios de actualización que se usan en esta guía.

## <a name="group-policy-settings-for-wsus-client-updates"></a>Configuración de directiva de grupo para actualizaciones del cliente de WSUS
En esta sección se proporciona información sobre las tres extensiones de la directiva de grupo. En estas extensiones encontrarás la configuración que puedes usar para configurar el modo en que los clientes de WSUS pueden interactuar con Windows Update para recibir actualizaciones automáticas.

-   [Configuración del equipo &gt; Configuración de la directiva de Windows Update](#computer-configuration--windows-update-policy-settings)

-   [Configuración del equipo &gt; Configuración de la directiva del programador de mantenimiento](#computer-configuration--maintenance-scheduler-policy-settings)

-   [Configuración de usuario &gt; Configuración de la directiva de Windows Update](#user-configuration--windows-update-policy-settings)

> [!NOTE]
> En este tema se da por supuesto que ya usas y estás familiarizado con la directiva de grupo. Si no estás familiarizado con la directiva de grupo, te recomendamos revisar la información de la sección [Información complementaria](#supplemental-information) de este documento antes de intentar configurar las opciones de directiva para WSUS.

### <a name="computer-configuration--windows-update-policy-settings"></a>Configuración del equipo > Configuración de la directiva de Windows Update
En esta sección se proporcionan detalles acerca de las siguientes configuraciones de directiva basadas en equipos:

-   [Permitir la instalación inmediata de Actualizaciones automáticas](#allow-automatic-updates-immediate-installation)

-   [Permitir a los usuarios que no son administradores recibir notificaciones de actualización](#allow-non-administrators-to-receive-update-notifications)

-   [Permitir las actualizaciones firmadas procedentes de una ubicación del servicio en Internet de Microsoft Update](#allow-signed-updates-from-an-intranet-microsoft-update-service-location)

-   [Frecuencia de detección de actualizaciones automáticas](#automatic-updates-detection-frequency)

-   [Configurar Actualizaciones automáticas](#configure-automatic-updates)

-   [Retrasar el reinicio para las instalaciones programadas](#delay-restart-for-scheduled-installations)

-   [No ajustar la opción predeterminada en Instalar actualizaciones y apagar en el cuadro de diálogo Cerrar Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [No mostrar la opción Instalar actualizaciones y apagar en el cuadro de diálogo Cerrar Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [Habilitar la orientación de cliente](#enable-client-side-targeting)

-   [Habilitar Administración de energía de Windows Update para que reactive automáticamente el equipo a fin de instalar actualizaciones programadas](#enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates)

-   [No reiniciar automáticamente para instalar actualizaciones automáticas programadas con usuarios que hayan iniciado sesión](#no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations)

-   [Volver a pedir la intervención del usuario para reiniciar con instalaciones programadas](#re-prompt-for-restart-with-scheduled-installations)

-   [Volver a programar las instalaciones programadas de actualizaciones automáticas](#reschedule-automatic-updates-scheduled-installations)

-   [Especificar la ubicación del servicio Microsoft Update en la intranet](#specify-intranet-microsoft-update-service-location)

-   [Activar actualizaciones recomendadas mediante Actualizaciones automáticas](#turn-on-recommended-updates-via-automatic-updates)

-   [Activar notificaciones de software](#turn-on-software-notifications)

En GPME, las directivas de Windows Update para la configuración basada en equipos se encuentran en la ruta de acceso: *PolicyName* > **Configuración del equipo** > **Directivas** > **Plantillas administrativas** > **Componentes de Windows** > **Windows Update**.

> [!NOTE]
> De forma predeterminada, estas opciones no están configuradas.

#### <a name="allow-automatic-updates-immediate-installation"></a>Permitir la instalación inmediata de Actualizaciones automáticas
Especifica si Actualizaciones automáticas instalará automáticamente las actualizaciones que no interrumpan los servicios de Windows o reinicien Windows.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Si la configuración de directiva Configurar Actualizaciones automáticas está establecida en **Deshabilitada**, esta directiva no surte efecto.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica que las actualizaciones no se instalan inmediatamente. Los administradores locales pueden cambiar esta configuración mediante el Editor de directivas de grupo local.|
|**Habilitado**|Especifica que Actualizaciones automáticas instala inmediatamente las actualizaciones una vez descargadas y listas para instalar.|
|**Deshabilitado**|Especifica que las actualizaciones no se instalan inmediatamente.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="allow-non-administrators-to-receive-update-notifications"></a>Permitir a los usuarios que no son administradores recibir notificaciones de actualización
Especifica si los usuarios que no son administradores recibirán notificaciones de actualización en función de la configuración de la directiva Configurar Actualizaciones automáticas.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Consulta detalles en la tabla siguiente.|

> [!NOTE]
> Si la configuración de directiva Configurar Actualizaciones automáticas está deshabilitada o no está configurada, esta configuración de directiva no surte efecto.

> [!IMPORTANT]
> A partir de Windows 8 y Windows RT, esta configuración de directiva está habilitada de forma predeterminada. En todas las versiones anteriores de Windows, está deshabilitada de forma predeterminada.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica que los usuarios verán siempre una ventana de control de cuentas y requieren permisos elevados para realizar estas tareas. Un administrador local puede cambiar esta configuración mediante el Editor de directivas de grupo local.|
|**Habilitado**|Especifica que las actualizaciones automáticas de Windows y Microsoft Update incluirán a los usuarios que no son administradores al determinar qué usuario con sesión iniciada recibirá notificaciones de actualización. Los usuarios que no son administradores podrán instalar todo el contenido de actualización opcional, recomendado e IMPORTANTE para el que recibieron una notificación. Los usuarios no verán una ventana de control de cuentas de usuario y no necesitan permisos elevados para instalar estas actualizaciones, excepto en el caso de las actualizaciones que contengan la interfaz de usuario, el contrato de licencia para el usuario final o los cambios de configuración de Windows Update.<p>Hay dos situaciones en las que el efecto de esta configuración depende del equipo operativo:<p>1.  **Ocultar** o **Restaurar** actualizaciones<br />2.  **Cancelar** una instalación de actualización<p>En Windows Vista o Windows XP, si esta configuración de directiva está habilitada, los usuarios no verán una ventana de control de cuentas de usuario y no necesitan permisos elevados para ocultar, restaurar ni cancelar actualizaciones.<p>En Windows Vista, si esta configuración de directiva está habilitada, los usuarios no verán una ventana de control de cuentas de usuario y no necesitan permisos elevados para ocultar, restaurar ni cancelar actualizaciones. Si esta configuración de directiva está habilitada, los usuarios no verán una ventana de control de cuentas y necesitan permisos elevados para ocultar, restaurar o cancelar actualizaciones.<p>En Windows 7, esta configuración de directiva no tiene ningún efecto. Los usuarios verán siempre una ventana de control de cuentas y necesitan permisos elevados para realizar estas tareas.<p>En Windows 8 y Windows RT, esta configuración de directiva no tiene ningún efecto.|
|**Deshabilitado**|Especifica que solo los administradores que han iniciado sesión reciben notificaciones de actualización. **Nota:** En Windows 8 y Windows RT, esta configuración de directiva está habilitada de forma predeterminada. En todas las versiones anteriores de Windows, está deshabilitada de forma predeterminada.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="allow-signed-updates-from-an-intranet-microsoft-update-service-location"></a>Permitir las actualizaciones firmadas procedentes de una ubicación del servicio en Internet de Microsoft Update
Especifica si Actualizaciones automáticas acepta actualizaciones que están firmadas por entidades que no sean de Microsoft cuando la actualización se encuentra en una ubicación del servicio Microsoft Update de la intranet.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Las actualizaciones de un servicio que no sean de un servicio de Microsoft Update de la intranet siempre deben estar firmadas por Microsoft y no se ven afectadas por esta configuración de directiva.

> [!NOTE]
> Esta directiva no se admite en Windows RT. La habilitación de esta directiva no tendrá ningún efecto en los equipos que ejecutan Windows RT.

**Opciones:** No hay opciones para esta configuración.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica que las actualizaciones de la ubicación del servicio Microsoft Update en la intranet deben estar firmadas por Microsoft.|
|**Habilitado**|Especifica que Actualizaciones automáticas acepta las actualizaciones recibidas a través de una ubicación del servicio Microsoft Update en la intranet si están firmadas por un certificado que se encuentra en el almacén de certificados Editores de confianza del equipo local.|
|**Deshabilitado**|Especifica que las actualizaciones de la ubicación del servicio Microsoft Update en la intranet deben estar firmadas por Microsoft.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="always-automatically-restart-at-the-scheduled-time"></a>Reiniciar automáticamente siempre en la hora programada
Especifica si un temporizador de reinicio siempre se iniciará inmediatamente después de que Windows Update instale actualizaciones IMPORTANTES, en lugar de notificar primero a los usuarios de la pantalla de inicio de sesión durante al menos dos días.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Si está habilitada la configuración de directiva No reiniciar automáticamente para instalar actualizaciones automáticas programadas con usuarios que hayan iniciado sesión, esta directiva no tiene efecto.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica que Windows Update no modificará el comportamiento de reinicio del equipo.|
|**Habilitado**|Especifica que un temporizador de reinicio siempre se iniciará inmediatamente después de que Windows Update instale actualizaciones IMPORTANTES, en lugar de notificar primero a los usuarios de la pantalla de inicio de sesión durante al menos dos días.<p>El temporizador de reinicio puede configurarse para que se inicie con cualquier valor entre 15 y 180 minutos. Cuando se agote el tiempo de espera del temporizador, el reinicio continuará incluso si el equipo tiene usuarios que han iniciado sesión.|
|**Deshabilitado**|Especifica que Windows Update no modificará el comportamiento de reinicio del equipo.|

**Opciones:** si esta opción está habilitada, puedes especificar la cantidad de tiempo que transcurrirá después de la instalación de las actualizaciones antes de que se produzca un reinicio forzado del equipo.

#### <a name="automatic-updates-detection-frequency"></a>Frecuencia de detección de actualizaciones automáticas
Especifica las horas que tardará Windows en determinar cuánto tiempo debe esperar antes de comprobar si hay actualizaciones disponibles. El tiempo de espera exacto se determina mediante el uso de las horas especificadas aquí menos cero hasta el veinte por ciento de las horas especificadas. Por ejemplo, si esta directiva se usa para especificar una frecuencia de detección de 20 horas, todos los clientes al cual se aplica esta directiva comprobarán si hay actualizaciones en cualquier lugar entre 16 y las 20 horas.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> La configuración de Especificar ubicación del servicio Microsoft Update en la intranet debe estar habilitada para que esta directiva tenga efecto.
>
> Si la configuración de directiva Configurar Actualizaciones automáticas está deshabilitada, esta directiva no surte efecto.

> [!NOTE]
> Esta directiva no se admite en Windows RT. La habilitación de esta directiva no tendrá ningún efecto en los equipos que ejecutan Windows RT.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica que Windows comprobará si hay actualizaciones disponibles en el intervalo predeterminado de 22 horas.|
|**Habilitado**|Especifica que Windows comprobará si hay actualizaciones disponibles en el intervalo especificado.|
|**Deshabilitado**|Especifica que Windows comprobará si hay actualizaciones disponibles en el intervalo predeterminado de 22 horas.|

**Opciones:** si esta opción está habilitada, puedes especificar el intervalo de tiempo (en horas) que Windows Update esperará antes de comprobar si hay actualizaciones.

#### <a name="configure-automatic-updates"></a>Configurar Actualizaciones automáticas
Especifica si las actualizaciones automáticas están habilitadas en este equipo.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

Si está habilitada, debes seleccionar una de las cuatro opciones proporcionadas en esta configuración de directiva de grupo.

Para usar esta configuración, selecciona **Habilitada** y, después, en **Opciones**, selecciona una de las opciones del parámetro **Configurar actualización automática** (2, 3, 4 o 5).

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica que el uso de actualizaciones automáticas no se especifica en el nivel de directiva de grupo. Sin embargo, un administrador del equipo puede seguir configurando las actualizaciones automáticas en el panel de control.|
|**Habilitado**|Especifica que Windows reconoce cuando el equipo está en línea y utiliza su conexión a Internet para buscar las actualizaciones disponibles en Windows Update.<p>Cuando está habilitada, los administradores locales podrán usar el panel de control de Windows Update para seleccionar una opción de configuración. Sin embargo, los administradores locales no podrán deshabilitar la configuración de las actualizaciones automáticas.<p>-   **2 - Notificar descarga y notificar instalación**<br />    Cuando Windows Update encuentra actualizaciones que se aplican al equipo, se notificará a los usuarios que las actualizaciones están listas para su descarga. Los usuarios pueden ejecutar Windows Update para descargar e instalar las actualizaciones disponibles.<br />-   **3 - Descargar automáticamente y notificar instalación** (configuración predeterminada)<br />    Windows Update encuentra las actualizaciones aplicables y las descarga en segundo plano. No se notifica al usuario ni se le interrumpe durante el proceso. Cuando se completen las descargas, se notificará a los usuarios que están listas para instalarse. A continuación, los usuarios pueden ejecutar Windows Update para instalar las actualizaciones descargadas.<br />-   **4 - Descargar automáticamente y programar la instalación**<br />    Puedes especificar la programación mediante las opciones de este directiva de grupo configuración. Si no se especifica ninguna programación, la programación predeterminada de todas las instalaciones será todos los días a las 3:00 a. m. Si alguna actualización requiere un reinicio para completar la instalación, Windows reiniciará el equipo automáticamente. (Si un usuario ha iniciado sesión en el equipo cuando Windows está listo para reiniciarse, se le notificará al usuario y se le dará la opción de retrasar el reinicio). **Nota:** A partir de Windows 8, puedes establecer las actualizaciones que se instalarán durante el mantenimiento automático en lugar de usar una programación específica vinculada a Windows Update. El mantenimiento automático instalará las actualizaciones cuando el equipo no esté en uso y evitará la instalación de actualizaciones cuando el equipo esté funcionando con batería. Si el mantenimiento automático no puede instalar actualizaciones en el plazo de unos días, Windows Update instalará las actualizaciones inmediatamente. A continuación, se notificará a los usuarios acerca de un reinicio pendiente. Un reinicio pendiente solo tendrá lugar si no hay ninguna posibilidad de pérdida accidental de datos.    Las opciones de programación se pueden especificar en la configuración del programador de mantenimiento de GPME, que se encuentran en la ruta de acceso, *PolicyName* > **Configuración del equipo** > **Directivas** > **Plantillas administrativas** > **Componentes de Windows** > **Programador de mantenimiento** > **Límite de activación del Mantenimiento automático**. Consulta la sección de esta referencia titulada: [Configuración del programador de mantenimiento](#computer-configuration--maintenance-scheduler-policy-settings) para establecer los detalles.    **5 - Permitir que el administrador local elija la opción**<br />Especifica si los administradores locales pueden usar el panel de control de Actualizaciones automáticas para seleccionar una opción de configuración de su elección, por ejemplo, si los administradores locales pueden elegir una hora de instalación programada.<br />    Los administradores locales no podrán deshabilitar la configuración de las actualizaciones automáticas.|
|**Deshabilitado**|Especifica que las actualizaciones de cliente que están disponibles desde el servicio Windows Update público deben descargarse manualmente desde Internet e instalarse.|

#### <a name="delay-restart-for-scheduled-installations"></a>Retrasar el reinicio para las instalaciones programadas
Especifica la cantidad de tiempo que Actualizaciones automáticas esperará antes de continuar con un reinicio programado.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta directiva solo es válida cuando Actualizaciones automáticas se configura para realizar instalaciones de actualizaciones programadas. Si la configuración de directiva Configurar Actualizaciones automáticas está deshabilitada, esta directiva no surte efecto.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica que, una vez instaladas las actualizaciones, el tiempo de espera predeterminado de quince minutos transcurrirá antes de que se produzca cualquier reinicio programado.|
|**Habilitado**|Especifica que, una vez finalizada la instalación, se producirá un reinicio programado después de haber expirado el número de minutos especificado.|
|**Deshabilitado**|Especifica que, una vez instaladas las actualizaciones, el tiempo de espera predeterminado de quince minutos transcurrirá antes de que se produzca cualquier reinicio programado.|

**Opciones:** si esta configuración está habilitada, puedes usar esta opción para especificar la cantidad de tiempo (en minutos) que Actualizaciones automáticas espera antes de continuar con un reinicio programado.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog"></a>No ajustar la opción predeterminada a "Instalar actualizaciones y apagar" en el cuadro de diálogo Cerrar Windows
Esta configuración de directiva te permite especificar si se permite la opción **Instalar actualizaciones y apagar** como opción predeterminada en el cuadro de diálogo **Cerrar Windows**.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta configuración de directiva no tiene ningún efecto si la configuración de directiva *PolicyName* > **Configuración del equipo** > **Directivas** > **Plantillas administrativas** > **Componentes de Windows** > **Windows Update** > **No mostrar la opción Instalar actualizaciones y apagar en el cuadro de diálogo Cerrar Windows** está habilitada.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica que **Instalar actualizaciones y apagar** será la opción predeterminada en el cuadro de diálogo **Cerrar Windows** si hay actualizaciones disponibles para su instalación en el momento en que el usuario selecciona la opción Apagar el equipo.|
|**Habilitado**|Si habilitas esta configuración de directiva, la última opción de apagado del usuario (por ejemplo, Hibernar o Reiniciar) es la opción predeterminada en el cuadro de diálogo **Cerrar Windows**, independientemente de si la opción **Instalar actualizaciones y apagar** está disponible en el menú **¿Qué quieres que haga tu equipo?** .|
|**Deshabilitado**|Especifica que **Instalar actualizaciones y apagar** será la opción predeterminada en el cuadro de diálogo **Cerrar Windows** si hay actualizaciones disponibles para su instalación en el momento en que el usuario selecciona la opción Apagar el equipo.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="do-not-connect-to-any-windows-update-internet-locations"></a>No conectarse a ninguna ubicación de Internet de Windows Update
Incluso cuando Windows Update está configurado para recibir actualizaciones de un servicio de actualización de la intranet, periódicamente recuperará información desde el servicio Windows Update público para permitir conexiones futuras a Windows Update y otros servicios como Microsoft Update o Microsoft Store.

Habilitar esta directiva deshabilitará la funcionalidad para recuperar periódicamente información del servicio de actualización de Windows Server Update Services y puede provocar que la conexión a servicios públicos como Microsoft Store deje de funcionar.

|Compatible en:|Excepto:|
|---------|-------|
|A partir de Windows Server 2012 R2, Windows 8.1 o Windows RT 8.1, los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta directiva solo se aplica cuando el equipo está configurado para conectarse a un servicio de actualización de intranet mediante la configuración de directiva Especificar la ubicación del servicio Microsoft Update en la intranet.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|El comportamiento predeterminado para recuperar información de Windows Server Update Service persiste.|
|**Habilitado**|Especifica que los equipos no recuperarán información de Windows Server Update Service.|
|**Deshabilitado**|El comportamiento predeterminado para recuperar información de Windows Server Update Service persiste.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog"></a>No mostrar la opción "Instalar actualizaciones y apagar" en el cuadro de diálogo Cerrar Windows
Especifica si la opción **Instalar actualizaciones y apagar** se muestra en el cuadro de diálogo **Cerrar Windows**.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica que la opción **Instalar actualizaciones y apagar** está disponible en el cuadro de diálogo **Cerrar Windows** si hay actualizaciones disponibles cuando el usuario selecciona la opción Apagar el equipo. Un administrador local puede cambiar esta configuración mediante una directiva local.|
|**Habilitado**|Especifica que la opción **Instalar actualizaciones y apagar** no aparecerá como una opción en el cuadro de diálogo **Cerrar Windows**, incluso si hay actualizaciones disponibles para su instalación cuando el usuario selecciona la opción Apagar el equipo.|
|**Deshabilitado**|Especifica que la opción **Instalar actualizaciones y apagar** será la predeterminada en el cuadro de diálogo **Cerrar Windows** si hay actualizaciones disponibles para la instalación en el momento en que el usuario selecciona la opción Apagar el equipo.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="enable-client-side-targeting"></a>Habilitar la orientación de cliente
Especifica el nombre o los nombres de grupo de destino que están configurados en la consola de WSUS y que van a recibir actualizaciones de WSUS.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Esta directiva solo se aplica cuando este equipo está configurado para admitir los nombres de grupo de destino especificados en WSUS. Si el nombre del grupo de destino no existe en WSUS, se omitirá hasta que se cree. Si la configuración de directiva Especificar la ubicación del servicio Microsoft Update en la intranet está deshabilitada o no configurada, esta directiva no tiene efecto.

> [!NOTE]
> Esta directiva no se admite en Windows RT. La habilitación de esta directiva no tendrá ningún efecto en los equipos que ejecutan Windows RT.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica que no se envía información de grupo de destino a WSUS. Un administrador local puede cambiar esta configuración mediante una directiva local.|
|**Habilitado**|Especifica que la información del grupo de destino especificado se envía a WSUS, que la usa para determinar las actualizaciones que se deben implementar en este equipo. Si WSUS admite varios grupos de destino, puedes usar esta directiva para especificar varios nombres de grupo, separados por punto y coma, si has agregado los nombres de grupo de destino en la lista de grupos de equipos de WSUS. De lo contrario, se debe especificar un solo grupo.|
|**Deshabilitado**|Especifica que no se envía información de grupo de destino a WSUS.|

**Opciones:** Usa este espacio para especificar uno o más nombres de grupos de destino.

#### <a name="enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates"></a>Habilitar Administración de energía de Windows Update para que reactive automáticamente el equipo a fin de instalar actualizaciones programadas
Especifica si Windows Update usará las características Administración de energía de Windows u Opciones de energía para reactivar automáticamente el equipo desde la hibernación si hay actualizaciones programadas para la instalación.

El equipo se reactivará automáticamente solo si Windows Update está configurado para instalar las actualizaciones automáticamente. Si el equipo está en hibernación cuando se produce la hora de instalación programada y se aplican actualizaciones, Windows Update usará las características Administración de energía de Windows u Opciones de energía para reactivar automáticamente el equipo a fin de instalar las actualizaciones. Windows Update también reactivará el equipo e instalará una actualización en caso de que se produzca una fecha límite de instalación.

El equipo no se reactivará a menos que se instalen actualizaciones. Si el equipo funciona con batería, cuando Windows Update lo reactive, no instalará actualizaciones y el equipo volverá automáticamente a hibernación en dos minutos.

|Compatible en:|Excepto:|
|---------|-------|
|A partir de Windows Vista y Windows Server 2008 (Windows 7), los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Windows Update no reactiva el equipo contra la hibernación para instalar actualizaciones. Un administrador local puede cambiar esta configuración mediante una directiva local.|
|**Habilitado**|Windows Update activa el equipo desde la hibernación para instalar actualizaciones en las condiciones mencionadas anteriormente.|
|**Deshabilitado**|Windows Update no reactiva el equipo contra la hibernación para instalar actualizaciones.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations"></a>No reiniciar automáticamente para instalar actualizaciones automáticas programadas con usuarios que hayan iniciado sesión
Especifica que para completar una instalación programada, Actualizaciones automáticas esperará a que cualquier usuario que haya iniciado sesión reinicie el equipo, en lugar de que el equipo se reinicie automáticamente.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta directiva solo es válida cuando Actualizaciones automáticas se configura para realizar instalaciones de actualizaciones programadas. Si la configuración de directiva Configurar Actualizaciones automáticas está deshabilitada, esta directiva no surte efecto.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica que Actualizaciones automáticas notificará al usuario de que el equipo se reiniciará automáticamente en cinco minutos para completar la instalación.|
|**Habilitado**|Algunas actualizaciones requieren que se reinicie el equipo para que las actualizaciones surtan efecto. Si el estado se establece en Habilitado, Actualizaciones automáticas no reiniciará un equipo automáticamente durante una instalación programada si un usuario ha iniciado sesión en el equipo. En su lugar, Actualizaciones automáticas notificará al usuario para que reinicie el equipo.|
|**Deshabilitado**|Especifica que Actualizaciones automáticas notificará al usuario de que el equipo se reiniciará automáticamente en cinco minutos para completar la instalación.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="re-prompt-for-restart-with-scheduled-installations"></a>Volver a pedir la intervención del usuario para reiniciar con instalaciones programadas
Especifica la cantidad de tiempo que Actualizaciones automáticas esperará antes de volver a pedir un reinicio programado.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!IMPORTANT]
> Esta directiva solo es válida cuando Actualizaciones automáticas se configura para realizar instalaciones de actualizaciones programadas. Si la configuración de directiva Configurar Actualizaciones automáticas está deshabilitada, esta directiva no surte efecto.

> [!NOTE]
> Esta directiva no tiene efecto en los equipos que ejecutan Windows RT.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Un reinicio programado se produce diez minutos después de que se descarta el mensaje de solicitud de reinicio. Un administrador local puede cambiar esta configuración mediante una directiva local.|
|**Habilitado**|Especifica que, tras posponer la solicitud de reinicio anterior, se producirá un reinicio programado una vez transcurrido el número de minutos especificado.|
|**Deshabilitado**|Un reinicio programado se produce diez minutos después de que se descarta el mensaje de solicitud de reinicio.|

**Opciones:** Cuando está habilitada, puedes usar esta opción de configuración para especificar (en minutos) la cantidad de tiempo que transcurrirá antes de que los usuarios vuelvan a solicitar un reinicio programado.

#### <a name="reschedule-automatic-updates-scheduled-installations"></a>Volver a programar las instalaciones programadas de actualizaciones automáticas
Especifica el tiempo que esperará Actualizaciones automáticas, tras el inicio del equipo, antes de continuar con una instalación programada que no se pudo ejecutar anteriormente.

Si el estado se establece en **No configurado**, se realizará una instalación programada no ejecutada un minuto después del siguiente reinicio del equipo.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta directiva solo es válida cuando Actualizaciones automáticas se configura para realizar instalaciones de actualizaciones programadas. Si la configuración de directiva Configurar Actualizaciones automáticas está deshabilitada, esta directiva no surte efecto.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica que se realizará una instalación programada no ejecutada un minuto después del siguiente reinicio del equipo.|
|**Habilitado**|Especifica que una instalación programada que no se pudo realizar con anterioridad se llevará a cabo tras el siguiente inicio, una vez trascurrido el número de minutos especificado.|
|**Deshabilitado**|Especifica que una instalación programada no ejecutada se llevará a cabo con la siguiente instalación programada.|

**Opciones:** Cuando esta configuración de directiva está habilitada, puedes usarla para especificar el número de minutos tras el siguiente inicio para que se lleve a cabo una instalación programada que no se pudo realizar con anterioridad.

#### <a name="specify-intranet-microsoft-update-service-location"></a>Especificar la ubicación del servicio Microsoft Update en la intranet
Indica a un servidor de la intranet que hospede actualizaciones de Microsoft Update. A continuación, puedes usar WSUS para actualizar automáticamente los equipos de la red.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT.|

Esta opción permite especificar un servidor WSUS en la red para que funcione como un servicio de actualización interno. En lugar de usar las actualizaciones de Microsoft en Internet, los clientes WSUS buscarán en este servicio actualizaciones que sean aplicables.

Para usar esta opción, debes establecer dos valores de nombre de servidor: el servidor desde el que el cliente detecta y descarga las actualizaciones y el servidor para que las estaciones de trabajo actualizados cargan estadísticas. Los valores no deben ser diferentes si ambos servicios están configurados en el mismo servidor.

> [!NOTE]
> Si la configuración de directiva Configurar Actualizaciones automáticas está deshabilitada, esta directiva no surte efecto.

> [!NOTE]
> Esta directiva no se admite en Windows RT. La habilitación de esta directiva no tendrá ningún efecto en los equipos que ejecutan Windows RT.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Si Actualizaciones automáticas no está deshabilitado por la directiva o por las preferencias del usuario, esta directiva especifica que los clientes se conectan directamente al sitio de Windows Update en Internet.|
|**Habilitado**|Especifica que el cliente se conecta al servidor WSUS especificado, en lugar de Windows Update, para buscar y descargar actualizaciones. Al habilitar esta configuración, los usuarios finales de la organización no tendrán que usar un firewall para obtener actualizaciones. Además, tendrás la posibilidad de probar las actualizaciones antes de implementarlas.|
|**Deshabilitado**|Si Actualizaciones automáticas no está deshabilitado por la directiva o por las preferencias del usuario, esta directiva especifica que los clientes se conectan directamente al sitio de Windows Update en Internet.|

**Opciones:** Cuando esta configuración de directiva está habilitada, debes especificar el servicio de actualización de la intranet que los clientes WSUS usarán al detectar actualizaciones, así como el servidor de estadísticas de Internet en el que los clientes WSUS actualizados cargarán las estadísticas. Valores de ejemplo:


|                    Opción de configuración:                    |    Valor de ejemplo:    |
|-------------------------------------------------------|----------------------|
| Establecer el servicio de actualización de la intranet para detectar actualizaciones |  http://wsus01:8530  |
|          Establecer el servidor de estadísticas de la intranet           | http://IntranetUpd01 |

#### <a name="turn-on-recommended-updates-via-automatic-updates"></a>Activar actualizaciones recomendadas mediante Actualizaciones automáticas
Especifica si Actualizaciones automáticas proporcionará las actualizaciones IMPORTANTES y recomendadas desde WSUS.

|Compatible en:|Excepto:|
|---------|-------|
|A partir de Windows Vista y Windows Server 2008 (Windows 7), los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica que Actualizaciones automáticas continuará proporcionando actualizaciones IMPORTANTES si ya está configurado para ello.|
|**Habilitado**|Especifica que Actualizaciones automáticas instalará las actualizaciones recomendadas y las actualizaciones IMPORTANTES desde WSUS.|
|**Deshabilitado**|Especifica que Actualizaciones automáticas continuará proporcionando actualizaciones IMPORTANTES si ya está configurado para ello.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="turn-on-software-notifications"></a>Activar notificaciones de software
Esta configuración de directiva te permite controlar si los usuarios verán mensajes de notificación mejorados y detallado del servicio Microsoft Update acerca de software destacado. Los mensajes de notificación mejorados destacan el valor y promueven la instalación y el uso de software opcional. Esta configuración de directiva está destinada a usarse en entornos poco administrados en los que se permite que el usuario final tenga acceso al servicio Microsoft Update.

Si no usas el servicio Microsoft Update, la configuración de directiva Notificaciones de software no surte efecto.

Si la configuración de directiva Configurar Actualizaciones automáticas está deshabilitada o no está configurada, la configuración de directiva Notificaciones de software no surte efecto.

|Compatible en:|Excepto:|
|---------|-------|
|A partir de Windows Server 2008 (Windows Vista) y Windows 7, los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> De manera predeterminada, esta configuración de directiva está deshabilitada.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|No se ofrecen mensajes para aplicaciones opcionales a los usuarios de equipos que ejecutan Windows 7. No se ofrecen mensajes para aplicaciones ni para actualizaciones opcionales a los usuarios de equipos que ejecutan Windows Vista. Un administrador local puede cambiar esta configuración mediante el Panel de control o una directiva local.|
|**Habilitado**|Si habilitas esta configuración de directiva, cuando haya software destacado disponible, aparecerá un mensaje de notificación en el equipo del usuario. El usuario puede hacer clic en la notificación para abrir Windows Update y obtener más información acerca del software o su instalación. El usuario también puede hacer clic en **Cerrar este mensaje** o **Mostrármelas más tarde** para aplazar la notificación según convenga.<p>En Windows 7, esta configuración de directiva controlará las notificaciones detalladas solo para aplicaciones opcionales. En Windows Vista, esta configuración de directiva controlará las notificaciones detalladas solo para aplicaciones y actualizaciones opcionales.|
|**Deshabilitado**|Especifica que los usuarios que ejecutan Windows 7 no recibirán mensajes de notificación detallados para aplicaciones opcionales y los usuarios que ejecutan Windows Vista no recibirán mensajes de notificación detallados para aplicaciones ni para actualizaciones opcionales.|

**Opciones:** No hay opciones para esta configuración.

### <a name="computer-configuration--maintenance-scheduler-policy-settings"></a>Configuración del equipo > Configuración de la directiva del programador de mantenimiento
En la opción Configurar Actualizaciones automáticas, seleccionaste la opción **4 - Descargar automáticamente y programar la instalación**. Puedes especificar la configuración del programador de mantenimiento en la GPMC para los equipos que ejecutan Windows 8 y Windows RT. Si no seleccionaste la opción 4 en el valor Configurar Actualizaciones automáticas, no es necesario configurar estas opciones para las actualizaciones automáticas. La configuración del programador de mantenimiento se encuentra en la ruta de acceso: *PolicyName* > **Configuración del equipo** > **Directivas** > **Plantillas administrativas** > **Componentes de Windows** > **Programador de mantenimiento**. La extensión del programador de mantenimiento de la directiva de grupo contiene las siguientes opciones:

-   [Límite de activación del Mantenimiento automático](#automatic-maintenance-activation-boundary)

-   [Retraso aleatorio del Mantenimiento automático](#automatic-maintenance-random-delay)

-   [Directiva de activación automática](#automatic-wakeup-policy)

#### <a name="automatic-maintenance-activation-boundary"></a>Límite de activación del Mantenimiento automático
Esta directiva te permite configurar la opción Límite de activación del Mantenimiento automático.

El límite de activación de mantenimiento es la hora programada diaria a la que se inicia el mantenimiento automático.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Este valor está relacionado con la opción 4 en **Configurar Actualizaciones automáticas**. Si no seleccionaste la opción 4 en **Configurar Actualizaciones automáticas**, no es necesario configurar esta opción.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Si esta configuración de directiva no está configurada, se aplicará la hora del día programada que se especifica en los equipos cliente en el panel de control **Centro de actividades** > **Mantenimiento automático**.|
|**Habilitado**|Al habilitar esta configuración de directiva, se invalida cualquier valor predeterminado o modificado configurado en los equipos cliente en **Panel de control** > **Centro de actividades** > **Mantenimiento automático** (o en algunas versiones de cliente, **Mantenimiento**).|
|**Deshabilitado**|Si estableces esta configuración de directiva en **Deshabilitada**, se aplicará la hora del día programada que se especifica en el **Centro de actividades** > **Mantenimiento automático**, en el Panel de control.|

#### <a name="automatic-maintenance-random-delay"></a>Retraso aleatorio del Mantenimiento automático
Esta configuración de directiva te permite configurar el retraso aleatorio del Mantenimiento automático.

El retraso aleatorio del mantenimiento es el tiempo máximo que se retrasará el inicio del Mantenimiento automático desde su límite de activación. Esta configuración es útil para las máquinas virtuales en las que el mantenimiento aleatorio puede ser un requisito de rendimiento.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Este valor está relacionado con la opción 4 en **Configurar Actualizaciones automáticas**. Si no seleccionaste la opción 4 en **Configurar Actualizaciones automáticas**, no es necesario configurar esta opción.

De forma predeterminada, cuando está habilitado, el retraso aleatorio del mantenimiento regular se establece en **PT4H**.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Se aplica un retraso aleatorio de cuatro horas para **Automático**.|
|**Habilitado**|El inicio del Mantenimiento automático se retrasará desde su límite de activación hasta el espacio de tiempo especificado.|
|**Deshabilitado**|No se aplica un retraso aleatorio en Mantenimiento automático.|

#### <a name="automatic-wakeup-policy"></a>Directiva de activación automática
Esta configuración de directiva te permite configurar la directiva de reactivación de Mantenimiento automático.

La directiva de reactivación de mantenimiento especifica si Mantenimiento automático debe llevar a cabo una solicitud de reactivación al equipo operativo para el mantenimiento programado diario.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Si la directiva de activación de energía del equipo operativo está deshabilitada explícitamente, esta configuración no surte efecto.

> [!NOTE]
> Este valor está relacionado con la opción 4 en **Configurar Actualizaciones automáticas**. Si no seleccionaste la opción 4 en **Configurar Actualizaciones automáticas**, no es necesario configurar esta opción.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Si no configuras esta configuración de directiva, se aplicará la opción de reactivación que se especifica en el panel de control **Centro de actividades** > **Mantenimiento automático**.|
|**Habilitado**|Si habilitas esta configuración de directiva, Mantenimiento automático intentará establecer una directiva de reactivación del sistema operativo y realizar una solicitud de reactivación para la hora del día programada, si es necesario.|
|**Deshabilitado**|Si deshabilitas esta configuración de directiva, se aplicará la opción de reactivación que se especifica en el panel de control **Centro de actividades** > **Mantenimiento automático**.|

### <a name="user-configuration--windows-update-policy-settings"></a>Configuración de usuario > Configuración de la directiva de Windows Update
En esta sección se proporcionan detalles acerca de las siguientes configuraciones de directiva basadas en el usuario:

-   [No mostrar la opción Instalar actualizaciones y apagar en el cuadro de diálogo Cerrar Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [No ajustar la opción predeterminada en Instalar actualizaciones y apagar en el cuadro de diálogo Cerrar Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [Desactivar el acceso al uso de todas las características de Windows Update](#remove-access-to-use-all-windows-update-features)

En la GPMC, la configuración de usuario para las actualizaciones automáticas del equipo se encuentra en la ruta de acceso: *PolicyName* > **Configuración del usuario** > **Directivas** > **Plantillas administrativas** > **Componentes de Windows** > **Windows Update**. Las opciones de configuración se muestran en el mismo orden en que aparecen en las extensiones Configuración del equipo y Configuración de usuario en la directiva de grupo, cuando se selecciona la pestaña **Configuración** de la directiva de Windows Update para ordenar alfabéticamente las opciones de configuración.

> [!NOTE]
> De forma predeterminada, a menos que se indique lo contrario, estas opciones no se configuran.

> [!TIP]
> En cada una de estas opciones, puedes usar los pasos siguientes para habilitar, deshabilitar o desplazarte por la configuración:

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog-box"></a>No mostrar la opción "Instalar actualizaciones y apagar" en el cuadro de diálogo Cerrar Windows
Especifica si la opción **Instalar actualizaciones y apagar** se muestra en el cuadro de diálogo **Cerrar Windows**.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica que la opción **Instalar actualizaciones y apagar** aparecerá en el cuadro de diálogo **Cerrar Windows** si hay actualizaciones disponibles cuando el usuario selecciona la opción Apagar el equipo.|
|**Habilitado**|Si habilitas esta configuración de directiva, **Instalar actualizaciones y apagar** no aparecerá como una opción en el cuadro de diálogo **Cerrar Windows**, incluso si hay actualizaciones disponibles para su instalación cuando el usuario selecciona la opción Apagar el equipo.|
|**Deshabilitado**|Especifica que la opción **Instalar actualizaciones y apagar** aparecerá en el cuadro de diálogo **Cerrar Windows** si hay actualizaciones disponibles cuando el usuario selecciona la opción Apagar el equipo.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog-box"></a>No ajustar la opción predeterminada en Instalar actualizaciones y apagar en el cuadro de diálogo Cerrar Windows
Especifica si la opción **Instalar actualizaciones y apagar** se permite como opción predeterminada en el cuadro de diálogo **Cerrar Windows**.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

> [!NOTE]
> Esta configuración de directiva no tiene ningún efecto si la opción *PolicyName* > **Configuración de usuario** > **Directivas** > **Plantillas administrativas** > **Componentes de Windows** > **Windows Update** > **No mostrar la opción Instalar actualizaciones y apagar en el cuadro de diálogo Cerrar Windows** está habilitada.

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Especifica si la opción **Instalar actualizaciones y apagar** será la predeterminada en el cuadro de diálogo **Cerrar Windows** si hay actualizaciones disponibles para la instalación en el momento en que el usuario selecciona la opción Apagar el equipo.|
|**Habilitado**|Especifica si la última opción de apagado del usuario (por ejemplo, Hibernar o Reiniciar) es la opción predeterminada en el cuadro de diálogo **Cerrar Windows**, independientemente de si la opción **Instalar actualizaciones y apagar** está disponible en el menú **¿Qué quieres que haga tu equipo?** .|
|**Deshabilitado**|Especifica si la opción **Instalar actualizaciones y apagar** será la predeterminada en el cuadro de diálogo **Cerrar Windows** si hay actualizaciones disponibles para la instalación en el momento en que el usuario selecciona la opción Apagar el equipo.|

**Opciones:** No hay opciones para esta configuración.

#### <a name="remove-access-to-use-all-windows-update-features"></a>Desactivar el acceso al uso de todas las características de Windows Update
Esta configuración te permite quitar el acceso de cliente de WSUS a Windows Update.

|Compatible en:|Excepto:|
|---------|-------|
|Los sistemas operativos Windows que aún están dentro de su [ciclo de vida de soporte técnico de los productos de Microsoft](https://support.microsoft.com/gp/lifeselect).|nulo|

|||
|-|-|
|**Estado de configuración de la directiva**|**Comportamiento**|
|**No configurado**|Los usuarios pueden conectarse al sitio web de Windows Update.|
|**Habilitado**|**IMPORTANTE:** si se habilita, se quitarán todas las características de Windows Update. Esto incluye bloquear el acceso al sitio web de Windows Update en https://windowsupdate.microsoft.com, desde el hipervínculo de Windows Update del menú Inicio o la pantalla Inicio, y también del menú **Herramientas** en Internet Explorer. También se deshabilita la actualización automática de Windows; no se notificará al usuario ni recibirá actualizaciones críticas de Windows Update. Esta opción impide además que Administrador de dispositivos instale automáticamente actualizaciones de controladores del sitio web de Windows Update.<p>Cuando está habilitada, puedes configurar una de las siguientes opciones de notificación:<p>-   **0 - No mostrar ninguna notificación**<br />    Esta opción quitará cualquier acceso a las características de Windows Update y no se mostrarán notificaciones.<br />-   **1 - Mostrar las notificaciones necesarias para reiniciar**<br />    Esta opción mostrará las notificaciones sobre las veces que es necesario reiniciar para completar la instalación. **Nota:** En los equipos que ejecutan Windows 8 y Windows RT, si se habilita esta directiva, solo se mostrarán las notificaciones relacionadas con los reinicios y la imposibilidad de detectar actualizaciones. Las opciones de notificaciones no se admiten. Las notificaciones de la pantalla Inicio de sesión siempre se mostrarán.|
|**Deshabilitado**|Los usuarios pueden conectarse al sitio web de Windows Update.|

**Opciones:** Consulta **Habilitada** en la tabla para esta configuración.

## <a name="supplemental-information"></a>Información complementaria
En esta sección se proporciona información adicional acerca del uso de opciones de apertura y guardado de WSUS en directivas de grupo, así como las definiciones de los términos que se usan en esta guía. Para los administradores familiarizados con las versiones anteriores de WSUS (WSUS 3.2 y versiones anteriores), hay una tabla que resume brevemente las diferencias entre las versiones de WSUS.

### <a name="accessing-the-windows-update-settings-in-group-policy"></a>Acceso a la configuración de Windows Update en una directiva de grupo
En el procedimiento siguiente se describe cómo abrir GPMC en el controlador de dominio. A continuación, el procedimiento describe cómo abrir un objeto de directiva de grupo (GPO) existente de nivel de dominio para editarlo, o cómo crear un nuevo GPO de nivel de dominio y abrirlo para su edición.

> [!NOTE]
> Para realizar este procedimiento, debes ser miembro del grupo **Admins. del dominio** o uno equivalente.

##### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Para abrir o agregar y abrir un objeto directiva de grupo

1.  En el controlador de dominio, ve a **Administrador del servidor**, > **Herramientas**, > **Administración de directivas de grupo**. Se abre la Consola de administración de directivas de grupo.

2.  En el panel izquierdo, expande el bosque. Por ejemplo, haz doble clic en **bosque: example.com**.

3.  En el panel izquierdo, haz doble clic en **Dominios** y, a continuación, haz doble clic en el dominio para el que deseas administrar un objeto de directiva de grupo. Por ejemplo, haz doble clic en **example.com**.

4.  Realice una de las siguientes acciones:

    -  **Si deseas abrir un GPO de nivel de dominio existente para editarlo**, haz doble clic en el dominio que contiene el objeto de directiva de grupo que quieres administrar, haz clic con el botón derecho en la directiva de dominio que deseas administrar y, a continuación, haz clic en **Editar**. Se abre el Editor de administración de directivas de grupo (GPME).

    -  **Para crear un nuevo objeto directiva de grupo y abrirlo para editarlo**:
        1.  Haz clic con el botón derecho en el dominio para el que deseas crear un nuevo objeto de directiva de grupo y, después, haz clic en **Crear un GPO en este dominio y vincularlo aquí**.

        2.  En el cuadro **Nuevo GPO**, en **Nombre**, escribe un nombre para el nuevo objeto de directiva de grupo y, a continuación, haz clic en **Aceptar**.

        3.  Haz clic con el botón derecho en el nuevo objeto de directiva de grupo y, después, selecciona **Editar**. Se abre GPME.

##### <a name="to-open-the-windows-update-or-maintenance-scheduler-extensions-of-group-policy"></a>Para abrir las extensiones Windows Update o Programador de mantenimiento de la directiva de grupo

1.  En el Editor de administración de directivas de grupo, realiza una de las acciones siguientes:

    -   **Abre la extensión Configuración del equipo > Windows Update de la directiva de grupo**. Vaya a: *PolicyName* > **Configuración del equipo** > **Directivas** / **Plantillas administrativas** > **Componentes de Windows** > **Windows Update**.

    -   **Abre la extensión Configuración de usuario > Windows Update de la directiva de grupo**. Vaya a: *PolicyName* > **Configuración del usuario** > **Directivas** > **Plantillas administrativas** > **Componentes de Windows** > **Windows Update**.

    -   **Abre la extensión Configuración del equipo > Programador de mantenimiento de la directiva de grupo**. En GPOE, navega a *PolicyName* > **Configuración del equipo** > **Directivas** > **Plantillas administrativas** > **Componentes de Windows** > **Programador de mantenimiento**.

Si deseas obtener más información sobre la directiva de grupo, consulta [Información general sobre directivas de grupo](https://technet.microsoft.com/library/hh831791.aspx(v=ws.12)).

> [!TIP]
> Después de abrir la extensión de directiva de grupo que deseas, puedes usar los pasos siguientes para habilitar, deshabilitar o desplazarte por las opciones de configuración:

##### <a name="to-configure-group-policy-settings"></a>Para definir la configuración de directiva de grupo

1.  En *ExtensionOfGroupPolicy*, haz doble clic en el valor que deseas ver o modificar.

2.  Para configurar la opción, realiza una de las acciones siguientes:

    -   Para conservar el estado predeterminado no especificado de la configuración, selecciona **No configurado**.

    -   Para habilitar la opción, selecciona **Habilitado**.

    -   Para deshabilitar la opción, selecciona **Deshabilitado**.

3.  En **Opciones**, si aparece alguna opción, conserva los valores predeterminados o modifícalos según sea necesario.

4.  Realice una de las siguientes acciones:

    -   Para guardar los cambios y continuar con la configuración siguiente, haz clic en **Aplicar** y, después, en **Valor siguiente**.

    -   Para guardar los cambios y cerrar el cuadro de diálogo, haz clic en **Aceptar**.

    -   Para descartar los cambios no guardados y cerrar el cuadro de diálogo, haz clic en **Cancelar**.

### <a name="changes-to-wsus-relevant-to-this-guide"></a>Cambios en WSUS relevantes para esta guía
En la tabla siguiente se resumen las diferencias principales entre las versiones actuales y anteriores de WSUS que son relevantes para esta guía.

|Versiones anteriores de Windows Server y WSUS|Descripción|
|------------------|--------|
| Windows Server 2012 R2 con WSUS 6.0 y versiones posteriores|A partir de Windows Server 2012, el rol del servidor de WSUS está integrado con el sistema operativo y la configuración de directiva de grupo asociada de los clientes WSUS se incluye, de forma predeterminada, en la directiva de grupo.|
| Windows Server 2008 (y versiones anteriores de Windows Server) con WSUS 3.2 y versiones anteriores|En Windows Server 2008 (y versiones anteriores de Windows Server) con las versiones de WSUS 3.2 (y versiones anteriores), la configuración de directiva de grupo que rige los clientes WSUS no se incluye en estos sistemas operativos de Windows Server. La configuración de directiva está en la plantilla administrativa de WSUS, **wuau. adm**. En estas versiones de servidor, primero se debe agregar la plantilla administrativa de WSUS en la Consola de administración de directivas de grupo (GPMC) para poder configurar la configuración del cliente WSUS.|

### <a name="terms-and-definitions"></a>Términos y definiciones
A continuación se muestra una lista de los términos utilizados en esta guía.

|Término|Definición|
|----|-------|
|Actualizaciones automáticas|**Un servicio que se ejecuta en equipos Windows** (Actualizaciones automáticas): Hace referencia al componente de equipo cliente integrado en los sistemas operativos Microsoft Windows Vista, Windows Server 2003, Windows XP y Windows 2000 con SP3 para obtener actualizaciones de Microsoft Update o Windows Update.<p>**Referencia casual** (actualizaciones automáticas): El término que se usa para describir cuándo el Agente de Windows Update programa y descarga automáticamente las actualizaciones.|
|servidor autónomo|Se usa para hacer referencia a un servidor de Windows Server Update Services (WSUS) que sigue en la cadena en el que los administradores pueden administrar componentes de WSUS.|
|servidor que sigue en la cadena|Se usa para hacer referencia a un servidor de Windows Server Update Services (WSUS) que obtiene actualizaciones desde otro servidor WSUS y no desde Microsoft Update o Windows Update.|
|Extensión de directiva de grupo (y: extensión de directiva de grupo)|Colección de valores de directiva de grupo que se usan para controlar el modo en que los usuarios y equipos (a los que se aplican las directivas) pueden configurar y usar diversos servicios y características de Windows. Los administradores pueden usar WSUS con directiva de grupo para la configuración del cliente de Actualizaciones automáticas, a fin de ayudar a garantizar que los usuarios finales no puedan deshabilitar o ignorar las directivas de actualización corporativas.<p>WSUS no requiere el uso de Active Directory o directiva de grupo. La configuración del cliente también se puede aplicar mediante la directiva de grupo local o modificando el registro de Windows.|
|servicio de actualización interno|Una referencia casual a una infraestructura de red que usa uno o más servidores WSUS para distribuir actualizaciones.|
|servidor de réplicas|Se usa para hacer referencia a un servidor de Windows Server Update Services (WSUS) que sigue en la cadena y que refleja las aprobaciones y la configuración del servidor que precede en la cadena al que está conectado. No puedes administrar WSUS en un servidor de réplicas.|
|Microsoft Update|**Un sitio de descarga de Microsoft basado en Internet:** Un sitio de Internet de Microsoft que almacena y distribuye actualizaciones para equipos Windows (controladores de dispositivos), sistemas operativos Windows y otros productos de software de Microsoft.|
|Servicios de actualización de software (SUS)|SUS era el producto anterior para Windows Server Update Services (WSUS).|
|actualizaciones|Cualquier elemento de una colección de revisiones de software, revisiones, Service Pack, paquetes de características y controladores de dispositivos que se pueden instalar en un equipo para ampliar la funcionalidad o para mejorar el rendimiento y la seguridad.|
|archivos de actualización|Los archivos necesarios para instalar una actualización en un equipo.|
|información de actualización (se conoce también como metadatos de actualización)|La información acerca de una actualización, en lugar de los archivos binarios de actualización de un paquete de actualización. Por ejemplo, los metadatos proporcionan información para las propiedades de una actualización, lo que te permite averiguar cuál es la utilidad de actualización. Los metadatos también incluyen términos de licencia del software de Microsoft. El paquete de metadatos descargado para una actualización suele tener un tamaño bastante menor que el del paquete de archivos de actualización real.|
|origen de la actualización|La ubicación en la que se sincroniza un servidor de Windows Server Update Services (WSUS) para obtener los archivos de actualización. Esta ubicación puede ser Microsoft Update o un servidor WSUS que precede en la cadena.|
|servidor que precede en la cadena|Un servidor de Windows Server Update Services (WSUS) que proporciona archivos de actualización a otro servidor WSUS, que a su vez se conoce como servidor que sigue en la cadena.|
|Windows Server Update Services (WSUS)|Programa de rol de servidor que se ejecuta en uno o más equipos con Windows Server en una red corporativa. Una infraestructura de WSUS te permite administrar las actualizaciones de los equipos de la red que se van a instalar.<p>Puedes usar WSUS para aprobar o rechazar actualizaciones antes de la versión, para forzar la instalación de actualizaciones en una fecha determinada y para obtener informes completos sobre qué actualizaciones requiere cada equipo de la red. Puedes configurar WSUS para que apruebe ciertas clases de actualizaciones automáticamente (actualizaciones críticas, actualizaciones de seguridad, Service Pack, controladores, etc.). WSUS también te permite aprobar actualizaciones solo para la detección, de modo que puedas ver qué equipos requerirán una actualización determinada sin tener que instalar las actualizaciones.<p>En una implementación WSUS, como mínimo, un servidor WSUS de la red debe poder conectarse a Microsoft Update para obtener actualizaciones disponibles. En función de la configuración y seguridad de la red, el administrador puede determinar cuántos servidores más se conectan directamente a Microsoft Update.<p>Puedes configurar un servidor WSUS para obtener actualizaciones a través de Internet desde lugares como:<p>- el sitio web público de Microsoft Update<br />- el sitio web público de Windows Update<br />-   Microsoft Store|
|Windows Update|**Un sitio de descarga de Microsoft basado en Internet:** Un sitio de Internet de Microsoft que almacena y distribuye actualizaciones para equipos Windows (controladores de dispositivos) y sistemas operativos Windows.<p>**servicio de equipo:** El nombre del servicio Windows Update que se ejecuta en los equipos. Windows Update detecta, descarga e instala actualizaciones en equipos Windows.<p>En función de las configuraciones de equipo y directiva, el Agente de Windows Update puede descargar actualizaciones desde:<p>-   Microsoft Update<br />-   Windows Update<br />-   Microsoft Store<br />- Un servicio de actualización de Internet (red) (WSUS)<p>Los equipos que no están administrados en un entorno basado en WSUS normalmente usarán Windows Update para conectarse directamente (a través de Internet) a Windows Update, Microsoft Update o Microsoft Store para obtener actualizaciones.|
|Cliente WSUS|Un equipo que recibe actualizaciones de un servicio de actualización de la intranet de WSUS.<p>En el caso de la configuración de directiva de grupo que controla la interacción del usuario final con Actualizaciones automáticas: un usuario de un equipo en un entorno de WSUS.|
