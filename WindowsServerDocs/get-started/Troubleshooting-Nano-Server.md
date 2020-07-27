---
title: Solución de problemas de Nano Server
description: Consola de recuperación, servicios de gestión de emergencia, depuración del kernel
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.topic: article
ms.assetid: e427c66f-9571-4b8c-b65d-e7370d91544d
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: ce71c2d11343be62d47f8957fa9414915fcc7847
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964417"
---
# <a name="troubleshooting-nano-server"></a>Solución de problemas de Nano Server

>Se aplica a: Windows Server 2016

> [!IMPORTANT]
> A partir de Windows Server, versión 1709, Nano Server estará disponible solo como [imagen base del sistema operativo del contenedor](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Consulta [Cambios en Nano Server](nano-in-semi-annual-channel.md) para más información. 

Este tema incluye información acerca de las herramientas que puede usar para conectarse para diagnosticar y reparar las instalaciones de Nano Server.  
  
## <a name="using-the-nano-server-recovery-console"></a>Uso de la Consola de recuperación de Nano Server 
 
Nano Server incluye una consola de recuperación que garantiza que puede tener acceso a Nano Server incluso si un error de configuración de red interfiere con la conexión a Nano Server. Puede utilizar la consola de recuperación para reparar la red y luego usar las herramientas de administración remota habituales.  
  
Al arrancar Nano Server en una máquina virtual o en un equipo físico que tiene un monitor y un teclado conectados, aparece una pantalla completa con un mensaje de inicio de sesión en modo de texto. Inicie sesión en este símbolo del sistema con una cuenta de administrador para ver el nombre de equipo y la dirección IP de Nano Server. Puede usar estos comandos para navegar en esta consola:  
  
-   Use las teclas de flecha para desplazarse  
  
-   Use TAB para desplazarse a cualquier texto que comienza con **>** ; a continuación, presione ENTRAR para seleccionar.  
  
-   Para retroceder una pantalla o página, presione ESC. Si se encuentra en la página de inicio, presione ESC para cerrará la sesión.  
  
-   Algunas pantallas tienen capacidades adicionales que se muestran en la última línea de la pantalla. Por ejemplo, si explora un adaptador de red, F4 deshabilitará el adaptador de red.  
  
La consola de recuperación permite ver y configurar adaptadores de red y la configuración TCP/IP, así como las reglas de firewall.
> [!NOTE]
> La Consola de recuperación solo admite funciones básicas de teclado. No se admiten las luces del teclado, las secciones de 10 teclas y el cambio de la distribución del teclado entre Bloq Mayús y Bloq num. Solo se admiten los teclados en inglés y el juego de caracteres.

## <a name="accessing-nano-server-over-a-serial-port-with-emergency-management-services"></a>Acceso a Nano Server a través de un puerto serie con los Servicios de administración de emergencia  
Los Servicios de administración de emergencia (EMS) permiten realizar la solución básica, obtener el estado de la red y abrir las sesiones de consola (incluidos CMD o PowerShell) mediante un emulador de terminal a través de un puerto serie. Esto reemplaza la necesidad de un teclado y un monitor para solucionar problemas de un servidor. Para obtener más información sobre EMS, vea [Emergency Management Services Technical Reference](/previous-versions/windows/it-pro/windows-server-2003/cc784411(v=ws.10)) (Referencia técnica de servicios de administración de emergencia).

Para habilitar EMS en una imagen Nano Server para que esté listo cuando lo necesite más adelante, ejecute este cmdlet:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\EnablingEMS.vhdx   -EnableEMS   -EMSPort 3   -EMSBaudRate 9600`  
  
Este ejemplo habilita EMS en el puerto serie 3 con una velocidad en baudios de 9600 bps. Si no incluye los parámetros, los valores predeterminados son el puerto 1 y 115200 bps. Para usar este cmdlet para medios VHDX, asegúrese de incluir la característica de Hyper-V y los módulos de Windows PowerShell correspondientes.

## <a name="kernel-debugging"></a>Depuración del kernel  
Puede configurar la imagen Nano Server para admitir la depuración del kernel mediante una amplia variedad de métodos. Para usar al depuración del kernel con una imagen VHDX, asegúrese de incluir la característica de Hyper-V y los módulos de Windows PowerShell correspondientes. Para obtener más información sobre la depuración remota de kernel, por lo general, vea [Setting Up Kernel-Mode Debugging over a Network Cable Manually](/windows-hardware/drivers/debugger/setting-up-a-network-debugging-connection) (Configuración manual de la depuración de modo kernel a través de un cable de red) y [Remote Debugging Using WinDbg](/windows-hardware/drivers/debugger/setting-up-a-network-debugging-connection) (Depuración remota con WinDbg).  
  
### <a name="debugging-using-a-serial-port"></a>Depuración con un puerto serie  
Use este cmdlet de ejemplo para habilitar la imagen que se desea depurar mediante un puerto serie:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingSerial   -DebugMethod Serial   -DebugCOMPort 1   -DebugBaudRate 9600`  
  
Este ejemplo habilita la depuración de serie a través del puerto 2 con una velocidad en baudios de 9600 bps. Si no especifica los parámetros, los valores predeterminados son el puerto 2 y la velocidad de 115200 bps. Si piensa utilizar EMS y la depuración del kernel, tendrá que configurarlos para utilizar dos puertos serie independientes.  
  
### <a name="debugging-over-a-tcpip-network"></a>Depuración a través de una red TCP/IP  
Use este cmdlet de ejemplo para habilitar la imagen que se desea depurar mediante una red TCP/IP:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingNetwork   -DebugMethod Net   -DebugRemoteIP 192.168.1.100   -DebugPort 64000`  
  
Este cmdlet permite la depuración del kernel, de tal forma que el equipo con la dirección IP de 192.168.1.100 pueda conectarse, con todas las comunicaciones a través del puerto 64000. Los parámetros -DebugRemoteIP y -DebugPort son obligatorios, con un número de puerto mayor que 49152. Este cmdlet genera una clave de cifrado en un archivo junto con el VHD resultante que es necesario para la comunicación a través del puerto. Como alternativa, puede especificar su propia clave con el parámetro -DebugKey, como en este ejemplo:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingNetwork   -DebugMethod Net   -DebugRemoteIP 192.168.1.100   -DebugPort 64000   -DebugKey 1.2.3.4`  
  
### <a name="debugging-using-the-ieee1394-protocol-firewire"></a>Depuración con el protocolo IEEE 1394 (Firewire)  
Para habilitar la depuración a través de IEEE 1394, use este cmdlet de ejemplo:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingFireWire   -DebugMethod 1394   -DebugChannel 3`  
  
El parámetro -DebugChannel es obligatorio.  
  
### <a name="debugging-using-usb"></a>Depuración mediante USB  
Puede habilitar la depuración a través de USB con este cmdlet:  
  
`New-NanoServerImage   -MediaPath \\Path\To\Media\en_us   -BasePath .\Base   -TargetPath .\KernelDebuggingUSB   -DebugMethod USB   -DebugTargetName KernelDebuggingUSBNano`  
  
Cuando el depurador remoto se conecta al servidor Nano resultante, especifique el nombre de destino establecido por el parámetro -DebugTargetName.    
