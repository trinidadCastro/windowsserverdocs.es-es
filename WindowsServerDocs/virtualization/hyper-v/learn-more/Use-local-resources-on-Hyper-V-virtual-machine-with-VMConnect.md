---
title: Use local resources on Hyper-V virtual machine with VMConnect
description: Describe los requisitos para usar los recursos locales con VMConnect
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18eface5-7518-4c6b-9282-93e2e3e87492
author: KBDAzure
ms.author: kathyDav
ms.date: 12/06/2016
ms.openlocfilehash: a7e465313c68ee793715aba045cc56a2ca5fd1de
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222842"
---
# <a name="use-local-resources-on-hyper-v-virtual-machine-with-vmconnect"></a>Use local resources on Hyper-V virtual machine with VMConnect

>Se aplica a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2

Conexión a máquina virtual (VMConnect) le permite usar recursos locales de un equipo en una máquina virtual, como un extraíble unidad flash USB o una impresora. Modo de sesión mejorada también permite cambiar el tamaño de la ventana de VMConnect. En este artículo se muestra cómo configurar el host y, a continuación, permitir el acceso de máquina virtual a un recurso local.

Modo de sesión mejorada y escribir texto del Portapapeles solo están disponibles para máquinas virtuales que ejecutan sistemas operativos de Windows recientes. \(Consulte [requisitos para usar los recursos locales](#requirements-for-using-local-resources), a continuación.\) 

Para las máquinas virtuales que ejecutan Ubuntu, consulte [cambiar la resolución de pantalla de Ubuntu en una máquina virtual de Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/). 
  
## <a name="turn-on-enhanced-session-mode-on-a-hyper-v-host"></a>Activar el modo de sesión mejorada en un host de Hyper-V  
Si el host de Hyper-V ejecuta Windows 10 o Windows 8.1, modo de sesión mejorada es en forma predeterminada, por lo que puede omitir este paso y pasar a la sección siguiente. Pero si el host ejecuta Windows Server 2016 o Windows Server 2012 R2, hacerlo en primer lugar. 
  
Activar el modo de sesión mejorada:

1.  Conéctese al equipo que hospeda la máquina virtual.  
  
2.  En el Administrador de Hyper-V, seleccione el nombre del equipo del host.  
  
    ![Captura de pantalla que muestra un host de nombre de equipo aparecerá en Administrador de Hyper-V en el panel izquierdo.](media/Hyper-V-HyperVManager-HostNameSelected.png)  
  
3.  Seleccione **Configuración de Hyper-V**.  
  
    ![Captura de pantalla que muestra la opción de configuración de Hyper-V en acciones en el panel derecho.](media/HyperV-ActionsHyperVSettings.png)  
  
4.  En **Servidor**, seleccione **Directiva de modo de sesión mejorada**.  
  
    ![Captura de pantalla que muestra la opción de directiva de modo de sesión mejorada en la sección seguridad.](media/Hyper-V-Settings-ServerEnhancedSessionModePolicy.png)  
  
5.  Seleccione la casilla **Permitir modo de sesión mejorada** .  
  
    ![Captura de pantalla que muestra la permitir ha mejorado la casilla de verificación de modo de sesión de directiva de modo de sesión mejorada.](media/Hyper-V-Settings-EnhancedSessionModePolicyCheckBox.png)  
  
6.  En **Usuario**, seleccione **Modo de sesión mejorada**.  
  
    ![Captura de pantalla que muestra la opción de modo de sesión mejorada en la sección de usuario. ](media/Hyper-V-Settings-UserEnhancedSessionMode.png)  
  
7.  Seleccione la casilla **Permitir modo de sesión mejorada** .  
  
8.  Haga clic en **Aceptar**.  
  
## <a name="choose-a-local-resource"></a>Elija un recurso local

Los recursos locales incluyen impresoras, el Portapapeles y una unidad local en el equipo donde se ejecuta VMConnect. Para obtener más información, consulte [requisitos para usar los recursos locales](#requirements-for-using-local-resources), a continuación.  
  
Para elegir un recurso local:
  
1.  Abra VMConnect.  
  
2.  Seleccione la máquina virtual a la que desea conectarse.  
  
3.  Haga clic en **Mostrar opciones**.  
  
    ![Captura de pantalla que se indican las opciones de presentación en la parte inferior izquierda del cuadro de diálogo.](media/HyperV-VMConnect-DisplayConfig.png)  
  
4.  Seleccione **Recursos locales**.  
  
    ![Captura de pantalla que se llama a la ficha recursos locales.](media/HyperV-VMConnect-DisplayConfig-LocalResources.png)  
  
5.  Haga clic en **Más**.  
  
    ![Captura de pantalla que resalta el botón más.](media/HyperV-VMConnect-DisplayConfig-LocalResourcesMore.png)  
  
6.  Seleccione la unidad que desea usar en la máquina virtual y haga clic en **Aceptar**.  
  
    ![Captura de pantalla que muestra los recursos locales y las unidades que se pueden seleccionar.](media/HyperV-VMConnect-Settings-LocalResourcesDrives.png)  
  
7.  Seleccione **Guardar mi configuración para conexiones futuras en esta máquina virtual**.  
  
    ![Captura de pantalla que se llama a la casilla de verificación para seleccionar esta opción.](media/HyperV-VMConnect-SaveSettings.png)  
  
8.  Haga clic en **Conectar**.  
  
## <a name="edit-vmconnect-settings"></a>Editar la configuración de VMConnect

La configuración de conexión para VMConnect se puede editar fácilmente con el comando siguiente de Windows PowerShell o el símbolo del sistema:  
  
`VMConnect.exe <ServerName> <VMName> /edit`  
  
## <a name="requirements-for-using-local-resources"></a>Requisitos para usar los recursos locales

Para que pueda usar recursos locales de un equipo en una máquina virtual:  
  
-   El host de Hyper-V debe tener **directiva de modo de sesión mejorada** y **modo de sesión mejorada** configuración activa.  
  
-   El equipo en el que se usa VMConnect debe ejecutar Windows 10, Windows 8.1, Windows Server 2016 o Windows Server 2012 R2.  
  
-   La máquina virtual debe tener servicios de escritorio remoto habilitado y ejecutar Windows 10, Windows 8.1, Windows Server 2016 o Windows Server 2012 R2 como sistema operativo invitado.  
  
Si el equipo que ejecuta la máquina virtual y VMConnect ambos cumplen los requisitos, puede usar cualquiera de los siguientes recursos locales si están disponibles:  
  
-   Configuración de pantalla  
  
-   Audio
  
-   Impresoras  
  
-   Portapapeles para copiar y pegar  
  
-   Tarjetas inteligentes  
  
-   Dispositivos USB  
  
-   Unidades  
  
-   Dispositivos Plug and Play admitidos  
  
## <a name="why-use-a-computers-local-resources"></a>¿Por qué usar recursos locales de un equipo?
Puede ser conveniente usar recursos de un equipo local:  
  
-   Solucionar problemas en una máquina virtual sin una conexión de red a dicha máquina virtual.  
  
-   Copiar y pegar archivos en la máquina virtual de la misma manera que mediante una Conexión a Escritorio remoto (RDP).  
  
-   Iniciar sesión en la máquina virtual con una tarjeta inteligente.  
  
-   Imprimir desde una máquina virtual en una impresora local.  
  
-   Realizar pruebas y solucionar problemas en aplicaciones de desarrollador que requieren redireccionamiento de sonido y USB sin usar RDP.  
  
## <a name="see-also"></a>Vea también  
[Conectarse a una máquina Virtual](https://technet.microsoft.com/library/cc742407.aspx)  
[¿Debo crear una máquina virtual de generación 1 o 2 en Hyper-V?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)



