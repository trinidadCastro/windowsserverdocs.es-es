---
title: Instalar el protocolo de puente del centro de datos (DCB) en Windows Server o en el cliente
description: En este tema se proporcionan instrucciones sobre cómo instalar el protocolo de puente del centro de datos en Windows Server o en el cliente de Windows.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: edca8269178d9e1de9f8d57abac04400da0ac5c1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312802"
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>Instalar el protocolo de puente del centro de datos \(DCB\) en Windows Server 2016 o Windows 10

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información sobre cómo instalar DCB en Windows Server 2016 o Windows 10.

## <a name="prerequisites-for-using-dcb"></a>Requisitos previos para usar DCB

A continuación se indican los requisitos previos para configurar y administrar DCB.

### <a name="install-a-compatible-operating-system"></a>Instalar un sistema operativo compatible

Puede usar los comandos DCB de esta guía en los sistemas operativos siguientes.

- Windows Server (Canal semianual)
- Windows Server 2016
- Windows 10 \(todas las versiones\)

Los siguientes sistemas operativos incluyen versiones anteriores de DCB que no son compatibles con los comandos que se usan en la documentación de DCB para Windows Server 2016 y Windows 10.

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>Requisitos de hardware

A continuación se muestra una lista de los requisitos de hardware para DCB.

- El adaptador de red Ethernet compatible con\-de DCB\(s\) debe estar instalado en los equipos que proporcionan el DCB de Windows Server 2016.
- Los conmutadores de hardware compatibles con\-DCB deben estar implementados en la red.


## <a name="install-dcb-in-windows-server-2016"></a>Instalar DCB en Windows Server 2016

Puede usar las siguientes secciones para instalar DCB en un equipo que ejecute Windows Server 2016.

**Credenciales administrativas**

Para llevar a cabo estos procedimientos, debe ser miembro de **los administradores**.

### <a name="install-dcb-using-windows-powershell"></a>Instalar DCB con Windows PowerShell

Puede usar el siguiente procedimiento para instalar DCB mediante Windows PowerShell.

1. En un equipo que ejecute Windows Server 2016, haga clic en **Inicio**y, a continuación, haga clic con el botón secundario en el icono de Windows PowerShell. Aparece un menú. En el menú, haga clic en **más**y, a continuación, haga clic en **Ejecutar como administrador**. Si se le solicita, escriba las credenciales de una cuenta que tenga privilegios de administrador en el equipo. Windows PowerShell se abre con privilegios de administrador.
2. Escriba el siguiente comando y presione ENTRAR.

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>Instalar DCB con Administrador del servidor

Puede usar el siguiente procedimiento para instalar DCB mediante Administrador del servidor.

>[!NOTE]
>Después de realizar el primer paso de este procedimiento, no se muestra la página **antes de comenzar** del Asistente para agregar roles y características si ya ha seleccionado **omitir esta página de forma predeterminada** cuando se ejecutó el Asistente para agregar roles y características. Si no se muestra la página **antes de comenzar** , vaya del paso 1 al paso 3.

1. En DC1, en Administrador del servidor, haga clic en **administrar**y, a continuación, haga clic en **Agregar roles y características**. Se abre el Asistente para agregar roles y características.
2. En **Antes de comenzar**, haga clic en **Siguiente**.
3. En **Seleccionar tipo de instalación**, asegúrese de que la opción **Instalación basada en características o en roles** está seleccionada y, a continuación, haga clic en **Siguiente**.
4. En **Seleccionar servidor de destino**, asegúrese de que la opción **Seleccionar un servidor del grupo de servidores** está seleccionada. En **Grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haga clic en **Siguiente**.
5. En **Seleccionar roles de servidor**, haz clic en **Siguiente**.
6. En **seleccionar características**, en **características**, haga clic en protocolo de **puente del centro de datos**. Se abre un cuadro de diálogo en el que se le pregunta si desea agregar las características requeridas de DCB. Haga clic en **Agregar características**.
7. En **seleccionar características**, haga clic en **siguiente**. 
8. 7.In **confirmar selecciones de instalación**, haga clic en **instalar**. La página progreso de la **instalación** muestra el estado durante el proceso de instalación. Cuando aparezca el mensaje que indica que la instalación se realizó correctamente, haga clic en **cerrar**.

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>Configurar el depurador de kernel para permitir QoS \(\) opcional

 De forma predeterminada, los depuradores de kernel bloquean NetQos. Independientemente del método que use para instalar DCB, si tiene un depurador de kernel instalado en el equipo, debe configurar el depurador para permitir que se habilite y configure QoS mediante la ejecución del siguiente comando.

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>Instalar DCB en Windows 10

Puede realizar el siguiente procedimiento en un equipo con Windows 10.

Para llevar a cabo este procedimiento, debe ser miembro de **los administradores**.

### <a name="install-dcb"></a>Instalar DCB

1. Haga clic en **Inicio**, desplácese hacia abajo hasta y haga clic en **sistema de Windows**.
2. Haga clic en **Panel de control**. Se abrirá el cuadro de diálogo **Panel de control** .
3. En el **Panel de control**, haga clic en **ver por**y, a continuación, haga clic en **iconos grandes** o iconos **pequeños**.
4. Haga clic en **Programas y características**. Se abre el cuadro de diálogo programas y características.
5. En **programas y características**, en el panel izquierdo, haga clic en **activar o desactivar las características de Windows**. Se abrirá el cuadro de diálogo **características de Windows** .
6. En **características de Windows**, haga clic en protocolo de **puente del centro de datos**y, a continuación, en **Aceptar**.

![Cuadro de diálogo activar o desactivar las características de Windows](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


