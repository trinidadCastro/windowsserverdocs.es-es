---
title: Administrar dispositivos en Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
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
ms.openlocfilehash: 288d62a3fe4d9073ba2c0e3fdff385d8317f20d4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="manage-devices-in-windows-server-essentials"></a>Administrar dispositivos en Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Las siguientes secciones describen las características de administración de dispositivo de un servidor y explican cómo configurar y usar dispositivos de la red:  
  
-   [Administrar dispositivos mediante el panel](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Asignar permisos para iniciar sesión en los equipos de red específicos de cuentas de usuario](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Quitar un equipo desde el servidor](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Establecer la configuración de directiva de grupo de redirección de carpetas y seguridad](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Conectarse a un equipo de red mediante una sesión de escritorio remoto](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Ver propiedades de equipo](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_8)  
  
##  <a name="BKMK_1"></a>Administrar dispositivos mediante el panel  
 Windows Server Essentials hace posible realizar tareas de administración comunes mediante el panel de Windows Server Essentials. La **dispositivos** página del panel proporciona las siguientes acciones:  
  
-   Una lista de equipos de la red, que muestra:  
  
    -   El nombre del equipo  
  
    -   El estado del equipo, ya sea **Online** o **sin conexión**  
  
    -   La descripción del equipo  
  
    -   El estado de copia de seguridad del equipo  
  
    -   El estado de actualización del equipo  
  
    -   El estado de seguridad del equipo  
  
    -   El estado de alertas del equipo  
  
    -   Información de la directiva de grupo para el equipo  
  
-   Un panel de detalles con información adicional sobre un equipo seleccionado  
  
-   Un panel de tareas que incluye un conjunto de tareas administrativas del dispositivo, como ver alertas y propiedades del equipo, configuración de copia de seguridad del equipo y restaurar archivos y carpetas desde una copia de seguridad  
  
#### <a name="to-view-the-status-of-network-computers"></a>Para ver el estado de los equipos de la red  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **dispositivos**.  
  
3.  Ver el estado de todos los equipos de la red en el panel de lista.  
  
 La siguiente tabla describe las distintas equipo y las tareas de copia de seguridad que están disponibles en el escritorio de Windows Server Essentials. Algunas de las tareas son específicos del equipo y solo están visibles cuando se selecciona un equipo en la lista.  
  
### <a name="computer-tasks-in-the-dashboard"></a>Tareas del equipo en el panel  
  
|Nombre de la tarea|Descripción|  
|---------------|-----------------|  
|Ver las propiedades del equipo|Muestra información general de un equipo seleccionado y te permite ver los detalles de las copias de seguridad del equipo.|  
|Configurar copia de seguridad para este equipo|Ejecuta el Asistente para la copia de seguridad configurar.|  
|Personalizar copias de seguridad para el equipo|Abre las propiedades de copia de seguridad, desde el que puedes realizar cambios en la configuración de copia de seguridad para el equipo seleccionado.|  
|Iniciar una copia de seguridad para el equipo|Inicia una copia de seguridad para un equipo seleccionado.|  
|Detener la copia de seguridad para el equipo|Detiene la copia de seguridad para un equipo seleccionado.|  
|Restaurar archivos o carpetas para el equipo|Se ejecuta para restaurar archivos y carpetas asistente, que le permite restaurar unidades, carpetas o archivos específicos.|  
|Alertas de vista para el equipo|Muestra críticas y de otras alertas informativas y permite tomar medidas correctivas cuando sea posible.|  
|Escritorio remoto en el equipo|Abre la conexión a Escritorio remoto en el equipo seleccionado.|  
|Quitar el equipo|Ejecuta la quitar un asistente de equipo, que se separa el equipo desde el escritorio de Windows Server Essentials.|  
|Personalizar la configuración de historial de archivos y la copia de seguridad del equipo|Abre la página de configuración de copia de seguridad, desde el que puedes realizar cambios en el plan de copia de seguridad y la configuración del historial de archivos para el cliente de equipos.|  
|¿Cómo me conecto equipos al servidor?|Abre un tema de ayuda que se describen los pasos para llevar a cabo para unir un equipo a la red.|  
|Implementar Directiva de grupo|Aplica la configuración de directiva a equipos de Windows 8 y Windows 7 que están unidos al dominio.|  
  
##  <a name="BKMK_2"></a>Asignar permisos para iniciar sesión en los equipos de red específicos de cuentas de usuario  
 Puedes asignar permisos a cuentas de usuario para que los usuarios pueden iniciar sesión a los equipos de red solo específicos al obtener acceso a la red de Windows Server Essentials desde una ubicación remota.  
  
#### <a name="to-change-the-computer-access-for-a-user-account"></a>Para cambiar el acceso a los equipos para una cuenta de usuario  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **usuarios**.  
  
3.  En la lista de cuentas de usuario, selecciona la cuenta de usuario que quieres cambiar.  
  
4.  En la **< usuario Account\ > tareas** panel, haz clic en **ver las propiedades de la cuenta**. La **propiedades** para aparece de la cuenta de usuario de la página.  
  
5.  En la **acceso al equipo**, seleccione el equipo que este usuario puede acceder de forma remota y, a continuación, haz clic en **Aceptar**.  
  
##  <a name="BKMK_3"></a>Quitar un equipo desde el servidor  
 Cuando quites un equipo de un servidor que ejecuta Windows Server Essentials mediante el panel, ya no está administrado por el servidor. Como resultado, el servidor de detener la creación de copias de seguridad del equipo o supervisar su estado después de su eliminación de la red.  
  
> [!NOTE]
>  Eliminación de un equipo desde el servidor no desconecta el equipo desde la red. El equipo aún puede acceder a recursos de la red de la misma manera que podría antes de que se está conectado al servidor. Para impedir que el equipo de acceso a recursos del servidor y se desconecta del servidor, debes quitar el equipo del dominio. Además, eliminación del equipo desde el servidor no automáticamente la desinstala el software del conector o del Launchpad desde el equipo que se ha quitado. Debes quitar manualmente el software del conector desde el equipo. Para obtener más información, consulta la sección desinstalar el software del conector en [conectarse](../use/Get-Connected-in-Windows-Server-Essentials.md).  
  
#### <a name="to-remove-a-computer-from-the-network-by-using-the-dashboard"></a>Para quitar un equipo de la red mediante el panel  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en el **dispositivos** pestaña.  
  
3.  En la lista de equipos, haz clic en el equipo que desea quitar de la red y, a continuación, haz clic en **quitar el equipo**.  
  
##  <a name="BKMK_5"></a>Establecer la configuración de directiva de grupo de redirección de carpetas y seguridad  
 Puedes configurar la directiva de grupo e implementarla en equipos de la red de Windows Server Essentials mediante el panel de Windows Server Essentials. Directiva de grupo en Windows Server Essentials incluye la configuración de seguridad y la redirección de carpetas que afectan a Windows Update, Windows Defender y firewall de red.  
  
#### <a name="to-configure-group-policy-in-windows-server-essentials"></a>Para configurar la directiva de grupo en Windows Server Essentials  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **dispositivos**.  
  
3.  Para Windows Server Essentials: En la variable global **tareas de los usuarios** panel, haz clic en **implementar Directiva de grupo**.  
  
     Para Windows Server Essentials: En la variable global **dispositivos tareas** panel, haz clic en **implementar Directiva de grupo**.  
  
4.  Abre el Asistente para la directiva de grupo de implementar.  
  
5.  En la **habilitar la directiva de grupo de redirección de carpetas** página del asistente, puedes elegir las carpetas de usuario que desee redirigir.  
  
6.  En la **habilitar la configuración de directiva de seguridad** página del asistente, puedes habilitar la configuración de directiva de grupo para **Windows Update**, **Windows Defender**y el **Firewall de red**.  
  
7.  Haz clic en **finalizar** para implementar la configuración de directiva de grupo.  
  
##  <a name="BKMK_7"></a>Conectarse a un equipo de red mediante una sesión de escritorio remoto  
 Para tener acceso remoto a su equipo de red de Windows Server Essentials cuando estás lejos de la oficina, usar el explorador Web para iniciar sesión en tu sitio Web de organización s acceso Web remoto y, en la **equipos** pestaña, haz clic en el nombre del equipo.  
  
 La **estado** columna muestra si puedes conectarte a un equipo en la red y puede contener los siguientes valores:  
  
-   **Disponible**  
  
     El equipo está activado y está disponible para una conexión remota. Incluso si ves este estado, aún no podrás para conectarse a este equipo si un firewall de terceros bloquea la conexión.  
  
-   **Sin conexión o durmiendo**  
  
     El equipo está apagado o está en modo de suspensión o hibernación. Si un equipo está sin conexión o en modo de suspensión, el estado se actualiza en tiempo real para que sepa cuando el equipo esté disponible.  
  
-   **Sistema operativo no compatible**  
  
     El sistema operativo en el equipo no admite escritorio remoto. Puede tardar hasta 6 horas para este estado se actualice en el servidor si se produce un cambio.  
  
-   **Conexión está deshabilitada**  
  
     La conexión del equipo está bloqueada por un firewall o el escritorio remoto está deshabilitado en el equipo o por la directiva de grupo. Puede tardar hasta 6 horas para este estado se actualice en el servidor si se produce un cambio.  
  
##  <a name="BKMK_8"></a>Ver propiedades de equipo  
 La **dispositivos** sección del panel de Essentials de servidor de Windows muestra una lista de equipos de la red. La lista también proporciona información adicional acerca de cada equipo.  
  
#### <a name="to-view-a-list-of-computers"></a>Para ver una lista de equipos  
  
1.  Abre el panel de Windows Server Essentials.  
  
2.  En la barra de navegación, haz clic en **dispositivos**.  
  
3.  El panel muestra una lista actual de equipos.  
  
#### <a name="to-view-or-change-properties-for-a-computer"></a>Para ver o cambiar las propiedades de un equipo  
  
1.  En la lista de equipos, selecciona la cuenta que quieres ver o cambiar las propiedades.  
  
2.  En la **< Computername\ > tareas** panel, haz clic en **ver las propiedades**. La **propiedades** aparecerá la página para los equipos.  
  
3.  Haz clic en una pestaña para mostrar las propiedades de ese equipo.  
  
4.  Para guardar los cambios que realices en las propiedades del equipo, haz clic en **aplicar**.  
  
## <a name="see-also"></a>Consulta también  
  
-   [Administrar el acceso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Usar el acceso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Administrar cuentas de usuario mediante el panel](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
-   [Administrar Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usar Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
