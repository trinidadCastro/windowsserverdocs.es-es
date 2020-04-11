---
title: Use local resources on Hyper-V virtual machine with VMConnect
description: Describe los requisitos de uso de recursos locales con VMConnect.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 18eface5-7518-4c6b-9282-93e2e3e87492
author: kbdazure
ms.author: kathydav
ms.date: 12/06/2016
ms.openlocfilehash: dccc4ccf66d457da9dcc2a71ff8d259565fe2714
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860478"
---
# <a name="use-local-resources-on-hyper-v-virtual-machine-with-vmconnect"></a>Use local resources on Hyper-V virtual machine with VMConnect

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2

Conexión a máquina virtual (VMConnect) permite usar los recursos locales de un equipo en una máquina virtual (por ejemplo, una unidad flash USB extraíble o una impresora). El modo de sesión mejorada también permite cambiar el tamaño de la ventana de VMConnect. En este artículo se muestra cómo configurar el host y, a continuación, conceder a la máquina virtual acceso a un recurso local.

Modo de sesión mejorada y Escribir texto del Portapapeles solo están disponibles para las máquinas virtuales que ejecutan sistemas operativos Windows recientes. \(Consulta [Requisitos para usar los recursos locales](#requirements-for-using-local-resources) a continuación.\) 

Para las máquinas virtuales que ejecutan Ubuntu, consulta [Cambio de la resolución de pantalla de Ubuntu en una VM de Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/). 
  
## <a name="turn-on-enhanced-session-mode-on-a-hyper-v-host"></a>Activar el modo de sesión mejorada en un host de Hyper-V  
Si el host de Hyper-V ejecuta Windows 10 o Windows 8.1, el modo de sesión mejorada estará activado de forma predeterminada, por lo que puede omitir este paso y continuar con la sección siguiente. Pero si el host ejecuta Windows Server 2016 o Windows Server 2012 R2, realice este paso primero. 
  
Active el modo de sesión mejorada:

1.  Conéctese al equipo que hospeda la máquina virtual.  
  
2.  En el Administrador de Hyper-V, seleccione el nombre de equipo del host.  
  
    ![Captura de pantalla que muestra un nombre de equipo host que aparece en el Administrador de Hyper-V en el panel izquierdo.](media/Hyper-V-HyperVManager-HostNameSelected.png)  
  
3.  Seleccione **Configuración de Hyper-V**.  
  
    ![Captura de pantalla que muestra la opción de configuración de Hyper-V en Acciones en el panel derecho.](media/HyperV-ActionsHyperVSettings.png)  
  
4.  En **Servidor**, seleccione **Directiva de modo de sesión mejorada**.  
  
    ![Captura de pantalla que muestra la opción Directiva de modo de sesión mejorada en la sección Seguridad.](media/Hyper-V-Settings-ServerEnhancedSessionModePolicy.png)  
  
5.  Seleccione la casilla **Permitir modo de sesión mejorada** .  
  
    ![Captura de pantalla que muestra la casilla Permitir modo de sesión mejorada de Directiva de modo de sesión mejorada.](media/Hyper-V-Settings-EnhancedSessionModePolicyCheckBox.png)  
  
6.  En **Usuario**, seleccione **Modo de sesión mejorada**.  
  
    ![Captura de pantalla que muestra la opción Modo de sesión mejorada en la sección Seguridad. ](media/Hyper-V-Settings-UserEnhancedSessionMode.png)  
  
7.  Seleccione la casilla **Permitir modo de sesión mejorada** .  
  
8.  Haga clic en **Aceptar**.  
  
## <a name="choose-a-local-resource"></a>Elegir un recurso local

Los recursos locales incluyen impresoras, el Portapapeles y una unidad local en el equipo en el que se ejecuta VMConnect. Para más información, consulta [Requisitos para usar recursos locales](#requirements-for-using-local-resources) a continuación.  
  
Para elegir un recurso local:
  
1.  Abra VMConnect.  
  
2.  Seleccione la máquina virtual a la que desea conectarse.  
  
3.  Haga clic en **Mostrar opciones**.  
  
    ![Captura de pantalla que llama a Mostrar opciones en la parte inferior izquierda del cuadro de diálogo.](media/HyperV-VMConnect-DisplayConfig.png)  
  
4.  Seleccione **Recursos locales**.  
  
    ![Captura de pantalla que llama a la pestaña Recursos locales.](media/HyperV-VMConnect-DisplayConfig-LocalResources.png)  
  
5.  Haga clic en **Más**.  
  
    ![Captura de pantalla que llama al botón Más.](media/HyperV-VMConnect-DisplayConfig-LocalResourcesMore.png)  
  
6.  Seleccione la unidad que desea usar en la máquina virtual y haga clic en **Aceptar**.  
  
    ![Captura de pantalla que muestra los recursos locales y las unidades que puedes seleccionar.](media/HyperV-VMConnect-Settings-LocalResourcesDrives.png)  
  
7.  Seleccione **Guardar mi configuración para conexiones futuras en esta máquina virtual**.  
  
    ![Captura de pantalla que llama a la casilla de verificación para seleccionar esta opción.](media/HyperV-VMConnect-SaveSettings.png)  
  
8.  Haga clic en **Conectar**.  
  
## <a name="edit-vmconnect-settings"></a>Editar la configuración de VMConnect

La configuración de conexión para VMConnect se puede editar fácilmente con el comando siguiente de Windows PowerShell o el símbolo del sistema:  
  
`VMConnect.exe <ServerName> <VMName> /edit`  
  
## <a name="requirements-for-using-local-resources"></a>Requisitos para usar los recursos locales

Para poder usar los recursos locales de un equipo en una máquina virtual:  
  
-   El host de Hyper-V debe tener activadas las opciones **Directiva de modo de sesión mejorada** y **Modo de sesión mejorada**.  
  
-   El equipo en el que se usa VMConnect debe ejecutar Windows 10, Windows 8.1, Windows Server 2016 o Windows Server 2012 R2.  
  
-   La máquina virtual debe tener habilitados los Servicios de Escritorio remoto y ejecutar Windows 10, Windows 8.1, Windows Server 2016 o Windows Server 2012 R2 como sistema operativo invitado.  
  
Si el equipo que ejecuta VMConnect y la máquina virtual cumplen los requisitos, puedes usar cualquiera de los siguientes recursos locales si están disponibles:  
  
-   Configuración de pantalla  
  
-   Audio
  
-   Impresoras  
  
-   Portapapeles para copiar y pegar  
  
-   Tarjetas inteligentes  
  
-   Dispositivos USB  
  
-   Unidades  
  
-   Dispositivos Plug and Play admitidos  
  
## <a name="why-use-a-computers-local-resources"></a>¿Por qué usar los recursos locales de un equipo?
Por ejemplo, los recursos locales de un equipo pueden usarse para:  
  
-   Solucionar problemas en una máquina virtual sin una conexión de red a dicha máquina virtual.  
  
-   Copiar y pegar archivos en la máquina virtual de la misma manera que mediante una Conexión a Escritorio remoto (RDP).  
  
-   Iniciar sesión en la máquina virtual con una tarjeta inteligente.  
  
-   Imprimir desde una máquina virtual en una impresora local.  
  
-   Realizar pruebas y solucionar problemas en aplicaciones de desarrollador que requieren redireccionamiento de sonido y USB sin usar RDP.  
  
## <a name="see-also"></a>Consulte también  
[Conectarse a una máquina virtual](https://technet.microsoft.com/library/cc742407.aspx)  
[¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)



