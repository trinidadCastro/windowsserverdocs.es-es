---
title: Solución de problemas convergente configuraciones de NIC
description: En este tema forma parte de convergente NIC Configuration Guide para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3004ff9d6fe874410c24d174755a6d26f99f8f2a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876486"
---
# <a name="troubleshooting-converged-nic-configurations"></a>Solución de problemas convergente configuraciones de NIC

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar el siguiente script para comprobar si la configuración de RDMA es correcta en el host de Hyper-V.

- [Descargar el script de prueba Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

También puede usar los siguientes comandos de Windows PowerShell para solucionar problemas y comprobar la configuración de las NIC convergentes.

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

Para comprobar la configuración de RDMA de adaptador de red, ejecute el siguiente comando de Windows PowerShell en el servidor de Hyper-V.

    
    Get-NetAdapterRdma | fl *
    

Puede usar para identificar y resolver problemas después de ejecutar este comando en el host de Hyper-V siguiente esperadas y resultados inesperados.

### <a name="get-netadapterrdma-expected-results"></a>Resultados de Get-NetAdapterRdma esperado

VNIC de host y la NIC física muestran las funcionalidades de RDMA distinto de cero.

![Resultados de la espera de PowerShell de Windows](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Get-NetAdapterRdma resultados inesperados

Realice los pasos siguientes si recibe resultados inesperados al ejecutar el **Get NetAdapterRdma** comando.

1. Asegúrese de que los controladores de bus de Mlnx y minipuerto Mlnx son más recientes. Para Mellanox, use drop al menos 42. 
2. Compruebe que coinciden con los controladores de minipuerto y bus Mlnx comprobando la versión del controlador a través del Administrador de dispositivos. El controlador de bus puede encontrarse en los dispositivos del sistema. El nombre debe comenzar con Mellanox Connect-X 3 PRO VPI, como se muestra en la siguiente captura de pantalla de propiedades del adaptador de red.

![Propiedades del adaptador de red](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. Asegúrese de que la red directo (RDMA) está habilitada en la NIC física y la vNIC de host.
5. Asegúrese de que se crea un vSwitch a través del adaptador físico derecha comprobando sus capacidades RDMA.
6. Compruebe el registro del sistema del Visor de eventos y filtrar por origen "Hyper-V-VmSwitch".

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Como paso adicional para comprobar la configuración de RDMA, ejecute el siguiente comando de Windows PowerShell en el servidor de Hyper-V.


    Get-SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>Resultados de Get-SmbClientNetworkInterface esperado

La vNIC de host debe aparecer como compatible con RDMA desde la perspectiva de SMB también.

![Propiedades del adaptador de red](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface resultados inesperados

1. Asegúrese de que los controladores de bus de Mlnx y minipuerto Mlnx son más recientes. Para Mellanox, use drop al menos 42. 
2. Compruebe que coinciden con los controladores de minipuerto y bus Mlnx comprobando la versión del controlador a través del Administrador de dispositivos. El controlador de bus puede encontrarse en los dispositivos del sistema. El nombre debe comenzar con Mellanox Connect-X 3 PRO VPI, como se muestra en la siguiente captura de pantalla de propiedades del adaptador de red.
3. Asegúrese de que la red directo (RDMA) está habilitada en la NIC física y la vNIC de host.
4. Asegúrese de que se crea el conmutador Virtual de Hyper-V a través del adaptador físico directamente mediante la comprobación de sus capacidades RDMA.
5. Compruebe los registros del Visor de eventos de "Cliente de SMB" en **de aplicaciones y servicios | Microsoft | Windows**.

--- 

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

Puede ver la calidad del adaptador de red del servicio \(QoS\) configuración ejecutando el siguiente comando de Windows PowerShell.

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Resultados de Get-NetAdapterQos esperado

Las prioridades y clases de tráfico deben mostrarse según el primer paso de configuración que realiza mediante esta guía.

![Calidad de las prioridades del servicio y clases](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-NetAdapterQos resultados inesperados

Si los resultados son los esperados, realice los pasos siguientes.

1. Asegúrese de que el adaptador de red físico admite el protocolo de puente del centro de datos \(DCB\) y calidad de servicio
2. Asegúrese de que los controladores de adaptador de red están actualizados.

--- 

## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

Puede usar el siguiente comando de Windows PowerShell para comprobar que la dirección IP de nodo remoto es RDMA\-capaz.

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Resultados de Get-SmbMultiChannelConnection esperado

Dirección IP del nodo remoto se muestra como RDMA capaz.

![Dirección IP de nodo remoto compatibles con RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Get-SmbMultiChannelConnection resultados inesperados

Si los resultados son los esperados, realice los pasos siguientes.

1. Asegúrese de que el comando ping funciona en ambas direcciones.
2. Asegúrese de que el firewall no está bloqueando la iniciación de la conexión SMB. En concreto, habilite la regla de firewall para el puerto SMB directo 5445 para iWARP y 445 para ROCE.

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Puede usar el siguiente comando para comprobar que la NIC virtual habilitado para RDMA se notifica como RDMA\-capaz por SMB.

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Resultados de Get-SmbClientNetworkInterface esperado

NIC virtual habilitada para RDMA debe considerarse como compatible con RDMA mediante SMB.

![SMB informa de que las NIC son compatibles con RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface resultados inesperados

Si los resultados son los esperados, realice los pasos siguientes.

1. Asegúrese de que el comando ping funciona en ambas direcciones.
2. Asegúrese de que el firewall no bloquea el inicio de la conexión de SMB.

--- 

## <a name="vstat-mellanox-specific"></a>vstat \(Mellanox específico\)

Si usa adaptadores de red Mellanox, puede usar el **vstat** comando para comprobar el RDMA sobre Ethernet convergente \(RoCE\) versión en nodos de Hyper-V.

### <a name="vstat-expected-results"></a>resultados de vstat esperado

La versión de RoCE en ambos nodos debe ser el mismo. También es una buena forma de comprobar que la versión de firmware en ambos nodos es más reciente.

![Ejemplos de resultados de comprobación de versión de RoCE](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>vstat resultados inesperados

Si los resultados son los esperados, realice los pasos siguientes.

1. Establezca la versión de RoCE correcta mediante Set-MlnxDriverCoreSetting
2. Instale el firmware más reciente desde el sitio Web de Mellanox.

--- 

## <a name="perfmon-counters"></a>Contadores de rendimiento

Puede revisar los contadores de Monitor de rendimiento para comprobar la actividad RDMA de la configuración.

![Ejemplos de resultados del monitor de rendimiento](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

--- 

## <a name="related-topics"></a>Temas relacionados

- [Configuración de NIC convergentes con un único adaptador de red](cnic-single.md)
- [Configuración de NIC convergente NIC asociadas](cnic-datacenter.md)
- [Configuración del conmutador físico de NIC convergente](cnic-app-switch-config.md)

---
