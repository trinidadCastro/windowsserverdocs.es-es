---
title: vRSS preguntas más frecuentes
description: En este tema, encontrará que algunas frecuentes preguntas y respuestas sobre el uso de vRSS.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 61ae242e-82a8-430d-b07d-52b86c01e686
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3fafe6c39285e65a9d39a76cc6b652dac5c3efbd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840246"
---
# <a name="vrss-frequently-asked-questions"></a>vRSS preguntas más frecuentes

En este tema, encontrará que algunas frecuentes preguntas y respuestas sobre el uso de vRSS.

## <a name="what-are-the-requirements-for-the-physical-network-adapters-that-i-use-with-vrss"></a>¿Cuáles son los requisitos para los adaptadores de red físico que utilizo con vRSS?

Adaptadores de red deben ser compatibles con Virtual Machine Queue \(VMQ\) y debe tener una velocidad de vínculo de 10 Gbps o más.

Para obtener más información, consulte [planear el uso de vRSS](vrss-plan.md).

## <a name="does-vrss-work-with-hyper-threaded-processor-cores"></a>¿Funciona vRSS con hyper\-un subproceso de núcleos de procesador?

No. VRSS y VMQ omiten hyper\-núcleos de procesador de un subproceso.

## <a name="does-vrss-work-for-host-virtual-nics-vnics"></a>¿Funciona vRSS para hospedar las NIC virtuales \(VNIC\)?

Sí. Use la **- ManagementOS** parámetro en lugar de la máquina virtual \(VM\) nombre en el **Set-VMNetworkAdapter** comando de Windows PowerShell y  **Enable-NetAdapterRss** en la vNIC de host.

Para obtener más información, consulte [comandos de Windows PowerShell para RSS y vRSS](vrss-wps.md).

## <a name="how-many-logical-processors-does-a-vm-need-to-use-vrss"></a>¿Cuántos procesadores lógicos necesita una máquina virtual para usar vRSS?

Las máquinas virtuales deben dos o más procesadores lógicos \(LPs\) para poder usar vRSS.

Para obtener más información, consulte [planear el uso de vRSS](vrss-plan.md).

## <a name="is-vrss-compatible-with-nic-teaming"></a>¿Es compatible con la formación de equipos NIC vRSS?

Sí. Si usa la formación de equipos NIC, es importante configurar correctamente VMQ para trabajar con la configuración de formación de equipos NIC. Para obtener información detallada sobre la administración e implementación de formación de equipos NIC, consulte [formación de equipos NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## <a name="vrss-is-enabled-but-how-do-i-know-if-it-is-working"></a>vRSS se habilita, pero ¿cómo se puede saber si está funcionando? 

Podrá saber vRSS funciona, abra el Administrador de tareas en la máquina virtual y ver la utilización del procesador virtual. Si hay varias conexiones establecidas en la máquina virtual, puede ver más de un núcleo por encima de 0% de uso.

Dado que una sola sesión TCP no puede ser de carga equilibrada entre varios núcleos de procesador lógico, la máquina virtual debe recibir TCP varias sesiones antes de ver si está trabajando vRSS.

Si la máquina virtual recibe varias sesiones TCP, pero no se ve más de un núcleo LP por encima de 0% de uso, asegúrese de que ha completado todos los pasos de preparación en el tema [planear el uso de vRSS](vrss-plan.md).

## <a name="im-looking-at-the-host-and-not-all-of-the-processors-are-being-used-it-looks-like-every-other-one-is-being-skipped"></a>Estoy observando el host y veo que no se usan todos los procesadores. Parece como si se omitiera uno de cada dos.
  
Compruebe si Hyper-Threading está habilitado. VMQ y vRSS están diseñados para omitir hyper\-núcleos de un subproceso.

## <a name="are-there-different-windows-powershell-commands-for-rss-and-vrss"></a>¿Existen distintos comandos de Windows PowerShell para RSS y vRSS?

Sí y no. Mientras que usan los mismos comandos de RSS en hosts nativos y RSS en máquinas virtuales, vRSS también requiere VMQ esté habilitado en la NIC física - y para la máquina virtual y vRSS esté habilitado en el puerto del conmutador.

Para obtener más información, consulte [comandos de Windows PowerShell para RSS y vRSS](vrss-wps.md).

---
