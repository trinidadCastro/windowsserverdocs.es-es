---
title: Administrar cuentas en línea para usuarios de Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824096"
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>Administrar cuentas en línea para usuarios de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Al integrar su servidor Windows Server Essentials con Microsoft Office 365, puede administrar sus cuentas en línea junto con las cuentas de usuario desde el panel. En este tema, ll averiguar cómo administrar direcciones de correo electrónico y grupos de distribución para Exchange Online, cómo crear y administrar cuentas en línea desde el panel y lo que puede obtener mediante la administración de los usuarios de cuentas de Microsoft Online Services desde el panel desde el panel.  

  
> [!NOTE]
>  Para administrar las cuentas de Microsoft Online Services en Windows Server Essentials, debe integrar su servidor con Office 365. Para obtener instrucciones, consulte [administrar Office 365](Manage-Office-365-in-Windows-Server-Essentials.md).  
  
> [!IMPORTANT]
>  Si usted administra cuentas en línea en Windows Server Essentials, están acostumbrados a ver las cuentas de Microsoft Online Services como *cuentas de Office 365*. En el panel en Windows Server Essentials, las etiquetas se llaman ahora *cuentas de Microsoft Online Services*, o *cuentas de Microsoft online* para abreviar. Las cuentas y los procedimientos son iguales, salvo las etiquetas. En la mayoría de los procedimientos de este tema se usa el término *cuenta en línea*.  
  
## <a name="in-this-topic"></a>En este tema  
  
-   [¿Por qué debo administrar Mis cuentas en línea desde el panel?](#BKMK_WhyManageOnlineAccounts)  
  
-   [Crear cuentas en línea](#BKMK_SECTION_CreateOnlineAccounts)  
  
-   [Administrar cuentas en línea](#BKMK_SECTION_ManageOnlineAccounts)  
  
-   [Administrar direcciones de correo electrónico de Exchange Online](#BKMK_SECTION_ManageEmailAddresses)  
  
-   [Administrar grupos de distribución para Exchange Online](#BKMK_SECTION_ManageDistributionGroups)  
  
##  <a name="BKMK_WhyManageOnlineAccounts"></a> ¿Por qué debo administrar Mis cuentas en línea desde el panel?  
 Cuando se usa el panel para asignar una cuenta de Microsoft Online Services a una cuenta de usuario, las contraseñas de cuenta se sincronizan automáticamente y puede mantener las dos cuentas juntas a lo largo del ciclo de vida de s de cuenta de usuario.  
  
 Lo s cómoda para el usuario, que puede usar la misma contraseña para acceder a recursos en el servidor y en Office 365. Y puede aplicar los mismos requisitos de contraseña para el acceso a los recursos de Office 365 que necesite para sus recursos internos.  
  
### <a name="how-does-password-synchronization-work"></a>¿Cómo funciona la sincronización de contraseña?  
 Cuando se usa el panel para asignar una cuenta de Microsoft Online Services a una cuenta de usuario, contraseña de la cuenta de usuario se sincroniza automáticamente con la cuenta de usuario s en línea. Esto significa que un usuario solo necesita una contraseña para acceder a los recursos en el servidor y en Office 365. Además, puede usar el mismo nombre para la cuenta de usuario y el identificador de usuario s en línea.  
  
 La sincronización de contraseñas tiene lugar inmediata y automáticamente cuando un usuario cambia la contraseña de su cuenta de usuario desde un equipo unido a un dominio o mediante el acceso web remoto.  
  
> [!IMPORTANT]
>  Si se integra Office 365 con Windows Server Essentials, los usuarios no deben cambiar la contraseña de su cuenta en línea de Microsoft desde el portal de Office 365. Al hacerlo, se interrumpirá la sincronización de la contraseña.  
  
### <a name="simplified-account-creation"></a>Creación de cuentas simplificada  
 Allí s otra ventaja al crear sus cuentas en línea iniciales desde el panel. Puede crear cuentas en línea para todos los usuarios con una sola acción. Por otro lado, si los empleados ya usan Office 365 y configuración de un nuevo servidor de Windows Server Essentials, puede crear todas las cuentas de usuario de las cuentas en línea con una sola acción. Para obtener más información, consulte [Crear cuentas en línea](#BKMK_SECTION_CreateOnlineAccounts).  
  
### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>Administrar direcciones de correo electrónico y grupos de distribución desde el panel  
 Podrá administrar sus direcciones de correo electrónico y grupos de distribución de Exchange Online desde el panel. Y puede usar su dominio de Internet de s de organización en las direcciones de correo electrónico. Puede hacer todo esto desde el panel, sin iniciar sesión en Office 365. (Ll debe usar Windows Server Essentials para administrar grupos de distribución desde el panel. Esta característica no se admite en Windows Server Essentials.) Para obtener más información, vea cómo [administrar direcciones de correo electrónico para Exchange Online](#BKMK_SECTION_ManageEmailAddresses) y [administrar grupos de distribución para Exchange Online](#BKMK_SECTION_ManageDistributionGroups).  
  
### <a name="manage-the-user-account-and-online-account-together"></a>Administrar la cuenta de usuario y la cuenta en línea de forma conjunta  
 Y puede administrar una cuenta en línea junto con la cuenta de usuario durante el ciclo de vida de s de cuenta. Si desactiva la cuenta de usuario, también se desactiva la cuenta en línea de Microsoft Online Services. Si quita una cuenta de usuario, también se quita la cuenta en línea. Para obtener más información, vea cómo [administrar cuentas en línea](#BKMK_SECTION_ManageOnlineAccounts).  
  
##  <a name="BKMK_SECTION_CreateOnlineAccounts"></a> Crear cuentas en línea  
 Después de integrar su servidor con Office 365, lo cuentas s aprovechar para crear Microsoft Online Services para los usuarios desde el panel. Ll tiene mucha flexibilidad para crear las cuentas en línea. Si tiene una nueva suscripción de Office 365, puede bulk-crear cuentas en línea para todos los usuarios. Si ya ha creado sus cuentas en línea en Office 365, no se preocupe de t. Si la configuración de un servidor nuevo, se pueden crear las cuentas de usuario en el servidor importando las cuentas en línea. Y puede asignar una nueva o una cuenta en línea existente al crear una cuenta de usuario individual o al agregar una cuenta en línea a una cuenta de usuario existente.  
  
 **Requisitos de licencia** necesitará una licencia de usuario para cada cuenta en línea que cree. Compruebe el **Office 365** página en el panel para ver cuántas licencias de usuario están disponibles a través de su suscripción de Office 365. Si necesita agregar más licencias de usuario, ll, podrá abrir su suscripción de Office 365 en Office 365 para hacerlo.  
  
 **Seguimiento de los usuarios** después de agregar una cuenta en línea para una cuenta de usuario, el usuario debe cambiar la contraseña de su cuenta de usuario la próxima vez que inicien sesión. La nueva contraseña se sincroniza inmediatamente con su cuenta en línea. Después de eso, puede usar la contraseña para iniciar sesión en Office 365 con su identificador en línea.  
  
> [!IMPORTANT]
>  Advierta a los usuarios que nunca deben cambiar su contraseña de cuenta en línea en Office 365. La contraseña se cambiará automáticamente cada vez que cambie la contraseña de su cuenta de usuario. Si cambia la contraseña en línea en Office 365, se interrumpirá la sincronización de contraseña.  
  
 Use los procedimientos de esta sección para:  
  
-   [Crear cuentas en línea para las cuentas de usuario de forma masiva](#BKMK_ToBulkCreateOnlineAccounts)  
  
-   [Importar cuentas de usuario de las cuentas de Microsoft Online Services](#BKMK_ToImportUserAccounts)  
  
-   [Crear una nueva cuenta de usuario con una cuenta en línea asignada](#BKMK_ToCreateaNewUserAccount)  
  
-   [Asignar una cuenta en línea a una cuenta de usuario](#BKMK_ToAssignAnOnlineAccount)  
  
> [!NOTE]
>  Si se utiliza Windows Server Essentials, verá *cuenta de Office 365* en lugar de *cuenta de Microsoft Online Services* a lo largo de estos procedimientos. El proceso es el mismo, pero la terminología ha cambiado en Windows Server Essentials.  
  
###  <a name="BKMK_ToBulkCreateOnlineAccounts"></a> Para crear cuentas en línea para las cuentas de usuario de forma masiva  
  
1.  Inicie sesión en el servidor como administrador y abra el panel de Windows Server Essentials.  
  
2.  En el panel, abra la página **Usuarios**.  
  
3.  En **Tareas de los usuarios**, haga clic en **Agregar cuentas en línea de Microsoft**.  
  
     La página **Agregar cuentas de Microsoft Online Services** del asistente muestra todas las cuentas de usuario que no tienen una cuenta en línea de Microsoft. Todas las cuentas están seleccionadas de forma predeterminada y se sugiere el nombre de usuario para el identificador en línea de Microsoft. Si ha vinculado un dominio de Internet personalizado a su suscripción de Office 365, se usará ese dominio de forma predeterminada.  
  
4.  En la página **Agregar cuentas de Microsoft Online Services** , revise las cuentas que va a crear. Por ejemplo, compruebe los usuarios que ya tienen una cuenta con un identificador en línea distinto y asegúrese de que el dominio que quiere usar en las direcciones de correo electrónico esté seleccionado. Cuando termine de hacer los cambios necesarios, haga clic en **Siguiente**.  
  
5.  En el **licencias asignar Microsoft Online Services** página, seleccionados servicios de Office 365 que usarán los usuarios. Al hacer clic en **Siguiente**, se iniciará la creación de cuentas.  
  
    > [!NOTE]
    >  Debe tener una licencia de servicio de Office 365 para cada cuenta en línea. Puede consultar las licencias disponibles y, si fuera necesario, abrir la suscripción para agregar licencias desde la página **Office 365** del panel.  
  
6.  Notifique a los usuarios que ya tienen una cuenta en línea de Microsoft. Debe cambiar la contraseña de su cuenta de usuario de red antes de iniciar sesión en Office 365. Para obtener instrucciones, consulte cómo [empezar a usar una nueva cuenta en línea de Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
###  <a name="BKMK_ToBeginUsingAnOnlineAccount"></a> Para empezar a usar una nueva cuenta en línea de Microsoft  
  
1.  Inicie sesión en el equipo con la cuenta de usuario de red.  
  
2.  Cambie la contraseña de la cuenta. Para empezar, presione Ctrl+Alt+Supr y, a continuación, haga clic en **Cambiar una contraseña**.  
  
     Al cambiar la contraseña, esta se sincroniza con la nueva cuenta en línea. Ahora puede usar la misma contraseña para iniciar sesión en Office 365.  
  
3.  [Inicie sesión en Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out) con su nuevo identificador en línea y su contraseña de cuenta de usuario.  
  
    > [!IMPORTANT]
    >  No cambie la contraseña de su cuenta en línea en Office 365. ya que interrumpirá la sincronización de la contraseña. La contraseña en línea se actualizará cada vez que cambie la contraseña para la cuenta de usuario de red.  
  
###  <a name="BKMK_ToImportUserAccounts"></a> Para importar las cuentas de usuario de sus cuentas en línea existentes  
  
1.  En el panel, abra la página **Usuarios**.  
  
2.  En **Tareas de los usuarios**, haga clic en **Importar cuentas de Office 365**.  
  
     La siguiente página muestra todas las cuentas en línea para la suscripción de Office 365 que no tienen una cuenta de usuario en el servidor. Todas las cuentas están seleccionadas de forma predeterminada y se sugiere el identificador en línea para el nombre de usuario.  
  
3.  Para crear las cuentas de usuario:  
  
    1.  Realice los cambios necesarios para las cuentas de usuario propuestas.  
  
    2.  También puede hacer clic en el vínculo para ver las contraseñas temporales que se asignarán a las cuentas de usuario. Tendrá que darle a los usuarios su contraseña temporal junto con su nuevo nombre de cuenta.  
  
         (Después de crear las cuentas, se encontrará estas contraseñas enumeradas en este archivo: *SystemDrive*\Users\\*Office365admin*\\*NewServerUser*.txt donde *Office365admin* es la cuenta de red que se utiliza para administrar Office 365 en el servidor y *NewServerUser* es el nuevo nombre de cuenta de usuario.)  
  
    3.  Haga clic en **Siguiente** para crear las cuentas de usuario.  
  
4.  Informar a los usuarios sus nuevas cuentas de usuario y las contraseñas temporales usará para iniciar sesión en el servidor para el primer œ tiempo y que tendrá que cambiar la contraseña después de iniciar sesión. Para obtener instrucciones para los usuarios, consulte [Para empezar a usar una nueva cuenta en línea de Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
     Asegúrese de que sepan que las contraseñas de su cuenta en línea se sincronizarán con su cuenta de usuario en el futuro, y que no deben cambiar su contraseña en línea en Office 365.  
  
###  <a name="BKMK_ToCreateaNewUserAccount"></a> Para crear una nueva cuenta de usuario con una cuenta en línea asignada  
  
1.  En el panel, haga clic en **Usuarios**.  
  
2.  En **Tareas de los usuarios**, haga clic en **Agregar una cuenta de usuario**. Se abrirá el asistente para agregar una cuenta de usuario.  
  
3.  Siga las instrucciones que se indican.  
  
4.  En la página **Asignar una cuenta de Microsoft Online Services**, cree una cuenta en línea para el usuario o asigne una cuenta en línea existente:  
  
    -   Para crear una cuenta en línea, haga clic en **Crear una cuenta de Microsoft Online Services y asignarla a esta cuenta de usuario**y escriba un nombre para la cuenta de Microsoft Online Services (de forma predeterminada, el nombre de usuario se usa para el identificador en línea). Haga clic en **Siguiente**.  
  
    -   Para asignar una cuenta de Microsoft en línea existente, haga clic en **Asignar una cuenta de Microsoft Online Services existente a esta cuenta de usuario**y seleccione una cuenta existente en la lista desplegable. Haga clic en **Siguiente**.  
  
    > [!NOTE]
    >  En Windows Server Essentials, las cuentas de Microsoft Online Services se conocen como cuentas de Office 365 en los asistentes y las etiquetas del panel.  
  
5.  Siga las instrucciones para completar el asistente.  
  
6.  Notifique al usuario que debe cambiar su contraseña de cuenta de usuario para iniciar sesión en Office 365 con su cuenta en línea. Para obtener instrucciones, consulte cómo [empezar a usar una nueva cuenta en línea de Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
#### <a name="BKMK_ToAssignAnOnlineAccount"></a>Para asignar una cuenta en línea a una cuenta de usuario  
  
1.  En el panel, haga clic en **Usuarios**.  
  
2.  Haga clic con el botón secundario en la cuenta de usuario de la lista y, a continuación, haga clic en **Asignar una cuenta en línea de Microsoft**. Se abrirá el asistente para asignar una cuenta de Microsoft Online Services.  
  
3.  Asigne una cuenta en línea existente o crear una nueva para el usuario. El identificador en línea predeterminado para una nueva cuenta es el nombre de usuario. Después, haga clic en **Siguiente** para agregar la cuenta en línea a la cuenta de usuario.  
  
4.  Revise la información en la última página del asistente y, a continuación, haga clic en **Cerrar**.  
  
5.  Notifique al usuario que debe cambiar su contraseña de cuenta de usuario para iniciar sesión en Office 365 con su cuenta en línea. Para obtener instrucciones, consulte cómo [empezar a usar una nueva cuenta en línea de Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
##  <a name="BKMK_SECTION_ManageOnlineAccounts"></a> Administrar cuentas en línea  
 Al agregar una cuenta en línea a una cuenta de usuario en Windows Server Essentials, puede administrar ambas cuentas juntas a lo largo del ciclo de vida de la cuenta s.  
  
###  <a name="BKMK_UnderstandingAccountStatus"></a> Descripción del estado de la cuenta en línea  
 Al asignar una cuenta de Microsoft Online Services a una cuenta de usuario, la dirección de correo electrónico de la cuenta aparece en la columna **Cuenta en línea de Microsoft**, en la página **Usuarios** del panel. (En Windows Server Essentials, la etiqueta de columna es **cuenta de Office 365**.)  
  
-   El icono azul que hay junto a la dirección de correo electrónico indica que la cuenta en línea está activa. Es decir, la cuenta tiene una licencia de Office 365, y el usuario puede utilizar el identificador en línea para iniciar sesión en Office 365.  
  
-   Junto a la dirección de correo electrónico el icono atenuado indica que la cuenta en línea está inactiva œ porque ya no está activa la licencia o la cuenta en línea se ha anulado la asignación. Al anular la asignación de una cuenta de usuario s en línea, se quita la licencia y el usuario no podrá iniciar sesión en Office 365 con la cuenta. Sin embargo, el servidor mantiene la asignación entre el nombre de cuenta de usuario y la dirección de correo electrónico de Office 365.  
  
###  <a name="BKMK_UnassignOnlineAccount"></a> Restringir el acceso a una cuenta en línea  
 ¿Qué hacer si un usuario deja la organización o desea restringir el acceso de usuario s a los servicios de Office 365? Si usted administrar las cuentas de usuarios en línea junto con sus cuentas de usuario en Windows Server Essentials, tiene tres opciones:  
  
-   **¿Anular la asignación de la cuenta en línea** ? Si desea impedir que un usuario usa Office 365 sin impedir el acceso a los recursos del servidor, debe quitar la asignación de la cuenta en línea. Se publicará la licencia de Office 365, y el usuario no podrá iniciar sesión en Office 365. Sin embargo, el servidor mantiene la asignación entre el nombre de cuenta de usuario y la dirección de correo electrónico de Office 365. Para obtener instrucciones, consulte [desasignar una cuenta en línea de una cuenta de usuario](#BKMK_ToUnassignAnOnlineAccount).  
  
-   **¿Desactivar la cuenta de usuario** ? Si desactiva una cuenta de usuario porque un empleado deja la empresa temporal o permanentemente, también se desactiva la cuenta de usuario s en línea. No se puede usar la cuenta en línea, pero los datos del usuario, incluido el correo electrónico, se conservan en Microsoft Online Services. Para obtener instrucciones, consulte [desactivar una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6) en [administrar cuentas de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
-   **¿Quitar la cuenta de usuario** ? Si quita una cuenta de usuario, la cuenta en línea se quita también de Microsoft Online Services. Para obtener instrucciones, consulte [quitar una cuenta de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove) en [administrar cuentas de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
    > [!WARNING]
    >  Tenga en cuenta que, cuando se elimina una cuenta en línea, los datos del usuario dependen de las directivas de retención de Microsoft Online Services. Si necesita conservar los datos de usuario de persona s después de un empleado deja la empresa, desactive la cuenta de usuario en lugar de quitarla.  
  
####  <a name="BKMK_ToUnassignAnOnlineAccount"></a> Para desasignar una cuenta en línea de una cuenta de usuario  
  
1.  En el panel, haga clic en **Usuarios**.  
  
2.  Haga clic en la cuenta de usuario en la lista y, a continuación, haga clic en **anular la asignación de una cuenta de Microsoft online** (en Windows Server Essentials, haga clic en **anular la asignación de una cuenta de Office 365**).  
  
3.  En el mensaje de confirmación, haga clic en **Sí**.  
  
##  <a name="BKMK_SECTION_ManageEmailAddresses"></a> Administrar direcciones de correo electrónico de Exchange Online  
 Al agregar direcciones de correo electrónico a la cuenta de usuario s en línea en Windows Server Essentials, puede permitir que el usuario reciba correo electrónico en varias direcciones de correo electrónico en Exchange Online.  
  
###  <a name="BKMK_PROC_AddEmailAliases"></a> Para agregar direcciones de correo electrónico adicionales a un usuario s cuenta de Microsoft online  
  
1.  En el panel de Windows Server Essentials, haga clic en **usuarios**.  
  
2.  Haga clic con el botón secundario en la cuenta de usuario en la lista y, a continuación, haga clic en **Ver las propiedades de la cuenta**.  
  
3.  En el **en línea de Microsoft** ficha de propiedades de la cuenta (o el **Office 365** ficha en Windows Server Essentials), haga clic en **agregar**.  
  
4.  Escriba el nuevo alias de correo electrónico y, a continuación, seleccione el dominio de correo electrónico.  
  
5.  Haga clic en **Aceptar** dos veces.  
  
##  <a name="BKMK_SECTION_ManageDistributionGroups"></a> Administrar grupos de distribución para Exchange Online (solo Windows Server Essentials)  
 Después de integrar su servidor Windows Server Essentials con Office 365, puede crear y administrar grupos de distribución de Exchange Online desde el panel de Windows Server Essentials. Ll puede hacer esto en el **grupos de distribución** ficha que se agrega a la **usuarios** página. Esta pestaña solo se ve si tiene una suscripción a Exchange Online. Esta característica no está disponible en Windows Server Essentials.  
  
 Use los siguientes procedimientos para:  
  
-   [Agregar un grupo de distribución](#BKMK_PROCEDURE_AddDistGroup)  
  
-   [Cambiar a los miembros de un grupo de distribución](#BKMK_ChangeGroupMembers)  
  
-   [Cambiar una pertenencia a grupos de distribución de usuario s](#BKMK_EditUserMemberships)  
  
-   [Quitar un grupo de distribución](#BKMK_RemoveDistributionGroup)  
  
###  <a name="BKMK_PROCEDURE_AddDistGroup"></a> Para agregar un grupo de distribución  
  
1.  En el panel de Windows Server Essentials, haga clic en **usuarios**y, a continuación, haga clic en el **grupos de distribución** ficha.  
  
2.  En **Tareas de los grupos de distribución**, haga clic en **Agregar un grupo de distribución**.  
  
     Se abrirá el asistente para agregar un nuevo grupo de distribución.  
  
3.  En la página **Agregar un nuevo grupo de distribución** , escriba la información siguiente y, a continuación, haga clic en **Siguiente**:  
  
    -   Escriba un nombre de grupo, una descripción opcional y un alias de correo electrónico para el nuevo grupo de distribución.  
  
    -   De forma predeterminada, el grupo de distribución puede recibir correo electrónico de usuarios fuera de su organización. Si no desea permitir esto, desactive la opción.  
  
4.  En la página **Agregar miembros de grupo**, use el botón **Agregar** para agregar cuentas de usuario activas que tengan una cuenta en línea asignada a ellos, y a otros grupos de distribución, al nuevo grupo de distribución. Haga clic en **Siguiente**.  
  
     El nuevo grupo de distribución se crea en Exchange Online.  
  
###  <a name="BKMK_ChangeGroupMembers"></a> Para cambiar a los miembros de un grupo de distribución  
  
1.  En el panel, haga clic en **Configuración**y, a continuación, en la pestaña **Grupos de distribución** .  
  
2.  Haga clic con el botón derecho en el grupo de distribución de la lista y, a continuación, haga clic en **Cambiar pertenencia a grupos**.  
  
3.  Use los botones **Agregar** y **Quitar** para agregar o quitar cuentas en línea activas desde el grupo de distribución. Luego haga clic en **Siguiente** para actualizar la pertenencia a grupos de distribución de Exchange Online.  
  
###  <a name="BKMK_EditUserMemberships"></a> Para cambiar la pertenencia a un grupo de distribución de usuario s  
  
1.  En el panel, haga clic en **Usuarios**.  
  
2.  Haga clic con el botón secundario en la cuenta de usuario en la lista y, a continuación, haga clic en **Ver las propiedades de la cuenta**.  
  
3.  En las propiedades de cuenta de usuario, haga clic en el **Grupos de distribución** y, a continuación, en **Editar**.  
  
4.  En el cuadro **Editar pertenencia a grupos** , use los botones **Agregar** y **Quitar** para agregar o quitar grupos de distribución de la cuenta de usuario y, a continuación, haga clic en **Cerrar**.  
  
5.  Haga clic en **Aceptar** para guardar las propiedades actualizadas de la cuenta de usuario.  
  
###  <a name="BKMK_RemoveDistributionGroup"></a> Para quitar un grupo de distribución  
  
1.  En el panel, haga clic en **Configuración**y, a continuación, en la pestaña **Grupos de distribución** .  
  
2.  Haga clic con el botón secundario en el grupo de distribución de la lista y, a continuación, en **Quitar el grupo**.  
  
3.  En el mensaje de confirmación, haga clic en **Eliminar grupo**.  
  
     Se quitará el grupo de distribución de Exchange Online.  
  
## <a name="see-also"></a>Vea también  
  
-   [Administrar cuentas de usuario](Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  
-   [Administración de Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Administrar Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
