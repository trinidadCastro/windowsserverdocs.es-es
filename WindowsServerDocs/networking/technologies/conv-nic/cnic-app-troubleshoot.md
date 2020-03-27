---
title: Solución de problemas de configuraciones de NIC convergentes
description: Este tema forma parte de la guía de configuración de NIC convergente para Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c20c21c39e44d7eb3da812bbe71f175d0688d6c0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309657"
---
# <a name="troubleshooting-converged-nic-configurations"></a>Solución de problemas de configuraciones de NIC convergentes

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar el siguiente script para comprobar si la configuración de RDMA es correcta en el host de Hyper-V.

- [Descargar script Test-Rdma. ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

También puede usar los siguientes comandos de Windows PowerShell para solucionar problemas y comprobar la configuración de las NIC convergentes.

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

Para comprobar la configuración de RDMA del adaptador de red, ejecute el siguiente comando de Windows PowerShell en el servidor de Hyper-V.

    
    Get-NetAdapterRdma | fl *
    

Puede usar los siguientes resultados esperados e inesperados para identificar y resolver los problemas después de ejecutar este comando en el host de Hyper-V.

### <a name="get-netadapterrdma-expected-results"></a>Resultados esperados de Get-NetAdapterRdma

Host vNIC y la NIC física muestran funcionalidades RDMA que no son cero.

![Resultados esperados de Windows PowerShell](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Resultados inesperados de Get-NetAdapterRdma

Siga estos pasos si recibe resultados inesperados al ejecutar el comando **Get-NetAdapterRdma** .

1. Asegúrese de que los controladores de Mlnx Miniport y Mlnx bus son los más recientes. Para Mellanox, use al menos 42. 
2. Compruebe que el minipuerto Mlnx y los controladores de bus coinciden comprobando la versión del controlador a través de Device Manager. El controlador de bus se puede encontrar en dispositivos del sistema. El nombre debe comenzar con la VPI de conexión Mellanox-X 3 PRO, como se muestra en la siguiente captura de pantalla de las propiedades del adaptador de red.

![Propiedades del adaptador de red](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. Asegúrese de que la red directa (RDMA) está habilitada en la NIC física y en el host vNIC.
5. Asegúrese de que se crea vSwitch en el adaptador físico correcto comprobando sus capacidades de RDMA.
6. Compruebe el registro del sistema EventViewer y filtre por el origen "Hyper-V-VmSwitch".

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Como paso adicional para comprobar la configuración de RDMA, ejecute el siguiente comando de Windows PowerShell en el servidor de Hyper-V.


    Get-SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>Resultados esperados de Get-SmbClientNetworkInterface

El host vNIC debe aparecer también como compatible con RDMA desde la perspectiva de SMB.

![Propiedades del adaptador de red](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Resultados inesperados de Get-SmbClientNetworkInterface

1. Asegúrese de que los controladores de Mlnx Miniport y Mlnx bus son los más recientes. Para Mellanox, use al menos 42. 
2. Compruebe que el minipuerto Mlnx y los controladores de bus coinciden comprobando la versión del controlador a través de Device Manager. El controlador de bus se puede encontrar en dispositivos del sistema. El nombre debe comenzar con la VPI de conexión Mellanox-X 3 PRO, como se muestra en la siguiente captura de pantalla de las propiedades del adaptador de red.
3. Asegúrese de que la red directa (RDMA) está habilitada en la NIC física y en el host vNIC.
4. Asegúrese de que el conmutador virtual de Hyper-V se crea a través del adaptador físico correcto; para ello, compruebe sus capacidades de RDMA.
5. Compruebe los registros de EventViewer para "cliente SMB" en la **aplicación y los servicios | Microsoft | Windows**.

--- 

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

Puede ver la configuración de calidad de servicio \(QoS del adaptador de red\) mediante la ejecución del siguiente comando de Windows PowerShell.

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Resultados esperados de Get-NetAdapterQos

Las prioridades y las clases de tráfico deben mostrarse según el primer paso de configuración que haya realizado con esta guía.

![Clases y prioridades de calidad de servicio](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Resultados inesperados de Get-NetAdapterQos

Si los resultados son inesperados, realice los pasos siguientes.

1. Asegúrese de que el adaptador de red físico admite el protocolo de puente del centro de datos \(DCB\) y QoS
2. Asegúrese de que los controladores del adaptador de red estén actualizados.

--- 

## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

Puede usar el siguiente comando de Windows PowerShell para comprobar que la dirección IP del nodo remoto es RDMA\-compatible.

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Resultados esperados de Get-SmbMultiChannelConnection

La dirección IP del nodo remoto se muestra como compatible con RDMA.

![Dirección IP del nodo remoto compatible con RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Resultados inesperados de Get-SmbMultiChannelConnection

Si los resultados son inesperados, realice los pasos siguientes.

1. Asegúrese de que el comando ping funciona de ambas maneras.
2. Asegúrese de que el Firewall no está bloqueando la iniciación de la conexión SMB. En concreto, habilite la regla de Firewall para el puerto SMB directo 5445 para iWARP y 445 para ROCE.

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Puede usar el siguiente comando para comprobar que la NIC virtual que habilitó para RDMA se notifique como RDMA\-compatible con SMB.

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Resultados esperados de Get-SmbClientNetworkInterface

La NIC virtual que se habilitó para RDMA debe verse como compatible con RDMA por SMB.

![Informes SMB que son compatibles con RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Resultados inesperados de Get-SmbClientNetworkInterface

Si los resultados son inesperados, realice los pasos siguientes.

1. Asegúrese de que el comando ping funciona de ambas maneras.
2. Asegúrese de que el Firewall no bloquea el inicio de la conexión SMB.

--- 

## <a name="vstat-mellanox-specific"></a>vstat \(\) de Mellanox específico

Si usa adaptadores de red Mellanox, puede usar el comando **vstat** para comprobar la versión de RDMA sobre Ethernet convergente \(RoCE\) en los nodos de Hyper-V.

### <a name="vstat-expected-results"></a>resultados esperados de vstat

La versión de RoCE en ambos nodos debe ser la misma. También es una buena manera de comprobar que la versión de firmware en ambos nodos es la más reciente.

![Ejemplos de resultados de comprobación de versión de RoCE](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>resultados inesperados de vstat

Si los resultados son inesperados, realice los pasos siguientes.

1. Establecer la versión correcta de RoCE mediante Set-MlnxDriverCoreSetting
2. Instale el firmware más reciente desde el sitio web de Mellanox.

--- 

## <a name="perfmon-counters"></a>Contadores de Perfmon

Puede revisar los contadores del monitor de rendimiento para comprobar la actividad RDMA de la configuración.

![Ejemplos de resultados del monitor de rendimiento](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

--- 

## <a name="related-topics"></a>Temas relacionados

- [Configuración de NIC convergente con un solo adaptador de red](cnic-single.md)
- [Configuración de NIC en equipo NIC convergente](cnic-datacenter.md)
- [Configuración del conmutador físico para la NIC convergente](cnic-app-switch-config.md)

---
