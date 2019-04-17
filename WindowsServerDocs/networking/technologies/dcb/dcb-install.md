---
title: Instalación de centro de datos puente (DCB) en Windows Server o cliente
description: En este tema proporciona instrucciones sobre cómo instalar el puente de centro de datos en Windows Server o cliente de Windows.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 491bdeb1a7458be1f991be68724e7a7b51f67ecf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>Instalar el centro de datos puente \(DCB\) en Windows Server 2016 o Windows 10

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información sobre cómo instalar DCB en Windows Server 2016 o Windows 10.

## <a name="prerequisites-for-using-dcb"></a>Requisitos previos para usar DCB

Los siguientes son los requisitos previos para configurar y administrar DCB.

### <a name="install-a-compatible-operating-system"></a>Instalar un sistema operativo compatible

Puedes usar los comandos DCB desde esta guía en los siguientes sistemas operativos.

- Windows Server (punto y anual canal)
- Windows Server 2016
- Windows 10 \(all versions\)

Los siguientes sistemas operativos incluir versiones anteriores de DCB que no son compatibles con los comandos que se usan en la documentación de DCB para Windows Server 2016 y Windows 10.

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>Requisitos de hardware

Siguiente es una lista de requisitos de hardware para DCB.

- Compatible con DCB\ adapter\(s\) de red Ethernet debe instalarse en los equipos que están proporcionando DCB de Windows Server 2016.
- Modificadores de hardware compatibles con DCB\ deben implementarse en la red.


## <a name="install-dcb-in-windows-server-2016"></a>Instalar DCB en Windows Server 2016

Puedes usar las siguientes secciones para instalar DCB en un equipo que ejecute Windows Server 2016.

**Credenciales administrativas**

Para realizar estos procedimientos, debe ser miembro del **administradores**.

### <a name="install-dcb-using-windows-powershell"></a>Instalar DCB mediante Windows PowerShell

Puedes usar el siguiente procedimiento para instalar DCB mediante Windows PowerShell.

1. En un equipo con Windows Server 2016, haz clic en **inicio**, a continuación, haz clic en el icono de Windows PowerShell. Aparece un menú. En el menú, haz clic en **más**y, a continuación, haz clic en **ejecutar como administrador**. Si se te pide, escribe las credenciales para una cuenta con privilegios de administrador en el equipo. Abre Windows PowerShell con privilegios de administrador.
2. Escribe el siguiente comando y, a continuación, presione ENTRAR.

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>Instalar DCB con el administrador del servidor

Puedes usar el siguiente procedimiento para instalar DCB mediante el administrador del servidor.

>[!NOTE]
>Después de realizar el primer paso en este procedimiento, el **antes de comenzar** no se muestra la página del agregar Roles y características de asistente si anteriormente seleccionaste **omitir esta página predeterminada** cuando se ejecutó el agregar Roles y características de asistente. Si la **antes de comenzar** no se muestra la página, vaya del paso 1 al paso 3.

1. En DC1, en el administrador del servidor, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**. Abre el agregar Roles and Features Wizard.
2. En **antes de comenzar**, haz clic en **siguiente**.
3. En **seleccionar el tipo de instalación**, asegúrate de que **instalación basada en rol o característica** está seleccionado y, a continuación, haz clic en **siguiente**.
4. En **servidor de destino selecciona**, asegúrate de que **seleccionar un servidor desde el grupo de servidores** está seleccionado. En **grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haz clic en **siguiente**.
5. En **seleccionar roles de servidor**, haz clic en **siguiente**.
6. En **Select features**, en **características**, haz clic en **puente de centro de datos**. Abre un cuadro de diálogo para preguntará si desea agregar características DCB necesario. Haz clic en **agregar características**.
7. En **Select features**, haz clic en **siguiente**. 
8. 7. En **Confirmar selecciones de instalación**, haz clic en **instalar**. La **progreso de la instalación** página muestra el estado durante el proceso de instalación. Después de que el mensaje aparece el mensaje que se instaló correctamente, haz clic en **cerrar**.

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>Configurar el depurador de kernel para permitir QoS \(Optional\)

 De manera predeterminada, los depuradores de kernel bloquean NetQos. Independientemente del método que usaste para instalar DCB, si tienes un depurador de kernel instalado en el equipo, debes configurar el depurador para permitir QoS esté habilitado y configurado ejecutando el siguiente comando.

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>Instalar DCB en Windows 10

Puedes realizar el siguiente procedimiento en un equipo de Windows 10.

Para realizar este procedimiento, debe ser miembro del **administradores **.

### <a name="install-dcb"></a>Instalar DCB

1. Haz clic en **inicio**, a continuación, desplácese hacia abajo y haz clic en **sistema Windows **.
2. Haz clic en **Panel de Control **. La **Panel de Control** abre el cuadro de diálogo.
3. En **Panel de Control**, haz clic en **ver por**y, a continuación, haga clic en **iconos grandes** o **iconos pequeños **.
4. Haz clic en **programas y características **. Abre el cuadro de diálogo de programas y características.
5. En **programas y características**, en el panel izquierdo, haz clic en **activar características de Windows o desactivar **. La **características de Windows** abre el cuadro de diálogo.
6. En **características de Windows**, haz clic en **puente de centro de datos**y, a continuación, haz clic en **Aceptar **.

![Activar o desactivar el cuadro de diálogo la características de Windows](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


