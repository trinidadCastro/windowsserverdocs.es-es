---
title: Crear un conmutador virtual para máquinas virtuales de Hyper-V
description: Proporciona instrucciones sobre cómo crear un conmutador virtual con el administrador de Hyper-V o Windows PowerShell.
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
ms.openlocfilehash: 3c0ba19183dd68a86d995293f663accf10e91df9
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546390"
---
# <a name="create-a-virtual-switch-for-hyper-v-virtual-machines"></a>Crear un conmutador virtual para máquinas virtuales de Hyper-V

>Se aplica a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019
  
Un conmutador virtual permite que las máquinas virtuales creadas en hosts de Hyper-V se comuniquen con otros equipos. Puede crear un conmutador virtual la primera vez que instale el rol de Hyper-V en Windows Server. Para crear conmutadores virtuales adicionales, use el administrador de Hyper-V o Windows PowerShell. Para obtener más información acerca de los conmutadores virtuales, consulte [conmutador virtual de Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
Las redes de máquinas virtuales pueden ser un asunto complejo. Y hay varias características nuevas del conmutador virtual que puede usar como [Switch Embedded Teaming (Set)](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md#switch-embedded-teaming-set). Pero la red básica es bastante fácil de hacer. En este tema se trata lo suficiente para que pueda crear máquinas virtuales en red en Hyper-V. Para obtener más información acerca de cómo configurar la infraestructura de red, consulte la documentación de [red](../../../networking/Networking.md) .   
  
## <a name="create-a-virtual-switch-by-using-hyper-v-manager"></a>Crear un conmutador virtual con el administrador de Hyper-V  
  
1.  Abra el administrador de Hyper-V y seleccione el nombre del equipo host de Hyper-V.  
  
2.  Seleccione **Action** > **Virtual Switch Manager**.  
  
    ![Captura de pantalla que muestra la acción de la opción de menú > Administrador de conmutadores virtuales](../media/Hyper-V-Action-VSwitchManager.png)  
  
3.  Elija el tipo de conmutador virtual que desee.  
  
    |Tipo de conexión|Descripción|  
    |-------------------|---------------|  
    |Externo|Proporciona acceso a las máquinas virtuales a una red física para comunicarse con los servidores y los clientes de una red externa. Permite que las máquinas virtuales del mismo servidor de Hyper-V se comuniquen entre sí.|  
    |Interno|Permite la comunicación entre máquinas virtuales en el mismo servidor de Hyper-V y entre las máquinas virtuales y el sistema operativo del host de administración.|  
    |Private|Solo permite la comunicación entre máquinas virtuales en el mismo servidor de Hyper-V. Una red privada se aísla de todo el tráfico de red externo en el servidor de Hyper-V. Este tipo de red es útil cuando se debe crear un entorno de red aislado, como un dominio de prueba aislado.|  
  
4.  Seleccione **crear conmutador virtual**.  
  
5.  Agregue un nombre para el conmutador virtual.  
  
6.  Si selecciona externo, elija el adaptador de red (NIC) que desee usar y cualquier otra opción que se describe en la tabla siguiente.  
  
    ![Captura de pantalla que muestra las opciones de red externa](../media/Hyper-V-NewVSwitch-ExternalOptions.png)  
  
    |Nombre del valor de configuración|Descripción|  
    |----------------|---------------|  
    |Permitir que el sistema operativo de administración comparta este adaptador de red|Seleccione esta opción si desea permitir que el host de Hyper-V comparta el uso del conmutador virtual y el equipo NIC o NIC con la máquina virtual. Con esta opción habilitada, el host puede usar cualquiera de las opciones que configure para el conmutador virtual, como la configuración de calidad de servicio (QoS), la configuración de seguridad u otras características del conmutador virtual de Hyper-V.|  
    |Habilitar la virtualización de e/s de raíz única (SR-IOV)|Seleccione esta opción solo si desea permitir que el tráfico de la máquina virtual omita el conmutador de máquina virtual y vaya directamente a la NIC física. Para obtener más información, consulte virtualización de [e/s de raíz única](https://technet.microsoft.com/library/dn641211.aspx#Sec4) en la referencia complementaria del póster: Redes de Hyper-V.|  
  
7.  Si desea aislar el tráfico de red del sistema operativo del host de Hyper-V de administración o de otras máquinas virtuales que comparten el mismo conmutador virtual, seleccione **Habilitar la identificación de LAN virtual para el sistema operativo de administración**. Puede cambiar el identificador de VLAN a cualquier número o dejar el valor predeterminado. Este es el número de identificación de LAN virtual que usará el sistema operativo de administración para todas las comunicaciones de red a través de este conmutador virtual.  
  
    ![Captura de pantalla que muestra las opciones de ID. de VLAN](../media/Hyper-V-NewSwitch-VLAN.png)  
  
8.  Haga clic en **Aceptar**.  
  
9. Haga clic en **Sí**.  
  
    ![Captura de pantalla que muestra el mensaje "los cambios pendientes pueden interrumpir la conectividad de red"](../media/Hyper-V-NewVSwitch-DisruptNetwork.png)  
  
## <a name="create-a-virtual-switch-by-using-windows-powershell"></a>Crear un conmutador virtual mediante Windows PowerShell  
  
1.  En el Escritorio de Windows, haga clic en el botón Inicio y escriba cualquier parte del nombre **Windows PowerShell**.  
  
2.  Haga clic con el botón derecho en Windows PowerShell y seleccione **Ejecutar como administrador**.  
  
3.  Busque los adaptadores de red existentes mediante la ejecución del cmdlet [Get-NetAdapter](https://technet.microsoft.com/library/jj130867.aspx) . Anote el nombre del adaptador de red que desea usar para el conmutador virtual.  
  
    ```  
    Get-NetAdapter  
    ```  
  
4.  Cree un conmutador virtual con el cmdlet [New-VMSwitch](https://technet.microsoft.com/library/hh848455.aspx) . Por ejemplo, para crear un conmutador virtual externo denominado ExternalSwitch, con el adaptador de red Ethernet y con **permitir que el sistema operativo de administración comparta este adaptador de red** activado, ejecute el siguiente comando.  
  
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
  
Para obtener scripts de Windows PowerShell más avanzados que cubran las características mejoradas o nuevas de los conmutadores virtuales en Windows Server 2016, vea [acceso directo a memoria remota y cambiar la formación de equipos incrustados](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  

  
## <a name="next-step"></a>Paso siguiente  
[Crear una máquina virtual en Hyper-V](Create-a-virtual-machine-in-Hyper-V.md)  
  


