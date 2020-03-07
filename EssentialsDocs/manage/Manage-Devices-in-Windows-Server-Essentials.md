---
title: Administrar dispositivos en Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5fe1088-ebe7-4799-a47d-075b0048dea1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 48eb7009215e484fb00e704c7b328340240321d2
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371243"
---
# <a name="manage-devices-in-windows-server-essentials"></a>Administrar dispositivos en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Las siguientes secciones abordan las características de administración de los dispositivos de un servidor y explican cómo configurar y usar dispositivos en la red:  
  
-   [Administrar dispositivos mediante el panel](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Asignar permisos de cuentas de usuario para iniciar sesión en equipos de red específicos](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Quitar un equipo del servidor](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Configuración de directiva de grupo para la redirección de carpetas y la seguridad](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Conexión a un equipo de red mediante una sesión de Escritorio remoto](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Ver las propiedades del equipo](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_8)  
  
##  <a name="BKMK_1"></a>Administrar dispositivos mediante el panel  
 Windows Server Essentials permite realizar tareas administrativas comunes mediante su panel. En la página **Dispositivos** del panel encontrará lo siguiente:  
  
-   Una lista de equipos de la red, que muestra:  
  
    -   El nombre del equipo  
  
    -   El estado del equipo, ya sea **En línea** o **Sin conexión**  
  
    -   La descripción del equipo  
  
    -   El estado de copia de seguridad del equipo  
  
    -   El estado de actualización del equipo  
  
    -   El estado de seguridad del equipo  
  
    -   El estado de las alertas del equipo  
  
    -   Información de directiva de grupo para el equipo  
  
-   Un panel de detalles con información adicional sobre un equipo seleccionado  
  
-   Un panel de tareas que incluye un conjunto de tareas administrativas, como ver las propiedades del equipo y las alertas, configurar una copia de seguridad del equipo y restaurar archivos y carpetas desde una copia de seguridad  
  
#### <a name="to-view-the-status-of-network-computers"></a>Para ver el estado de los equipos de la red  
  
1. Abra el panel de Windows Server Essentials.  
  
2. En la barra de navegación, haga clic en **Dispositivos**.  
  
3. Vea el estado de todos los equipos de la red en el panel de la lista.  
  
   En la tabla siguiente se describen las diversas tareas de copia de seguridad y del equipo que están disponibles en el panel del servidor. Algunas tareas son para complementos, por lo que solo están visibles cuando se selecciona un complemento específico en la lista.  
  
### <a name="computer-tasks-in-the-dashboard"></a>Tareas de equipo en el panel  
  
|Nombre de tarea|Descripción|  
|---------------|-----------------|  
|Ver las propiedades del equipo|Se muestra información general del equipo seleccionado y se le permite ver los detalles de las copias de seguridad del equipo.|  
|Configurar las copias de seguridad de este equipo|Se ejecuta el asistente para la configuración de copias de seguridad.|  
|Personalizar la copia de seguridad del equipo|Se abren las propiedades de copia de seguridad, desde las que puede realizar cambios en la configuración del equipo seleccionado.|  
|Iniciar una copia de seguridad del equipo|Se inicia una copia de seguridad para el equipo seleccionado.|  
|Detener la copia de seguridad del equipo|Se detiene la copia de seguridad del equipo seleccionado.|  
|Restaurar archivos o carpetas del equipo.|Se ejecuta el asistente de restauración de archivos y carpetas, que le permite restaurar archivos concretos, carpetas o unidades.|  
|Ver alertas del equipo|Se muestran alertas informativas y críticas, que le permiten tomar medidas correctivas cuando es posible.|  
|Escritorio remoto en el equipo|Se abre una conexión de Escritorio remoto en el equipo seleccionado.|  
|Quitar el equipo|Se ejecuta el asistente para quitar un equipo, que separa el equipo del panel de Windows Server Essentials.|  
|Personalizar la configuración del Historial de archivos y las copias de seguridad del equipo|Se abre la página de configuración de copia de seguridad, desde la que se pueden realizar cambios en la programación de copia de seguridad y en la configuración del Historial de archivos para los equipos cliente.|  
|¿Cómo conecto equipos al servidor?|Se abre un tema de ayuda que describen los pasos necesarios para realizar para unir un equipo a la red.|  
|Implementar la directiva de grupo|Se aplica la configuración de directiva a los equipos de Windows 8 y Windows 7 que están unidos al dominio.|  
  
##  <a name="BKMK_2"></a>Asignar permisos de cuentas de usuario para iniciar sesión en equipos de red específicos  
 Puede asignar permisos a las cuentas de usuario para que los usuarios inicien sesión en solo determinados equipos de la red al obtener acceso a la red de Windows Server Essentials desde una ubicación remota.  
  
#### <a name="to-change-the-computer-access-for-a-user-account"></a>Para cambiar el acceso al equipo de una cuenta de usuario  
  
1.  Abra el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haga clic en **Usuarios**.  
  
3.  En la lista de cuentas de usuario, seleccione la cuenta de usuario que quiera cambiar.  
  
4.  En el panel tareas de la **cuenta de usuario de <\>** , haga clic en **ver las propiedades de la cuenta**. Aparece la página **Propiedades** de la cuenta de usuario.  
  
5.  En la pestaña **Acceso al equipo**, seleccione el equipo al que este usuario puede acceder de forma remota y, a continuación, haga clic en **Aceptar**.  
  
##  <a name="BKMK_3"></a>Quitar un equipo del servidor  
 Al quitar un equipo de un servidor que ejecuta Windows Server Essentials utilizando el panel, el servidor deja de administrar el equipo. Por lo tanto, el servidor detendrá la creación de copias de seguridad del equipo o supervisará su estado después de quitarse de la red.  
  
> [!NOTE]
>  La eliminación de un equipo desde el servidor no desconecta el equipo de la red. El equipo puede acceder a recursos de la red de la misma manera que podía antes de conectarse al servidor. Para evitar que el equipo acceda a los recursos del servidor y desconectarlo de este, debe quitar el equipo del dominio. Además, la eliminación del equipo del servidor no desinstala automáticamente el software del Conector o el Launchpad del equipo que se está quitando. Debe quitar el software del Conector manualmente desde el equipo. Para obtener más información, consulte la sección desinstalar el software del conector en [Get Connected](../use/Get-Connected-in-Windows-Server-Essentials.md).  
  
#### <a name="to-remove-a-computer-from-the-network-by-using-the-dashboard"></a>Para quitar un equipo de la red mediante el panel  
  
1.  Abra el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haga clic en la pestaña **Dispositivos**.  
  
3.  En la lista de equipos, haga clic en el equipo que quiera quitar de la red y, a continuación, haga clic en **Quitar el equipo**.  
  
##  <a name="BKMK_5"></a>Configuración de directiva de grupo para la redirección de carpetas y la seguridad  
 Puede configurar la directiva de grupo e implementarla en los equipos de la red de Windows Server Essentials utilizando el panel de Windows Server Essentials. La directiva de grupo en Windows Server Essentials incluye la configuración de seguridad y redirección de carpetas que afecta a Windows Update, Windows Defender y al firewall de red.  
  
#### <a name="to-configure-group-policy-in-windows-server-essentials"></a>Para configurar la directiva de grupo en Windows Server Essentials  
  
1.  Abra el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haga clic en **DISPOSITIVOS**.  
  
3.  En Windows Server Essentials: en el panel **tareas de usuarios** globales, haga clic en **implementar Directiva de grupo**.  
  
     En Windows Server Essentials: en el panel **tareas de dispositivos** globales, haga clic en **implementar Directiva de grupo**.  
  
4.  Se abrirá el asistente para implementar directivas de grupo.  
  
5.  En la página **Habilitar la directiva de grupo de redirección de carpetas** del asistente, puede elegir las carpetas de usuario que quiera redirigir.  
  
6.  En la página **Habilitar la configuración de directivas de seguridad** del asistente, puede habilitar la configuración de directiva de grupo para **Windows Update**, **Windows Defender** y el **firewall de red**.  
  
7.  Haga clic en **Finalizar** para implementar la configuración de directiva de grupo.  
  
##  <a name="BKMK_7"></a>Conexión a un equipo de red mediante una sesión de Escritorio remoto  
 Para acceder de forma remota al equipo de red de Windows Server Essentials cuando esté fuera de la oficina, use el explorador Web para iniciar sesión en el sitio web de acceso Web remoto de su organización y, en la pestaña **equipos** , haga clic en el nombre del equipo.  
  
 La columna **Estado** indica si puede conectarse a un equipo en la red, y puede incluir los siguientes valores:  
  
-   **Disponible**  
  
     El equipo está activado y está disponible para una conexión remota. Aunque vea este estado, es posible que aún no pueda conectarse a este equipo si un firewall de terceros bloquea la conexión.  
  
-   **Sin conexión o en suspensión**  
  
     El equipo se apaga o está en modo de hibernación o de suspensión. Si un equipo está sin conexión o en modo de suspensión, el estado se actualiza en tiempo real para que sepa cuándo está disponible el equipo.  
  
-   **Sistema operativo no admitido**  
  
     El sistema operativo en el equipo no es compatible con el Escritorio remoto. Este estado puede tardar hasta 6 horas en actualizarse en el servidor si se produce algún cambio.  
  
-   **La conexión está deshabilitada**  
  
     La conexión del equipo está bloqueada por un firewall o el escritorio remoto está deshabilitado en el equipo o por la directiva de grupo. Este estado puede tardar hasta 6 horas en actualizarse en el servidor si se produce algún cambio.  
  
##  <a name="BKMK_8"></a>Ver las propiedades del equipo  
 La sección **Dispositivos** del panel de Windows Server Essentials muestra una lista de equipos de red. La lista también proporciona información adicional sobre cada equipo.  
  
#### <a name="to-view-a-list-of-computers"></a>Para ver una lista de equipos  
  
1.  Abra el panel de Windows Server Essentials.  
  
2.  En la barra de navegación principal, haga clic en **Dispositivos**.  
  
3.  El panel muestra una lista de equipos actual.  
  
#### <a name="to-view-or-change-properties-for-a-computer"></a>Para ver o cambiar las propiedades de un equipo  
  
1.  En la lista de equipos, seleccione la cuenta en la que quiera ver o cambiar las propiedades.  
  
2.  En el panel **tareas de < nombredeequipo\>** , haga clic en **ver las propiedades del equipo**. Aparece la página **Propiedades** para los equipos.  
  
3.  Haga clic en una pestaña para ver las propiedades de ese equipo.  
  
4.  Para guardar los cambios que realice en las propiedades del equipo, haga clic en **Aplicar**.  
  
## <a name="see-also"></a>Vea también  
  
-   [Administrar el acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Usar acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar cuentas de usuario mediante el panel](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
