---
title: Conexión a máquina virtual de Hyper-V
description: Describe la característica Conexión a máquina virtual, que proporciona acceso remoto a una máquina virtual. Incluye detalles sobre cómo realizar tareas comunes, como enviar CTRL-Alt-Suprimir a la máquina virtual.
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
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364155"
---
# <a name="hyper-v-virtual-machine-connection"></a>Conexión a máquina virtual de Hyper-V

>Se aplica a: Windows Server 2016, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 8

Conexión a máquina virtual \(VMConnect\) es una herramienta que puedes usar para conectarte a una máquina virtual de manera que puedas instalar o interactuar con el sistema operativo invitado en una máquina virtual. Entre las tareas que puede llevar a cabo con VMConnect se incluyen las siguientes:  
  
-   Iniciar y apagar una máquina virtual  
  
-   Conectarte al \(archivo .iso\) de una imagen de DVD o a una unidad flash USB  
  
-   Crear un punto de control  
  
-   Modificar la configuración de una máquina virtual  
    
## <a name="tips-for-using-vmconnect"></a>Sugerencias para usar VMConnect  
Puede que te resulte útil la siguiente información para utilizar VMConnect:  
  
|Para|Haz esto|  
|---------------|------------|  
|Enviar clics del mouse o entradas del teclado a la máquina virtual|Haz clic en cualquier lugar de la ventana de la máquina virtual. El puntero del mouse puede aparecer como un punto pequeño cuando te conectas a una máquina virtual en ejecución.|  
|Devolver clics del mouse o entradas del teclado al equipo físico|Presiona CTRL\+Alt\+flecha izquierda y, a continuación, mueve el puntero del mouse fuera de la ventana de la máquina virtual. Esta combinación de teclas de liberación del mouse se puede cambiar en la configuración de Hyper\-V del Administrador de Hyper\-V.|  
|Enviar la combinación de teclas CTRL\+Alt\+Suprimir a una máquina virtual|Selecciona **Acción** > **Ctrl\+Alt\+Suprimir** o usa la combinación de teclas CTRL\+Alt\+Fin.|  
|Cambiar de un modo de ventana a un modo de pantalla completa|Selecciona **Ver** > **Modo de pantalla completa**. Para volver al modo de ventana, presiona CTRL\+Alt\+ENTRAR.|  
|Crear un punto de control para capturar el estado actual de la máquina para solucionar problemas|Selecciona **Acción** > **Punto de control** o usa la combinación de teclas CTRL\+N.|  
|Cambiar la configuración de la máquina virtual|Selecciona **Archivo** > **Configuración**.|  
|Conectarte al \(archivo .iso\) de una imagen de DVD o al \(archivo .vfd\) de un disquete virtual|Selecciona **Medios**.<br /><br />Las máquinas virtuales de generación 2 no admiten unidades de disquete virtuales. Para más información, consulta [¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)|  
|Usar los recursos locales de un host en la máquina virtual de Hyper\-V como una unidad flash USB|Activa el modo de sesión mejorada en el host de Hyper-V, usa VMConnect para conectarte a la máquina virtual y, antes de conectarte, elige el recurso local que quieres usar. Para conocer los pasos específicos, consulta [Uso de recursos locales en la máquina virtual de Hyper\-V con VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).|  
|Cambiar la configuración de VMConnect guardada de una máquina virtual|Ejecuta el comando siguiente en Windows PowerShell o el símbolo del sistema:<br /><br />`VMConnect.exe <ServerName> <VMName> \/edit`|  
|Impedir que un usuario de VMConnect tome el control de la sesión de VMConnect de otro usuario|[Activa el modo de sesión mejorada en el host de Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host).<br /><br />No tener activado el modo de sesión mejorada puede suponer un riesgo para la seguridad y la privacidad. Si un usuario se ha conectado a una máquina virtual e iniciado sesión en esta a través de VMConnect y otro usuario autorizado se conecta a la misma máquina virtual, el segundo usuario tomará el control de la sesión y el primero la perderá. El segundo usuario podrá ver el escritorio, los documentos y las aplicaciones del primero.|
|Administrar los componentes o los servicios de integración que permiten que la VM se comunique con el host de Hyper-V| En los hosts de Hyper-V que ejecutan Windows 10 o Windows Server 2016, no se pueden administrar los servicios de integración con VMConnect. Consulta estos temas: <br />- [Activar o desactivar los servicios de integración desde el host de Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics) <br />- [Activar o desactivar los servicios de integración desde una máquina virtual con Windows](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-windows)<br />- [Activar o desactivar los servicios de integración desde una máquina virtual con Linux](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-linux) <br />- [Mantener los servicios de integración actualizados para la máquina virtual](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#integration-service-maintenance)  <br />En el caso de los hosts que ejecutan Windows Server 2012 o Windows Server 2012 R2, consulta [Servicios de integración](https://technet.microsoft.com/library/dn798297(v=ws.11).aspx).|
|Cambiar el tamaño de la ventana de VMConnect|Puedes cambiar el tamaño de la ventana de VMConnect para las máquinas virtuales de generación 2 que ejecutan un sistema operativo Windows. Para ello, puede que tengas que activar el modo de sesión mejorada en el host de Hyper-V. Para más información, consulta [Activar el modo de sesión mejorada en el host de Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host). Para las máquinas virtuales que ejecutan Ubuntu, consulta [Cambio de la resolución de pantalla de Ubuntu en una VM de Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/).|


## <a name="keyboard-shortcuts"></a>Métodos abreviados de teclado  
De forma predeterminada, las entradas de teclado y los clics del mouse se envían a la máquina virtual. Por lo tanto, es posible que tengas que presionar CTRL + Alt + flecha izquierda antes de usar las teclas de método abreviado siguientes. 

|Combinación de teclas|Descripción|  
|-------------------|---------------|  
|CTRL\+Alt\+flecha izquierda|Liberación del mouse|  
|CTRL\+Alt\+Fin|Equivalente a CTRL\+Alt\+Suprimir en la máquina virtual|  
|CTRL\+Alt\+ENTRAR|Cambiar del modo de pantalla completa de nuevo al modo de ventana|  
|CTRL\+O|Abrir la configuración de la máquina virtual|  
|CTRL\+S|Iniciar la máquina virtual|  
|CTRL\+N|Crear un punto de control|  
|CTRL\+E|Revertir a un punto de control|  
|CTRL\+C|Realizar una captura de pantalla|  

## <a name="see-also"></a>Consulte también  
-   [Usos de recursos locales en la máquina virtual de Hyper-V con VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)  
-   [Hyper-V en Windows Server 2016](../Hyper-V-on-Windows-Server.md)  
-   [Hyper-V en Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/windows_welcome)  
  
  
