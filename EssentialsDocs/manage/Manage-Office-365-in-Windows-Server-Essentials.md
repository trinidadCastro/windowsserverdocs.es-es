---
title: Administrar Office 365 en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
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
ms.openlocfilehash: d897c9186ff74a80d531cf84961709de54459f82
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433281"
---
# <a name="manage-office-365-in-windows-server-essentials"></a>Administrar Office 365 en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Al integrar su servidor Windows Server Essentials con Microsoft Office 365, puede administrar los servicios de Office 365 y las cuentas online junto con sus recursos locales desde el panel de Windows Server Essentials. En este tema, podrá averiguar lo que obtiene al integrar su servidor con Office 365, para saber cómo hacerlo y cómo administrar y solucionar problemas de la integración de Office 365.  
  
  
  
> [!IMPORTANT]
>   Integración de Office 365 solo se admite en un entorno de controlador de dominio único. Además, debe ejecutar el Asistente para la integración de Office 365 en un controlador de dominio.  
  
## <a name="in-this-topic"></a>En este tema  
  
-   [¿Por qué debo integrar Office 365 con mi servidor?](#BKMK_IntegrationOverview)  
  
-   [Configurar la integración de Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Configure)  
  
-   [Administrar la integración de Office 365](#BKMK_ManageIntegration)  
  
-   [Solucionar problemas de integración de Office 365](Manage-Office-365-in-Windows-Server-Essentials.md#BKMK_Troubleshoot)  
  
##  <a name="BKMK_IntegrationOverview"></a> ¿Por qué debo integrar Office 365 con mi servidor?  
 Hay muchas buenas razones para integrar Office 365 con su servidor de Windows Server Essentials. Si administra algunos de los recursos internos, pero usa Office 365 para otros servicios, podrá administrar los servicios de Office 365 y recursos desde el panel, junto con sus recursos locales, en lugar de trabajar en dos lugares.  
  
- Administrar las cuentas en línea que permiten a los usuarios acceso a Office 365 junto con sus cuentas de usuario:  
  
  -   Cree cuentas de Microsoft Online Services para los usuarios en un solo paso, o cree cuentas de usuario en el servidor para las cuentas existentes en línea. También puede agregar una cuenta en línea para una cuenta de usuario nueva o existente.  
  
       Al crear las cuentas en línea desde el panel, los usuarios iniciar sesión en Office 365 con la misma contraseña que se usan en el servidor. Si cambia la contraseña de su cuenta de usuario, también cambia la contraseña en línea. Así estará seguro de que sus contraseñas de cuenta en línea siempre cumplan los requisitos de seguridad establecidos para las cuentas de usuario.  
  
  -   Administre una cuenta en línea junto con la cuenta de usuario durante el ciclo de vida de la cuenta de usuario. Si desactiva una cuenta de usuario, también se desactiva la cuenta en línea. Si quita una cuenta de usuario, también se quita la cuenta en línea.  
  
  -   En un servidor Windows Server Essentials, administre también los grupos de distribución de Exchange Online para el correo electrónico.  
  
- Enviar y recibir correo electrónico de dominio de Internet de su organización (por ejemplo, contoso.com) vinculando un dominio de Internet personalizado a su suscripción de Office 365.  
  
- Administrar su suscripción y la integración de Office 365 desde el panel.  
  
- Si la suscripción incluye bibliotecas de SharePoint Online, integración de Office 365 con un servidor de Windows Server Essentials le permite:  
  
  - Crear y administrar las bibliotecas de SharePoint Online desde el panel.  
  
    > [!NOTE]
    >  También podrá usar la aplicación My Server 2012 R2 para trabajar con documentos en las bibliotecas de SharePoint Online desde su portátil, dispositivo móvil o teléfono de Windows. Esta característica solo está disponible para Windows Server Essentials. Para obtener más información, consulte [usar la aplicación My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).  
  
  - Cambiar los permisos de un sitio de grupo SharePoint Online desde el panel o abrir el sitio del equipo desde el panel para realizar otros cambios.  
  
    Para obtener más información, consulte [administrar SharePoint Online](Manage-SharePoint-Online-in-Windows-Server-Essentials.md).  
  
- Si se suscribe a Exchange Online, integración de Office 365 con un servidor de Windows Server Essentials le permite administrar los dispositivos móviles que los usuarios usan para conectarse al servidor de correo electrónico de la compañía:  
  
  -   Exija la protección con contraseña cuando los dispositivos móviles se conecten al servidor de correo electrónico de la compañía. Establezca una longitud mínima de contraseña, el número de intentos incorrectos de inicio de sesión permitidos y el tiempo mínimo necesario entre estos intentos.  
  
  -   Impida que un dispositivo móvil se conecte a Exchange Online si hay problemas de seguridad con el modelo del dispositivo.  
  
  -   En caso de pérdida o robo de un dispositivo móvil, borre el dispositivo para eliminar los datos confidenciales de la compañía la próxima vez que el dispositivo esté activado.  
  
- Integración de Office 365 proporciona nuevas formas de conectarse a los recursos y servicios de Office 365:  
  
  -   Abra Servicios de Office 365 desde el Launchpad de Windows Server Essentials. Para obtener información, consulte [Quick Start Guide to Using Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
  -   Use la aplicación My Server 2012 R2 para trabajar con documentos en las bibliotecas de SharePoint Online desde su portátil, dispositivo móvil o teléfono de Windows. Para obtener información, consulte [usar la aplicación My Server](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md). Esta característica solo está disponible en Windows Server Essentials.  
  
##  <a name="BKMK_Configure"></a> Configurar la integración de Office 365  
 Puede integrar su servidor con Office 365 en cualquier momento después de completar la instalación del servidor. Si aún no tiene una suscripción a Office 365, puede adquirir uno o registrarse para obtener una suscripción de prueba gratuita.  
  
 Realizará las tareas siguientes:  
  
-   [Paso 1: Compruebe los requisitos de integración de Office 365](#BKMK_StepOne_VERIFY)  
  
-   [Paso 2: Integrar el servidor con Microsoft Office 365](#BKMK_StepTwo)  
  
-   [Paso 3: Vincular el nombre de dominio de Internet de su organización a Office 365 (opcional)](#BKMK_StepThree)  
  
###  <a name="BKMK_StepOne_VERIFY"></a> Paso 1: Compruebe los requisitos de integración de Office 365  
 Antes de empezar, asegúrese de que el servidor cumpla con estos requisitos:  
  
-   El servidor puede tener cualquiera de estos sistemas operativos:  Windows Server Essentials, Windows Server Essentials o el sistema operativo Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter con el rol experiencia con Windows Server Essentials instalado.  
  
-   El entorno solo puede tener un controlador de dominio y debe realizar la integración de Office 365 en el controlador de dominio.  
  
-   Debe ser capaz de conectarse a Internet desde el servidor.  
  
-   Debe instalar todas las actualizaciones críticas e importantes en el servidor antes de iniciar la integración de Office 365.  
  
-   Si desea usar el dominio de Internet de su organización en direcciones de correo electrónico y con los recursos de SharePoint Online, deberá registrar el nombre de dominio, por lo que puede vincular el dominio con Office 365 durante la integración. Para obtener más información, consulte [Cómo adquirir nombres de dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
> [!NOTE]
>  No es necesario suscribirse a Office 365 de antemano. Podrá comprar una suscripción o registrarse para obtener una evaluación gratuita durante la integración de Office 365. Si desea echar un vistazo a planes y precios de Office 365, [comparar los planes de Office 365 para empresas](https://office.microsoft.com/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_subscribe-to-office-365_Text).  
  
###  <a name="BKMK_StepTwo"></a> Paso 2: Integrar el servidor con Microsoft Office 365  
 Realice el procedimiento siguiente en el controlador de dominio para integrar su servidor Windows Server Essentials con Office 365.  
  
> [!NOTE]
>  El procedimiento 's el mismo si tiene Windows Server Essentials o Windows Server Essentials, pero podrá empezar desde una ubicación diferente en la página principal. En Windows Server Essentials, debe integrar el servidor con Office 365 y otros servicios de Microsoft Online Services en el **servicios** ficha. En Windows Server Essentials, la integración de Office 365 se realiza en el **correo electrónico** ficha.  
  
##### <a name="to-integrate-the-server-with-office-365"></a>Para integrar el servidor con Office 365  
  
1. Inicie sesión en el servidor como administrador y abra el panel de Windows Server Essentials.  
  
2. En el **inicio** página, haga clic en **servicios** (en Windows Server Essentials, haga clic en **correo electrónico**), haga clic en **integrar con Microsoft Office 365**, y, a continuación, haga clic en **configurar la integración de Microsoft Office 365**.  
  
    Se abrirá el Asistente para integración con Microsoft Office 365.  
  
3. En la página **Comenzar**, realice una de las acciones siguientes:  
  
   -   Si no tiene una suscripción a Office 365, haga clic en **siguiente**y siga las instrucciones para suscribirse a Office 365 o de inicio de sesión de una suscripción de prueba.  
  
        Deberá iniciar sesión Office 365 antes de volver al asistente. Pero no es necesario realizar ninguna de las tareas en el **comience aquí** sección del portal de Office 365.  
  
   -   Si ya tiene una suscripción a Office 365 que desea integrar con el servidor, seleccione **ya tengo una suscripción para Office 365**y, a continuación, haga clic en **siguiente**.  
  
4. Siga las instrucciones para completar el asistente.  
  
   Cuando el asistente finalice correctamente, verá los siguientes cambios en el panel:  
  
-   Hay un nuevo **Office 365** página, que se usa para administrar la integración y la suscripción a Office 365.  
  
-   En el **usuarios** página, verá un **cuentas en línea** pestaña donde puede crear y administrar las cuentas de Microsoft Online Services que ofrecer a los usuarios acceso a Office 365. Si está usando Exchange Online y tiene un servidor Windows Server Essentials, también verá un **grupos de distribución** ficha.  
  
-   El **almacenamiento** página en un servidor de Windows Server Essentials tiene un **las bibliotecas de SharePoint** ficha para administrar las bibliotecas de SharePoint Online y cambiar los permisos de los sitios de grupo. Cada plan de negocios para Office 365 incluye estas características básicas de SharePoint Online.  
  
###  <a name="BKMK_StepThree"></a> Paso 3: Vincular el nombre de dominio de Internet de su organización a Office 365 (opcional)  
 Si desea usar su propio dominio de Internet de correo electrónico dirigido a su organización y las direcciones URL para los recursos de SharePoint Online, puede vincular un dominio personalizado a su suscripción de Office 365. Si integra su servidor Windows Server Essentials con Office 365, puede hacerlo desde el panel.  
  
 Es más fácil hacerlo antes de crear cuentas en línea para los usuarios para que pueda usar el dominio al que crea de forma masiva las cuentas en línea.  
  
 Obtener un nombre de dominio al registrarse para Office 365? por ejemplo, *contoso*. onmicrosoft.com. Si prefiere usar otro nombre de dominio? por ejemplo, contoso.com simplemente? es posible. Necesitará comprar un nombre de dominio si no dispone de uno y cambiar algunos de los registros DNS.  
  
 Cómo configurar un dominio personalizado para usar con Office 365, debe realizar cuatro tareas:  
  
1. **Compre un nombre de dominio.** Es decir, regístrese en un registrador de dominios o un proveedor de hospedaje de DNS.  
  
   -   Elija un nombre de dominio que funciona con Office 365. Puede usar un nombre de dominio de nivel 2 º? buycontoso.com, por ejemplo,?, pero no un nombre de dominio de nivel 3? por ejemplo, marketing.contoso.com. Para obtener más información acerca de cómo elegir un dominio para usarlo en Office 365, consulte [dominios](https://technet.microsoft.com/library/office-365-domains.aspx).  
  
   -   Compre en un registrador de dominios que admita los registros de servidor (DNS) de nombres de dominio requeridos por Office 365. Para averiguar qué registradores de dominios admiten los registros DNS necesarios, consulte [Cómo adquirir nombres de dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660). Si ha registrado su dominio con un registrador diferente, no se preocupe; puede transferir el dominio a un registrador diferente cuando vincule el dominio a Office 365.  
  
2. **Configure los registros DNS que permitan a Office 365 usar el nombre de dominio.** La forma más sencilla es dejar que el asistente configure los registros DNS cuando vincule el dominio a su suscripción de Office 365 en el paso 3. Si se prefiere hacerlo usted mismo, consulte [cómo configurar manualmente DNS los registros para la integración de Office 365](#BKMK_ManuallyConfigureDNS).  
  
3. **Vincule su dominio de Internet personalizado a su suscripción de Office 365.** Deberá usar el **vincular un dominio con Office 365** acción.  
  
4. **Compruebe que los servicios de Office 365 utilizan el nuevo nombre de dominio.**  
  
   Si completa los pasos 1 y 2 antes de usar el **vincular un dominio con Office 365** tarea, el asistente puede vincular el nombre de dominio con Office 365. También puede usar el asistente, que le guiará por uno de los pasos o por los dos.  
  
##### <a name="to-link-your-organizations-internet-domain-to-office-365"></a>Para vincular el dominio de Internet de su organización a Office 365  
  
1.  En el panel, abra la página **Office 365** y, después, haga clic en **Vincular un dominio con Office 365**.  
  
2.  Siga las instrucciones para completar el asistente.  
  
     El asistente puede ayudarle con algunos o todos los pasos para registrar, configurar y vincular un nombre de dominio de Internet nuevo o existente para usar en Office 365.  
  
     Haga clic en el vínculo de ayuda en la página del asistente para obtener la información necesaria para completar una tarea. O consulte la sección de nombre de dominio configurar [administrar acceso Web remoto en Windows Server Essentials](https://technet.microsoft.com/library/jj628152\(d=printer\).aspx) para una descripción del proceso y los requisitos.  
  
    > [!NOTE]
    >  S quiere utilizar al asistente para registrar un nombre de dominio nuevo, debe usar uno de los proveedores de servicio de nombres de dominio asociados con Microsoft. De esta forma, podrá realizar una integración sin complicaciones con el asistente. Para buscar un registrador de nombres de dominio, consulte [Adquirir nombres de dominio](https://office.microsoft.com/office365-suite-help/how-to-buy-a-domain-name-HA102819883.aspx?CTT=5&origin=HA102818660).  
  
3.  Si el asistente detecta que el nombre de dominio no está administrado por el servidor, deberá configurar manualmente los registros DNS necesarios para completar la configuración. Para obtener instrucciones, consulte [cómo configurar manualmente los registros DNS para la integración de Office 365](#BKMK_ManuallyConfigureDNS), más adelante en este tema.  
  
4.  Compruebe que se está usando el dominio en Office 365.  
  
     Hay una espera poco después de completar el asistente, mientras que el registrador de nombres de dominio comprueba los registros DNS. Esto sucede automáticamente; no tiene que hacer nada. Proceso suele tardar aproximadamente una hora? y a veces un poco más. Una vez completada la comprobación del dominio, el **Office 365** página mostrará una lista de dominio de su organización.  
  
####  <a name="BKMK_ManuallyConfigureDNS"></a> Cómo configurar manualmente los registros DNS para la integración de Office 365  
 Si el asistente para vincular su dominio con Office 365 detecta que el nombre de dominio no está administrado por el servidor, debe configurar manualmente los registros DNS necesarios del servidor de nombres de dominio para completar la configuración. En ese caso, encontrará una lista de los registros DNS que se deben configurar en **%username%\NewDNSRecords_ (n) .txt**, donde *(n)* es un número aleatorio.  
  
 La siguiente tabla describe los registros DNS que se deben agregar. Los métodos de entrada pueden variar según los registradores de nombres de dominio. Si tiene alguna pregunta, pida ayuda al registrador de nombres de dominio.  
  
### <a name="required-dns-records-for-linking-a-custom-internet-domain-name-to-office-365"></a>Registros DNS necesarios para vincular un nombre de dominio de Internet personalizado a Office 365  
  
|Servicio|Registros DNS necesarios|Finalidad|  
|-------------|--------------------------|-------------|  
|(Varios servicios)|MX| Office 365 usa este registro para comprobar que posee un nombre de dominio específico. Este registro MX no interfiere con el enrutamiento de mensajes de correo electrónico.|  
|Exchange Online|MX|Proporciona el enrutamiento de mensajes de correo electrónico. **Importante:**  Si está migrando de correo electrónico, no asigne una preferencia de cero (**0**) en el nuevo registro MX. Asegúrese de que el valor del registro sea mayor que el valor asignado al registro MX actual. Cuando se completa la migración del correo electrónico y esté listo para cambiar el servidor de correo electrónico a Office 365, tiene el registrador de nombres de dominio que restablezca el valor de preferencia del nuevo registro MX.|  
|Exchange Online|Alias (CNAME)|Registro de detección automática que ayuda a los usuarios a establecer fácilmente una conexión entre Exchange Online y el cliente de escritorio de Outlook o un cliente de correo electrónico móvil. **Nota:**  Si prefiere obtener acceso a Outlook Web Access mediante su nombre de dominio de la organización (por ejemplo, http://mail.contoso.com) en lugar de la dirección URL estándar (https://outlook.com/owa/office365.com), puede configurar el registro de Alias (CName) como sigue: **Type=CNAME, TTL=01:00:00, HostName=mail, Address=mail.office365.com**|  
|Exchange Online|TXT|Especifica que outlook.com, el dominio que usan los servidores de correo electrónico de Office 365, está autorizado para enviar correo electrónico en nombre de su dominio. Cree este registro para evitar que el correo electrónico de salida se considere correo no deseado.|  
|Lync Online|SRV|Permite habilitar la federación con otros servicios de mensajería instantánea como Windows Live o Yahoo!.|  
|Lync Online|SRV|Registro de detección automática que ayuda a los usuarios a establecer fácilmente una conexión entre el cliente de escritorio de Lync y Microsoft Lync Online.|  
  
> [!IMPORTANT]
>  Después de dominio completada la comprobación, no intente agregar ni realizar más cambios en los registros de DNS desde el portal de Office 365.  
  
###  <a name="BKMK_StepFour_ACCOUNTS"></a> Paso siguiente  
  
-   Crear cuentas de Microsoft Online Services para los usuarios.  
  
     Para usar servicios de Office 365, los usuarios deben tener una cuenta de Microsoft Online Services asociada con su cuenta de usuario de red. El panel facilita esta tarea. Si usa una nueva suscripción de Office 365, puede bulk-crear cuentas en línea para las cuentas de usuario existentes. Si está realizando la integración de un nuevo servidor con una suscripción a Office 365 que ya esté usando, puede importar las cuentas de usuario de las cuentas en línea existentes. Para conocer los procedimientos, consulte [administrar cuentas en línea para los usuarios](Manage-Online-Accounts-for-Users.md).  
  
> [!NOTE]
>  En el panel en Windows Server Essentials, las cuentas de Microsoft Online Services se conocen como cuentas de Office 365. Las cuentas son iguales, salvo por el nombre.  
  
##  <a name="BKMK_ManageIntegration"></a> Administrar la integración de Office 365  
 Después de integrar su servidor con Office 365, el **Office 365** página en el panel muestra información acerca de la suscripción a Office 365 y pone a disposición estas tareas:  
  
-   [¿Administrar la suscripción de Office 365](#BKMK_ManageO365) ? Cambiar la cuenta de administrador que usa para administrar la suscripción. Abra el panel de administración de Office 365 para administrar su suscripción.  
  
-   [¿Vincular el dominio de Internet de su organización con Office 365](#BKMK_StepThree) ? Si desea ser capaz de enviar y recibir correo electrónico dirigido a su propio dominio, puede vincular el dominio con Office 365. (Se ha descrito anteriormente, en [paso 3: Vincular el dominio de su organización con Office 365](#BKMK_StepThree).)  
  
-   [¿Deshabilitar la integración de Office 365](#BKMK_Disable) ? Si no desea administrar los servicios de Office 365, suscripción y cuentas en línea desde el panel, puede deshabilitar la integración de Office 365. Los servicios siguen estando disponibles en el portal de Office 365.  
  
###  <a name="BKMK_ManageO365"></a> Administrar la suscripción de Office 365  
 Si necesita realizar cambios en su suscripción de Office 365 mientras trabaja en el servidor, puede abrir la suscripción de Office 365 desde el **Office 365** página del panel. También puede cambiar la cuenta de administrador que utiliza el servidor para realizar cambios en los servicios de Office 365.  
  
##### <a name="to-open-your-subscription-on-the-office-365-admin-dashboard"></a>Para abrir su suscripción en el panel de administración de Office 365  
  
1.  En el panel de Windows Server Essentials, abra el **Office 365** página.  
  
2.  En **Tareas de configuración**, haga clic en **Administrar Office 365**.  
  
3.  Inicie sesión en Office 365 con la cuenta de Microsoft online que utilizan para administrar la suscripción.  
  
     Se abre el panel de administración de Office 365.  
  
##### <a name="to-change-the-office-365-administrator-account"></a>Para cambiar la cuenta de administrador de Office 365  
  
1.  En el panel, haga clic en **Office 365**.  
  
2.  En **Tareas de configuración**, haga clic en **Cambiar la cuenta de administrador de Office 365**. Se abrirá el asistente para cambiar la cuenta de administrador (En Windows Server Essentials, el asistente se denomina configurar the Office 365 cuenta del administrador).  
  
3.  Escriba las credenciales de la cuenta que desea usar para conectarse a su suscripción de Office 365 y, a continuación, haga clic en **siguiente**.  
  
4.  Haga clic en **Cerrar**. El panel se reiniciará.  
  
###  <a name="BKMK_Disable"></a> Deshabilitar la integración de Office 365  
 Si decide que no desea administrar los servicios de Office 365 y cuentas en línea desde el panel, puede deshabilitar la integración de Office 365. Permanece activa su suscripción de Office 365, así como los cambios de configuración realizados desde el panel. Por ejemplo, recibirá correo electrónico dirigido a un nombre de dominio que ha vinculado a su suscripción de Office 365. No perderá cualquier correo electrónico y los controles establecidos para dispositivos móviles son todavía usa Exchange Online.  
  
 A partir de ahora, va a administrar su suscripción a Office 365, servicios y recursos de Office 365 y los usuarios necesitan administrar las contraseñas de sus cuentas en línea en Office 365. Ya no se realiza la sincronización de contraseñas y la deshabilitación o eliminación de una cuenta de usuario no tendrá efecto en la cuenta de usuario en línea.  
  
 Dado que el software de integración de Office 365 está instalado en el servidor local, puede deshabilitar la característica incluso si no se puede conectar el servicio de integración de Office 365.  
  
##### <a name="to-disable-office-365-integration-with-the-server"></a>Para deshabilitar la integración de Office 365 con el servidor  
  
1.  En el panel, haga clic en **Office 365**.  
  
2.  Haga clic en **Deshabilitar la integración de Office 365**. Se abrirá el asistente para deshabilitar Office 365.  
  
3.  Siga las instrucciones para completar el asistente.  
  
> [!NOTE]
>  Para habilitar de nuevo la integración de Office 365, use el **integrar con Office 365** de tareas en el **servicio** ficha del panel **inicio** página. Para obtener instrucciones, consulte [paso 2: Integrar su servidor de Windows Server Essentials con Microsoft Office 365](#BKMK_StepTwo), anteriormente en este tema.  
  
##  <a name="BKMK_Troubleshoot"></a> Solucionar problemas de integración de Office 365  
 Esta sección proporciona información para ayudarle a solucionar problemas comunes que pueden surgir al usar las características de integración de Office 365 en Windows Server Essentials.  
  
###  <a name="BKMK_AcctsNotCreated"></a> No se han creado algunas cuentas de Microsoft Online Services  
 **Descripción**  
  
 Un intento de crear una o varias cuentas de Microsoft Online Services desde el panel no era correcta.  
  
 **Solución**  
  
1.  Haga clic en el vínculo de la página de finalización del asistente para abrir un archivo de resultados que contiene información detallada sobre cada error de solicitud de creación de cuenta. Por ejemplo, un resultado podría indicarle que ya existe una cuenta de Microsoft Online Services con el mismo nombre que una cuenta solicitada.  
  
2.  Siga las recomendaciones para resolver cada error.  
  
3.  Si el problema continúa, reinicie el servidor y, a continuación, intente crear de nuevo las cuentas en línea.  
  
###  <a name="BKMK_ProblemUninstalling"></a> Hubo un problema desinstalando la integración de Office 365  
 **Descripción**  
  
 Se produjo un error desconocido al intentar deshabilitar la integración de Office 365.  
  
 **Solución**  
  
1.  Asegúrese de que el equipo esté conectado a Internet y, a continuación, vuelva a intentarlo.  
  
2.  Si el error ocurre de nuevo, reinicie el servidor y, a continuación, vuelva a intentarlo.  
  
## <a name="see-also"></a>Vea también  
  
-   [Descripción de la integración de servicios de Windows Server Essentials - parte 1](http://blogs.technet.com/b/sbs/archive/2013/11/04/services-integration-overview-for-windows-server-2012-r2-essentials-part-1.aspx)  
  
-   [Descripción de la integración de servicios de Windows Server Essentials - parte 2](http://blogs.technet.com/b/sbs/archive/2013/11/06/services-integration-overview-for-windows-server-2012-r2-essentials-part-2.aspx)  
  
-   [Guía de inicio rápido de Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md)  
  
-   [Administrar Microsoft Online Services](../manage/Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Administrar Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)
