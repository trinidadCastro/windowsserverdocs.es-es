---
title: Crear un conmutador virtual para las máquinas virtuales de Hyper-V
description: Proporciona instrucciones sobre cómo crear un conmutador virtual con el administrador de Hyper-V o Windows PowerShell.
ms.topic: how-to
ms.assetid: fdc8063c-47ce-4448-b445-d7ff9894dc17
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/04/2016
ms.openlocfilehash: da1082a90f5d028ae6be0581245d298f3a4c00d0
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946901"
---
# <a name="create-a-virtual-switch-for-hyper-v-virtual-machines"></a>Crear un conmutador virtual para las máquinas virtuales de Hyper-V

> Se aplica a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Un conmutador virtual permite que las máquinas virtuales creadas en hosts de Hyper-V se comuniquen con otros equipos. Puede crear un conmutador virtual la primera vez que instale el rol de Hyper-V en Windows Server. Para crear conmutadores virtuales adicionales, use el administrador de Hyper-V o Windows PowerShell. Para obtener más información acerca de los conmutadores virtuales, consulte [Conmutador virtual de Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

Las redes de máquinas virtuales pueden ser un asunto complejo. Y hay varias características nuevas del conmutador virtual que puede usar como [Switch Embedded Teaming (Set)](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md#switch-embedded-teaming-set). Pero la red básica es bastante fácil de hacer. En este tema se trata lo suficiente para que pueda crear máquinas virtuales en red en Hyper-V. Para obtener más información acerca de cómo configurar la infraestructura de red, consulte la documentación de [red](../../../networking/index.yml) .

## <a name="create-a-virtual-switch-by-using-hyper-v-manager"></a>Crear un conmutador virtual con el administrador de Hyper-V

1. Abra el administrador de Hyper-V y seleccione el nombre del equipo host de Hyper-V.

2. Seleccione **Action**  >  **Virtual Switch Manager**.

    ![Captura de pantalla que muestra la acción de la opción de menú > administrador de conmutadores virtuales](../media/Hyper-V-Action-VSwitchManager.png)

3. Elija el tipo de conmutador virtual que desee.

    | Tipo de conexión | Descripción |
    | --------------- | ----------- |
    |     Externo    | Proporciona acceso a las máquinas virtuales a una red física para comunicarse con los servidores y los clientes de una red externa. Permite que las máquinas virtuales del mismo servidor de Hyper-V se comuniquen entre sí. |
    |     Interno    | Permite la comunicación entre máquinas virtuales en el mismo servidor de Hyper-V y entre las máquinas virtuales y el sistema operativo del host de administración. |
    |     Privada     | Solo permite la comunicación entre máquinas virtuales en el mismo servidor de Hyper-V. Una red privada se aísla de todo el tráfico de red externo en el servidor de Hyper-V. Este tipo de red es útil cuando se debe crear un entorno de red aislado, como un dominio de prueba aislado. |

4. Seleccione **crear conmutador virtual**.

5. Agregue un nombre para el conmutador virtual.

6. Si selecciona externo, elija el adaptador de red (NIC) que desee usar y cualquier otra opción que se describe en la tabla siguiente.

    ![Captura de pantalla que muestra las opciones de red externa](../media/Hyper-V-NewVSwitch-ExternalOptions.png)

    | Nombre del valor | Descripción |
    | ------------ | ----------- |
    | Permitir que el sistema operativo de administración comparta este adaptador de red | Seleccione esta opción si desea permitir que el host de Hyper-V comparta el uso del conmutador virtual y el equipo NIC o NIC con la máquina virtual. Con esta opción habilitada, el host puede usar cualquiera de las opciones que configure para el conmutador virtual, como la configuración de calidad de servicio (QoS), la configuración de seguridad u otras características del conmutador virtual de Hyper-V. |
    | Habilitar virtualización de E/S de raíz única (SR-IOV) | Seleccione esta opción solo si desea permitir que el tráfico de la máquina virtual omita el conmutador de máquina virtual y vaya directamente a la NIC física. Para obtener más información, consulte [virtualización de e/s de raíz única](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn641211(v=ws.11)#Sec4) en la referencia complementaria de póster: redes de Hyper-V. |

7. Si desea aislar el tráfico de red del sistema operativo del host de Hyper-V de administración o de otras máquinas virtuales que comparten el mismo conmutador virtual, seleccione **Habilitar la identificación de LAN virtual para el sistema operativo de administración**. Puede cambiar el identificador de VLAN a cualquier número o dejar el valor predeterminado. Este es el número de identificación de LAN virtual que usará el sistema operativo de administración para todas las comunicaciones de red a través de este conmutador virtual.

    ![Captura de pantalla que muestra las opciones de ID. de VLAN](../media/Hyper-V-NewSwitch-VLAN.png)

8. Haga clic en **Aceptar**.

9. Haga clic en **Sí**.

    ![Captura de pantalla que muestra el mensaje "los cambios pendientes pueden interrumpir la conectividad de red"](../media/Hyper-V-NewVSwitch-DisruptNetwork.png)

## <a name="create-a-virtual-switch-by-using-windows-powershell"></a>Crear un conmutador virtual mediante Windows PowerShell

1. En el Escritorio de Windows, haga clic en el botón Inicio y escriba cualquier parte del nombre **Windows PowerShell**.

2. Haga clic con el botón derecho en Windows PowerShell y seleccione **Ejecutar como administrador**.

3. Busque los adaptadores de red existentes mediante la ejecución del cmdlet [Get-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) . Anote el nombre del adaptador de red que desea usar para el conmutador virtual.

    ```PowerShell
    Get-NetAdapter
    ```

4. Cree un conmutador virtual con el cmdlet [New-VMSwitch](https://docs.microsoft.com/powershell/module/hyper-v/new-vmswitch) . Por ejemplo, para crear un conmutador virtual externo denominado ExternalSwitch, con el adaptador de red Ethernet y con **permitir que el sistema operativo de administración comparta este adaptador de red** activado, ejecute el siguiente comando.

    ```PowerShell
    New-VMSwitch -name ExternalSwitch  -NetAdapterName Ethernet -AllowManagementOS $true
    ```

    Para crear un conmutador interno, ejecute el siguiente comando.

    ```PowerShell
    New-VMSwitch -name InternalSwitch -SwitchType Internal
    ```

    Para crear un conmutador privado, ejecute el siguiente comando.

    ```PowerShell
    New-VMSwitch -name PrivateSwitch -SwitchType Private
    ```

Para obtener scripts de Windows PowerShell más avanzados que cubran las características mejoradas o nuevas de los conmutadores virtuales en Windows Server 2016, vea [acceso directo a memoria remota y cambiar la formación de equipos incrustados](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).


## <a name="next-step"></a>Paso siguiente

[Crear una máquina virtual en Hyper-V](Create-a-virtual-machine-in-Hyper-V.md)
