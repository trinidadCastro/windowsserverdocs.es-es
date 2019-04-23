---
title: Planear el uso de vRSS
description: Puede utilizar este tema para preparar la máquina virtual y el host de Hyper-V con vRSS en Windows Server 2016.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850446"
---
# <a name="plan-the-use-of-vrss"></a>Planear el uso de vRSS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En Windows Server 2016, vRSS se habilita de forma predeterminada, sin embargo, debe preparar el entorno para permitir vRSS funcione correctamente en una máquina virtual \(VM\) o en un adaptador de host virtual \(vNIC\). En Windows Server 2012 R2, vRSS se deshabilitó de forma predeterminada.

Al planear y preparar el uso de vRSS, asegúrese de que:

- El adaptador de red físico es compatible con Virtual Machine Queue \(VMQ\) y tiene una velocidad de vínculo de 10 Gbps o más.
- VMQ se encuentre habilitado en la NIC física y en el Hyper\-puerto del conmutador Virtual V
- No hay ninguna entrada de raíz única\-salida virtualización \(SR\-IOV\) configurado para la máquina virtual.
- Formación de equipos NIC está configurado correctamente.
- La máquina virtual tiene varios procesadores lógicos \(LPs\).

>[!NOTE]
>vRSS también está habilitada de forma predeterminada para cualquier VNIC de host que tiene RSS habilitado.

La siguiente es información adicional que necesita completar estos pasos de preparación.
  
1. **Capacidad del adaptador de red**. Compruebe que el adaptador de red es compatible con Virtual Machine Queue \(VMQ\) y tiene una velocidad de vínculo de 10 Gbps o más. Si la velocidad de vínculo es inferior a 10 Gbps, Hyper\-V Virtual Switch deshabilita VMQ de forma predeterminada, aunque sigue mostrando VMQ habilitada en los resultados del comando de Windows PowerShell **Get-NetAdapterVmq**. Un método que puede usar para verificar que VMQ está habilitado o deshabilitado es usar el comando **Get-NetAdapterVmqQueue**.  Si VMQ está deshabilitado, los resultados de este comando muestran que no hay ningún QueueID asignado al adaptador de red virtual de host o máquina virtual. 
  
2. **Habilitar VMQ**. Verifique que VMQ se encuentre habilitado en el equipo host. vRSS no funciona si el host no admite VMQ. Puede verificar que VMQ se encuentre habilitado mediante la ejecución de **Get-VMSwitch** y localizando el adaptador que está usando el conmutador virtual. A continuación, ejecute **Get-NetAdapterVmq** y asegúrese de que el adaptador aparece entre los resultados y que tiene habilitado VMQ.
  
3. **Ausencia de SR\-IOV**. Compruebe que la entrada de una raíz única\-salida virtualización \(SR\-IOV\) función Virtual \(VF\) controlador no está conectado a la interfaz de red de máquina virtual. Puede comprobarlo mediante el uso de la **Get-NetAdapterSriov** comando. Si se carga un controlador de VF, RSS usa los valores de escalado desde este controlador en lugar de los configurados por vRSS. Si el controlador de VF no es compatible con RSS, vRSS se encuentra deshabilitado.
  
4. **Configuración de formación de equipos NIC**. Si usa la formación de equipos NIC, es importante configurar correctamente VMQ para trabajar con la configuración de formación de equipos NIC. Para obtener información detallada sobre la administración e implementación de formación de equipos NIC, consulte [formación de equipos NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

5. **Número de LPs**. Compruebe que la máquina virtual tiene más de un procesador lógico \(LP\). vRSS se basa en RSS en la máquina virtual o en el host de Hyper-V para equilibrar la carga recibido a varios LPs para procesamiento en paralelo. Puede observar cuántas LPs tiene la máquina virtual, ejecute el comando de Windows PowerShell **Get-VMProcessor** en el host. Después de ejecutar el comando, puede observar la entrada de la columna Recuento para el número de LPs.

La vNIC de host siempre tiene acceso a todos los procesadores físicos; Para configurar la vNIC de host para usar un número específico de procesadores, use la configuración de **- BaseProcessorNumber** y **- MaxProcessors** al ejecutar el **Set-NetAdapterRss** Comando de Windows PowerShell.

---