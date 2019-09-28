---
title: Planear el uso de vRSS
description: Puede usar este tema para preparar la máquina virtual y el host de Hyper-V para usar vRSS en Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 695e6192-5e84-4ab4-b33e-8ebf6b8f5cbb
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: 3addb500a654ff9f23c56388fec3ef2c3855a1da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395824"
---
# <a name="plan-the-use-of-vrss"></a>Planear el uso de vRSS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En Windows Server 2016, vRSS está habilitado de forma predeterminada; sin embargo, debe preparar el entorno para permitir que vRSS funcione correctamente en una máquina virtual \(VM @ no__t-1 o en un adaptador virtual de host \(vNIC @ no__t-3. En Windows Server 2012 R2, vRSS estaba deshabilitado de forma predeterminada.

Al planear y preparar el uso de vRSS, asegúrese de que:

- El adaptador de red físico es compatible con Virtual Machine Queue \(VMQ @ no__t-1 y tiene una velocidad de vínculo de 10 Gbps o más.
- VMQ está habilitado en la NIC física y en el puerto del conmutador virtual de Hyper @ no__t-0V
- No hay ninguna entrada raíz única @ no__t-0Output Virtualization \(SR @ no__t-2IOV @ no__t-3 configurada para la máquina virtual.
- La formación de equipos NIC está configurada correctamente.
- La máquina virtual tiene varios procesadores lógicos \(LPs @ no__t-1.

>[!NOTE]
>vRSS también está habilitado de forma predeterminada para cualquier VNIC de host que tenga RSS habilitado.

A continuación se indica información adicional que necesita para completar estos pasos de preparación.
  
1. **Capacidad del adaptador de red**. Compruebe que el adaptador de red sea compatible con Virtual Machine Queue \(VMQ @ no__t-1 y que tenga una velocidad de vínculo de 10 Gbps o más. Si la velocidad de vínculo es inferior a 10 Gbps, el conmutador virtual de Hyper @ no__t-0V deshabilita VMQ de forma predeterminada, aunque siga mostrando VMQ como habilitado en los resultados del comando de Windows PowerShell **Get-NetAdapterVmq**. Un método que puede usar para comprobar que VMQ está habilitado o deshabilitado es usar el comando **Get-NetAdapterVmqQueue**.  Si VMQ está deshabilitado, los resultados de este comando muestran que no hay ningún QueueID asignado a la máquina virtual o al adaptador de red virtual del host. 
  
2. **Habilite VMQ**. Verifique que VMQ se encuentre habilitado en el equipo host. vRSS no funciona si el host no admite VMQ. Para comprobar que VMQ está habilitado, ejecute **Get-VMSwitch** y busque el adaptador que está usando el conmutador virtual. A continuación, ejecute **Get-NetAdapterVmq** y asegúrese de que el adaptador aparece entre los resultados y que tiene habilitado VMQ.
  
3. **Ausencia de Sr @ no__t-1IOV**. Compruebe que una única entrada raíz @ no__t-0Output Virtualization \(SR @ no__t-2IOV @ no__t-3 virtual function \(VF @ no__t-5 driver no esté conectada a la interfaz de red de VM. Puede comprobarlo mediante el comando **Get-NetAdapterSriov** . Si se carga un controlador de VF, RSS usa la configuración de escalado de este controlador en lugar de los configurados por vRSS. Si el controlador de VF no admite RSS, vRSS está deshabilitado.
  
4. **Configuración de formación de equipos NIC**. Si usa la formación de equipos NIC, es importante que configure VMQ correctamente para que funcione con la configuración de formación de equipos NIC. Para obtener información detallada sobre la implementación y administración de formación de equipos NIC, consulte [formación de equipos NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

5. **Número de LPS**. Compruebe que la máquina virtual tiene más de un procesador lógico \(LP @ no__t-1. vRSS se basa en RSS en la VM o en el host de Hyper-V para equilibrar la carga del tráfico recibido en varios LPs para el procesamiento en paralelo. Puede observar cuántos LPs tiene la máquina virtual mediante la ejecución del comando de Windows PowerShell **Get-VMProcessor** en el host. Después de ejecutar el comando, puede observar la entrada de la columna recuento para el número de LPs.

El host vNIC siempre tiene acceso a todos los procesadores físicos; para configurar el host vNIC para que use un número específico de procesadores, use los valores **-BaseProcessorNumber** y **-MaxProcessors** cuando ejecute el comando **set-NetAdapterRss de** Windows PowerShell.

---