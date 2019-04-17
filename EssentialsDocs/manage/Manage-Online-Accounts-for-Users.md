---
title: "Administrar cuentas en línea para los usuarios de Windows Server Essentials"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c09f4cf6-4d12-49fe-9ae4-e6cb14027b9d
8author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 95f401bbec9bb503d19e2d9918a05851c04f7ef4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>Administrar cuentas en línea para los usuarios de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Al integrar tu servidor de Windows Server Essentials con Microsoft Office 365, puedes administrar tus cuentas en línea junto con las cuentas de usuario desde el panel. En este tema, todo se descubre lo que se obtiene mediante la administración de los usuarios de cuentas de Microsoft Online Services en el panel, cómo crear y administrar cuentas en línea desde el panel y cómo administrar direcciones de correo electrónico y los grupos de distribución de Exchange Online en el panel.  

  
> [!NOTE]
>  Para administrar tus cuentas de Microsoft Online Services en Windows Server Essentials, debes integrar el servidor con Office 365. Para obtener instrucciones, consulta [administrar Office 365](Manage-Office-365-in-Windows-Server-Essentials.md).  
  
> [!IMPORTANT]
>  Si puedes administrar las cuentas en línea en Windows Server Essentials, acostumbrados a ver las cuentas de Microsoft Online Services denominadas *cuentas de Office 365*. En el panel en Windows Server Essentials, las etiquetas se han cambiado a *cuentas de Microsoft Online Services*, o *cuentas en línea de Microsoft* para abreviar. Las cuentas y los procedimientos son los mismos; solo las etiquetas cambiadas. La mayoría de los procedimientos de este tema usan el término *cuenta en línea*.  
  
## <a name="in-this-topic"></a>En este tema  
  
-   [¿Por qué debo administrar Mis cuentas en línea desde el panel?](#BKMK_WhyManageOnlineAccounts)  
  
-   [Crear cuentas en línea](#BKMK_SECTION_CreateOnlineAccounts)  
  
-   [Administrar cuentas en línea](#BKMK_SECTION_ManageOnlineAccounts)  
  
-   [Administrar direcciones de correo electrónico de Exchange Online](#BKMK_SECTION_ManageEmailAddresses)  
  
-   [Administrar grupos de distribución de Exchange Online](#BKMK_SECTION_ManageDistributionGroups)  
  
##  <a name="BKMK_WhyManageOnlineAccounts"></a>¿Por qué debo administrar Mis cuentas en línea desde el panel?  
 Al usar el panel para asignar una cuenta de Microsoft Online Services para una cuenta de usuario, las contraseñas de cuenta se sincronizan automáticamente, y puede mantener las dos cuentas juntos durante el ciclo de vida de s de cuenta de usuario.  
  
 Es conveniente para el usuario, que puede utilizar la misma contraseña para acceder a los recursos en el servidor y en Office 365. Y puedes aplicar los mismos requisitos de contraseña para acceder a los recursos en Office 365 que necesite para los recursos internos.  
  
### <a name="how-does-password-synchronization-work"></a>¿Cómo funciona la sincronización de contraseñas  
 Al usar el panel para asignar una cuenta de Microsoft Online Services para una cuenta de usuario, la contraseña de cuenta de usuario se sincroniza automáticamente con la cuenta en línea de s de usuario. Esto significa que un usuario solo necesita una sola contraseña para acceder a los recursos en el servidor y en Office 365. Además, puedes usar el mismo nombre para la cuenta de usuario y el identificador de usuario s en línea.  
  
 Sincronización de contraseña se produce inmediatamente y automáticamente cuando un usuario cambia la contraseña de su cuenta de usuario desde un equipo unido al dominio o mediante acceso Web remoto.  
  
> [!IMPORTANT]
>  Si Office 365 está integrado en Windows Server Essentials, los usuarios no deben cambiar la contraseña de su cuenta de Microsoft en línea desde el portal de Office 365. Si lo haces, se interrumpirá la sincronización de contraseñas.  
  
### <a name="simplified-account-creation"></a>Creación de cuentas simplificada  
 Hay otra ventaja cuando crees tus cuentas en línea iniciales desde el panel. Puedes crear cuentas en línea para todos los usuarios con una única acción. Por otro lado, si los empleados usar Office 365 ya y puedes configurar un servidor de Windows Server Essentials de nuevo, puedes crear todas tus cuentas de usuario de las cuentas en línea con una única acción. Para obtener más información, consulta [crear cuentas en línea](#BKMK_SECTION_CreateOnlineAccounts).  
  
### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>Administrar direcciones de correo electrónico y los grupos de distribución desde el panel  
 Podrás administrar tus direcciones de correo electrónico y los grupos de distribución de Exchange Online en el panel. Y puedes usar el dominio de Internet de s de organización en las direcciones de correo electrónico. Puedes hacerlo todo esto desde el panel, sin iniciar sesión en Office 365. (Puedes tendrá que usar Windows Server Essentials para administrar los grupos de distribución desde el panel. Esta característica no se admite en Windows Server Essentials.) Para obtener más información, consulta [administrar direcciones de correo electrónico de Exchange Online](#BKMK_SECTION_ManageEmailAddresses) y [administrar grupos de distribución de Exchange Online](#BKMK_SECTION_ManageDistributionGroups).  
  
### <a name="manage-the-user-account-and-online-account-together"></a>Administrar la cuenta de usuario y la cuenta en línea juntos  
 Y puedes administrar una cuenta en línea junto con la cuenta de usuario durante el ciclo de vida de s de cuenta. Si desactiva la cuenta de usuario, también se desactiva la cuenta en línea de Microsoft Online Services. Si quitas una cuenta de usuario, también se quita la cuenta en línea. Para obtener más información, consulta [administrar cuentas en línea](#BKMK_SECTION_ManageOnlineAccounts).  
  
##  <a name="BKMK_SECTION_CreateOnlineAccounts"></a>Crear cuentas en línea  
 Después de integrar el servidor con Office 365, es la ventaja de crear cuentas de Microsoft Online Services para los usuarios desde el panel. Tiene mucha flexibilidad en la creación de las cuentas en línea. Si tienes una nueva suscripción a Office 365, puedes masiva-crear cuentas en línea para todos los usuarios. Si ya has creado tus cuentas en línea en Office 365, no te preocupes. Si puedes configurar un servidor nuevo, puedes crear tus cuentas de usuario en el servidor mediante la importación de las cuentas en línea. Y puedes asignar un nuevo o una cuenta en línea cuando creas una cuenta de usuario o cuando agregas una cuenta en línea a una cuenta de usuario.  
  
 **Requisitos de licencia** necesitarás una licencia de usuario para cada cuenta en línea que crees. Comprobar la **Office 365** página en el panel para ver cuántas licencias de usuario están disponibles a través de tu suscripción a Office 365. Si es necesario agregar más licencias de usuario, todo se podrá abrir tu suscripción a Office 365 en Office 365 para hacerlo.  
  
 **Seguimiento de los usuarios** después de agregar una cuenta en línea para una cuenta de usuario, el usuario debe cambiar la contraseña de su cuenta de usuario la próxima vez que inicien sesión. La nueva contraseña se sincroniza inmediatamente con su cuenta en línea. Después, pueden usar la contraseña para iniciar sesión en Office 365 con su identificador en línea.  
  
> [!IMPORTANT]
>  Resalte a los usuarios que nunca deben cambiar su contraseña de cuenta en línea en Office 365. La contraseña se cambiará automáticamente cada vez que cambie la contraseña de su cuenta de usuario. Si cambia la contraseña en línea en Office 365, se interrumpirá la sincronización de contraseñas.  
  
 Usa los procedimientos descritos en esta sección para:  
  
-   [Crear de forma masiva cuentas en línea para las cuentas de usuario existentes](#BKMK_ToBulkCreateOnlineAccounts)  
  
-   [Importar cuentas de usuario de tus cuentas de Microsoft Online Services](#BKMK_ToImportUserAccounts)  
  
-   [Crear una nueva cuenta de usuario con una cuenta en línea que asigna a](#BKMK_ToCreateaNewUserAccount)  
  
-   [Asignar una cuenta en línea a una cuenta de usuario](#BKMK_ToAssignAnOnlineAccount)  
  
> [!NOTE]
>  Si se utiliza Windows Server Essentials, verá *cuenta de Office 365* en lugar de *cuenta de Microsoft Online Services* a lo largo de estos procedimientos. El proceso es el mismo, pero la terminología cambiado en Windows Server Essentials.  
  
###  <a name="BKMK_ToBulkCreateOnlineAccounts"></a>Para crear cuentas en línea para las cuentas de usuario existentes masiva  
  
1.  Inicia sesión en el servidor como administrador y abre el escritorio de Windows Server Essentials.  
  
2.  En el panel, abre el **usuarios** página.  
  
3.  En **tareas de los usuarios**, haz clic en **cuentas en línea de Microsoft agregar**.  
  
     La **cuentas agregar Microsoft Online Services** página del asistente muestra todas las cuentas de usuario que no tengan una cuenta de Microsoft en línea. Todas las cuentas se seleccionan de forma predeterminada y se sugiere el nombre de usuario para el identificador de Microsoft en línea. Si un dominio de Internet personalizado que has vinculado a tu suscripción a Office 365, ese dominio se usará de manera predeterminada.  
  
4.  En la **cuentas agregar Microsoft Online Services** página, revisa las cuentas que se crearán. Por ejemplo, se comprueba para los usuarios que ya tienen una cuenta en línea con un identificador en línea diferente y asegúrate de que el dominio que quieras usar direcciones de correo electrónico está seleccionado. Cuando termines de realizar cualquiera los cambios necesarios, haz clic en **siguiente**.  
  
5.  En la **licencias asignar Microsoft Online Services** página, los usuarios usarán seleccionados servicios de Office 365. Al hacer clic en **siguiente**, empezará a la creación de cuentas.  
  
    > [!NOTE]
    >  Debes tener una licencia de servicio en Office 365 para cada cuenta en línea. Puedes comprobar las licencias disponibles y, si es necesario, abre tu suscripción para agregar licencias de la **Office 365** página en el panel.  
  
6.  Notificar a los usuarios que ya tienen una cuenta de Microsoft en línea. Deben cambiar su contraseña de cuenta de usuario de red antes de poder iniciar sesión Office 365. Para obtener instrucciones, consulta [a empezar a usar una nueva cuenta en línea de Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
###  <a name="BKMK_ToBeginUsingAnOnlineAccount"></a>Para empezar a usar una nueva cuenta en línea de Microsoft  
  
1.  Inicia sesión en el equipo con tu cuenta de usuario de la red.  
  
2.  Cambiar la contraseña de tu cuenta de usuario. Para empezar, presiona Ctrl + Alt + Supr y haz clic en **cambiar una contraseña**.  
  
     Cuando cambias la contraseña, la contraseña se sincroniza con la nueva cuenta en línea. Ahora puedes usar la misma contraseña para iniciar sesión Office 365.  
  
3.  [inicia sesión en Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out) con el nuevo identificador en línea y la contraseña de tu cuenta de usuario.  
  
    > [!IMPORTANT]
    >  No se cambia la contraseña de cuenta en línea en Office 365. Esto interrumpirá la sincronización de contraseñas. La contraseña en línea se actualizará cada vez que cambie la contraseña de tu cuenta de usuario de la red.  
  
###  <a name="BKMK_ToImportUserAccounts"></a>Para importar cuentas de usuario de tus cuentas en línea existentes  
  
1.  En el panel, abre el **usuarios** página.  
  
2.  En **tareas de los usuarios**, haz clic en **importar cuentas de Office 365**.  
  
     La siguiente página muestra todas las cuentas en línea para tu suscripción a Office 365 que no tengan una cuenta de usuario en el servidor. Todas las cuentas se seleccionan de forma predeterminada, y el identificador en línea se sugiere el nombre de usuario.  
  
3.  Para crear las cuentas de usuario:  
  
    1.  Realiza los cambios que son necesarios para las cuentas de usuario propuesto.  
  
    2.  Opcionalmente, haz clic en el vínculo para ver las contraseñas temporales que se asignará a las cuentas de usuario. Debes ofrecer a los usuarios su contraseña temporal junto con su nuevo nombre de cuenta.  
  
         (Después de crear las cuentas, todo puedes encontrar estas contraseñas enumeradas en este archivo: *SystemDrive*\Users\\*Office365admin*\\*NewServerUser*.txt donde *Office365admin* es la cuenta de red que se usa para administrar Office 365 en el servidor y *NewServerUser* es el nuevo nombre de cuenta de usuario.)  
  
    3.  Haz clic en **siguiente** para crear las cuentas de usuario.  
  
4.  Permite que los usuarios a saber sus nuevas cuentas de usuario y las contraseñas temporales se usan para iniciar sesión en el servidor para la primera œ tiempo y que tendrán que cambiar la contraseña después de iniciar sesión. Para obtener instrucciones para los usuarios, consulta [a empezar a usar una nueva cuenta en línea de Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
     Asegúrate de que sabe que las contraseñas de su cuenta en línea se sincronizarán con su cuenta de usuario en el futuro, y no deben cambiar su contraseña en línea en Office 365.  
  
###  <a name="BKMK_ToCreateaNewUserAccount"></a>Para crear una nueva cuenta de usuario con una cuenta en línea que asigna a  
  
1.  En el panel, haz clic en **usuarios**.  
  
2.  En **tareas de los usuarios**, haz clic en **agregar una cuenta de usuario**. La adición de que aparece un asistente de cuenta de usuario.  
  
3.  Sigue las instrucciones para crear la cuenta de usuario.  
  
4.  En la **asignar una cuenta de Microsoft Online Services** página, crear un nuevo en línea de la cuenta del usuario o asignar una cuenta en línea:  
  
    -   Para crear una nueva cuenta en línea, haz clic en **crear una nueva cuenta de Microsoft Online Services y asigna a esta cuenta de usuario**y escribe un nombre para la cuenta de Microsoft Online Services (de manera predeterminada, el nombre de usuario se usa para el identificador en línea). A continuación, haz clic en **siguiente**.  
  
    -   Para asignar una cuenta en línea de Microsoft existente, haz clic en **asignar una cuenta de Microsoft Online Services existente a esta cuenta de usuario**y selecciona una cuenta existente en la lista desplegable. A continuación, haz clic en **siguiente**.  
  
    > [!NOTE]
    >  En Windows Server Essentials, cuentas de Microsoft Online Services se conocen como cuentas de Office 365 en asistentes y las etiquetas del panel.  
  
5.  Sigue las instrucciones para completar al asistente.  
  
6.  Notificar al usuario que se necesitan para cambiar la contraseña de su cuenta de usuario antes de que puede iniciar sesión en Office 365 con su nueva cuenta en línea. Para obtener instrucciones, consulta [a empezar a usar una nueva cuenta en línea de Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
#### <a name="BKMK_ToAssignAnOnlineAccount"></a>Para asignar una cuenta en línea a una cuenta de usuario  
  
1.  En el panel, haz clic en **usuarios**.  
  
2.  Haz clic en la cuenta de usuario en la lista y, a continuación, haz clic en **asignar una cuenta de Microsoft en línea**. Aparece de la asignar un asistente de cuenta de servicios en línea de Microsoft.  
  
3.  Asignar una cuenta en línea existente o crear uno nuevo para el usuario. El identificador en línea predeterminado para una nueva cuenta es el nombre de usuario. A continuación, haz clic en **siguiente** para agregar la cuenta en línea a la cuenta de usuario.  
  
4.  Revisa la información de la última página del asistente y, a continuación, haz clic en **cerrar**.  
  
5.  Notificar al usuario que se necesitan para cambiar la contraseña de su cuenta de usuario antes de que puede iniciar sesión en Office 365 con su nueva cuenta en línea. Para obtener instrucciones, consulta [a empezar a usar una nueva cuenta en línea de Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
##  <a name="BKMK_SECTION_ManageOnlineAccounts"></a>Administrar cuentas en línea  
 Cuando agregas una cuenta en línea a una cuenta de usuario en Windows Server Essentials, puedes administrar ambas cuentas juntos durante el ciclo de vida de la cuenta s.  
  
###  <a name="BKMK_UnderstandingAccountStatus"></a>Descripción del estado de la cuenta en línea  
 Al asignar una cuenta de Microsoft Online Services para una cuenta de usuario, la dirección de correo electrónico de la cuenta aparece en la **cuenta en línea de Microsoft** columna en la **usuarios** página del panel. (En Windows Server Essentials, la etiqueta de columna es **cuenta de Office 365**.)  
  
-   Un icono azul junto a una dirección de correo electrónico indica que la cuenta en línea está activa. Es decir, la cuenta tiene una licencia de Office 365 actual y el usuario puede usar el identificador en línea para iniciar sesión Office 365.  
  
-   Un icono junto a la dirección de correo electrónico sombreado indica la cuenta en línea está inactivo œ porque ya no está activa la licencia o la cuenta en línea se ha sin asignar. Al anular la asignación de una cuenta en línea de s de usuario, se quita la licencia y el usuario podrá iniciar sesión en Office 365 con la cuenta. Sin embargo, el servidor mantiene la asignación entre el nombre de cuenta de usuario y la dirección de correo electrónico de Office 365.  
  
###  <a name="BKMK_UnassignOnlineAccount"></a>Restringir el acceso a una cuenta en línea  
 ¿Qué hacer si un usuario sale de la organización, o quieres restringir el acceso de usuario s a los servicios de Office 365? Si puedes administrar tus cuentas de usuarios en línea junto con sus cuentas de usuario en Windows Server Essentials, tienes tres opciones:  
  
-   **¿Anular la asignación de la cuenta en línea** ? Si quieres impedir que un usuario mediante Office 365 sin impedir el acceso a los recursos en el servidor, debe anular la asignación de la cuenta en línea. Se lanzará la licencia de Office 365 y el usuario podrá iniciar sesión en Office 365. Sin embargo, el servidor mantiene la asignación entre el nombre de cuenta de usuario y la dirección de correo electrónico de Office 365. Para obtener instrucciones, consulta [para anular la asignación de una cuenta en línea desde una cuenta de usuario](#BKMK_ToUnassignAnOnlineAccount).  
  
-   **¿Desactivar la cuenta de usuario** ? Si desactiva una cuenta de usuario porque un empleado abandona, temporal o permanente, también se desactiva la cuenta en línea de s de usuario. No se puede usar la cuenta en línea, pero se conservan los datos de usuario, incluido el correo electrónico, en Microsoft Online Services. Para obtener instrucciones, consulta [desactivar una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6) en [administrar cuentas de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
-   **¿Quitar la cuenta de usuario** ? Si quitas una cuenta de usuario, se quita la cuenta en línea de Microsoft Online Services también. Para obtener instrucciones, consulta [quitar una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove) en [administrar cuentas de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
    > [!WARNING]
    >  Ten en cuenta que cuando se quita una cuenta en línea, los datos de usuario están sujeta a los datos de directivas de retención de Microsoft Online Services. Si necesitas conservar los datos de usuario de la persona s después de un empleado abandona, desactivar la cuenta de usuario en lugar de quitarla.  
  
####  <a name="BKMK_ToUnassignAnOnlineAccount"></a>Para anular la asignación de una cuenta en línea desde una cuenta de usuario  
  
1.  En el panel, haz clic en **usuarios**.  
  
2.  Haz clic en la cuenta de usuario en la lista y, a continuación, haz clic en **anular la asignación de una cuenta de Microsoft en línea** (en Windows Server Essentials, haz clic en **anular la asignación de una cuenta de Office 365**).  
  
3.  En el aviso de confirmación, haz clic en **Sí**.  
  
##  <a name="BKMK_SECTION_ManageEmailAddresses"></a>Administrar direcciones de correo electrónico de Exchange Online  
 Al agregar direcciones de correo electrónico a la cuenta de usuario s en línea en Windows Server Essentials, puede permitir al usuario recibir correo electrónico en varias direcciones de correo electrónico de Exchange en línea.  
  
###  <a name="BKMK_PROC_AddEmailAliases"></a>Para agregar direcciones de correo electrónico adicional a un usuario s cuenta en línea de Microsoft  
  
1.  En el panel de Windows Server Essentials, haz clic en **usuarios**.  
  
2.  Haz clic en la cuenta de usuario en la lista y, a continuación, haz clic en **ver las propiedades de la cuenta**.  
  
3.  En la **Microsoft online** ficha de las propiedades de la cuenta (o la **Office 365** ficha en Windows Server Essentials), haz clic en **agregar**.  
  
4.  Escribe el nuevo alias de correo electrónico y, a continuación, seleccione el dominio de correo electrónico.  
  
5.  Haz clic en **Aceptar** dos veces.  
  
##  <a name="BKMK_SECTION_ManageDistributionGroups"></a>Administrar grupos de distribución de Exchange Online (solo Windows Server Essentials)  
 Después de integrar el servidor de Windows Server Essentials con Office 365, puedes crear y administrar grupos de distribución de Exchange Online desde el escritorio de Windows Server Essentials. Hacer todo esto en la **grupos de distribución** pestaña que se agrega a la **usuarios** página. Solo verás esta pestaña si tienes una suscripción a Exchange Online. Esta característica no está disponible en Windows Server Essentials.  
  
 Usa los siguientes procedimientos para:  
  
-   [Agregar un grupo de distribución](#BKMK_PROCEDURE_AddDistGroup)  
  
-   [Cambiar a los miembros de un grupo de distribución](#BKMK_ChangeGroupMembers)  
  
-   [Cambiar una pertenencia a grupos de distribución de usuario s](#BKMK_EditUserMemberships)  
  
-   [Quitar un grupo de distribución](#BKMK_RemoveDistributionGroup)  
  
###  <a name="BKMK_PROCEDURE_AddDistGroup"></a>Para agregar un grupo de distribución  
  
1.  En el panel en Windows Server Essentials, haz clic en **usuarios**y, a continuación, haz clic en el **grupos de distribución** pestaña.  
  
2.  En **tareas de grupo de distribución**, haz clic en **agregar un grupo de distribución**.  
  
     Agregar un Asistente para nuevo grupo de distribución aparece.  
  
3.  En la **agregar un nuevo grupo de distribución** página, escribe la información siguiente y, a continuación, haz clic en **siguiente**:  
  
    -   Escribe un nombre de grupo, la descripción opcional y el alias de correo electrónico para el nuevo grupo de distribución.  
  
    -   De manera predeterminada, el grupo de distribución puede recibir correo electrónico de las personas fuera de tu organización. Si no desea permitirlo, desactive la opción.  
  
4.  En la **agregar miembros del grupo** página, usa el **agregar** botón para agregar cuentas de usuario activa que tienen una cuenta en línea asignada a ellos, y los grupos de distribución de otro, al nuevo grupo de distribución. A continuación, haz clic en **siguiente**.  
  
     El nuevo grupo de distribución se crea en Exchange Online.  
  
###  <a name="BKMK_ChangeGroupMembers"></a>Para cambiar a los miembros de un grupo de distribución  
  
1.  En el panel, haz clic en **usuarios**y, a continuación, haz clic en el **grupos de distribución** pestaña.  
  
2.  Haz clic en el grupo de distribución en la lista y, a continuación, haz clic en **cambiar la pertenencia al grupo**.  
  
3.  Usa el **agregar** y **quitar** botones para agregar o quitar cuentas en línea activas desde el grupo de distribución. A continuación, haz clic en **siguiente** para actualizar la pertenencia al grupo de distribución en Exchange Online.  
  
###  <a name="BKMK_EditUserMemberships"></a>Para cambiar la pertenencia a grupos de distribución un usuario s  
  
1.  En el panel, haz clic en **usuarios**.  
  
2.  Haz clic en la cuenta de usuario en la lista y, a continuación, haz clic en **ver las propiedades de la cuenta**.  
  
3.  En las propiedades de la cuenta de usuario, haz clic en el **grupos de distribución** pestaña y, a continuación, haz clic en **editar**.  
  
4.  En la **pertenencia al grupo de editar** cuadro, usa el **agregar** y **quitar** botones para agregar o quitar grupos de distribución de la cuenta de usuario y, a continuación, haz clic en **cerrar**.  
  
5.  Haz clic en **Aceptar** ahorrar al usuario actualizado propiedades de la cuenta.  
  
###  <a name="BKMK_RemoveDistributionGroup"></a>Para quitar un grupo de distribución  
  
1.  En el panel, haz clic en **usuarios**y, a continuación, haz clic en el **grupos de distribución** pestaña.  
  
2.  Haz clic en el grupo de distribución en la lista y, a continuación, haz clic en **quitar el grupo**.  
  
3.  En el aviso de confirmación, haz clic en **eliminación de un grupo**.  
  
     Se quita el grupo de distribución de Exchange Online.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar cuentas de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  
-   [Administrar Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Administrar Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
