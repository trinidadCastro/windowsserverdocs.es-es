---
title: Crear un conmutador virtual para las máquinas virtuales de Hyper-V
description: Proporciona instrucciones sobre cómo crear un conmutador virtual mediante el Administrador de Hyper-V o Windows PowerShell
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: fdc8063c-47ce-4448-b445-d7ff9894dc17
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: e7c43a1b9173d347a3b6d6e1f8bd9127c62bd081
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880226"
---
# <a name="create-a-virtual-switch-for-hyper-v-virtual-machines"></a>Crear un conmutador virtual para las máquinas virtuales de Hyper-V

>Se aplica a: Windows 10, Windows Server 2016 y Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019
  
Un conmutador virtual permite que las máquinas virtuales creadas en hosts de Hyper-V para comunicarse con otros equipos. Puede crear un conmutador virtual cuando se instala primero el rol Hyper-V en Windows Server. Para crear conmutadores virtuales adicionales, use el Administrador de Hyper-V o Windows PowerShell. Para obtener más información acerca de los conmutadores virtuales, consulte [conmutador Virtual de Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
Redes de máquinas virtuales pueden ser un asunto complejo. Y hay varias características nuevas de conmutador virtual que desea usar como [Switch Embedded Teaming (SET)](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md#bkmk_sswitchembedded). Pero es bastante fácil de hacer la red básica. Este tema trata lo suficiente para que pueda crear máquinas virtuales en red de Hyper-V. Para obtener más información sobre cómo configurar la infraestructura de red, revise el [red](../../../networking/Networking.md) documentación.   
  
## <a name="BKMK_HyperVMan"></a>Crear un conmutador virtual mediante el Administrador de Hyper-V  
  
1.  Abra el Administrador de Hyper-V, seleccione el nombre del equipo de host de Hyper-V.  
  
2.  Seleccione **acción** > **Administrador de conmutadores virtuales**.  
  
    ![Captura de pantalla que muestra la opción de menú Acción > Administrador de conmutadores virtuales](../media/Hyper-V-Action-VSwitchManager.png)  
  
3.  Elija el tipo de conmutador virtual que desee.  
  
    |Tipo de conexión|Descripción|  
    |-------------------|---------------|  
    |External|Proporciona acceso a las máquinas virtuales a una red física para comunicarse con servidores y clientes en una red externa. Permite que las máquinas virtuales en el mismo servidor de Hyper-V para comunicarse entre sí.|  
    |Interno|Permite la comunicación entre máquinas virtuales en el mismo servidor de Hyper-V y entre las máquinas virtuales y el sistema operativo de host de administración.|  
    |Private|Solo permite la comunicación entre máquinas virtuales en el mismo servidor de Hyper-V. Una red privada se aísla de todo el tráfico de red externo en el servidor de Hyper-V. Este tipo de red es útil cuando se debe crear un entorno de red aislado, como un dominio de prueba aislada.|  
  
4.  Seleccione **crear conmutador Virtual**.  
  
5.  Agregar un nombre para el conmutador virtual.  
  
6.  Si selecciona externo, elija el adaptador de red (NIC) que desea usar y cualquier otra opción que se describe en la tabla siguiente.  
  
    ![Captura de pantalla que muestra las opciones de red externa](../media/Hyper-V-NewVSwitch-ExternalOptions.png)  
  
    |Nombre del valor de configuración|Descripción|  
    |----------------|---------------|  
    |Permitir que el sistema operativo de administración comparta este adaptador de red|Seleccione esta opción si desea permitir que el host de Hyper-V compartir el uso del conmutador virtual y NIC o NIC del equipo de la máquina virtual. Con esta opción habilitada, el host puede utilizar cualquiera de las opciones que configure para el conmutador virtual, como la configuración de calidad de servicio (QoS), configuración de seguridad u otras características del conmutador virtual de Hyper-V.|  
    |Habilitar virtualización de E/S de raíz única (SR-IOV)|Seleccione esta opción sólo si desea permitir el tráfico de máquina virtual omitir el conmutador de máquina virtual y vaya directamente a la NIC física. Para obtener más información, consulte [virtualización de E/S de raíz única](https://technet.microsoft.com/library/dn641211.aspx#Sec4) en la referencia de póster complementarias: Redes de Hyper-V.|  
  
7.  Si desea aísla el tráfico de red desde el sistema operativo de host de Hyper-V management o a otras máquinas virtuales que comparten el mismo conmutador virtual, seleccione **habilite la identificación de LAN virtual para el sistema operativo de administración**. Puede cambiar el identificador de VLAN a cualquier número o deje el valor predeterminado. Este es el número de identificación de LAN virtual que va a usar el sistema operativo de administración para todas las comunicaciones de red a través de este conmutador virtual.  
  
    ![Captura de pantalla que muestra las opciones de Id. de VLAN](../media/Hyper-V-NewSwitch-VLAN.png)  
  
8.  Haga clic en **Aceptar**.  
  
9. Haga clic en **Sí**.  
  
    ![Captura de pantalla que muestra el mensaje "Los cambios pendientes pueden afectar a la conectividad de red"](../media/Hyper-V-NewVSwitch-DisruptNetwork.png)  
  
## <a name="BKMK_WPS"></a>Crear un conmutador virtual con Windows PowerShell  
  
1.  En el Escritorio de Windows, haga clic en el botón Inicio y escriba cualquier parte del nombre **Windows PowerShell**.  
  
2.  Haga clic en Windows PowerShell y seleccione **ejecutar como administrador**.  
  
3.  Buscar los adaptadores de red existentes mediante la ejecución de la [Get-NetAdapter](https://technet.microsoft.com/library/jj130867.aspx) cmdlet. Tome nota del nombre del adaptador de red que desea usar para el conmutador virtual.  
  
    ```  
    Get-NetAdapter  
    ```  
  
4.  Crear un conmutador virtual mediante la [New-VMSwitch](https://technet.microsoft.com/library/hh848455.aspx) cmdlet. Por ejemplo, para crear un conmutador virtual externo denominado ExternalSwitch, mediante el adaptador de red ethernet y con **permitir que el sistema operativo de administración comparta este adaptador de red** activado, ejecute el siguiente comando.  
  
    ```  
    New-VMSwitch -name ExternalSwitch  -NetAdapterName Ethernet -AllowManagementOS $true  
    ```  
  
    Para crear un conmutador interno, ejecute el siguiente comando.  
  
    ```  
    New-VMSwitch -name InternalSwitch -SwitchType Internal  
    ```  
  
    Para crear un conmutador privado, ejecute el siguiente comando.  
  
    ```  
    New-VMSwitch -name PrivateSwitch -SwitchType Private  
    ```  
  
Para los scripts de Windows PowerShell más avanzados que se tratan las características de conmutador virtual nueva o mejorada en Windows Server 2016, consulte [remoto el acceso directo a memoria y cambiar Embedded Teaming](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  

  
## <a name="next-step"></a>Paso siguiente  
[Crear una máquina virtual de Hyper-V](Create-a-virtual-machine-in-Hyper-V.md)  
  


