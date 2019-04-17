---
title: vRSS preguntas más frecuentes
description: En este tema, encontrarás que algunas frecuentes preguntas y respuestas sobre el uso de vRSS.
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133821"
---
# vRSS preguntas más frecuentes

En este tema, encontrarás que algunas frecuentes preguntas y respuestas sobre el uso de vRSS.

## ¿Cuáles son los requisitos para los adaptadores de red física que puedo usar con vRSS?

Adaptadores de red deben ser compatibles con la cola de máquina Virtual \(VMQ\) y deben tener una velocidad de vínculo de 10 Gbps o más.

Para obtener más información, consulta [Planear el uso de vRSS](vrss-plan.md).

## ¿Funciona con núcleos subprocesamiento hyper\ vRSS?

No. VRSS y VMQ ignoran núcleos hyper\ subproceso.

## ¿Funciona vRSS para host virtual NIC \(vNICs\)?

Sí. Usa el parámetro **- ManagementOS** en lugar del nombre de máquina virtual \(VM\) en el comando de Windows PowerShell **Conjunto VMNetworkAdapter** y **Habilitar NetAdapterRss** en la vNIC de host.

For more information, see [Windows PowerShell Commands for RSS and vRSS](vrss-wps.md).

## ¿Cuántos procesadores lógicos necesita usar vRSS una máquina virtual?

Las máquinas virtuales necesitan dos o más \(LPs\) procesadores lógicos para que puedan usar vRSS.

Para obtener más información, consulta [Planear el uso de vRSS](vrss-plan.md).

## ¿Es compatible con la formación de equipos NIC vRSS?

Sí. Si estás usando la formación de equipos NIC, es importante configurar de forma adecuada VMQ para trabajar con la configuración de equipos NIC. Para obtener información detallada sobre la formación de equipos NIC de implementación y administración, consulta la [Formación de equipos NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## vRSS está habilitado, pero ¿cómo saber si está trabajando? 

Podrás saber vRSS está funcionando al abrir el Administrador de tareas en la máquina virtual y ver la utilización del procesador virtual. Si hay varias conexiones establecidas a la máquina virtual, puedes ver más de un núcleo por encima de la utilización del 0%.

Dado que una sola sesión TCP no puede ser de carga equilibrada a través de varios núcleos lógicos, la máquina virtual debe recibir TCP varias sesiones antes de que se pueden observar independientemente de si está trabajando vRSS.

Si la máquina virtual recibe varias sesiones TCP, pero no ves más de un núcleo LP por encima de la utilización del 0%, asegúrate de que has completado todos los pasos de preparación en el tema [planificar el uso de vRSS](vrss-plan.md).

## He mirando el host y no todos los procesadores se están usando. Parece que cada otros uno se omitirá.
  
Comprobar si está habilitada la tecnología hyper threading. VMQ y vRSS están diseñadas para omitir el subproceso de hyper\ núcleos.

## Are there different Windows PowerShell commands for RSS and vRSS?

Sí y no. While you use the same commands for both RSS in native hosts and RSS in VMs, vRSS also requires VMQ to be enabled on the physical NIC - and for the VM and vRSS to be enabled on the switch port.

For more information, see [Windows PowerShell Commands for RSS and vRSS](vrss-wps.md).

---
