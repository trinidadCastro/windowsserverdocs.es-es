---
title: Conexión de máquina virtual de Hyper-V
description: Describe la conexión a máquina virtual, que proporciona acceso remoto a una máquina virtual. Incluye detalles sobre cómo realizar tareas comunes, como enviar CTRL-ALT-SUPR a la máquina virtual.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fba83d22d9e5d9f31a5809781aa04943cc4cd3af
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364155"
---
# <a name="hyper-v-virtual-machine-connection"></a>Conexión de máquina virtual de Hyper-V

>Se aplica a: Windows Server 2016, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 8

Conexión \(a máquina virtual\) VMConnect es una herramienta que se usa para conectarse a una máquina virtual, de modo que pueda instalar o interactuar con el sistema operativo invitado en una máquina virtual. Entre las tareas que puede realizar con VMConnect se incluyen las siguientes:  
  
-   Iniciar y apagar una máquina virtual  
  
-   Conectar con un archivo \(\) . ISO de imagen de DVD o una unidad flash USB  
  
-   Crear un punto de control  
  
-   Modificar la configuración de una máquina virtual  
    
## <a name="tips-for-using-vmconnect"></a>Sugerencias para usar VMConnect  
Puede que le resulte útil la siguiente información para el uso de VMConnect:  
  
|Para hacer esto...|Haga esto...|  
|---------------|------------|  
|Enviar clics del mouse o la entrada del teclado a la máquina virtual|Haga clic en cualquier lugar de la ventana de la máquina virtual. El puntero del mouse puede aparecer como un punto pequeño cuando se conecta a una máquina virtual en ejecución.|  
|Devolver clics del mouse o la entrada del teclado al equipo físico|Presione Ctrl\+ALT\+flecha izquierda y, a continuación, mueva el puntero del mouse fuera de la ventana de la máquina virtual. Esta combinación de teclas de versión de mouse se puede cambiar\-en la configuración de\-Hyper-v en el administrador de Hyper-v.|  
|Enviar combinación\+de\+teclas Ctrl Alt Supr a una máquina virtual|Seleccione **acción** > \+\+**CtrlAltSupr\+o use la combinación de teclas Ctrl Alt end.\+**|  
|Cambiar de un modo de ventana a un\-modo de pantalla completa|Seleccione **Ver** > **modo de pantalla completa**. Para volver al modo de ventana, presione Ctrl\+ALT\+interrumpir.|  
|Crear un punto de control para capturar el estado actual de la máquina para la solución de problemas|Seleccione > **punto de control** de acción o use la\+combinación de teclas Ctrl N.|  
|Cambiar la configuración de la máquina virtual|Seleccione**configuración**del **archivo** > .|  
|Conéctese a un archivo \(\) . ISO de imagen de DVD o a \(un archivo. VFD de disquete virtual\)|Seleccione **medios**.<br /><br />Los disquetes virtuales no se admiten para las máquinas virtuales de generación 2. Para obtener más información, consulte ¿ [debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)|  
|Usar los recursos locales de un host en\-la máquina virtual de Hyper-V, como una unidad flash USB|Active el modo de sesión mejorada en el host de Hyper-V, use VMConnect para conectarse a la máquina virtual y antes de conectarse, elija el recurso local que desea usar. Para conocer los pasos específicos, consulte [uso de recursos locales\-en una máquina virtual de Hyper V con VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).|  
|Cambiar la configuración de VMConnect guardada para una máquina virtual|Ejecute el siguiente comando en Windows PowerShell o en el símbolo del sistema:<br /><br />`VMConnect.exe <ServerName> <VMName> \/edit`|  
|Impedir que un usuario de VMConnect tome la sesión de VMConnect de otro usuario|[Active el modo de sesión mejorada en el host de Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host).<br /><br />No tener activado el modo de sesión mejorada puede suponer un riesgo de seguridad y privacidad. Si un usuario está conectado e iniciado sesión en una máquina virtual a través de VMConnect y otro usuario autorizado se conecta a la misma máquina virtual, el segundo usuario tomará la sesión y el primer usuario perderá la sesión. El segundo usuario podrá ver el escritorio, los documentos y las aplicaciones del primer usuario.|
|Administrar Integration Services o componentes que permiten que la máquina virtual se comunique con el host de Hyper-V| En los hosts de Hyper-V que ejecutan Windows 10 o Windows Server 2016, no se pueden administrar los servicios de integración con VMConnect. Vea estos temas: <br />- [Activar o desactivar Integration Services desde el host de Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics) <br />- [Activar o desactivar Integration Services desde una máquina virtual de Windows](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-windows)<br />- [Activar o desactivar Integration Services desde una máquina virtual Linux](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-linux) <br />- [Mantener los servicios de integración actualizados para la máquina virtual](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#integration-service-maintenance)  <br />En el caso de los hosts que ejecutan Windows Server 2012 o Windows Server 2012 R2, consulte [Integration Services](https://technet.microsoft.com/library/dn798297(v=ws.11).aspx).|
|Cambiar el tamaño de la ventana de VMConnect|Puede cambiar el tamaño de la ventana de VMConnect para las máquinas virtuales de generación 2 que ejecutan un sistema operativo Windows. Para ello, puede que tenga que activar el modo de sesión mejorada en el host de Hyper-V. Para obtener más información, vea [activar el modo de sesión mejorada en el host de Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host). Para las máquinas virtuales que ejecutan Ubuntu, consulte [cambio de la resolución de pantalla de Ubuntu en una máquina virtual de Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/).|


## <a name="keyboard-shortcuts"></a>Accesos rápidos de teclado  
De forma predeterminada, los clics de teclado y del mouse se envían a la máquina virtual. Por lo tanto, es posible que tenga que presionar CTRL + ALT + Flecha izquierda antes de usar las teclas de método abreviado siguientes. 

|Combinación de teclas|Descripción|  
|-------------------|---------------|  
|Ctrl\+ALT\+flecha izquierda|Versión del mouse|  
|CTRL\+ALT\+FIN|Equivalente a Ctrl\+ALT\+Supr en la máquina virtual|  
|CTRL\+ALT\+INTER|Cambiar desde el\-modo de pantalla completa de nuevo al modo de ventana|  
|CTRL\+O|Abre la configuración de la máquina virtual|  
|CTRL\+S|Inicia la máquina virtual|  
|CTRL\+N|Crear un punto de control|  
|CTRL\+E|Revertir a un punto de control|  
|CTRL\+C|Realizar una captura de pantalla|  

## <a name="see-also"></a>Vea también  
-   [Usar recursos locales en una máquina virtual de Hyper-V con VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)  
-   [Hyper-V en Windows Server 2016](../Hyper-V-on-Windows-Server.md)  
-   [Hyper-V en Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/windows_welcome)  
  
  
