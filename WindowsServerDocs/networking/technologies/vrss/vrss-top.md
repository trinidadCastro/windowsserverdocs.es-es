---
title: Ajuste de escala en lado de recepción virtual (vRSS)
description: Obtenga información acerca de Virtual escala de recepción (vRSS) en Windows Server y cómo configurar un adaptador de red virtual para el tráfico de red entrante de equilibrio de carga entre varios núcleos de procesador lógico en una máquina virtual. También puede configurar múltiples de núcleos físicos para un host de tarjeta de interfaz de red virtual (vNIC).
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875236"
---
# <a name="virtual-receive-side-scaling-vrss"></a>Virtual escala de recepción \(vRSS\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, obtendrá información sobre cómo configurar un adaptador de red virtual para el tráfico de red entrante de equilibrio de carga entre varios núcleos de procesador lógico en una máquina virtual y Virtual escala de recepción (vRSS). También puede usar vRSS para configurar varios núcleos físicos para un host de virtual Network Interface Card \(vNIC\).

Esta configuración permite la carga de un adaptador de red virtual se distribuyan entre varios procesadores virtuales en una máquina virtual \(VM\), lo que permite la máquina virtual procesar más rápidamente de lo que puede hacer con una sola más tráfico de red procesador lógico.

>[!TIP]
>Puede usar vRSS en máquinas virtuales en Hyper\-hosts V que tienen varios procesadores, un único procesador de varios núcleos o más procesadores de varios núcleos instalado y configuran para su uso de la máquina virtual.

es compatible con todos los demás Hyper vRSS\-V las tecnologías de red. vRSS es dependiente de Virtual Machine Queue \(VMQ\) en Hyper\-host V y RSS en la máquina virtual o en la vNIC de host.

De forma predeterminada, Windows Server permite vRSS, pero puede deshabilitarlo en una máquina virtual mediante Windows PowerShell. Para obtener más información, consulte [administrar vRSS](vrss-manage.md) y [comandos de Windows PowerShell para RSS y vRSS](vrss-wps.md).



## <a name="operating-system-compatibility"></a>Compatibilidad con sistemas operativos

Puede usar RSS en cualquier equipo multiprocesador o multinúcleo - o vRSS en cualquier VM multiprocesador o multinúcleo - que se está ejecutando Windows Server 2016.

Multiprocesador o multinúcleo máquinas virtuales que ejecutan los siguientes sistemas operativos de Microsoft también admite vRSS.

- Windows Server 2016
- Windows 10 Pro o Enterprise
- Windows Server 2012 R2
- Windows 8.1 Pro o Enterprise
- Windows Server 2012 con los componentes de integración de Windows Server 2012 R2 instalados.
- Windows 8 con los componentes de integración de Windows Server 2012 R2 instalados.

Para obtener información sobre la compatibilidad de vRSS con máquinas virtuales que ejecutan FreeBSD o Linux como sistema operativo invitado en Hyper-V, consulte [Linux y FreeBSD máquinas virtuales de Hyper-V en Windows](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows).
  
## <a name="hardware-requirements"></a>Requisitos de hardware

Siguientes son los requisitos de hardware para vRSS.
 
- Adaptadores de red físico deben admitir Virtual Machine Queue \(VMQ\). Si VMQ está deshabilitado o no admite, vRSS está deshabilitado para el Hyper\-host V y las máquinas virtuales configuradas en el host.
- Adaptadores de red deben tener una velocidad de vínculo de 10 Gbps o más.
- Hyper\-hosts V deben estar configurados con varios procesadores o varios al menos un\-procesador para usar vRSS core.
- Las máquinas virtuales \(máquinas virtuales\) debe configurarse para usar dos o más procesadores lógicos.


## <a name="use-case-scenarios"></a>Escenarios de casos de uso

Los siguientes escenarios de casos de uso de dos describen el uso común de vRSS para equilibrio de carga de procesador y de equilibrio de carga de software.

### <a name="processor-load-balancing"></a>Equilibrado de la carga del procesador
  
Antonio es un administrador de red, es la configuración de un nuevo host de Hyper-V con el adaptador de red de dos que admite la virtualización de entrada de raíz única salida \(SR\-IOV\). Implementa Windows Server 2016 para hospedar un servidor de archivos de máquina virtual.

Después de instalar el hardware y software, Antonio configura una máquina virtual con ocho procesadores virtuales y 4096 MB de memoria. Lamentablemente, Antonio no tiene la opción de activar SR\-IOV porque sus máquinas virtuales se basan en la aplicación de directivas a través del conmutador virtual que creó con Hyper\-V Virtual Switch manager. Por este motivo, Anthony decide usar vRSS en lugar de SR\-IOV.

Inicialmente, Antonio asigna cuatro procesadores virtuales mediante el uso de Windows PowerShell esté disponible para su uso con vRSS. El uso del servidor de archivos después de una semana parecía ser bastante popular, por lo que Antonio comprueba el rendimiento de la máquina virtual.  Descubre utilización completa de los cuatro procesadores virtuales.

Por este motivo, Anthony decide agregar procesadores a la máquina virtual para su uso por vRSS.  Asigna dos procesadores virtuales más a la máquina virtual, que están disponibles automáticamente para vRSS para ayudar a controlar la carga de red de gran tamaño. Sus esfuerzos como resultado un mejor rendimiento para el servidor de archivos de máquina virtual con los procesadores de seis tratar la carga de tráfico de red de forma eficiente.


### <a name="software-load-balancing"></a>Equilibrado de la carga de software

Sandra es administradora de redes es la configuración de una sola máquina virtual de alto rendimiento en uno de sus sistemas para que actúe como un equilibrador de carga de software. Todos los procesadores lógicos disponibles ha asignado a esta máquina virtual única.

Después de instalar Windows Server, usa vRSS para obtener el tráfico de red paralelas de procesamiento en varios procesadores lógicos en la máquina virtual. Dado que Windows Server permite vRSS, Sandra no tiene que realizar ningún cambio de configuración.


## <a name="related-topics"></a>Temas relacionados

- [Planear el uso de vRSS](vrss-plan.md)
- [Habilite vRSS en un adaptador de red Virtual](vrss-enable.md)
- [Administrar vRSS](vrss-manage.md)
- [vRSS preguntas más frecuentes](vrss-faq.md)
- [Comandos de Windows PowerShell para RSS y vRSS](vrss-wps.md)

---
