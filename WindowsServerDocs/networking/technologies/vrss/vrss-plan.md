---
title: Planear el uso de vRSS
description: Puede usar este tema para preparar la máquina virtual y el host de Hyper-V para usar vRSS en Windows Server 2016.
ms.topic: article
ms.assetid: 695e6192-5e84-4ab4-b33e-8ebf6b8f5cbb
ms.localizationpriority: medium
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/04/2018
ms.openlocfilehash: ccfaa9fa02dd7324f1682592867b027cad4006a8
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993558"
---
# <a name="plan-the-use-of-vrss"></a>Planear el uso de vRSS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En Windows Server 2016, vRSS está habilitado de forma predeterminada; sin embargo, debe preparar el entorno para permitir que vRSS funcione correctamente en una VM de máquina virtual \( \) o en un adaptador virtual de host \( VNIC \) . En Windows Server 2012 R2, vRSS estaba deshabilitado de forma predeterminada.

Al planear y preparar el uso de vRSS, asegúrese de que:

- El adaptador de red físico es compatible con Virtual Machine Queue \( VMQ \) y tiene una velocidad de vínculo de 10 Gbps o más.
- VMQ está habilitado en la NIC física y en el \- Puerto del conmutador virtual de Hyper V
- No hay ninguna \- virtualización de salida de entrada raíz única \( Sr \- IOV \) configurada para la máquina virtual.
- La formación de equipos NIC está configurada correctamente.
- La máquina virtual tiene varios procesadores lógicos \( LPS \) .

>[!NOTE]
>vRSS también está habilitado de forma predeterminada para cualquier VNIC de host que tenga RSS habilitado.

A continuación se indica información adicional que necesita para completar estos pasos de preparación.

1. **Capacidad del adaptador de red**. Compruebe que el adaptador de red sea compatible con Virtual Machine Queue \( VMQ \) y que tenga una velocidad de vínculo de 10 Gbps o más. Si la velocidad de vínculo es inferior a 10 Gbps, el \- conmutador virtual de Hyper V deshabilita VMQ de forma predeterminada, aunque siga mostrando VMQ como habilitado en los resultados del comando de Windows PowerShell **Get-NetAdapterVmq**. Un método que puede usar para comprobar que VMQ está habilitado o deshabilitado es usar el comando **Get-NetAdapterVmqQueue**.  Si VMQ está deshabilitado, los resultados de este comando muestran que no hay ningún QueueID asignado a la máquina virtual o al adaptador de red virtual del host.

2. **Habilite VMQ**. Verifique que VMQ se encuentre habilitado en el equipo host. vRSS no funciona si el host no admite VMQ. Para comprobar que VMQ está habilitado, ejecute **Get-VMSwitch** y busque el adaptador que está usando el conmutador virtual. A continuación, ejecute **Get-NetAdapterVmq** y asegúrese de que el adaptador aparece entre los resultados y que tiene habilitado VMQ.

3. **Ausencia de Sr \- IOV**. Compruebe que un controlador de FV de función virtual de virtualización de salida de entrada de raíz única \- \( \- \) \( \) no está conectado a la interfaz de red de VM. Puede comprobarlo mediante el comando **Get-NetAdapterSriov** . Si se carga un controlador de VF, RSS usa la configuración de escalado de este controlador en lugar de los configurados por vRSS. Si el controlador de VF no admite RSS, vRSS está deshabilitado.

4. **Configuración de formación de equipos NIC**. Si usa la formación de equipos NIC, es importante que configure VMQ correctamente para que funcione con la configuración de formación de equipos NIC. Para obtener información detallada sobre la implementación y administración de formación de equipos NIC, consulte [formación de equipos NIC](../nic-teaming/nic-teaming.md).

5. **Número de LPS**. Compruebe que la máquina virtual tiene más de un procesador lógico \( LP \) . vRSS se basa en RSS en la VM o en el host de Hyper-V para equilibrar la carga del tráfico recibido en varios LPs para el procesamiento en paralelo. Puede observar cuántos LPs tiene la máquina virtual mediante la ejecución del comando de Windows PowerShell **Get-VMProcessor** en el host. Después de ejecutar el comando, puede observar la entrada de la columna recuento para el número de LPs.

El host vNIC siempre tiene acceso a todos los procesadores físicos; para configurar el host vNIC para que use un número específico de procesadores, use los valores **-BaseProcessorNumber** y **-MaxProcessors** cuando ejecute el comando **set-NetAdapterRss de** Windows PowerShell.

---