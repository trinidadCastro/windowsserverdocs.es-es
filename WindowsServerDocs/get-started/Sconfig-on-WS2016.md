---
title: Configuración de una instalación básica de Windows Server con Sconfig.cmd
description: Explica cómo usar Sconfig.cmd
ms.prod: windows-server
ms.date: 10/17/2017
ms.technology: server-general
ms.topic: article
ms.assetid: e6cac074-c6fc-46dd-9664-fa0342c0a5e8
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: e6c218b08cc39edd9b3d93ae78b0b5c7aa293858
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826678"
---
# <a name="configure-a-server-core-installation-of-windows-server-2016-or-windows-server-version-1709-with-sconfigcmd"></a>Configuración de una instalación básica de Windows Server 2016 o Windows Server, versión 1709, con Sconfig.cmd

> Se aplica a: Windows Server (canal semianual) y Windows Server 2016

En Windows Server 2016 y Windows Server, versión 1709 puedes usar la herramienta de configuración de servidores (Sconfig.cmd) para configurar y administrar varios aspectos comunes de las instalaciones básicas. Para usar esta herramienta, debe ser miembro del grupo de administradores.

Puedes usar Sconfig.cmd en las instalaciones básicas y de Servidor con Experiencia de escritorio (solo Windows Server 2016).

## <a name="start-the-server-configuration-tool"></a>Inicio de la herramienta de configuración de servidores

1. Cambie a la unidad del sistema.

2. Escriba `Sconfig.cmd` y presione ENTRAR. Se abrirá la interfaz de la herramienta Configuración del servidor:

    ![Captura de pantalla de la interfaz de usuario de Sconfig.cmd](media/mainsconfigpage.png)

## <a name="domainworkgroup-settings"></a>Configuración del dominio o grupo de trabajo

La configuración actual del dominio o grupo de trabajo se muestra en la pantalla predeterminada de la herramienta Configuración del servidor. Para unirse a un dominio o a un grupo de trabajo, obtén acceso a la página de configuración **Dominio o grupo de trabajo** en el menú principal y sigue las instrucciones, proporcionando toda la información requerida.

Si un usuario del dominio no se agregó al grupo de administradores locales, no podrás realizar cambios en el sistema, como cambiar el nombre del equipo, usando ese usuario del dominio. Para agregar un usuario del dominio al grupo de administradores locales, permita que el equipo se reinicie. A continuación, inicia sesión en el equipo como administrador local y sigue los pasos de la sección [Configuración del administrador local](#local-administrator-settings) de este documento.

> [!NOTE]
> Se te pedirá que reinicies el servidor para aplicar cualquier cambio en la pertenencia al dominio o grupo de trabajo. Sin embargo, puede realizar cambios adicionales y reiniciar el servidor después de todos los cambios para evitar tener que reiniciar el servidor varias veces. De forma predeterminada, las máquinas virtuales en ejecución se guardan automáticamente antes de reiniciar el servidor Hyper-V.

## <a name="computer-name-settings"></a>Configuración del nombre del equipo

El nombre actual del equipo se muestra en la pantalla predeterminada de la herramienta Configuración del servidor. Para cambiar el nombre del equipo, obtén acceso a la página de configuración **Nombre de equipo** desde el menú principal y sigue las instrucciones.

> [!NOTE]
> Se te pedirá que reinicies el servidor para aplicar cualquier cambio en la pertenencia al dominio o grupo de trabajo. Sin embargo, puede realizar cambios adicionales y reiniciar el servidor después de todos los cambios para evitar tener que reiniciar el servidor varias veces. De forma predeterminada, las máquinas virtuales en ejecución se guardan automáticamente antes de reiniciar el servidor Hyper-V.

## <a name="local-administrator-settings"></a>Configuración del administrador local

Para agregar usuarios adicionales al grupo de administradores local, use la opción **Agregar administrador local** en el menú principal. En la máquina unida al dominio, escriba el usuario con el siguiente formato: dominio\nombredeusuario. En una máquina no unida a un dominio (máquina de grupo de trabajo), escriba solo el nombre de usuario. Los cambios surtirán efecto inmediatamente.

## <a name="network-settings"></a>Configuración de red

Puede configurar la dirección IP para que un servidor DHCP la asigne automáticamente, o puede asignar manualmente una dirección IP estática. Esta opción le permite configurar las opciones del servidor DNS también para el servidor.

> [!NOTE]
> Estas opciones y muchas otras están ahora disponibles con los cmdlets de Windows PowerShell de funciones de red. Para obtener más información, consulte [cmdlets de adaptador de red](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps) en la biblioteca de Windows Server.

## <a name="windows-update-settings"></a>Configuración de Windows Update

La configuración actual de Windows Update se muestra en la pantalla predeterminada de la herramienta Configuración del servidor. Puede configurar el servidor para que usen actualizaciones automáticas o manuales en la opción de configuración **Configuración de Windows Update** en el menú principal.

Cuando la opción **Actualizaciones automáticas** está seleccionada, el sistema buscará e instalará actualizaciones todos los días a las 3:00 a.m. La configuración surte efecto inmediatamente. Cuando las actualizaciones **manuales** están seleccionadas, el sistema no buscará actualizaciones automáticamente.

En cualquier momento, puede descargar e instalar las actualizaciones aplicables desde la opción **Descargar e instalar las actualizaciones** en el menú principal.

La opción **Solo descarga** buscará actualizaciones, descargará las que estén disponibles y te notificará en el Centro de actividades que están listas para su instalación. Esta es la opción predeterminada.

## <a name="remote-desktop-settings"></a>Configuración del escritorio remoto

La configuración actual del estado del escritorio remoto se muestra en la pantalla predeterminada de la herramienta Configuración del servidor. Para configurar los siguientes valores de Escritorio remoto, obtenga acceso a la opción **Escritorio remoto** del menú principal y siga las instrucciones en pantalla.

- Habilita Escritorio remoto para clientes que ejecuten Escritorio remoto con Autenticación a nivel de red.

- Habilita Escritorio remoto para clientes que ejecuten cualquier versión de Escritorio remoto.

- Deshabilita Remoto.

## <a name="date-and-time-settings"></a>Configuración de fecha y hora

Para obtener acceso y cambiar la configuración de fecha y hora, usa la opción **Fecha y hora** del menú principal.

## <a name="telemetry-settings"></a>Configuración de la telemetría

Esta opción te permite configurar qué datos se envían a Microsoft.

## <a name="windows-activation-settings"></a>Configuración de la activación de Windows

Esta opción te permite configurar la activación de Windows.

## <a name="to-enable-remote-management"></a>Para habilitar la administración remota

Puede habilitar varios escenarios de administración remota desde la opción **Configurar administración remota** en el menú principal:

- Administración remota de Microsoft Management Console

- Windows PowerShell

- Administrador de servidores  

## <a name="to-log-off-restart-or-shut-down-the-server"></a>Para cerrar sesión, reiniciar o apagar el servidor

Para cerrar sesión, reiniciar o apagar el servidor, obtenga acceso al elemento del menú correspondiente en el menú principal. Estas opciones también están disponibles en el menú **Seguridad de Windows**, al que se puede obtener acceso desde cualquier aplicación en cualquier momento si presionas Ctrl + Alt + Supr.  

## <a name="to-exit-to-the-command-line"></a>Para salir a la línea de comandos
  
Seleccione la opción **Salir a la línea de comandos** y presione ENTRAR para salir a la línea de comandos. Para volver a la herramienta Configuración del servidor, escribe **Sconfig.cmd** y, a continuación, presiona ENTRAR.
