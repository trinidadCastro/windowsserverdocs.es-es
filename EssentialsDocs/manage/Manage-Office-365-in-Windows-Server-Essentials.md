---
title: Administrar Microsoft 365 en Windows Server Essentials
description: Obtenga información acerca de cómo administrar los servicios de Microsoft 365 y las cuentas en línea junto con los recursos locales desde el panel de Windows Server Essentials.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 3f8485e4-e10f-4f38-8a5e-d5227abd0d84
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 1df50d94d8ec118742d089c11afbc5fff17f9800
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811362"
---
# <a name="manage-microsoft-365-in-windows-server-essentials"></a>Administrar Microsoft 365 en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Al integrar el servidor de Windows Server Essentials con Microsoft 365, puede administrar los servicios de Microsoft 365 y las cuentas en línea junto con los recursos locales desde el panel de Windows Server Essentials. En este tema, descubrirá lo que puede obtener integrando el servidor con Microsoft 365, cómo hacerlo y cómo administrar y solucionar problemas de la integración de Microsoft 365.



> [!IMPORTANT]
>   La integración de Microsoft 365 solo se admite en un entorno de controlador de dominio único. Además, el Asistente para la integración de Microsoft 365 debe ejecutarse en un controlador de dominio.

## <a name="in-this-topic"></a>En este tema

-   [¿Por qué debo integrar Microsoft 365 con mi servidor?](#BKMK_IntegrationOverview)

-   [Configuración de la integración de Microsoft 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)

-   [Administrar la integración de Microsoft 365](#BKMK_ManageIntegration)

-   [Solucionar problemas de integración de Microsoft 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)

##  <a name="why-should-i-integrate-microsoft-365-with-my-server"></a><a name="BKMK_IntegrationOverview"></a> ¿Por qué debo integrar Microsoft 365 con mi servidor?
 Hay muchas razones para integrar Microsoft 365 con el servidor de Windows Server Essentials. Si administra algunos de los recursos internos, pero usa Microsoft 365 para otros servicios, podrá administrar los servicios y recursos de Microsoft 365 desde el panel, junto con sus recursos locales, en lugar de trabajar en dos lugares.

- Administrar las cuentas en línea que proporcionan a los usuarios acceso a Microsoft 365 junto con las cuentas de usuario:

  -   Cree cuentas de Microsoft Online Services para los usuarios en un solo paso, o cree cuentas de usuario en el servidor para las cuentas existentes en línea. También puede agregar una cuenta en línea para una cuenta de usuario nueva o existente.

       Al crear las cuentas en línea desde el panel, los usuarios inician sesión en Microsoft 365 con la misma contraseña que usan en el servidor. Si cambia la contraseña de su cuenta de usuario, también cambia la contraseña en línea. Así estará seguro de que sus contraseñas de cuenta en línea siempre cumplan los requisitos de seguridad establecidos para las cuentas de usuario.

  -   Administre una cuenta en línea junto con la cuenta de usuario durante el ciclo de vida de la cuenta de usuario. Si desactiva una cuenta de usuario, también se desactiva la cuenta en línea. Si quita una cuenta de usuario, también se quita la cuenta en línea.

  -   En un servidor de Windows Server Essentials, también puede administrar los grupos de distribución de Exchange Online para el correo electrónico.

- Envíe y reciba correo electrónico desde el dominio de Internet de su organización (por ejemplo, contoso.com) vinculando un dominio de Internet personalizado a su suscripción de Microsoft 365.

- Administrar la suscripción y la integración de Microsoft 365 desde el panel.

- Si la suscripción incluye bibliotecas de SharePoint Online, la integración de Microsoft 365 con un servidor de Windows Server Essentials le permite:

  - Cree y administre sus bibliotecas de SharePoint Online desde el panel.

    > [!NOTE]
    >  También podrá usar la aplicación My Server 2012 R2 para trabajar con documentos en las bibliotecas de SharePoint Online desde su portátil, dispositivo móvil o Windows Phone. Esta característica solo está disponible para Windows Server Essentials. Para obtener más información, consulte [usar la aplicación My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).

  - Cambiar los permisos de un sitio de grupo de SharePoint Online desde el panel o abrir el sitio del equipo desde el panel para realizar otros cambios.

    Para obtener más información, vea [administrar SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md).

- Si se suscribe a Exchange Online, la integración de Microsoft 365 con un servidor de Windows Server Essentials le permite administrar los dispositivos móviles que los usuarios usan para conectarse al servidor de correo electrónico de la empresa:

  -   Exija la protección con contraseña cuando los dispositivos móviles se conecten al servidor de correo electrónico de la compañía. Establezca una longitud mínima de contraseña, el número de intentos incorrectos de inicio de sesión permitidos y el tiempo mínimo necesario entre estos intentos.

  -   Impida que un dispositivo móvil se conecte a Exchange Online si hay problemas de seguridad con el modelo del dispositivo.

  -   En caso de pérdida o robo de un dispositivo móvil, borre el dispositivo para eliminar los datos confidenciales de la compañía la próxima vez que el dispositivo esté activado.

- La integración de Microsoft 365 proporciona nuevas formas de conectarse a Microsoft 365 servicios y recursos:

  -   Abra Microsoft 365 Services desde el Launchpad de Windows Server Essentials. Para obtener más información, vea [Guía de inicio rápido para usar Microsoft 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).

  -   Use la aplicación My Server 2012 R2 para trabajar con documentos en las bibliotecas de SharePoint Online desde su portátil, dispositivo móvil o Windows Phone. Para obtener información, consulte [usar la aplicación My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md). Esta característica solo está disponible en Windows Server Essentials.

##  <a name="set-up-microsoft-365-integration"></a><a name="BKMK_Configure"></a> Configuración de la integración de Microsoft 365
 Puede integrar el servidor con Microsoft 365 en cualquier momento después de completar la instalación del servidor. Si aún no tiene una suscripción a Microsoft 365, puede adquirir una suscripción de evaluación gratuita.

 Realizará las tareas siguientes:

-   [Paso 1: comprobar los requisitos de integración de Microsoft 365](#BKMK_StepOne_VERIFY)

-   [Paso 2: integrar el servidor con Microsoft 365](#BKMK_StepTwo)

-   [Paso 3: vincular el nombre de dominio de Internet de la organización a Microsoft 365 (opcional)](#BKMK_StepThree)

###  <a name="step-1-verify-microsoft-365-integration-requirements"></a><a name="BKMK_StepOne_VERIFY"></a> Paso 1: comprobar los requisitos de integración de Microsoft 365
 Antes de empezar, asegúrese de que el servidor cumpla con estos requisitos:

-   El servidor puede tener cualquiera de estos sistemas operativos: Windows Server Essentials, Windows Server Essentials o el sistema operativo Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con el rol experiencia con Windows Server Essentials instalado.

-   El entorno solo puede tener un controlador de dominio y debe realizar la integración de Microsoft 365 en el controlador de dominio.

-   Debe ser capaz de conectarse a Internet desde el servidor.

-   Debe instalar todas las actualizaciones críticas e importantes en el servidor antes de iniciar la integración de Microsoft 365.

-   Si desea usar el dominio de Internet de su organización en direcciones de correo electrónico y con sus recursos de SharePoint Online, deberá registrar el nombre de dominio para que pueda vincular el dominio a Microsoft 365 durante la integración. Para obtener más información, consulte [Cómo adquirir nombres de dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).

> [!NOTE]
>  No es necesario suscribirse a Microsoft 365 de antemano. Podrá comprar una suscripción o registrarse para obtener una evaluación gratuita durante la integración de Microsoft 365. Si desea echar un vistazo a los planes y los precios de Microsoft 365, [compare Microsoft 365 planes para empresas](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text).

###  <a name="step-2-integrate-the-server-with-microsoft-365"></a><a name="BKMK_StepTwo"></a> Paso 2: integrar el servidor con Microsoft 365
 Realice el procedimiento siguiente en el controlador de dominio para integrar el servidor de Windows Server Essentials con Microsoft 365.

> [!NOTE]
>  El procedimiento es el mismo si tiene Windows Server Essentials o Windows Server Essentials, pero comenzará desde un lugar diferente en la Página principal. En Windows Server Essentials, el servidor se integra con Microsoft 365 y otros servicios de Microsoft Online Services en la pestaña **servicios** . En Windows Server Essentials, la integración de Microsoft 365 se realiza en la pestaña **correo electrónico** .

##### <a name="to-integrate-the-server-with-microsoft-365"></a>Para integrar el servidor con Microsoft 365

1. Inicie sesión en el servidor como administrador y abra el panel de Windows Server Essentials.

2. En la página **principal** , haga clic en **servicios** (en Windows Server Essentials, haga clic en **correo electrónico**), en **integrar con Microsoft 365** y, a continuación, haga clic en **configurar la integración de Microsoft 365**.

    Aparece el Asistente para la integración con Microsoft 365.

3. En la página **Comenzar**, realice una de las acciones siguientes:

   -   Si no tiene una suscripción a Microsoft 365, haga clic en **siguiente** y siga las instrucciones para suscribirse a Microsoft 365 o registrarse para obtener una suscripción de prueba.

        Deberá iniciar sesión en Microsoft 365 antes de volver al asistente. Pero no es necesario realizar ninguna de las tareas de la sección **iniciar aquí** del portal de Microsoft 365.

   -   Si ya tiene una suscripción a Microsoft 365 que desea integrar con el servidor, seleccione **ya tengo una suscripción para Microsoft 365** y, a continuación, haga clic en **siguiente**.

4. Siga las instrucciones para completar el asistente.

   Cuando el asistente finalice correctamente, observará los siguientes cambios en el panel:

-   Hay una nueva página de **Microsoft 365** , que se usa para administrar la integración y la suscripción a Microsoft 365.

-   En la página **usuarios** , verá una pestaña **cuentas en línea** en la que puede crear y administrar las cuentas de Microsoft Online Services que proporcionan a los usuarios acceso a Microsoft 365. Si usa Exchange Online y tiene un servidor de Windows Server Essentials, también verá una pestaña **grupos de distribución** .

-   La página **almacenamiento** de un servidor de Windows Server Essentials tiene una pestaña **bibliotecas de SharePoint** para administrar las bibliotecas de SharePoint Online y cambiar los permisos de los sitios de grupo. Cada plan de negocio para Microsoft 365 incluye estas características básicas de SharePoint Online.

###  <a name="step-3-link-your-organizations-internet-domain-name-to-microsoft-365-optional"></a><a name="BKMK_StepThree"></a> Paso 3: vincular el nombre de dominio de Internet de la organización a Microsoft 365 (opcional)
 Si desea usar su propio dominio de Internet en el correo electrónico dirigido a su organización y las direcciones URL de los recursos de SharePoint Online, puede vincular un dominio personalizado a su suscripción de Microsoft 365. Si integra su servidor de Windows Server Essentials con Microsoft 365, puede hacerlo desde el panel.

 Es más fácil hacerlo antes de crear cuentas en línea para los usuarios, de modo que pueda usar el dominio cuando cree de forma masiva las cuentas en línea.

 Al suscribirse a Microsoft 365, obtendrá un nombre de dominio, por ejemplo, *contoso*. onmicrosoft.com. Si prefiere usar un nombre de dominio diferente, Imagine que simplemente contoso.com? puede. Tendrá que comprar un nombre de dominio si no dispone de uno y cambiar algunos de los registros DNS.

 La configuración de un dominio personalizado para usarlo con Microsoft 365 implica cuatro tareas:

1. **Compre un nombre de dominio.** Es decir, regístrese en un registrador de dominios o un proveedor de hospedaje de DNS.

   -   Elija un nombre de dominio que funcione con Microsoft 365. Puede usar un nombre de dominio de segundo nivel, por ejemplo, buycontoso.com? pero no un nombre de dominio de tercer nivel, por ejemplo, marketing.contoso.com. Para obtener más información sobre cómo elegir un dominio para usarlo en Microsoft 365, consulte [dominios](/office365/servicedescriptions/office-365-platform-service-description/domains).

   -   Cómprelo desde un registrador de dominios que permita los registros del servidor de nombres de dominio (DNS) necesarios para Microsoft 365. Para averiguar qué registradores de dominios admiten los registros DNS necesarios, consulte [Cómo adquirir nombres de dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660). Si ya ha registrado su dominio con un registrador diferente, no se preocupe; puede transferir el dominio a un registrador diferente cuando vincule el dominio a Microsoft 365.

2. **Configure los registros DNS que permitan que Microsoft 365 Services utilicen el nombre de dominio.** La manera más fácil es dejar que el asistente configure los registros DNS cuando vincule el dominio a su suscripción de Microsoft 365 en el paso 3. Si prefiere hacerlo usted mismo, consulte [Cómo configurar manualmente los registros DNS para la integración de Microsoft 365](#BKMK_ManuallyConfigureDNS).

3. **Vincule su dominio de Internet personalizado a su suscripción de Microsoft 365.** Usará la acción **vincular un dominio a Microsoft 365** .

4. **Compruebe que los servicios de Microsoft 365 estén usando el nuevo nombre de dominio.**

   Si completa los pasos 1 y 2 antes de usar la tarea **vincular un dominio a Microsoft 365** , el asistente puede vincular el nombre de dominio a Microsoft 365. También puede usar el asistente, que le guiará por uno de los pasos o por los dos.

##### <a name="to-link-your-organizations-internet-domain-to-microsoft-365"></a>Para vincular el dominio de Internet de su organización con Microsoft 365

1.  En el panel, abra la página **Microsoft 365** y haga clic en **vincular un dominio a Microsoft 365**.

2.  Siga las instrucciones para completar el asistente.

     El asistente puede ayudarle con algunos o todos los pasos para registrar, configurar y vincular un nombre de dominio de Internet nuevo o existente para usarlo en Microsoft 365.

     Haga clic en el vínculo de ayuda en la página del asistente para obtener la información necesaria para completar una tarea. O bien, consulte la sección configurar un nombre de dominio de [administrar el acceso Web remoto en Windows Server Essentials](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx) para obtener información general y los requisitos del proceso.

    > [!NOTE]
    >  S quiere utilizar al asistente para registrar un nombre de dominio nuevo, debe usar uno de los proveedores de servicio de nombres de dominio asociados con Microsoft. De esta forma, podrá realizar una integración sin complicaciones con el asistente. Para buscar un registrador de nombres de dominio, consulte [Adquirir nombres de dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).

3.  Si el asistente detecta que el nombre de dominio no está administrado por el servidor, deberá configurar manualmente los registros DNS necesarios para completar la configuración. Para obtener instrucciones, vea [Cómo configurar manualmente los registros DNS para la integración de Microsoft 365](#BKMK_ManuallyConfigureDNS), más adelante en este tema.

4.  Compruebe que el dominio se usa en Microsoft 365.

     Hay una pequeña espera después de que el asistente finalice, mientras que el registrador de nombres de dominio comprueba los registros DNS. Esto sucede automáticamente; no tiene que hacer nada. Pero generalmente tarda aproximadamente una hora y, a veces, un poco más. Cuando se complete la comprobación del dominio, la página **Microsoft 365** mostrará el dominio de su organización.

####  <a name="how-to-manually-configure-dns-records-for-microsoft-365-integration"></a><a name="BKMK_ManuallyConfigureDNS"></a> Cómo configurar manualmente los registros DNS para la integración de Microsoft 365
 Si el Asistente para vincular su dominio a Microsoft 365 detecta que el nombre de dominio no está administrado por el servidor, debe configurar manualmente los registros del servidor de nombres de dominio (DNS) necesarios para completar la configuración. En ese caso, encontrará una lista de registros de DNS que debe configurar en **% username% \ NewDNSRecords_ (n). txt**, donde *(n)* es un número aleatorio.

 La siguiente tabla describe los registros DNS que se deben agregar. Los métodos de entrada pueden variar según los registradores de nombres de dominio. Si tiene alguna pregunta, pida ayuda al registrador de nombres de dominio.

### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-microsoft-365"></a>Registros DNS necesarios para vincular un nombre de dominio de Internet personalizado a Microsoft 365

|Servicio|Registros DNS necesarios|Propósito|
|-------------|--------------------------|-------------|
|(Varios servicios)|MX| Microsoft 365 usa este registro para comprobar que posee un nombre de dominio específico. Este registro MX no interfiere con el enrutamiento de mensajes de correo electrónico.|
|Exchange Online|MX|Proporciona el enrutamiento de mensajes de correo electrónico. **Importante:**  Si va a migrar el correo electrónico, no asigne una preferencia de cero (**0**) al nuevo registro MX. Asegúrese de que el valor del registro sea mayor que el valor asignado al registro MX actual. Cuando se complete la migración de correo electrónico y esté listo para cambiar el servidor de correo electrónico a Microsoft 365, pida al registrador de nombres de dominio que restablezca el valor de preferencia del nuevo registro MX.|
|Exchange Online|Alias (CNAME)|Registro de detección automática que ayuda a los usuarios a establecer fácilmente una conexión entre Exchange Online y el cliente de escritorio de Outlook o un cliente de correo electrónico móvil. **Nota:**  Si prefiere tener acceso a Outlook Web Access mediante el nombre de dominio de su organización (por ejemplo, http://mail.contoso.com) en lugar de la dirección URL estándar ( https://outlook.com/owa/office365.com) ), puede configurar el registro de alias (CNAME) de la siguiente manera: **tipo = CNAME, TTL = 01:00:00, nombre de host = correo, dirección = correo. Office365. com**|
|Exchange Online|TXT|Especifica que outlook.com, el dominio que usan los servidores de correo electrónico de Microsoft 365, está autorizado para enviar correo electrónico en nombre de su dominio. Cree este registro para evitar que el correo electrónico de salida se considere correo no deseado.|
|Lync Online|SRV|Permite habilitar la federación con otros servicios de mensajería instantánea como Windows Live o Yahoo!.|
|Lync Online|SRV|Registro de detección automática que ayuda a los usuarios a establecer fácilmente una conexión entre el cliente de escritorio de Lync y Microsoft Lync Online.|

> [!IMPORTANT]
>  Una vez completada la comprobación del dominio, no intente agregar o realizar más cambios en los registros DNS desde el portal de Microsoft 365.

###  <a name="next-step"></a><a name="BKMK_StepFour_ACCOUNTS"></a> Paso siguiente

-   Crear cuentas de Microsoft Online Services para los usuarios.

     Para usar Microsoft 365 Services, los usuarios deben tener una cuenta de Microsoft Online Services asociada a su cuenta de usuario de red. El panel facilita esta tarea. Si utiliza una nueva suscripción de Microsoft 365, puede crear cuentas en línea de forma masiva para las cuentas de usuario existentes. Si está integrando un nuevo servidor con una suscripción Microsoft 365 que ya está usando, puede importar las cuentas de usuario de las cuentas en línea existentes. Para ver los procedimientos, consulte [administrar cuentas en línea para usuarios](Manage-Online-Accounts-for-Users.md).

> [!NOTE]
>  En el panel de Windows Server Essentials, las cuentas de Microsoft Online Services se conocen como Microsoft 365 cuentas. Las cuentas son iguales, salvo por el nombre.

##  <a name="manage-microsoft-365-integration"></a><a name="BKMK_ManageIntegration"></a> Administrar la integración de Microsoft 365
 Después de integrar el servidor con Microsoft 365, la página de **Microsoft 365** del panel muestra información sobre la suscripción de Microsoft 365 y hace que estas tareas estén disponibles:

-   ¿ [Administrar su suscripción de Microsoft 365](#BKMK_ManageO365) ? Cambie la cuenta de administrador que utiliza para administrar la suscripción. Abra el panel de administración de Microsoft 365 para administrar su suscripción.

-   ¿ [Vincular el dominio de Internet de su organización a Microsoft 365](#BKMK_StepThree) ? Si desea poder enviar y recibir correo electrónico dirigido a su propio dominio, puede vincular el dominio a Microsoft 365. (Se explicó anteriormente, en el [paso 3: vincular el dominio de la organización con Microsoft 365](#BKMK_StepThree)).

-   ¿ [Deshabilitar la integración de Microsoft 365](#BKMK_Disable) ? Si no desea administrar los servicios de Microsoft 365, la suscripción y las cuentas en línea desde el panel, puede deshabilitar la integración de Microsoft 365. Los servicios siguen estando disponibles en el portal de Microsoft 365.

###  <a name="manage-your-microsoft-365-subscription"></a><a name="BKMK_ManageO365"></a> Administrar la suscripción a Microsoft 365
 Si necesita realizar cambios en la suscripción de Microsoft 365 mientras trabaja en el servidor, puede abrir la suscripción en Microsoft 365 desde la página **Microsoft 365** del panel. También puede cambiar la cuenta de administrador que usa el servidor para realizar cambios en los servicios de Microsoft 365.

##### <a name="to-open-your-subscription-on-the-microsoft-365-admin-dashboard"></a>Para abrir su suscripción en el panel de administración de Microsoft 365

1.  En el panel de Windows Server Essentials, abra la página **Microsoft 365** .

2.  En **tareas de configuración**, haga clic en **administrar Microsoft 365**.

3.  Inicie sesión en Microsoft 365 con la cuenta en línea de Microsoft que usa para administrar su suscripción.

     Se abre el panel de administración de Microsoft 365.

##### <a name="to-change-the-microsoft-365-administrator-account"></a>Para cambiar la cuenta de administrador de Microsoft 365

1.  En el panel, haga clic en **Microsoft 365**.

2.  En **tareas de configuración**, haga clic en **cambiar la cuenta de administrador de Microsoft 365**. Se abrirá el asistente para cambiar la cuenta de administrador (En Windows Server Essentials, el asistente se denomina configurar la cuenta de administrador de Microsoft 365).

3.  Escriba las credenciales de la cuenta que desea usar para conectarse a su suscripción de Microsoft 365 y, a continuación, haga clic en **siguiente**.

4.  Haga clic en **Cerrar**. El panel se reiniciará.

###  <a name="disable-microsoft-365-integration"></a><a name="BKMK_Disable"></a> Deshabilitar la integración de Microsoft 365
 Si decide que no desea administrar los servicios de Microsoft 365 y las cuentas en línea desde el panel, puede deshabilitar la integración de Microsoft 365. La suscripción a Microsoft 365 permanece activa y los cambios de configuración realizados desde el panel permanecen en vigor. Por ejemplo, recibirá un correo electrónico dirigido a un nombre de dominio vinculado a su suscripción de Microsoft 365. No perderá ningún mensaje de correo electrónico y los controles que establezca para los dispositivos móviles seguirán utilizándose en Exchange Online.

 En adelante, podrá administrar la suscripción a Microsoft 365, los servicios y los recursos en Microsoft 365, y los usuarios tendrán que administrar las contraseñas de sus cuentas en línea en Microsoft 365. Ya no se produce la sincronización de contraseña, y la deshabilitación o eliminación de una cuenta de usuario no tendrá ningún efecto en la cuenta en línea del usuario.

 Dado que el software de integración de Microsoft 365 está instalado en el servidor local, puede deshabilitar la característica incluso si el servicio de integración no puede conectarse a Microsoft 365.

##### <a name="to-disable-microsoft-365-integration-with-the-server"></a>Para deshabilitar la integración de Microsoft 365 con el servidor

1.  En el panel, haga clic en **Microsoft 365**.

2.  Haga clic en **deshabilitar la integración de Microsoft 365**. Aparece el Asistente para deshabilitar Microsoft 365.

3.  Siga las instrucciones para completar el asistente.

> [!NOTE]
>  Para habilitar de nuevo la integración de Microsoft 365, use la tarea **integrar con Microsoft 365** en la pestaña **servicio** de la página **principal** del panel. Para obtener instrucciones, consulte [Step 2: Integrate your Windows Server Essentials Server with Microsoft 365](#BKMK_StepTwo), anteriormente en este tema.

##  <a name="troubleshoot-microsoft-365-integration"></a><a name="BKMK_Troubleshoot"></a> Solucionar problemas de integración de Microsoft 365
 En esta sección se proporciona información para ayudarle a solucionar problemas comunes que pueden surgir al usar las características de integración de Microsoft 365 en Windows Server Essentials.

###  <a name="some-microsoft-online-services-accounts-were-not-created"></a><a name="BKMK_AcctsNotCreated"></a> No se crearon algunas cuentas de Microsoft Online Services
 **Descripción**

 No se pudo crear una o varias cuentas de Microsoft Online Services desde el panel.

 **Solución**

1.  Haga clic en el vínculo de la página de finalización del asistente para abrir un archivo de resultados que contiene información detallada sobre cada error de solicitud de creación de cuenta. Por ejemplo, un resultado podría indicarle que ya existe una cuenta de Microsoft Online Services con el mismo nombre que una cuenta solicitada.

2.  Siga las recomendaciones para resolver cada error.

3.  Si el problema continúa, reinicie el servidor y, a continuación, intente crear de nuevo las cuentas en línea.

###  <a name="there-was-a-problem-uninstalling-microsoft-365-integration"></a><a name="BKMK_ProblemUninstalling"></a> Se produjo un problema al desinstalar la integración de Microsoft 365
 **Descripción**

 Se produjo un error desconocido al intentar deshabilitar la integración de Microsoft 365.

 **Solución**

1.  Asegúrese de que el equipo esté conectado a Internet y, a continuación, vuelva a intentarlo.

2.  Si el error ocurre de nuevo, reinicie el servidor y, a continuación, vuelva a intentarlo.

## <a name="additional-references"></a>Referencias adicionales

-   [Información general de la integración de servicios para Windows Server Essentials, parte 1](/archive/blogs/sbs/services-integration-overview-for-windows-server-2012-r2-essentials-part-1)

-   [Información general de la integración de servicios para Windows Server Essentials, parte 2](/archive/blogs/sbs/services-integration-overview-for-windows-server-2012-r2-essentials-part-2)

-   [Guía de inicio rápido a usar Microsoft 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)

-   [Administrar Microsoft Online Services](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)

-   [Administrar Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)