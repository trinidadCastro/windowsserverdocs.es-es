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
ms.localizationpriority: medium
ms.openlocfilehash: 617005fd2d4e63c3cfc11bed28404656b2a81d6e
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749580"
---
# <a name="configure-a-server-core-installation-of-windows-server-2016-or-windows-server-version-1709-with-sconfigcmd"></a>Configurar una instalación de Server Core de Windows Server 2016 o Windows Server, versión 1709, con Sconfig.cmd

> Se aplica a: Windows Server (canal semianual) y Windows Server 2016

En Windows Server 2016 y Windows Server, versión 1709 puedes usar la herramienta de configuración de servidores (Sconfig.cmd)para configurar y administrar varios aspectos comunes de las instalaciones de Server Core. Para usar esta herramienta, debe ser miembro del grupo de administradores.

Puedes usar Sconfig.cmd en las instalaciones de Server Core y de Servidor con Experiencia de escritorio (solo Windows Server 2016).

## <a name="start-the-server-configuration-tool"></a>Iniciar la herramienta de configuración de servidores

1. Cambie a la unidad del sistema.

2. Escriba `Sconfig.cmd` y presione ENTRAR. Se abrirá la interfaz de la herramienta Configuración del servidor:

    ![Captura de pantalla de la interfaz de usuario de Sconfig.cmd](media/mainsconfigpage.png)

## <a name="domainworkgroup-settings"></a>Configuración del dominio o grupo de trabajo

La configuración actual del dominio o grupo de trabajo se muestra en la pantalla predeterminada de la herramienta Configuración del servidor. Puede unirse a un dominio o un grupo de trabajo mediante el acceso a la **dominio/grupo de trabajo** página de configuración en el menú principal y siga las instrucciones, proporcionando toda la información necesaria.

Si un usuario de dominio no se ha agregado al grupo de administradores Local, no podrá realizar cambios al sistema, como cambiar el nombre del equipo, mediante el usuario de dominio. Para agregar un usuario del dominio al grupo de administradores locales, permita que el equipo se reinicie. A continuación, inicie sesión en el equipo como administrador local y siga los pasos descritos en la [configuración del administrador Local](#local-administrator-settings) sección más adelante en este artículo.

> [!NOTE]
> Es necesario reiniciar el servidor para aplicar los cambios en la pertenencia al dominio o grupo de trabajo. Sin embargo, puede realizar cambios adicionales y reiniciar el servidor después de todos los cambios para evitar tener que reiniciar el servidor varias veces. De forma predeterminada, las máquinas virtuales en ejecución se guardan automáticamente antes de reiniciar el servidor Hyper-V.

## <a name="computer-name-settings"></a>Configuración del nombre del equipo

El nombre actual del equipo se muestra en la pantalla predeterminada de la herramienta Configuración del servidor. Puede cambiar el nombre del equipo mediante el acceso a la **nombre_equipo** página de configuración en el menú principal y siga las instrucciones.

> [!NOTE]
> Es necesario reiniciar el servidor para aplicar los cambios en la pertenencia al dominio o grupo de trabajo. Sin embargo, puede realizar cambios adicionales y reiniciar el servidor después de todos los cambios para evitar tener que reiniciar el servidor varias veces. De forma predeterminada, las máquinas virtuales en ejecución se guardan automáticamente antes de reiniciar el servidor Hyper-V.

## <a name="local-administrator-settings"></a>Configuración del administrador local

Para agregar usuarios adicionales al grupo de administradores local, use la opción **Agregar administrador local** en el menú principal. En la máquina unida al dominio, escriba el usuario con el siguiente formato: dominio\nombredeusuario. En una máquina no unida a un dominio (máquina de grupo de trabajo), escriba solo el nombre de usuario. Los cambios surtirán efecto inmediatamente.

## <a name="network-settings"></a>Configuración de red

Puede configurar la dirección IP para que un servidor DHCP la asigne automáticamente, o puede asignar manualmente una dirección IP estática. Esta opción le permite configurar las opciones del servidor DNS también para el servidor.

> [!NOTE]
> Estas opciones y muchas otras están ahora disponibles con los cmdlets de Windows PowerShell de funciones de red. Para obtener más información, consulta [Network Adapter Cmdlets](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps) (Cmdlets de adaptadores de red) en la biblioteca de Windows Server.

## <a name="windows-update-settings"></a>Configuración de Windows Update

La configuración actual de Windows Update se muestra en la pantalla predeterminada de la herramienta Configuración del servidor. Puede configurar el servidor para que usen actualizaciones automáticas o manuales en la opción de configuración **Configuración de Windows Update** en el menú principal.

Cuando la opción **Actualizaciones automáticas** está seleccionada, el sistema buscará e instalará actualizaciones todos los días a las 3:00 a.m. La configuración surte efecto inmediatamente. Cuando las actualizaciones **manuales** están seleccionadas, el sistema no buscará actualizaciones automáticamente.

En cualquier momento, puede descargar e instalar las actualizaciones aplicables desde la opción **Descargar e instalar las actualizaciones** en el menú principal.

La opción **Solo descarga** buscará actualizaciones, descargará las que estén disponibles y te notificará en el Centro de actividades que están listas para su instalación. Esta es la opción predeterminada.

## <a name="remote-desktop-settings"></a>Configuración del escritorio remoto

La configuración actual del estado del escritorio remoto se muestra en la pantalla predeterminada de la herramienta Configuración del servidor. Para configurar los siguientes valores de Escritorio remoto, obtenga acceso a la opción **Escritorio remoto** del menú principal y siga las instrucciones en pantalla.

- Habilitar Escritorio remoto para clientes que ejecuten Escritorio remoto con autenticación a nivel de red

- Habilitar Escritorio remoto para clientes que ejecuten cualquier versión de Escritorio remoto

- Deshabilitar Escritorio remoto

## <a name="date-and-time-settings"></a>Configuración de fecha y hora

Puede acceder y cambiar la fecha y configuración de tiempo mediante el acceso a la **fecha y hora** opción de menú principal.

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

Para cerrar sesión, reiniciar o apagar el servidor, obtenga acceso al elemento del menú correspondiente en el menú principal. Estas opciones también están disponibles en el **Windows Security** menú que se puede acceder desde cualquier aplicación en cualquier momento presionando CTRL + ALT + SUPR.  

## <a name="to-exit-to-the-command-line"></a>Para salir a la línea de comandos
  
Seleccione la opción **Salir a la línea de comandos** y presione ENTRAR para salir a la línea de comandos. Para volver a la herramienta de configuración del servidor, escriba **Sconfig.cmd**, y, a continuación, presione ENTRAR.
