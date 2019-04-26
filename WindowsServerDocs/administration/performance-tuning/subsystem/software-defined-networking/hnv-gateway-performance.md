---
title: Rendimiento de la puerta de enlace HNV en Software de optimización de las redes definidas
description: Rendimiento de la puerta de enlace HNV directrices en redes de Software definidos para la optimización
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 217428b84a00b2e2231a15cb3878d0abcec1d9ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827326"
---
# <a name="hnv-gateway-performance-tuning-in-software-defined-networks"></a>Rendimiento de la puerta de enlace HNV en Software de optimización de las redes definidas

Este tema proporcionan las especificaciones de hardware y recomendaciones de configuración para servidores que ejecutan Hyper-V y hospeda las máquinas virtuales de puerta de enlace de Windows Server, además de los parámetros de configuración para las máquinas virtuales de puerta de enlace de Windows Server (VM) . Para extraer el mejor rendimiento de las máquinas virtuales de puerta de enlace de Windows Server, se espera que se seguirán estas directrices.
Las siguientes secciones contienen los requisitos de hardware y configuración para implementar la puerta de enlace de Windows Server.
1. Recomendaciones de hardware de Hyper-V
2. Configuración de host de Hyper-V
3. Configuración de máquina virtual de puerta de enlace de Windows Server

## <a name="hyper-v-hardware-recommendations"></a>Recomendaciones de hardware de Hyper-V

Esta es la configuración de hardware mínima recomendada para cada servidor que ejecuta Hyper-V y Windows Server 2016.

| Componente de servidor               | Especificación                                                                                                                                                                                                                                                                   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Unidad central de procesamiento (CPU)  | Nodos de arquitectura de memoria no uniforme (NUMA): 2 <br> Si hay varios Windows Server puerta de enlace de máquinas virtuales en el host, para obtener el mejor rendimiento, cada máquina virtual de puerta de enlace debe tener acceso total a un nodo NUMA. Y debe ser diferente del nodo NUMA utilizado por el adaptador físico del host. |
| Núcleos por nodo NUMA            | 2                                                                                                                                                                                                                                                                               |
| Hyper-Threading                | Deshabilitado. La tecnología Hyper-Threading no impulsa el rendimiento de la puerta de enlace de Windows Server.                                                                                                                                                                                           |
| Memoria de acceso aleatorio (RAM)     | 48 GB                                                                                                                                                                                                                                                                           |
| Tarjetas de interfaz de red (NIC) | Dos NIC de 10 GB, el rendimiento de la puerta de enlace depende de la velocidad de línea. Si la velocidad de línea es inferior a 10 Gbps, las cifras de rendimiento de túnel de puerta de enlace se también dejan de funcionar por el mismo factor.                                                                                          |

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

Una puerta de enlace de máquina virtual con ocho procesadores virtuales y al menos 8GB de RAM se recomienda cuando se selecciona el número de máquinas virtuales de puerta de enlace para instalar en cada host de Hyper-V cuando cada nodo NUMA tiene ocho núcleos. En este caso, un nodo NUMA está dedicado a la máquina host.

## <a name="hyper-v-host-configuration"></a>Configuración del Host de Hyper-V

Esta es la configuración recomendada para cada servidor que ejecuta Windows Server 2016 y Hyper-V y cuya carga de trabajo consiste en ejecutar máquinas virtuales de puerta de enlace de Windows Server. Estas instrucciones de configuración conllevan el uso de ejemplos de comandos de Windows PowerShell. En dichos ejemplos verá marcadores de posición correspondientes a los valores reales que deberá especificar cuando ejecute los comandos en su entorno. ¿Por ejemplo, los marcadores de posición del nombre de adaptador de red son "NIC1? ¿y "NIC2.? Cuando ejecute los comandos en los que se usan estos marcadores de posición, utilice los nombres reales de los adaptadores de red en los servidores y no los marcadores de posición o, de lo contrario, los comandos no funcionarán.

>[!Note]
> Para ejecutar los siguientes comandos de Windows PowerShell, debe ser miembro del grupo Administradores.

| Elemento de configuración                          | Configuración de Windows Powershell                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Switch Embedded Teaming                     | Cuando se crea un vswitch con varios adaptadores de red, habilita automáticamente switch embedded formación de equipos para dichos adaptadores. <br> ```New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"``` <br> No se admite la formación de equipos tradicionales a través de LBFO con SDN en Windows Server 2016. Switch Embedded Teaming le permite usar el mismo conjunto de tarjetas NIC para el tráfico virtual y el tráfico de RDMA. Esto no se admite con la formación de equipos de NIC en función de LBFO.                                                        |
| Moderación de la interrupción en NIC físicas       | Use la configuración predeterminada. Para comprobar la configuración, puede usar el siguiente comando de Windows PowerShell: ```Get-NetAdapterAdvancedProperty```                                                                                                                                                                                                                                                                                                                                                                    |
| Tamaño de los búferes de recepción en NIC físicas       | Puede comprobar si las NIC físicas admiten la configuración de este parámetro, ejecute el comando ```Get-NetAdapterAdvancedProperty```. ¿Si no admite este parámetro, la salida del comando no incluye la propiedad "búferes de recepción.? Si lo admiten, puede usar el siguiente comando de Windows PowerShell para definir el tamaño de los búferes de recepción: <br>```Set-NetAdapterAdvancedProperty “NIC1�? –DisplayName “Receive Buffers�? –DisplayValue 3000``` <br>                          |
| Tamaño de los búferes de envío en NIC físicas          | Puede comprobar si las NIC físicas admiten la configuración de este parámetro, ejecute el comando ```Get-NetAdapterAdvancedProperty```. ¿Si las NIC no admiten este parámetro, la salida del comando no incluye la propiedad "búferes de envío.? Si lo admiten, puede usar el siguiente comando de Windows PowerShell para definir el tamaño de los búferes de envío: <br> ```Set-NetAdapterAdvancedProperty “NIC1�? –DisplayName “Transmit Buffers�? –DisplayValue 3000``` <br>                           |
| Ajuste de escala en lado de recepción (RSS) en NIC físicas | Puede comprobar si las NIC físicas tienen RSS habilitado, ejecute el comando de Windows PowerShell Get-NetAdapterRss. Puede usar los siguientes comandos de Windows PowerShell para habilitar y configurar RSS en los adaptadores de red: <br> ```Enable-NetAdapterRss “NIC1�?,�?NIC2�?```<br> ```Set-NetAdapterRss “NIC1�?,�?NIC2�? –NumberOfReceiveQueues 16 -MaxProcessors``` <br> Nota: Si está habilitado VMMQ o VMQ, RSS no tiene esté habilitado en los adaptadores de red físico. Puede habilitarla en los adaptadores de red virtual de host |
| VMMQ                                        | Para habilitar VMMQ para una máquina virtual, ejecute el siguiente comando: <br> ```Set-VmNetworkAdapter -VMName <gateway vm name>,-VrssEnabled $true -VmmqEnabled $true``` <br> Nota: No todos los adaptadores de red admiten VMMQ. Actualmente, se admite en serie 45xxx Chelsio T5 y T6, Mellanox CX-3 y 4 de CX y QLogic                                                                                                                                                                                                                                      |
| Virtual Machine Queue (VMQ) en el equipo NIC | Puede habilitar VMQ en el equipo conjunto mediante el siguiente comando de Windows PowerShell: <br>```Enable-NetAdapterVmq``` <br> Nota: Esta opción debe habilitarse solo si el hardware no admite VMMQ. Si se admite, VMMQ debe habilitarse para mejorar el rendimiento.                                                                                                                                                                                                                                                               |
>[!Note]
> VMQ y vRSS entran en imagen solo cuando la carga en la máquina virtual es alta y la CPU se está usando el máximo. Solo entonces se al menos un procesador principales de max out. VMQ y vRSS, a continuación, será beneficiosa ayudar a distribuir la carga de procesamiento entre varios núcleos. Esto no es aplicable para el tráfico IPsec como el tráfico IPsec se limita a un único núcleo.

## <a name="windows-server-gateway-vm-configuration"></a>Configuración de máquinas virtuales de puerta de enlace de Windows Server

En ambos hosts de Hyper-V, puede configurar varias máquinas virtuales que se configuran como puertas de enlace con la puerta de enlace de Windows Server. Puede usar el Administrador de conmutadores virtuales para crear un conmutador virtual de Hyper-V que esté enlazado al equipo NIC en el host de Hyper-V. Tenga en cuenta que para un mejor rendimiento, debe implementar una puerta de enlace única máquina virtual en un host de Hyper-V.
Esta es la configuración recomendada para cada máquina virtual de puerta de enlace de Windows Server.

| Elemento de configuración                 | Configuración de Windows Powershell                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Memoria                             | 8 GB                                                                                                                                                                                                                                                                                                                                                                                           |
| Número de adaptadores de red virtuales | Utiliza 3 NIC con el siguiente valor concreto: 1 de administración que se usa el sistema operativo de administración, 1 externo que proporciona acceso a redes externas, 1 externo que proporciona acceso a las redes internas únicamente.                                                                                                                                                            |
| Receive Side Scaling (RSS)         | Puede mantener el valor predeterminado de configuración de RSS para la NIC de administración. La siguiente configuración de ejemplo corresponde a una máquina virtual con 8 procesadores virtuales. Para las NIC externa e interna, puede habilitar RSS con BaseProcNumber establecido en 0 y MaxRssProcessors establecido en 8 mediante el siguiente comando de Windows PowerShell: <br> ```Set-NetAdapterRss “Internal�?,�?External�? –BaseProcNumber 0 –MaxProcessorNumber 8``` <br> |
| Búfer de envío                   | Puede mantener el valor predeterminado de configuración de búfer de envío para la NIC de administración. Para las NIC externa e interna puede configurar el búfer de envío con 32 MB de RAM mediante el comando de Windows PowerShell siguiente: <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Send Buffer Size�? –DisplayValue “32MB�?``` <br>                                                       |
| Búfer de recepción                | Puede mantener el valor predeterminado de configuración de búfer de recepción para la NIC de administración. Para el interno y externo NIC, puede configurar el búfer de recepción con 16 MB de RAM mediante el comando de Windows PowerShell siguiente: <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Receive Buffer Size�? –DisplayValue “16MB�?``` <br>                                            |
| Optimización del reenvío               | Puede mantener el valor predeterminado de configuración de optimización del reenvío de la NIC de administración. Para el interno y externo NIC, puede habilitar la optimización del reenvío utilizando el siguiente comando de Windows PowerShell: <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Forward Optimization�? –DisplayValue “1�?``` <br>                                                                      |
