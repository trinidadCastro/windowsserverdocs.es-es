---
title: Centro de datos de instalación de protocolo de puente (DCB) en Windows Server o cliente
description: En este tema proporciona instrucciones sobre cómo instalar el protocolo de puente del centro de datos en Windows Server o cliente de Windows.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b89213d8-143a-45f3-a609-bc6a7027204c
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7c20ef027279780181ff176afa39a19f2976c4c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845446"
---
# <a name="install-data-center-bridging-dcb-in-windows-server-2016-or-windows-10"></a>Puente de centros de datos de instalación \(DCB\) en Windows Server 2016 o Windows 10

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para aprender a instalar DCB en Windows Server 2016 o Windows 10.

## <a name="prerequisites-for-using-dcb"></a>Requisitos previos para usar DCB

Estos son los requisitos previos para la configuración y administración de DCB.

### <a name="install-a-compatible-operating-system"></a>Instalar un sistema operativo compatible

Puede usar los comandos DCB de esta guía en los siguientes sistemas operativos.

- Windows Server (Canal semianual)
- Windows Server 2016
- Windows 10 \(todas las versiones\)

Los siguientes sistemas operativos incluyen las versiones anteriores de DCB que no son compatibles con los comandos que se usan en la documentación de DCB para Windows Server 2016 y Windows 10.

- Windows Server 2012 R2
- Windows Server 2012

###  <a name="hardware-requirements"></a>Requisitos de hardware

Siguiente es una lista de los requisitos de hardware de DCB.

- DCB\-adaptador de red Ethernet compatibles con\(s\) debe instalarse en equipos que proporcionen DCB de Windows Server 2016.
- DCB\-hardware compatible con conmutadores deben implementarse en la red.


## <a name="install-dcb-in-windows-server-2016"></a>Instalar DCB en Windows Server 2016

Puede usar las siguientes secciones para instalar DCB en un equipo que ejecuta Windows Server 2016.

**Credenciales administrativas**

Para llevar a cabo estos procedimientos, debe ser miembro de **administradores**.

### <a name="install-dcb-using-windows-powershell"></a>Instalar DCB mediante Windows PowerShell

Puede usar el siguiente procedimiento para instalar DCB mediante Windows PowerShell.

1. En un equipo que ejecuta Windows Server 2016, haga clic en **iniciar**, a continuación, haga clic en el icono de Windows PowerShell. Aparece un menú. En el menú, haga clic en **más**y, a continuación, haga clic en **ejecutar como administrador**. Si se le solicite, escriba las credenciales de una cuenta que tenga privilegios de administrador en el equipo. Se abre Windows PowerShell con privilegios de administrador.
2. Escriba el siguiente comando y presione ENTRAR.

````
    Install-WindowsFeature -Name Data-Center-Bridging -IncludeManagementTools
````

### <a name="install-dcb-using-server-manager"></a>Instalar DCB mediante el administrador del servidor

Puede usar el siguiente procedimiento para instalar DCB mediante el administrador del servidor.

>[!NOTE]
>Después de realizar el primer paso en este procedimiento, el **antes de comenzar** página del Asistente de las características y agregar Roles no se muestra si ha seleccionado anteriormente **omitir esta página predeterminada** cuando el complemento Se ejecutó el asistente. Si el **antes de comenzar** no se muestra la página, omitir del paso 1 al paso 3.

1. En DC1, en el administrador del servidor, haga clic en **administrar**y, a continuación, haga clic en **agregar Roles y características**. Se abre el Asistente para agregar roles y características.
2. En **Antes de comenzar**, haga clic en **Siguiente**.
3. En **Seleccionar tipo de instalación**, asegúrese de que la opción **Instalación basada en características o en roles** está seleccionada y, a continuación, haga clic en **Siguiente**.
4. En **Seleccionar servidor de destino**, asegúrese de que la opción **Seleccionar un servidor del grupo de servidores** está seleccionada. En **Grupo de servidores**, asegúrese de que el equipo local está seleccionado. Haz clic en **Siguiente**.
5. En **Seleccionar roles de servidor**, haz clic en **Siguiente**.
6. En **seleccionar características**, en **características**, haga clic en **puente del centro de datos**. Se abre un cuadro de diálogo para preguntar si desea agregar características necesaria de DCB. Haga clic en **agregar características**.
7. En **seleccionar características**, haga clic en **siguiente**. 
8. 7. en **Confirmar selecciones de instalación**, haga clic en **instalar**. El **progreso de la instalación** página muestra el estado durante el proceso de instalación. Después de que el mensaje aparece que indica que la instalación se realizó correctamente, haga clic en **cerrar**.

### <a name="configure-the-kernel-debugger-to-allow-qos-optional"></a>Configurar el depurador de kernel para permitir que QoS \(opcional\)

 De forma predeterminada, los depuradores de kernel bloquean NetQos. Independientemente del método que utilizó para instalar DCB, si tiene un depurador del núcleo instalado en el equipo, debe configurar el depurador para permitir que QoS habilitado y configurado, ejecute el comando siguiente.

````
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
````

## <a name="install-dcb-in-windows-10"></a>Instalar DCB en Windows 10

Puede realizar el procedimiento siguiente en un equipo Windows 10.

Para llevar a cabo este procedimiento, debe ser miembro de **administradores**.

### <a name="install-dcb"></a>Instalar DCB

1. Haga clic en **iniciar**, a continuación, desplácese hacia abajo y haga clic en **Windows System**.
2. Haga clic en **Panel de Control**. El **Panel de Control** abre el cuadro de diálogo.
3. En **Panel de Control**, haga clic en **ver**y, a continuación, haga clic en **iconos grandes** o **iconos pequeños**.
4. Haga clic en **programas y características**. Se abre el cuadro de diálogo de programas y características.
5. En **programas y características**, en el panel izquierdo, haga clic en **o desactivar las características de Windows Active**. El **características de Windows** abre el cuadro de diálogo.
6. En **características de Windows**, haga clic en **puente del centro de datos**y, a continuación, haga clic en **Aceptar**.

![Activar o desactivar el cuadro de diálogo características de Windows](../../media/Dcb-Scripting/Dcb-Scripting.jpg)


