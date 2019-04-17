---
title: Ajuste de escala en lado de recepción virtual (vRSS)
description: Obtén información sobre Virtual escala de recepción (vRSS) en Windows Server y cómo configurar un adaptador de red virtual para cargar equilibrar el tráfico de red entrantes a través de varios núcleos lógicos en una máquina virtual. También puedes configurar varios de núcleos físicos para un host de tarjeta de interfaz de red virtual (vNIC).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9be477b3-f81d-4e84-a6b0-ac4c1ea97715
ms.date: 09/05/2018
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0c1cb11cb8ce69463a31cfa5061290f79d8dda91
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133691"
---
# Virtual recibir lado \(vRSS\) de ajuste de escala

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, encontrarás información sobre cómo configurar un adaptador de red virtual para cargar equilibrar el tráfico de red entrantes a través de varios núcleos lógicos en una máquina virtual y recibir lado del ajuste de escala Virtual (vRSS). También puedes usar vRSS para configurar varios núcleos físicos para un host virtual tarjeta de interfaz de red \(vNIC\).

Esta configuración permite que la carga de un adaptador de red virtual a distribuirse a través de varios procesadores virtuales en una máquina virtual \(VM\), lo que permite a la máquina virtual procesar el tráfico de red más más rápidamente de lo que es posible con un único procesador lógico.

>[!TIP]
>Puedes usar vRSS en las máquinas virtuales en hosts de Hyper\-V con varios procesadores, un único procesador de varios núcleos, o más de uno varios procesadores de núcleo instalado y configurado para su uso de la máquina virtual.

vRSS es compatible con todas las otras tecnologías de redes de Hyper\-V. vRSS is dependent on Virtual Machine Queue \(VMQ\) in the Hyper\-V host and RSS in the VM or on the host vNIC.

De manera predeterminada, Windows Server permite vRSS, pero se puede deshabilitar en una máquina virtual mediante el uso de Windows PowerShell. For more information, see [Manage vRSS](vrss-manage.md) and [Windows PowerShell Commands for RSS and vRSS](vrss-wps.md).



## Compatibilidad del sistema operativo

You can use RSS on any multiprocessor or multicore computer - or vRSS on any multiprocessor or multicore VM - that is running Windows Server 2016.

Multiprocesador o multinúcleo máquinas virtuales que se ejecutan los siguientes sistemas operativos de Microsoft también admiten vRSS.

- WindowsServer2016
- Windows 10 Pro o Enterprise
- Windows Server2012R2
- Windows 8.1 Pro o Enterprise
- Windows Server 2012 con los componentes de integración de Windows Server 2012 R2 instalados.
- Los componentes de integración de Windows Server 2012 R2 instalados de Windows 8.

Para obtener información sobre la compatibilidad con vRSS para las máquinas virtuales que ejecutan FreeBSD o Linux, como un sistema operativo invitado en Hyper-V, consulte [las máquinas virtuales Linux y FreeBSD para Hyper-V en Windows](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows).
  
## Requisitos de hardware

A continuación es los requisitos de hardware para vRSS.
 
- Adaptadores de red física deben ser compatible con la cola de máquina Virtual \(VMQ\). Si está deshabilitada o no admite VMQ, vRSS está deshabilitada para el host de Hyper\-V y las máquinas virtuales configuradas en el host.
- Adaptadores de red deben tener una velocidad de vínculo de 10 Gbps o más.
- Hosts de Hyper\-V deben configurarse con varios procesadores o procesador de varios núcleos al menos un usar vRSS.
- Las máquinas virtuales \(VMs\) deben configurarse para utilizar dos o más procesadores lógicos.


## Entre las situaciones de uso

Los siguientes escenarios de casos de uso de dos representan uso común de vRSS equilibrio de carga del procesador y equilibrio de carga de software.

### Equilibrio de carga de procesador
  
Anthony, un administrador de red, es configurar un nuevo host de Hyper-V con el adaptador de red dos que admite la virtualización de la entrada de raíz solo salida \(SR\-IOV\). Implementa Windows Server 2016 para hospedar un servidor de archivos de la máquina virtual.

Después de instalar el hardware y software, Anthony configura una máquina virtual para usar ocho procesadores virtuales y 4096 MB de memoria. Por desgracia, Anthony no tiene la opción de activar SR\-IOV porque sus máquinas virtuales se basan en la aplicación de directivas a través del conmutador virtual que se creó con el Administrador de conmutador Virtual de Hyper\-V. Por este motivo, Anthony decide usar vRSS en lugar de SR\-IOV.

Inicialmente, Anthony asigna cuatro procesadores virtuales mediante el uso de Windows PowerShell que estarán disponibles para su uso con vRSS. El uso del servidor de archivos después de una semana apareció a ser bastante populares, por lo que Anthony comprueba el rendimiento de la máquina virtual.  Descubre uso completo de los cuatro procesadores virtuales.

Por este motivo, Anthony decide agregar procesadores a la máquina virtual para su uso por vRSS.  Dos procesadores virtuales más asigna a la máquina virtual, que están disponibles automáticamente para vRSS para ayudar a controlar la carga de red de gran tamaño. Su esfuerzo como resultado un mejor rendimiento para el servidor de archivos de la máquina virtual con los procesadores de seis controlar eficazmente la carga de tráfico de red.


### Equilibrio de carga de software

Sandra, un administrador de red, es configurar una sola máquina virtual de alto rendimiento en uno de sus sistemas para que actúe como un equilibrador de carga de software. Todos los procesadores lógicos disponibles ha asignado a esta máquina virtual único.

Después de instalar Windows Server, utiliza vRSS para obtener el tráfico de red paralelo procesamiento en varios procesadores lógicos en la máquina virtual. Dado que Windows Server permite vRSS, Sandra no tiene que realizar los cambios de configuración.


## Temas relacionados

- [Planear el uso de vRSS](vrss-plan.md)
- [Habilitar vRSS en un adaptador de red Virtual](vrss-enable.md)
- [Administrar vRSS](vrss-manage.md)
- [vRSS preguntas más frecuentes](vrss-faq.md)
- [Windows PowerShell Commands for RSS and vRSS](vrss-wps.md)

---
