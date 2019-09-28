---
title: Optimización del rendimiento de puerta de enlace de HNV en redes definidas por software
description: Directrices para la optimización del rendimiento de puerta de enlace HNV en redes definidas por software
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 907b160b143af18a8ede3a9a7975fa8b22753118
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383497"
---
# <a name="hnv-gateway-performance-tuning-in-software-defined-networks"></a>Optimización del rendimiento de puerta de enlace de HNV en redes definidas por software

En este tema se proporcionan especificaciones de hardware y recomendaciones de configuración para servidores que ejecutan Hyper-V y hospedan máquinas virtuales de puerta de enlace de Windows Server, además de los parámetros de configuración para máquinas virtuales (VM) de puerta de enlace de Windows Server. . Para extraer el mejor rendimiento de las máquinas virtuales de puerta de enlace de Windows Server, se espera que se sigan estas instrucciones.
Las siguientes secciones contienen los requisitos de hardware y configuración para implementar la puerta de enlace de Windows Server.
1. Recomendaciones de hardware de Hyper-V
2. Configuración de host de Hyper-V
3. Configuración de VM de puerta de enlace de Windows Server

## <a name="hyper-v-hardware-recommendations"></a>Recomendaciones de hardware de Hyper-V

Esta es la configuración de hardware mínima recomendada para cada servidor que ejecute Windows Server 2016 e Hyper-V.

| Componente de servidor               | Especificación                                                                                                                                                                                                                                                                   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Unidad central de procesamiento (CPU)  | Nodos de arquitectura de memoria no uniforme (NUMA): 2 <br> Si hay varias máquinas virtuales de puerta de enlace de Windows Server en el host, para obtener el mejor rendimiento, cada máquina virtual de puerta de enlace debe tener acceso total a un nodo NUMA. Y debe ser diferente del nodo NUMA que usa el adaptador físico del host. |
| Núcleos por nodo NUMA            | 2                                                                                                                                                                                                                                                                               |
| Hyper-Threading                | Deshabilitado. La tecnología Hyper-Threading no impulsa el rendimiento de la puerta de enlace de Windows Server.                                                                                                                                                                                           |
| Memoria de acceso aleatorio (RAM)     | 48 GB                                                                                                                                                                                                                                                                           |
| Tarjetas de interfaz de red (NIC) | NIC de 2 10 GB, el rendimiento de la puerta de enlace dependerá de la velocidad de línea. Si la velocidad de línea es menor que 10 Gbps, los números de rendimiento del túnel de puerta de enlace también se desactivarán en el mismo factor.                                                                                          |

Procure que el número de procesadores virtuales asignados a una máquina virtual de puerta de enlace de Windows Server no supere el número de procesadores del nodo NUMA. Si el nodo NUMA tiene 8 núcleos, por ejemplo, el número de procesadores virtuales deberá ser inferior a 8. Para obtener el mejor rendimiento, debe ser 8. Para averiguar el número de nodos NUMA y el número de núcleos por cada nodo NUMA, ejecute el siguiente script de Windows PowerShell en cada host de Hyper-V:

```PowerShell
$nodes = [object[]] $(gwmi –Namespace root\virtualization\v2 -Class MSVM_NumaNode)
$cores = ($nodes | Measure-Object NumberOfProcessorCores -sum).Sum
$lps = ($nodes | Measure-Object NumberOfLogicalProcessors -sum).Sum


Write-Host "Number of NUMA Nodes: ", $nodes.count
Write-Host ("Total Number of Cores: ", $cores)
Write-Host ("Total Number of Logical Processors: ", $lps)
```

>[!Important]
> Asignar procesadores virtuales en los nodos NUMA puede repercutir negativamente en la puerta de enlace de Windows Server. Si se ejecutan varias máquinas virtuales (cada una con su correspondiente procesador virtual en un nodo NUMA), posiblemente se logre un mejor rendimiento que cuando haya una sola máquina virtual a la que se asignen todos los procesadores virtuales.

Se recomienda una máquina virtual de puerta de enlace con ocho procesadores virtuales y al menos 8 GB de RAM al seleccionar el número de máquinas virtuales de puerta de enlace que se van a instalar en cada host de Hyper-V cuando cada nodo NUMA tiene ocho núcleos. En este caso, se dedica un nodo NUMA al equipo host.

## <a name="hyper-v-host-configuration"></a>Configuración del host de Hyper-V

A continuación se encuentra la configuración recomendada para cada servidor que ejecuta Windows Server 2016 e Hyper-V y cuya carga de trabajo es ejecutar máquinas virtuales de puerta de enlace de Windows Server. Estas instrucciones de configuración conllevan el uso de ejemplos de comandos de Windows PowerShell. En dichos ejemplos verá marcadores de posición correspondientes a los valores reales que deberá especificar cuando ejecute los comandos en su entorno. Por ejemplo, los marcadores de posición de nombre del adaptador de red son "NIC1" y "NIC2". Cuando ejecute los comandos en los que se usan estos marcadores de posición, utilice los nombres reales de los adaptadores de red en los servidores y no los marcadores de posición o, de lo contrario, los comandos no funcionarán.

>[!Note]
> Para ejecutar los siguientes comandos de Windows PowerShell, debe ser miembro del grupo Administradores.

| Elemento de configuración                          | Configuración de Windows PowerShell                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Switch Embedded Teaming                     | Cuando se crea un vSwitch con varios adaptadores de red, se habilita automáticamente cambiar la formación de equipos incrustados para esos adaptadores. <br> ```New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"``` <br> No se admite la formación de equipos tradicionales a través de LBFO con SDN en Windows Server 2016. Switch Embedded Teaming permite usar el mismo conjunto de NIC para el tráfico virtual y el tráfico RDMA. Esto no era compatible con la formación de equipos NIC basada en LBFO.                                                        |
| Moderación de la interrupción en NIC físicas       | Use la configuración predeterminada. Para comprobar la configuración, puede usar el siguiente comando de Windows PowerShell: ```Get-NetAdapterAdvancedProperty```                                                                                                                                                                                                                                                                                                                                                                    |
| Tamaño de los búferes de recepción en NIC físicas       | Puede comprobar si las NIC físicas admiten la configuración de este parámetro mediante la ejecución del comando ```Get-NetAdapterAdvancedProperty```. Si no admiten este parámetro, el resultado del comando no incluirá la propiedad "búferes de recepción". Si lo admiten, puede usar el siguiente comando de Windows PowerShell para definir el tamaño de los búferes de recepción: <br>```Set-NetAdapterAdvancedProperty "NIC1" –DisplayName "Receive Buffers" –DisplayValue 3000``` <br>                          |
| Tamaño de los búferes de envío en NIC físicas          | Puede comprobar si las NIC físicas admiten la configuración de este parámetro mediante la ejecución del comando ```Get-NetAdapterAdvancedProperty```. Si las NIC no admiten este parámetro, el resultado del comando no incluirá la propiedad "búferes de envío". Si lo admiten, puede usar el siguiente comando de Windows PowerShell para definir el tamaño de los búferes de envío: <br> ```Set-NetAdapterAdvancedProperty "NIC1" –DisplayName "Transmit Buffers" –DisplayValue 3000``` <br>                           |
| Ajuste de escala en lado de recepción (RSS) en NIC físicas | Puede comprobar si las NIC físicas tienen RSS habilitado mediante la ejecución del comando de Windows PowerShell Get-NetAdapterRss. Puede usar los siguientes comandos de Windows PowerShell para habilitar y configurar RSS en los adaptadores de red: <br> ```Enable-NetAdapterRss "NIC1","NIC2"```<br> ```Set-NetAdapterRss "NIC1","NIC2" –NumberOfReceiveQueues 16 -MaxProcessors``` <br> Nota: Si VMMQ o VMQ está habilitado, RSS no tiene que estar habilitado en los adaptadores de red físicos. Puede habilitarlo en los adaptadores de red virtuales del host. |
| VMMQ                                        | Para habilitar VMMQ para una máquina virtual, ejecute el siguiente comando: <br> ```Set-VmNetworkAdapter -VMName <gateway vm name>,-VrssEnabled $true -VmmqEnabled $true``` <br> Nota: No todos los adaptadores de red admiten VMMQ. Actualmente, se admite en Chelsio T5 y T6, Mellanox CX-3 y CX-4, y en la serie QLogic 45xxx                                                                                                                                                                                                                                      |
| Virtual Machine Queue (VMQ) en el equipo NIC | Puede habilitar VMQ en el equipo del conjunto mediante el siguiente comando de Windows PowerShell: <br>```Enable-NetAdapterVmq``` <br> Nota: Solo se debe habilitar si el hardware no admite VMMQ. Si se admite, VMMQ debe estar habilitado para mejorar el rendimiento.                                                                                                                                                                                                                                                               |
>[!Note]
> VMQ y vRSS solo aparecen en la imagen cuando la carga de la máquina virtual es alta y la CPU se está usando hasta el máximo. Solo entonces, al menos un núcleo de procesador supera el límite. VMQ y vRSS serán útiles para repartir la carga de procesamiento en varios núcleos. Esto no es aplicable para el tráfico IPsec, ya que el tráfico IPsec se limita a un solo núcleo.

## <a name="windows-server-gateway-vm-configuration"></a>Configuración de máquinas virtuales de puerta de enlace de Windows Server

En ambos hosts de Hyper-V, puede configurar varias máquinas virtuales configuradas como puertas de enlace con la puerta de enlace de Windows Server. Puede usar el Administrador de conmutadores virtuales para crear un conmutador virtual de Hyper-V que esté enlazado al equipo NIC en el host de Hyper-V. Tenga en cuenta que, para obtener el mejor rendimiento, debe implementar una única máquina virtual de puerta de enlace en un host de Hyper-V.
Esta es la configuración recomendada para cada máquina virtual de puerta de enlace de Windows Server.

| Elemento de configuración                 | Configuración de Windows PowerShell                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Memoria                             | 8 GB                                                                                                                                                                                                                                                                                                                                                                                           |
| Número de adaptadores de red virtuales | 3 NIC con los siguientes usos específicos: 1 para la administración que usa el sistema operativo de administración, 1 externo que proporciona acceso a redes externas, 1 que es interno y que solo proporciona acceso a redes internas.                                                                                                                                                            |
| Receive Side Scaling (RSS)         | Puede mantener la configuración de RSS predeterminada para la NIC de administración. La siguiente configuración de ejemplo corresponde a una máquina virtual con 8 procesadores virtuales. En el caso de las NIC externas e internas, puede habilitar RSS con BaseProcNumber establecido en 0 y MaxRssProcessors establecido en 8 con el siguiente comando de Windows PowerShell: <br> ```Set-NetAdapterRss "Internal","External" –BaseProcNumber 0 –MaxProcessorNumber 8``` <br> |
| Búfer de envío                   | Puede mantener la configuración predeterminada del búfer de envío de la NIC de administración. En el caso de las NIC internas y externas, puede configurar el búfer de envío con 32 MB de RAM mediante el siguiente comando de Windows PowerShell: <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Send Buffer Size" –DisplayValue "32MB"``` <br>                                                       |
| Búfer de recepción                | Puede mantener la configuración predeterminada del búfer de recepción para la NIC de administración. En el caso de las NIC internas y externas, puede configurar el búfer de recepción con 16 MB de RAM mediante el siguiente comando de Windows PowerShell: <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Receive Buffer Size" –DisplayValue "16MB"``` <br>                                            |
| Optimización del reenvío               | Puede mantener la configuración de optimización de reenvío predeterminada para la NIC de administración. En el caso de las NIC internas y externas, puede habilitar la optimización hacia delante mediante el siguiente comando de Windows PowerShell: <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Forward Optimization" –DisplayValue "1"``` <br>                                                                      |
