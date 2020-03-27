---
title: Preguntas más frecuentes sobre vRSS
description: En este tema, encontrará algunas preguntas y respuestas frecuentes sobre el uso de vRSS.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 61ae242e-82a8-430d-b07d-52b86c01e686
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0ee9bf121d64eebe98798df907a2584747a00c7a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315367"
---
# <a name="vrss-frequently-asked-questions"></a>Preguntas más frecuentes sobre vRSS

En este tema, encontrará algunas preguntas y respuestas frecuentes sobre el uso de vRSS.

## <a name="what-are-the-requirements-for-the-physical-network-adapters-that-i-use-with-vrss"></a>¿Cuáles son los requisitos para los adaptadores de red físicos que utilizo con vRSS?

Los adaptadores de red deben ser compatibles con Virtual Machine Queue \(VMQ\) y deben tener una velocidad de vínculo de 10 Gbps o más.

Para obtener más información, consulte [planear el uso de vRSS](vrss-plan.md).

## <a name="does-vrss-work-with-hyper-threaded-processor-cores"></a>¿Funciona vRSS con núcleos de procesador de subprocesos de Hyper\-?

No. VRSS y VMQ omiten los núcleos de procesador de subprocesos de Hyper\-.

## <a name="does-vrss-work-for-host-virtual-nics-vnics"></a>¿Funciona vRSS para NIC virtuales de host \(VNIC\)?

Sí. Use el parámetro **-managementos** en lugar del nombre de la máquina virtual \(VM\) en el comando **set-VMNetworkAdapter** de Windows PowerShell y **enable-NetAdapterRss** en el host VNIC.

Para obtener más información, vea [comandos de Windows PowerShell para RSS y vRSS](vrss-wps.md).

## <a name="how-many-logical-processors-does-a-vm-need-to-use-vrss"></a>¿Cuántos procesadores lógicos necesita una máquina virtual para usar vRSS?

Las máquinas virtuales necesitan dos o más procesadores lógicos \(LPs\) para poder usar vRSS.

Para obtener más información, consulte [planear el uso de vRSS](vrss-plan.md).

## <a name="is-vrss-compatible-with-nic-teaming"></a>¿Es compatible con vRSS con la formación de equipos NIC?

Sí. Si usa la formación de equipos NIC, es importante que configure VMQ correctamente para que funcione con la configuración de formación de equipos NIC. Para obtener información detallada sobre la implementación y administración de formación de equipos NIC, consulte [formación de equipos NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## <a name="vrss-is-enabled-but-how-do-i-know-if-it-is-working"></a>vRSS está habilitado, pero ¿cómo sé si funciona? 

Podrá indicar a vRSS que funciona abriendo el administrador de tareas en la máquina virtual y viendo la utilización del procesador virtual. Si hay varias conexiones establecidas en la máquina virtual, puede ver más de un núcleo por encima del 0% de uso.

Dado que no se puede equilibrar la carga de una sola sesión TCP a través de varios núcleos de procesador lógicos, la máquina virtual debe recibir varias sesiones TCP antes de que pueda observar si el vRSS está funcionando o no.

Si la máquina virtual está recibiendo varias sesiones TCP, pero no ve más de un núcleo LP por encima del 0% de uso, asegúrese de que ha completado todos los pasos de preparación del tema [planear el uso de vRSS](vrss-plan.md).

## <a name="im-looking-at-the-host-and-not-all-of-the-processors-are-being-used-it-looks-like-every-other-one-is-being-skipped"></a>Estoy observando el host y no se usan todos los procesadores. Parece como si se omitiera uno de cada dos.
  
Compruebe si Hyper-Threading está habilitado. VMQ y vRSS están diseñados para omitir núcleos de subprocesos de Hyper\-.

## <a name="are-there-different-windows-powershell-commands-for-rss-and-vrss"></a>¿Existen distintos comandos de Windows PowerShell para RSS y vRSS?

Sí y no. Aunque se usan los mismos comandos para RSS en hosts nativos y RSS en máquinas virtuales, vRSS también requiere que VMQ esté habilitado en la NIC física, y que la máquina virtual y vRSS estén habilitados en el puerto del conmutador.

Para obtener más información, vea [comandos de Windows PowerShell para RSS y vRSS](vrss-wps.md).

---
