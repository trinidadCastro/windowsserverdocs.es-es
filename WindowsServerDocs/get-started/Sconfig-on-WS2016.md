---
title: Configurar una instalación de Server Core de Windows Server con Sconfig.cmd
description: Explica cómo usar Sconfig.cmd
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 10/17/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e6cac074-c6fc-46dd-9664-fa0342c0a5e8
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: high
ms.openlocfilehash: 2473b4ffae79c29ec7505616c139c03b21a4427b
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "1284356"
---
# <a name="configure-a-server-core-installation-of-windows-server-2016-or-windows-server-version-1709-with-sconfigcmd"></a>Configurar una instalación de Server Core de Windows Server 2016 o Windows Server, versión 1709, con Sconfig.cmd
> Se aplica a: Windows Server (canal semianual) y Windows Server 2016

En Windows Server2016 y Windows Server, versión 1709 puedes usar la herramienta de configuración de servidores (Sconfig.cmd)para configurar y administrar varios aspectos comunes de las instalaciones de Server Core. Para usar la herramienta, debes ser miembro del grupo de administradores.  
  
Puedes usar Sconfig.cmd en las instalaciones de Server Core y de Servidor con Experiencia de escritorio (solo Windows Server 2016). 
  
## <a name="start-the-server-configuration-tool"></a>Iniciar la herramienta de configuración de servidores  
  
1.  Cambia a la unidad del sistema.  
  
2.  Escribe `Sconfig.cmd` y presiona ENTRAR. Se abre la interfaz de la herramienta de configuración de servidores:  
  
 <img src="mainsconfigpage.png" style='float:left; padding:.5em;' alt="Screenshot of Sconfig.cmd user interface">  
Captura de pantalla de la interfaz de usuario de Sconfig.cmd  
  
##  <a name="BKMK_Domainworkgroup"></a> Configuración del dominio o grupo de trabajo  
 La configuración actual del dominio o el grupo de trabajo se muestra en la pantalla predeterminada de la herramienta de configuración de servidores. Puedes unirte a un dominio o a un grupo de trabajo si accedes a la página de configuración **Dominio/grupo de trabajo** en el menú principal y sigues las instrucciones de las páginas siguientes, donde deberás proporcionar toda la información necesaria.  
  
 Si no se ha agregado ningún usuario del dominio al grupo de administradores locales, no podrás hacer cambios en el sistema, como modificar el nombre del equipo, mediante el empleo del usuario del dominio. Para agregar un usuario del dominio al grupo de administradores locales, reinicia el equipo. A continuación, inicia sesión en el equipo como el administrador local y sigue los pasos de la sección [Configuración del administrador local](assetId:///3c2f8ca4-6adc-4ebd-8daf-eb0de16c2c7d#BKMK_Localadministratorsettings) que aparece más adelante en este documento.  
  
> [!NOTE]
>  Es necesario reiniciar el servidor para aplicar los cambios de la pertenencia al dominio o al grupo de trabajo. Sin embargo, puedes realizar más cambios y reiniciar el servidor después de efectuarlos todos, para evitar tener que reiniciar el servidor varias veces. De manera predeterminada, las máquinas virtuales se guardan automáticamente antes de reiniciar el servidor Hyper-V.  
  
## <a name="computer-name-settings"></a>Configuración del nombre del equipo  
 El nombre actual del equipo se muestra en la pantalla predeterminada de la herramienta de configuración de servidores. Para cambiar el nombre del equipo, accede a la página de configuración "Nombre de equipo" en el menú principal y sigue las instrucciones.  
  
> [!NOTE]
>  Es necesario reiniciar el servidor para aplicar los cambios de la pertenencia al dominio o al grupo de trabajo. Sin embargo, puedes realizar más cambios y reiniciar el servidor después de efectuarlos todos, para evitar tener que reiniciar el servidor varias veces. De manera predeterminada, las máquinas virtuales se guardan automáticamente antes de reiniciar el servidor Hyper-V.  
  
##  <a name="BKMK_Localadministratorsettings"></a> Configuración del administrador local  
 Para agregar otros usuarios al grupo de administradores locales, usa la opción **Agregar administrador local** en el menú principal. En un equipo unido a un dominio, escribe el usuario con el siguiente formato: dominio\nombre_usuario. En un equipo que no esté unido a ningún dominio (equipo de grupos de trabajo), escribe solamente el nombre de usuario. Los cambios se hacen efectivos de inmediato.  
  
## <a name="network-settings"></a>Configuración de red  
 Puedes configurar que la dirección IP la asigne automáticamente un servidor DHCP o puedes asignar una dirección IP estática manualmente. Esta opción te permite configurar también las opciones del servidor DNS.  
  
> [!NOTE]
>  Ahora, estas opciones y muchas otras están disponibles mediante los cmdlets de Windows PowerShell para redes. Para obtener más información, consulta [Network Adapter Cmdlets](https://technet.microsoft.com/library/jj134956.aspx) (Cmdlets de adaptadores de red) en la biblioteca de Windows Server.  
  
## <a name="windows-update-settings"></a>Configuración de Windows Update  
 La configuración actual de Windows Update se muestra en la pantalla predeterminada de la herramienta de configuración de servidores. Puedes configurar el servidor para que use actualizaciones automáticas o manuales en la opción de configuración **Configuración de Windows Update** del menú principal.  
  
 Si se selecciona **Actualizaciones automáticas**, el sistema busca e instala actualizaciones todos los días a las 3:00. La configuración se hace efectiva de inmediato. Si se seleccionan las actualizaciones **Manuales**, sistema no buscará actualizaciones automáticamente.  
  
 Puedes descargar e instalar las actualizaciones aplicables en cualquier momento desde la opción **Descargar e instalar actualizaciones** del menú principal.

 La opción **Solo descarga** buscará actualizaciones, descargará las que estén disponibles y te notificará en el Centro de actividades que están listas para su instalación. Esta es la opción predeterminada.  
  
## <a name="remote-desktop-settings"></a>Configuración de Escritorio remoto  
 El estado actual de la configuración del Escritorio remoto se muestra en la pantalla predeterminada de la herramienta de configuración de servidores. Para configurar las siguientes opciones de configuración del Escritorio remoto, accede a la opción del menú principal **Escritorio remoto** y sigue las instrucciones en pantalla.  
  
-   Habilitar Escritorio remoto para clientes que ejecuten Escritorio remoto con autenticación a nivel de red  
  
-   Habilitar Escritorio remoto para clientes que ejecuten cualquier versión de Escritorio remoto  
  
-   Deshabilitar Escritorio remoto  
  
## <a name="date-and-time-settings"></a>Configuración de la fecha y la hora  
 Para acceder a la configuración de la fecha y la hora y cambiarla, accede a la opción del menú principal **Fecha y hora**. 

## <a name="telemetry-settings"></a>Configuración de la telemetría
Esta opción te permite configurar qué datos se envían a Microsoft.

## <a name="windows-activation-settings"></a>Configuración de la activación de Windows
Esta opción te permite configurar la activación de Windows.
  
## <a name="to-enable-remote-management"></a>Para habilitar la administración remota  
Puedes habilitar distintos escenarios de administración remota en la opción del menú principal **Configurar administración remota**:  
  
-   Administración remota de Microsoft Management Console  
-   Windows PowerShell  
-   Administrador de servidores  
  
## <a name="to-log-off-restart-or-shut-down-the-server"></a>Para cerrar sesión, reiniciar el servidor o apagarlo  
 Para cerrar sesión, reiniciar el servidor o apagarlo, accede al elemento de menú correspondiente en el menú principal. Estas opciones también están disponibles en el menú Seguridad de Windows, al que se puede acceder desde cualquier aplicación en cualquier momento presionando CTRL + ALT + SUPR.  
  
## <a name="to-exit-to-the-command-line"></a>Para salir a la línea de comandos  
 Selecciona la opción **Salir a la línea de comandos** y presiona ENTRAR para salir a la línea de comandos. Para volver a la herramienta de configuración de servidores, escribe **Sconfig.cmd** y presiona ENTRAR