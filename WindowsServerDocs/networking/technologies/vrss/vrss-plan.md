---
title: Planear el uso de vRSS
description: Puedes usar este tema para preparar la máquina virtual y el host de Hyper-V mediante vRSS en Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 695e6192-5e84-4ab4-b33e-8ebf6b8f5cbb
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: e6558b00e87721d8ab81c84946a14745c4faa812
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133391"
---
# Planear el uso de vRSS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En Windows Server 2016, vRSS está habilitada de manera predeterminada, sin embargo, debes preparar el entorno para permitir vRSS funcionar correctamente en una máquina virtual \(VM\) o en un host adaptador virtual \(vNIC\). En Windows Server 2012 R2, vRSS se ha deshabilitado de manera predeterminada.

Cuando se planea y prepara el uso de vRSS, asegúrate de que:

- El adaptador de red física es compatible con la cola de máquina Virtual \(VMQ\) y tiene una velocidad de vínculo de 10 Gbps o más.
- VMQ está habilitada en la NIC física y en el puerto de conmutador Virtual de Hyper\-V
- No hay ningún \(SR\-IOV\) Input\ de raíz solo salida virtualización configurados para la máquina virtual.
- Formación de equipos NIC está configurado correctamente.
- La máquina virtual tiene varios \(LPs\) procesadores lógicos.

>[!NOTE]
>vRSS is also enabled by default for any host vNICs that have RSS enabled.

Siguiente es información adicional que necesitas completar estos pasos de preparación.
  
1. **Capacidad de adaptador de red**. Comprueba que el adaptador de red es compatible con la cola de máquina Virtual \(VMQ\) y tiene una velocidad de vínculo de 10 Gbps o más. Si la velocidad del vínculo es menos de 10 Gbps como mínimo, el conmutador Virtual de Hyper\-V deshabilita VMQ de manera predeterminada, incluso aunque aún muestra VMQ habilitado en los resultados del comando de Windows PowerShell **Get-NetAdapterVmq**. Un método que puedes usar para comprobar que VMQ está habilitada o deshabilitada es usar el comando **Get-NetAdapterVmqQueue**.  Si se deshabilita VMQ, los resultados de este comando muestran que no hay ningún QueueID asignada al adaptador de red virtual host o máquina virtual. 
  
2. **Habilitar VMQ**. Comprueba que VMQ está habilitado en el equipo host. vRSS no funciona si el host no admite VMQ. Puedes comprobar que VMQ está habilitado, ejecute **Get-VMSwitch** y encontrar el adaptador que está usando el conmutador virtual. A continuación, ejecute **Get-NetAdapterVmq** y asegúrate de que el adaptador se muestra en los resultados y ha habilitado VMQ.
  
3. **Ausencia de SR\-IOV**. Comprueba que un \(SR\-IOV\) virtualización de salida Input\ de raíz único controlador de función Virtual \(VF\) no está conectado a la interfaz de red de máquina virtual. Puedes comprobar esto mediante el comando **Get-NetAdapterSriov** . If a VF driver is loaded, RSS uses the scaling settings from this driver instead of those configured by vRSS. If the VF driver does not support RSS, then vRSS is disabled.
  
4. **Configuración de formación de equipos NIC**. Si estás usando la formación de equipos NIC, es importante configurar de forma adecuada VMQ para trabajar con la configuración de equipos NIC. Para obtener información detallada sobre la formación de equipos NIC de implementación y administración, consulta la [Formación de equipos NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

5. **Número de lógicos (LP)**. Comprueba que la máquina virtual tiene más de un procesador lógico \(LP\). vRSS relies on RSS in the VM or on the Hyper-V host to load balance received traffic to multiple LPs for parallel processing. Puede observar cuántos lógicos (LP) tiene la máquina virtual ejecutando el comando de Windows PowerShell **Get-VMProcessor** en el host. Después de ejecutar el comando, puede observar la entrada de la columna Recuento del número de lógicos (LP).

El vNIC de host siempre tiene acceso a todos los procesadores físicos; Para configurar la vNIC de host para usar un número determinado de procesadores, usa la configuración **- BaseProcessorNumber** y **-MaxProcessors** al ejecutar el comando de Windows PowerShell **NetAdapterRss del conjunto** .

---