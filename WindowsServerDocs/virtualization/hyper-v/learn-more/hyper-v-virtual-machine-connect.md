---
title: Conexión a máquina Virtual de Hyper-V
description: Describe la conexión a máquina Virtual, que proporciona acceso remoto a una máquina virtual. Incluye detalles sobre cómo realizar tareas comunes, como Enviar Ctrl-Alt-Supr a la máquina virtual.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: deae35b9-7647-42b8-b6bf-45645a44c9c4
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: e1f3260fdbbd82a97c3b0949936afc6a04ec5e5a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887846"
---
# <a name="hyper-v-virtual-machine-connection"></a>Conexión a máquina Virtual de Hyper-V

>Se aplica a: Windows Server 2016, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 8

Conexión a máquina virtual \(VMConnect\) es una herramienta que utilice para conectarse a una máquina virtual para que pueda instalar o interactuar con el sistema operativo invitado en una máquina virtual. Algunas de las tareas que puede realizar mediante el uso de VMConnect incluyen lo siguiente:  
  
-   Iniciar y apagar una máquina virtual  
  
-   Conectarse a una imagen de DVD \(archivo .iso\) o una unidad flash USB  
  
-   Crear un punto de control  
  
-   Modificar la configuración de una máquina virtual  
    
## <a name="tips-for-using-vmconnect"></a>Sugerencias para usar VMConnect  
Puede encontrar útil la siguiente información para el uso de VMConnect:  
  
|Para hacer esto...|Hacer esto...|  
|---------------|------------|  
|Enviar clics del mouse o teclado a la máquina virtual|Haga clic en la ventana de la máquina virtual. El puntero del mouse puede aparecer como un pequeño punto cuando se conecta a una máquina virtual en ejecución.|  
|Devolver los clics del mouse o teclado como entrada para el equipo físico|Presione la tecla CTRL\+ALT\+DEJA de flecha y, después, mueva el puntero del mouse fuera de la ventana de la máquina virtual. Esta combinación de teclas de mouse versión puede cambiarse en el Hyper\-configuración V en Hyper\-V Manager.|  
|Enviar CTRL\+ALT\+combinación de teclas de DELETE en una máquina virtual|Seleccione **acción** > **Ctrl\+Alt\+eliminar** o use la combinación de teclas CTRL\+ALT\+final.|  
|Cambiar de un modo de ventana a un completo\-el modo de pantalla|Seleccione **vista** > **modo de pantalla completa**. Para volver al modo de ventana, presione la tecla CTRL\+ALT\+interrumpir.|  
|Crear un punto de control para capturar el estado actual de la máquina para solucionar problemas|Seleccione **acción** > **Checkpoint** o use la combinación de teclas CTRL\+N.|  
|Cambiar la configuración de la máquina virtual|Seleccione **archivo** > **configuración**.|  
|Conectarse a una imagen de DVD \(archivo .iso\) o un disquete virtual \(archivo .vfd\)|Seleccione **Media**.<br /><br />No se admiten los disquetes virtuales para máquinas virtuales de generación 2. Para obtener más información, consulte [debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md).|  
|Usar recursos locales de un host de Hyper\-V virtual machine, como una unidad flash USB|Activar el modo de sesión mejorada en el host de Hyper-V, use VMConnect para conectarse a la máquina virtual y antes de conectarse, elija el recurso local que desea usar. Para conocer los pasos específicos, consulte [usar los recursos locales en Hyper\-máquina virtual V con VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).|  
|Cambiar la configuración de VMConnect para una máquina virtual guardada|Ejecute el siguiente comando en Windows PowerShell o la línea de comandos:<br /><br />`VMConnect.exe <ServerName> <VMName> \/edit`|  
|Evitar que un usuario de VMConnect asumir VMConnect sesión de otro usuario|[Activar el modo de sesión mejorada en el host de Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#BKMK_OVER).<br /><br />No tener mejorado activado el modo de sesión puede suponer un riesgo de seguridad y privacidad. Si un usuario está conectado y ha iniciado sesión en una máquina virtual a través de VMConnect y otro usuario autorizado se conecta a la misma máquina virtual, la sesión se van a realizar el segundo usuario la tecla TAB y el primer usuario perderá la sesión. El segundo usuario podrá ver del primer usuario escritorio, documentos y aplicaciones.|
|Administrar integration services o componentes que permiten que la máquina virtual para comunicarse con el host de Hyper-V| En los hosts de Hyper-V que ejecutan Windows 10 o Windows Server 2016, no puede administrar servicios de integración con VMConnect. Consulte estos temas: <br />- [Activar o desactivar todos los servicios de integración desde el host de Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics) <br />- [Activar o desactivar todos los servicios de integración de una máquina virtual de Windows](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-windows)<br />- [Activar o desactivar todos los servicios de integración de una máquina virtual Linux](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-linux) <br />- [Mantener los servicios de integración actualizados para la máquina virtual](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#integration-service-maintenance)  <br />Para los hosts que ejecutan Windows Server 2012 o Windows Server 2012 R2, consulte [Integration Services](https://technet.microsoft.com/library/dn798297(v=ws.11).aspx).|
|Cambiar el tamaño de la ventana de VMConnect|Puede cambiar el tamaño de la ventana de VMConnect para las máquinas virtuales de generación 2 que ejecutan el sistema operativo Windows. Para ello, necesita activar el modo de sesión mejorada en el host de Hyper-V. Para obtener más información, consulte [activar el modo de sesión mejorada en el host de Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#BKMK_OVER). Para las máquinas virtuales que ejecutan Ubuntu, consulte [cambiar la resolución de pantalla de Ubuntu en una máquina virtual de Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/).|


## <a name="keyboard-shortcuts"></a>Accesos rápidos de teclado  
De forma predeterminada, los clics del mouse de entrada y salida de teclado se envían a la máquina virtual. Por lo que es posible que deba presionar CTRL + ALT + izquierda flecha antes de usar las siguientes teclas de método abreviado. 

|Combinación de teclas|Descripción|  
|-------------------|---------------|  
|CTRL\+ALT\+flecha izquierda|Versión del mouse|  
|CTRL\+ALT\+FINAL|Equivalente de CTRL\+ALT\+eliminar en la máquina virtual|  
|CTRL\+ALT\+INTERRUMPIR|Cambiar de completo\-el modo de pantalla al modo de ventana|  
|CTRL\+O|Se abre la configuración de la máquina virtual|  
|CTRL\+S|Inicia la máquina virtual|  
|CTRL\+N|Crear un punto de control|  
|CTRL\+E|Revertir a un punto de control|  
|CTRL\+C|Hacer una captura de pantalla|  

## <a name="see-also"></a>Vea también  
-   [Usar los recursos locales en la máquina virtual de Hyper-V con VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)  
-   [Hyper-V en Windows Server 2016](../Hyper-V-on-Windows-Server.md)  
-   [Hyper-V en Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/windows_welcome)  
  
  
