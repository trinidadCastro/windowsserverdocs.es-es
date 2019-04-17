---
title: Solución de problemas convergido configuraciones NIC
description: Este tema es parte de la convergido NIC configuración guía para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 373ecd9b9fff62aaabd8caa176ff091ec98ad81c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshooting-converged-nic-configurations"></a>Solución de problemas convergido configuraciones NIC

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar el siguiente script para comprobar si la configuración de RDMA es correcta en el host de Hyper-V.

- [Descargar el script Rdma.ps1 de prueba](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

También puedes usar los siguientes comandos de Windows PowerShell para solucionar problemas y comprueba la configuración de las NIC convergentes.

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

Para comprobar la configuración de RDMA del adaptador de red, ejecuta el siguiente comando de Windows PowerShell en el servidor de Hyper-V.

    
    Get-NetAdapterRdma | fl *
    

Puedes usar la siguiente lo esperado y resultados inesperados para identificar y resolver problemas después de ejecutar este comando en el host de Hyper-V.

### <a name="get-netadapterrdma-expected-results"></a>Resultados de Get-NetAdapterRdma esperado

VNIC host y el NIC física muestran las funcionalidades RDMA distinto de cero.

![Resultados de Windows PowerShell se esperaba](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Get-NetAdapterRdma resultados inesperados

Realiza los siguientes pasos si recibes resultados inesperados al ejecutar el **Get NetAdapterRdma** comando.

1. Asegúrate de que el minipuerto Mlnx y los controladores de bus Mlnx son más recientes. Para Mellanox, usa colocar al menos 42. 
2. Comprueba que coincidan con los controladores de minipuerto y bus Mlnx comprobando la versión del controlador mediante el Administrador de dispositivos. El controlador de bus puede encontrarse en los dispositivos del sistema. El nombre debe iniciarse con Mellanox conectar X 3 PRO VPI, como se muestra en la siguiente captura de pantalla de propiedades del adaptador de red.

![Propiedades del adaptador de red](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. Asegúrese de red directa (RDMA) está habilitada en el NIC física y el host vNIC.
5. Asegúrate de que se crea vSwitch sobre el adaptador derecha físico comprobando sus funcionalidades RDMA.
6. Comprueba el registro del sistema efentos y filtrar por origen "Hyper-V-VmSwitch".

## <a name="get-smbclientnetworkinterface"></a>Obtener SmbClientNetworkInterface

Como paso adicional para verificar la configuración de RDMA, ejecuta el siguiente comando de Windows PowerShell en el servidor de Hyper-V.


    Get SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>Obtener resultados SmbClientNetworkInterface esperado

La vNIC host debe aparecer como RDMA capaz desde la perspectiva SMB así.

![Propiedades del adaptador de red](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Obtener SmbClientNetworkInterface resultados inesperados

1. Asegúrate de que el minipuerto Mlnx y los controladores de bus Mlnx son más recientes. Para Mellanox, usa colocar al menos 42. 
2. Comprueba que coincidan con los controladores de minipuerto y bus Mlnx comprobando la versión del controlador mediante el Administrador de dispositivos. El controlador de bus puede encontrarse en los dispositivos del sistema. El nombre debe iniciarse con Mellanox conectar X 3 PRO VPI, como se muestra en la siguiente captura de pantalla de propiedades del adaptador de red.
3. Asegúrese de red directa (RDMA) está habilitada en el NIC física y el host vNIC.
4. Asegúrate de que el modificador virtuales de Hyper-V se crea sobre el adaptador derecha físico comprobando sus funcionalidades RDMA.
5. Comprobar registros efentos "Cliente SMB" en **de aplicaciones y servicios | Microsoft | Windows**.

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

Puedes ver la calidad de adaptador de red de configuración del servicio \(QoS\) ejecutando el siguiente comando de Windows PowerShell.

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Resultados de Get-NetAdapterQos esperado

Se deben mostrar las prioridades y clases de tráfico según el primer paso de configuración que realiza con esta guía.

![Calidad de las prioridades de servicio y clases](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-NetAdapterQos resultados inesperados

Si los resultados son los esperados, realiza los siguientes pasos.

1. Asegúrate de que el adaptador de red físico admita \(DCB\) puente de centro de datos y QoS
2. Asegúrate de que los controladores de adaptador de red estén actualizados.


## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

Puedes usar el siguiente comando de Windows PowerShell para comprobar que la dirección IP del nodo remoto es compatible con RDMA\.

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Resultados de Get-SmbMultiChannelConnection esperado

La dirección IP del nodo remoto se muestra como RDMA capaz.

![Dirección IP de RDMA capaz de nodo remoto](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Get-SmbMultiChannelConnection resultados inesperados

Si los resultados son los esperados, realiza los siguientes pasos.

1. Asegúrate de que el comando "ping" funciona en ambas direcciones.
2. Asegúrate de que el firewall no está bloqueando la iniciación de la conexión de SMB. Específicamente, habilita la regla de firewall para el puerto SMB directa 5445 para iWARP y 445 para ROCE.

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Puedes usar el siguiente comando para comprobar que el NIC virtual que habilitado para RDMA notificado como compatibles con RDMA\ por SMB.

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Resultados de Get-SmbClientNetworkInterface esperado

NIC virtual que se habilitó para RDMA debe verse como RDMA capaz de SMB.

![Informa de SMB que NIC son compatibles con RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface resultados inesperados

Si los resultados son los esperados, realiza los siguientes pasos.

1. Asegúrate de que el comando "ping" funciona en ambas direcciones.
2. Asegúrate de que el firewall no está bloqueando la iniciación de la conexión de SMB.

## <a name="vstat-mellanox-specific"></a>vstat \(Mellanox specific\)

Si estás usando Mellanox adaptadores de red, puedes usar la **vstat** comando para comprobar la RDMA sobre la versión de \(RoCE\) Ethernet convergido en nodos de Hyper-V.

### <a name="vstat-expected-results"></a>vstat esperada resultados

La versión RoCE en ambos nodos debe ser el mismo. También es una buena manera de comprobar que la versión de firmware en ambos nodos es más reciente.

![Ejemplos de resultado de comprobación de versión RoCE](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>vstat resultados inesperados

Si los resultados son los esperados, realiza los siguientes pasos.

1. Establece la versión correcta de RoCE con conjunto MlnxDriverCoreSetting
2. Instalar el firmware más reciente desde el sitio Web de Mellanox.


## <a name="perfmon-counters"></a>Contadores de rendimiento

Puedes revisar los contadores de Monitor de rendimiento para comprobar la actividad RDMA de la configuración.

![Ejemplos de resultado de monitor de rendimiento](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

## <a name="all-topics-in-this-guide"></a>Todos los temas de esta guía

Esta guía contiene los siguientes temas.

- [Configuración de NIC convergente con un adaptador de red](cnic-single.md)
- [Configuración de NIC convergente NIC de equipo](cnic-datacenter.md)
- [Configuración del conmutador físico para convergente NIC](cnic-app-switch-config.md)
- [Solución de problemas convergido configuraciones NIC](cnic-app-troubleshoot.md)
