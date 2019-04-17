---
title: Administrar Office 365 en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f8485e4-e10f-4f38-8a5e-d5227abd0d84
0author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b45bddb657b96c7cc5f9291a6c887b9d0801974b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="manage-office-365-in-windows-server-essentials"></a>Administrar Office 365 en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Al integrar tu servidor de Windows Server Essentials con Microsoft Office 365, puedes administrar los servicios de Office 365 y cuentas en línea junto con los recursos locales desde el escritorio de Windows Server Essentials. En este tema, todo se descubre lo que se obtiene mediante la integración de tu servidor con Office 365, cómo hacerlo y cómo administrar y solucionar problemas de la integración de Office 365.  
  
  
  
> [!IMPORTANT]
>   Solo se admite la integración con Office 365 en un entorno de controlador de dominio. Además, debes ejecutar el Asistente para la integración de Office 365 en un controlador de dominio.  
  
## <a name="in-this-topic"></a>En este tema  
  
-   [¿Por qué debo integrar Office 365 con mi servidor?](#BKMK_IntegrationOverview)  
  
-   [Configurar la integración de Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [Administrar la integración de Office 365](#BKMK_ManageIntegration)  
  
-   [Solucionar problemas de integración de Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="BKMK_IntegrationOverview"></a>¿Por qué debo integrar Office 365 con mi servidor?  
 Hay muchas buenas razones para integrar Office 365 con el servidor de Windows Server Essentials. Si administran algunas de tus recursos internos pero usar Office 365 para otros servicios, todo es que puede administrar los servicios de Office 365 y recursos desde el panel, junto con los recursos locales, en lugar de trabajar en dos lugares.  
  
-   Administrar las cuentas en línea que proporcionan los usuarios tienen acceso a Office 365 junto con sus cuentas de usuario:  
  
    -   Crear cuentas de Microsoft Online Services para los usuarios en un solo paso, o crear cuentas de usuario en el servidor para tus cuentas en línea existentes. También puedes agregar una cuenta en línea a una cuenta de usuario nuevo o existente.  
  
         Al crear las cuentas en línea desde el panel, los usuarios iniciar sesión Office 365 con la misma contraseña que se usan en el servidor. Si cambia la contraseña de su cuenta de usuario, también cambia la contraseña en línea. Y se sabe que las contraseñas de cuentas en línea siempre cumplan los requisitos de seguridad establecido para las cuentas de usuario.  
  
    -   Administrar una cuenta en línea junto con la cuenta de usuario durante el ciclo de vida de la cuenta de usuario. Si desactiva una cuenta de usuario, también desactiva la cuenta en línea. Si quitas una cuenta de usuario, también se quita la cuenta en línea.  
  
    -   En un servidor de Windows Server Essentials, administrar también Exchange Online grupos de distribución de correo electrónico.  
  
-   Enviar y recibir correo electrónico desde tu dominio de Internet de s de organización (por ejemplo, contoso.com) mediante un vínculo de un dominio de Internet personalizado a tu suscripción a Office 365.  
  
-   Administrar la suscripción y la integración de Office 365 desde el panel.  
  
-   Si tu suscripción incluye bibliotecas de SharePoint Online, integración de Office 365 con un servidor de Windows Server Essentials te permite:  
  
    -   Crear y administrar tus bibliotecas de SharePoint Online desde el panel.  
  
        > [!NOTE]
        >  Todo se también podrán usar la aplicación de mi Server 2012 R2 para trabajar con documentos en las bibliotecas de SharePoint Online desde tu portátil, dispositivos móviles o Windows phone. Esta característica solo está disponible para Windows Server Essentials. Para obtener más información, consulta [usar la aplicación del servidor mi](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).  
  
    -   Cambiar los permisos de un sitio de SharePoint Online equipo desde el panel, o abrir el sitio del equipo desde el panel para realizar otros cambios.  
  
     Para obtener más información, consulta [administrar SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md).  
  
-   Si se suscribe a Exchange Online, integración de Office 365 con un servidor de Windows Server Essentials te permite administrar los dispositivos móviles que los usuarios usar para conectarse al servidor de correo electrónico de empresa:  
  
    -   Requerir protección por contraseña cuando los dispositivos móviles que se conectan al servidor de correo electrónico de la compañía. Establece una longitud mínima de la contraseña, el número de intentos de inicio de sesión erróneos para permitir y el tiempo mínimo necesario entre los intentos de inicio de sesión.  
  
    -   Bloquear un dispositivo móvil de conectarse a Exchange Online Si sabes de problemas de seguridad con el modelo de dispositivo.  
  
    -   Si un dispositivo móvil pérdido o robo, borrar el dispositivo para eliminar los datos confidenciales de la compañía la próxima vez que el dispositivo está activado.  
  
-    Integración con Office 365 ofrece nuevas maneras de conectarse a recursos y servicios de Office 365:  
  
    -   Abrir los servicios de Office 365 desde el Launchpad de Windows Server Essentials. Para obtener información, consulta [Guía de inicio rápido para usar Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
    -   Usar la aplicación de mi Server 2012 R2 para trabajar con documentos en las bibliotecas de SharePoint Online desde tu portátil, dispositivos móviles o Windows phone. Para obtener información, consulta [usar la aplicación del servidor mi](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md). Esta característica solo está disponible en Windows Server Essentials.  
  
##  <a name="BKMK_Configure"></a>Configurar la integración de Office 365  
 Puedes integrar su servidor con Office 365 en cualquier momento después de completar la instalación del servidor. Si no t ya tienes una suscripción a Office 365, puedes adquirir uno o registrarse para una suscripción de prueba gratuita.  
  
 Realizará las siguientes tareas:  
  
-   [Paso 1: Comprobar requisitos de integración de Office 365](#BKMK_StepOne_VERIFY)  
  
-   [Paso 2: Integrar el servidor con Microsoft Office 365](#BKMK_StepTwo)  
  
-   [Paso 3: Vincular tu nombre de dominio de Internet de organización s a Office 365 (opcional)](#BKMK_StepThree)  
  
###  <a name="BKMK_StepOne_VERIFY"></a>Paso 1: Comprobar requisitos de integración de Office 365  
 Antes de comenzar, asegúrate de que el servidor cumpla estos requisitos:  
  
-   El servidor puede darse cualquiera de estos sistemas operativos: Windows Server Essentials, Windows Server Essentials o el sistema operativo Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con el rol de Windows Server Essentials Experience instalado.  
  
-   El entorno solo puede tener un controlador de dominio, y debes realizar la integración de Office 365 en el controlador de dominio.  
  
-   Debe ser capaz de conectarse a Internet desde el servidor.  
  
-   Debes instalar todas las actualizaciones críticas e importantes en el servidor antes de comenzar la integración de Office 365.  
  
-   Si quieres usar tu dominio de Internet de s organización direcciones de correo electrónico y con los recursos de SharePoint Online, tendrá que registrar el nombre de dominio para que puedes vincular el dominio a Office 365 durante la integración. Para obtener más información, consulta [cómo adquirir un nombre de dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
> [!NOTE]
>  Cuando no t necesario suscribirse a Office 365 por adelantado. Todo se pueda comprar una suscripción o registrarse para una prueba gratuita durante la integración de Office 365. Si d quieres echa un vistazo a los planes y precios para Office 365, [comparar los planes de Office 365 para empresas](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text).  
  
###  <a name="BKMK_StepTwo"></a>Paso 2: Integrar el servidor con Microsoft Office 365  
 El siguiente procedimiento se realiza en el controlador de dominio para integrar el servidor de Windows Server Essentials con Office 365.  
  
> [!NOTE]
>  El procedimiento s lo mismo si tienes Windows Server Essentials o Windows Server Essentials, pero puedes todo empezar desde otro lugar en la página principal. En Windows Server Essentials, integrar el servidor con Office 365 y otros servicios en línea de Microsoft en la **servicios** pestaña. En Windows Server Essentials, integración de Office 365 se realiza en el **correo electrónico** pestaña.  
  
##### <a name="to-integrate-the-server-with-office-365"></a>Integrar el servidor con Office 365  
  
1.  Inicia sesión en el servidor como administrador y abre el escritorio de Windows Server Essentials.  
  
2.  En la **Home** página, haz clic en **servicios** (en Windows Server Essentials, haz clic en **correo electrónico**), haz clic en **integración con Microsoft Office 365**y, a continuación, haz clic en **configurar la integración de Microsoft Office 365**.  
  
     Aparece la integración con el Asistente de Microsoft Office 365.  
  
3.  En la **Introducción** página, llevar a cabo una de las siguientes acciones:  
  
    -   Si don t tienes una suscripción a Office 365, haz clic en **siguiente**y sigue las instrucciones para suscribirte a Office 365 o inicio de sesión de una suscripción de prueba.  
  
         Se tendrá que iniciar sesión en Office 365 antes de volver al asistente. Pero no t necesidad de realizar cualquiera de las tareas en la **empieza aquí** sección del portal de Office 365.  
  
    -   Si ya tienes una suscripción a Office 365 que desees integrar con el servidor, selecciona **ya tengo una suscripción de Office 365**y, a continuación, haz clic en **siguiente**.  
  
4.  Sigue las instrucciones para completar al asistente.  
  
 Después de que el asistente se completa correctamente, puedes todo Ten en cuenta los siguientes cambios en el panel:  
  
-   Allí s un nuevo **Office 365** página, que se usa para administrar la integración y la suscripción a Office 365.  
  
-   En la **usuarios** página, puedes ver todo un **cuentas en línea** pestaña donde puedes crear y administrar las cuentas de Microsoft Online Services que permiten a los usuarios acceso a Office 365. Si se está utilizando Exchange Online y tener un servidor de Windows Server Essentials, todo se vea también un **grupos de distribución** pestaña.  
  
-   La **almacenamiento** página en un servidor de Windows Server Essentials tiene un **bibliotecas de SharePoint** ficha para administrar bibliotecas de SharePoint Online y cambiar los permisos de los sitios de equipo. Cada plan de negocios para Office 365 incluye estas características básicas de SharePoint Online.  
  
###  <a name="BKMK_StepThree"></a>Paso 3: Vincular tu nombre de dominio de Internet de organización s a Office 365 (opcional)  
 Si quieres usar tu propio dominio de Internet en un correo electrónico dirigida a la organización y las direcciones URL de los recursos de SharePoint Online, puedes vincular un dominio personalizado a tu suscripción a Office 365. Si el servidor de Windows Server Essentials se integra con Office 365, puede hacerlo desde el panel.  
  
 Lo más fácil de hacerlo antes de crear en línea s cuentas para los usuarios para que puedan usar el dominio al masiva-crear las cuentas en línea.  
  
 ¿Obtener un nombre de dominio cuando te registres para Office 365? por ejemplo, *contoso*. onmicrosoft.com. ¿Si d en su lugar usa un nombre de dominio diferente? decir, simplemente contoso.com? puedes. Se tendrá que comprar un nombre de dominio si no t ya propio y cambiar algunos de los registros DNS.  
  
 Configurar un dominio personalizado para usar con Office 365 implica a cuatro tareas:  
  
1.  **Comprar un nombre de dominio.** Es decir, registrar con un registrador de dominios o el proveedor de hospedaje de DNS.  
  
    -   Elige un nombre de dominio que funciona con Office 365. ¿Puedes usar un nombre de dominio de nivel 2? buycontoso.com por ejemplo,?, pero no un nombre de dominio de nivel 3? por ejemplo, marketing.contoso.com. Para obtener más información acerca de cómo elegir un dominio que se utiliza en Office 365, consulta [dominios](https://technet.microsoft.com/library/office-365-domains.aspx).  
  
    -   Comprarla desde un registrador de dominios que permite que los registros de servidor (DNS) del nombre de dominio exija Office 365. Para averiguar qué dominio registradores permitir que los registros DNS necesarios, consulta [cómo adquirir un nombre de dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660). Si registraste tu dominio registrador diferentes, no preocupe; Puedes transferir el dominio a una entidad de registro diferentes al vincular el dominio a Office 365.  
  
2.  **Configurar los registros DNS que permiten a los servicios de Office 365 usar el nombre de dominio.** La forma más sencilla es dejar que el Asistente para configurar los registros DNS para TI cuando se vincula el dominio a tu suscripción a Office 365 en el paso 3. D en su lugar lo que tú mismo, consulta [registra cómo configurar manualmente DNS para la integración de Office 365](#BKMK_ManuallyConfigureDNS).  
  
3.  **Vincular tu dominio personalizado de Internet a tu suscripción a Office 365.** Puedes usar todo el **vincular un dominio a Office 365** acción.  
  
4.  **Comprueba que los servicios de Office 365 están usando el nuevo nombre de dominio.**  
  
 Si completa los pasos 1 y 2 antes de usar la **vincular un dominio a Office 365** tarea, el Asistente para crear un vínculo el nombre de dominio a Office 365. Como alternativa, puedes tener un Asistente para ayudarte con algunos o todos los pasos 1 y 2.  
  
##### <a name="to-link-your-organization-s-internet-domain-to-office-365"></a>Para vincular tu dominio de Internet de la organización s a Office 365  
  
1.  En el panel, abre el **Office 365** página y haz clic en **vincular un dominio a Office 365**.  
  
2.  Sigue las instrucciones para completar al asistente.  
  
     El asistente puede ayudarte con algunos o todos los pasos para registrar, configurar y vincular un nombre de dominio de Internet de nuevo o existente para usar en Office 365.  
  
     Haz clic en el vínculo de ayuda en la página del Asistente para obtener la información que necesitas completar una tarea. O consulta la sección de nombre de dominio de configurar [administrar acceso Web remoto en Windows Server Essentials](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx) para una introducción al proceso y los requisitos.  
  
    > [!NOTE]
    >  Para usar al Asistente para registrar un nuevo nombre de dominio, debes usar uno de los proveedores de servicio de nombre de dominio que asociado de Microsoft para proporcionar una integración completa con el asistente. Para buscar un registrador de nombre de dominio, consulta [cómo adquirir un nombre de dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
3.  Si el asistente detecta que el nombre de dominio no administrados por el servidor, se tendrá que configurar manualmente los registros DNS necesarios para completar la configuración. Para obtener instrucciones, consulta [registra cómo configurar manualmente DNS para la integración de Office 365](#BKMK_ManuallyConfigureDNS)más adelante en este tema.  
  
4.  Comprueba que se utiliza el dominio en Office 365.  
  
     Allí s una espera poco después de que finalice el asistente, mientras el registrador de nombres de dominio comprueba los registros de DNS. Esto sucede automáticamente; Felipe t tienes que hacer nada. Pero por lo general, tarda aproximadamente una hora? y a veces un poco más. Cuando se completa la comprobación del dominio la **Office 365** página mostrará una lista de tu dominio de organización s.  
  
####  <a name="BKMK_ManuallyConfigureDNS"></a>Cómo configurar manualmente los registros DNS para la integración de Office 365  
 Si el dominio de su vínculo Asistente para Office 365 detecta que el nombre de dominio no está administrado por el servidor, para completar la configuración, debe configurar manualmente los registros de servidor (DNS) de nombre de dominio requerido. En ese caso, todo se encuentra una lista de registros DNS que se deben configurar en **%username%\NewDNSRecords_ (n) .txt**, donde *(n)* es un número aleatorio.  
  
 La siguiente tabla describe los registros DNS que se debe agregar. Métodos de entrada pueden variar con diferentes entidades de registro. Si tienes alguna pregunta, pida ayuda el registrador de nombres de dominio.  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>Registros DNS necesarios para vincular un nombre de dominio de Internet personalizado a Office 365  
  
|Servicio|Registros DNS necesarios|Propósito|  
|-------------|--------------------------|-------------|  
|(Varios servicios)|MX| Office 365 utiliza estos datos para comprobar que eres propietario de un nombre de dominio específico. Este registro MX no interfiera con el enrutamiento de mensajes de correo electrónico.|  
|Exchange en línea|MX|Proporciona el enrutamiento de mensajes de correo electrónico. **Importante:** si estás migrando de correo electrónico, no asigne una preferencia de cero (**0**) en el nuevo registro MX. Asegúrate de que el valor del registro es mayor que el valor que se asigna al registro MX actual. Cuando finalice la migración de correo electrónico y estás listo para cambiar el servidor de correo electrónico a Office 365, tienen el registrador de nombre de dominio que restablecer el valor de preferencia del nuevo registro MX.|  
|Exchange en línea|Alias (CNAME)|Registro de detección automática que se usa para ayudar a los usuarios fácilmente establece una conexión entre Exchange Online y el cliente de escritorio de Outlook o de un cliente de correo electrónico. **Nota:** si prefieres Outlook Web access mediante su nombre de dominio propia organización s (por ejemplo, http://mail.contoso.com) en lugar de la dirección URL estándar (https://outlook.com/owa/office365.com), puedes configurar el registro de Alias (CName) como sigue: **tipo = CNAME, TTL = 01: 00:00, nombre de host = dirección de correo = mail.office365.com**|  
|Exchange en línea|TXT|Especifica que outlook.com, el dominio de los servidores de correo electrónico Office 365, está autorizado para enviar correo electrónico en el nombre de tu dominio. Crear este registro para ayudar a impedir que el correo electrónico saliente se marcan como correo no deseado.|  
|Lync Online|SRV|Ayuda a Habilitar federación con otros servicios de mensajería instantánea como Windows Live o Yahoo!.|  
|Lync Online|SRV|Registro de detección automática que se usa para ayudar a los usuarios fácilmente establece una conexión entre el cliente de escritorio de Lync y Microsoft Lync Online.|  
  
> [!IMPORTANT]
>  Después de dominio verificación esté completa, no intentes agregar o realizar más cambios a los registros DNS desde el portal de Office 365.  
  
###  <a name="BKMK_StepFour_ACCOUNTS"></a>Paso siguiente  
  
-   Crear cuentas de Microsoft Online Services para los usuarios.  
  
     Para usar servicios de Office 365, los usuarios deben tener una cuenta de Microsoft Online Services asociada con su cuenta de usuario de la red. El panel facilita esta tarea. Si se está utilizando una nueva suscripción a Office 365, puedes masiva-crear cuentas en línea para las cuentas de usuario existentes. Si puedes re integración de un servidor nuevo con una suscripción a Office 365 que re ya está usando, se pueden importar cuentas de usuario de tu existentes en línea cuentas. Para conocer los procedimientos, consulta [administrar cuentas en línea para los usuarios](Manage-Online-Accounts-for-Users.md).  
  
> [!NOTE]
>  En el panel en Windows Server Essentials, cuentas de Microsoft Online Services se denominan cuentas de Office 365. Las cuentas son las mismas; solo la terminología cambiada.  
  
##  <a name="BKMK_ManageIntegration"></a>Administrar la integración de Office 365  
 Después de integrar el servidor con Office 365, la **Office 365** página en el panel se muestra información sobre tu suscripción a Office 365 y se pone a disposición estas tareas:  
  
-   [¿Administrar la suscripción a Office 365](#BKMK_ManageO365) ? Cambiar la cuenta de administrador que usan para administrar la suscripción. Abre el panel de administración de Office 365 para administrar la suscripción.  
  
-   [¿Vincular tu dominio de Internet de la organización s a Office 365](#BKMK_StepThree) ? Si quieres que pueda enviar y recibir correo electrónico dirigido a su propio dominio, puedes vincular el dominio a Office 365. (Se ha descrito anteriormente, en [paso 3: vincular tu dominio de organización s a Office 365](#BKMK_StepThree).)  
  
-   [¿Deshabilitar la integración de Office 365](#BKMK_Disable) ? Si quieres don t administrar los servicios de Office 365, suscripciones y cuentas en línea desde el panel, puedes deshabilitar la integración de Office 365. Los servicios siguen estando disponibles en el portal de Office 365.  
  
###  <a name="BKMK_ManageO365"></a>Administrar la suscripción a Office 365  
 Si necesitas realizar cambios en tu suscripción a Office 365 mientras trabaja en el servidor, puedes abrir la suscripción en Office 365 desde el **Office 365** página del panel. También puedes cambiar la cuenta de administrador que usa el servidor para realizar cambios en los servicios de Office 365.  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>Para abrir la suscripción en el panel de administración de Office 365  
  
1.  En el panel de Windows Server Essentials, abre el **Office 365** página.  
  
2.  En **tareas de configuración**, haz clic en **administrar Office 365**.  
  
3.  Inicia sesión en Office 365 con la cuenta en línea de Microsoft que usas para administrar la suscripción.  
  
     Abre el panel de administración de Office 365.  
  
##### <a name="to-change-the-office-365-administrator-account"></a>Para cambiar la cuenta de administrador de Office 365  
  
1.  En el panel, haz clic en **Office 365**.  
  
2.  En **tareas de configuración**, haz clic en **cambiar la cuenta de administrador de Office 365**. Aparece el Asistente para la cuenta de administrador de cambio. (En Windows Server Essentials, el asistente se denomina configurar la cuenta Office 365 administrador).  
  
3.  Escribe las credenciales de la cuenta que quieres usar para conectarte a tu suscripción a Office 365 y, a continuación, haz clic en **siguiente**.  
  
4.  Haz clic en **cerrar **. Reinicia el panel.  
  
###  <a name="BKMK_Disable"></a>Deshabilitar la integración de Office 365  
 Si decides que no t desee administrar los servicios de Office 365 y cuentas en línea desde el panel, puedes deshabilitar la integración de Office 365. Tu suscripción a Office 365 permanece activa y los cambios de configuración realizados en el panel permanezcan en vigor. Por ejemplo, puedes todo recibir correo electrónico dirigido a un nombre de dominio que vinculaste a tu suscripción a Office 365. Se ha ganado pierda cualquier correo electrónico y los controles que estableces para dispositivos móviles son aún usa Exchange Online.  
  
 En el futuro, va a administrar la suscripción a Office 365, servicios y recursos en Office 365 y los usuarios necesitan para administrar las contraseñas de sus cuentas en línea en Office 365. Ya no se produce la sincronización de contraseñas y deshabilitar o quitar una cuenta de usuario no tendrá efecto en la cuenta en línea de s de usuario.  
  
 Dado que el software de integración con Office 365 está instalado en el servidor local, puedes deshabilitar la característica incluso si el servicio de integración no se puede conectar a Office 365.  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>Para deshabilitar la integración de Office 365 con el servidor  
  
1.  En el panel, haz clic en **Office 365**.  
  
2.  Haz clic en **deshabilitar la integración de Office 365**. Aparece el Asistente para deshabilitar 365 de Office.  
  
3.  Sigue las instrucciones para completar al asistente.  
  
> [!NOTE]
>  Para habilitar la integración de Office 365 nuevo, usa el **integración con Office 365** de tareas en la **servicio** ficha del panel s **Home** página. Para obtener instrucciones, consulta [paso 2: integrar el servidor de Windows Server Essentials con Microsoft Office 365](#BKMK_StepTwo), anteriormente en este tema.  
  
##  <a name="BKMK_Troubleshoot"></a>Solucionar problemas de integración de Office 365  
 Esta sección proporciona información para ayudarle a solucionar problemas comunes que pueden surgir al usar las características de integración de Office 365 en Windows Server Essentials.  
  
###  <a name="BKMK_AcctsNotCreated"></a>No se han creado algunas cuentas de Microsoft Online Services  
 **Descripción**  
  
 Se intentó crear una o varias cuentas de Microsoft Online Services desde el panel t correcta.  
  
 **Solución**  
  
1.  Haz clic en el vínculo en la página de finalización del Asistente para abrir un archivo de resultados que contiene información detallada sobre cada solicitud de creación de cuenta que no se completó correctamente. Por ejemplo, un resultado podría informar que ya existe una cuenta de Microsoft Online Services con el nombre de una cuenta solicitado.  
  
2.  Realizar las acciones recomendadas para resolver estos errores.  
  
3.  Si el problema continúa, reinicia el servidor y, a continuación, intenta volver a crear las cuentas en línea.  
  
###  <a name="BKMK_ProblemUninstalling"></a>Hubo un problema al desinstalar la integración con Office 365  
 **Descripción**  
  
 Se ha producido un error al intentar deshabilitar la integración de Office 365.  
  
 **Solución**  
  
1.  Asegúrate de que el equipo está conectado a Internet y, a continuación, vuelve a intentarlo.  
  
2.  Si vuelve a producirse el error, reiniciar el servidor y, a continuación, vuelve a intentarlo.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Descripción de la integración de servicios para Windows Server Essentials - parte 1](http://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Descripción de la integración de servicios para Windows Server Essentials - parte 2](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [Guía de inicio rápido al uso de Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [Administrar Microsoft Online Services](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
