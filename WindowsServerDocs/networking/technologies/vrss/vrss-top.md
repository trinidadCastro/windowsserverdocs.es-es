---
title: Ajuste de escala en lado de recepción virtual (vRSS)
description: Obtenga información sobre el ajuste de escala en lado de recepción virtual (vRSS) en Windows Server y cómo configurar un adaptador de red virtual para equilibrar la carga del tráfico de red entrante entre varios núcleos de procesador lógico de una máquina virtual. También puede configurar varios núcleos físicos para una tarjeta de interfaz de red virtual (vNIC) del host.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9be477b3-f81d-4e84-a6b0-ac4c1ea97715
ms.date: 09/05/2018
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ae017d7d78adea565942a952aaea3da1669f39a9
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871798"
---
# <a name="virtual-receive-side-scaling-vrss"></a>VRSS de ajuste de \(escala en lado de recepción virtual\)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, obtendrá información sobre el ajuste de escala en lado de recepción virtual (vRSS) y cómo configurar un adaptador de red virtual para equilibrar la carga del tráfico de red entrante entre varios núcleos de procesador lógico de una máquina virtual. También puede usar vRSS para configurar varios núcleos físicos para una tarjeta \(de interfaz de red virtual del host VNIC.\)

Esta configuración permite que la carga de un adaptador de red virtual se distribuya entre varios procesadores virtuales en una máquina \(virtual\)de máquinas virtuales, lo que permite que la máquina virtual procese más tráfico de red más rápidamente de lo que puede hacerlo con una sola procesador lógico.

>[!TIP]
>Puede usar vRSS en máquinas virtuales en hosts de Hyper\--V que tienen varios procesadores, un solo procesador de varios núcleos o varios procesadores de núcleos instalados y configurados para uso de máquinas virtuales.

vRSS es compatible con todas las demás\-tecnologías de red de Hyper-V. vRSS depende de Virtual Machine Queue \(VMQ\) en el host de\-Hyper-V y RSS en la VM o en el host VNIC.

De forma predeterminada, Windows Server habilita vRSS, pero puede deshabilitarlo en una máquina virtual mediante Windows PowerShell. Para obtener más información, consulte [administrar](vrss-manage.md) los [comandos Vrss y Windows PowerShell para RSS y vRSS](vrss-wps.md).



## <a name="operating-system-compatibility"></a>Compatibilidad del sistema operativo

Puede usar RSS en cualquier equipo multiprocesador o multinúcleo, o vRSS en cualquier máquina virtual multiprocesador o multinúcleo, que ejecute Windows Server 2016.

Las máquinas virtuales multiprocesador o de varios núcleos que ejecutan los siguientes sistemas operativos de Microsoft también admiten vRSS.

- Windows Server 2016
- Windows 10 Pro o Enterprise
- Windows Server 2012 R2
- Windows 8.1 Pro o Enterprise
- Windows Server 2012 con los componentes de integración de Windows Server 2012 R2 instalados.
- Windows 8 con los componentes de integración de Windows Server 2012 R2 instalados.

Para obtener información sobre la compatibilidad de vRSS con máquinas virtuales que ejecutan FreeBSD o Linux como sistema operativo invitado en Hyper-V, consulte [máquinas virtuales Linux y FreeBSD compatibles con Hyper-v en Windows](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows).
  
## <a name="hardware-requirements"></a>Requisitos de hardware

A continuación se indican los requisitos de hardware para vRSS.
 
- Los adaptadores de red físicos deben admitir \(VMQ\)Virtual Machine Queue. Si VMQ está deshabilitado o no se admite, vRSS está deshabilitado para el host de Hyper\--V y las máquinas virtuales configuradas en el host.
- Los adaptadores de red deben tener una velocidad de vínculo de 10 Gbps o más.
- Los\-hosts de Hyper-V deben configurarse con varios procesadores\-o al menos un procesador de varios núcleos para usar vRSS.
- Las máquinas \(\) virtuales deben configurarse para que usen dos o más procesadores lógicos.


## <a name="use-case-scenarios"></a>Escenarios de casos de uso

Los dos escenarios de casos de uso más comunes muestran el uso común de vRSS para el equilibrio de carga del procesador y el equilibrio de carga de software.

### <a name="processor-load-balancing"></a>Equilibrado de la carga del procesador
  
Anthony, un administrador de red, está configurando un nuevo host de Hyper-V con dos adaptadores de red que admite la virtualización \(de\-entrada\)y salida de raíz única SR IOV. Implementa Windows Server 2016 para hospedar un servidor de archivos de máquina virtual.

Después de instalar el hardware y el software, Anthony configura una máquina virtual para usar ocho procesadores virtuales y 4096 MB de memoria. Desafortunadamente, Anthony no tiene la opción de activar Sr\-IOV porque sus máquinas virtuales se basan en la aplicación de directivas a través del conmutador virtual que creó con el administrador de conmutadores virtuales de Hyper\--V. Por este motivo, Anthony decide usar vRSS en lugar de Sr\-IOV.

Inicialmente, Anthony asigna cuatro procesadores virtuales mediante Windows PowerShell para que estén disponibles para su uso con vRSS. El uso del servidor de archivos después de una semana parecía ser bastante popular, por lo que Anthony comprueba el rendimiento de la máquina virtual.  Detecta el uso completo de los cuatro procesadores virtuales.

Por este motivo, Anthony decide agregar procesadores a la máquina virtual para su uso por vRSS.  Asigna dos procesadores virtuales más a la máquina virtual, que están disponibles automáticamente para vRSS para ayudar a administrar la gran carga de red. Su esfuerzo da como resultado un mejor rendimiento para el servidor de archivos de la máquina virtual, con los seis procesadores que controlan eficazmente la carga del tráfico de red.


### <a name="software-load-balancing"></a>Equilibrado de la carga de software

Sandra, un administrador de red, está configurando una única máquina virtual de alto rendimiento en uno de sus sistemas para que actúe como equilibrador de carga de software. Ha asignado todos los procesadores lógicos disponibles a esta única máquina virtual.

Después de instalar Windows Server, utiliza vRSS para obtener el procesamiento del tráfico de red paralelo en varios procesadores lógicos de la máquina virtual. Dado que Windows Server habilita vRSS, Sandra no tiene que realizar ningún cambio de configuración.


## <a name="related-topics"></a>Temas relacionados

- [Planear el uso de vRSS](vrss-plan.md)
- [Habilitar vRSS en un adaptador de Virtual Network](vrss-enable.md)
- [Administrar vRSS](vrss-manage.md)
- [Preguntas más frecuentes sobre vRSS](vrss-faq.md)
- [Comandos de Windows PowerShell para RSS y vRSS](vrss-wps.md)

---
