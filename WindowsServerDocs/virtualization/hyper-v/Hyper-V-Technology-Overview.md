---
title: Introducción a la tecnología Hyper-V
description: ¿Qué es Hyper-V, se describe cómo obtener, las características clave y los usos más comunes.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac069fed-7bf5-4cc3-aff5-25a2766040b8
author: KBDAzure
ms.author: kathydav
ms.date: 11/29/2016
ms.openlocfilehash: 9ae3c9dce36ad7d67a19ce167c9cb875b3c91810
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864316"
---
# <a name="hyper-v-technology-overview"></a>Introducción a la tecnología Hyper-V

>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server, Microsoft Hyper-V Server 2019 de 2019

Hyper-V es producto de virtualización de hardware de Microsoft. Le permite crear y ejecutar una versión de software de un equipo, denominado un *máquina virtual*. Cada máquina virtual actúa como un equipo completo, que ejecutan un sistema operativo y programas. Cuando necesite los recursos informáticos, las máquinas virtuales proporcionan más flexibilidad, ayudar a ahorrar tiempo y dinero y son una forma más eficaz utilizar hardware que simplemente ejecutando un sistema operativo en hardware físico.

Hyper-V ejecuta cada máquina virtual en su propio espacio aislado, lo que significa que puede ejecutar más de una máquina virtual en el mismo hardware al mismo tiempo. Es posible que desee hacerlo para evitar problemas, como un bloqueo que afecte a otras cargas de trabajo, o para proporcionar acceso de servicios, grupos o usuarios diferentes a diferentes sistemas.

## <a name="some-ways-hyper-v-can-help-you"></a>Algunas formas de Hyper-V puede ayudarle a

Hyper-V puede ayudarle:

- **Establecer o ampliar un entorno de nube privada.** Proporcionar servicios de TI más flexibles, y a petición moviendo a o ampliando el uso de recursos compartidos y ajustar la utilización a medida que cambia la demanda.

- **Usar el hardware de forma más eficaz.** Consolidar servidores y cargas de trabajo en equipos físicos menos, más eficaces utilizar menos energía y espacio físico.

- **Mejorar la continuidad empresarial.** Minimizar el impacto del tiempo de inactividad programado y de las cargas de trabajo.

- **Establecer o ampliar una infraestructura de escritorio virtual (VDI).** Use una estrategia de escritorio centralizado con VDI puede ayudar a aumentar la agilidad empresarial y la seguridad de los datos, así como simplificar el cumplimiento y administración de sistemas operativos de escritorio y aplicaciones. Implementar Hyper-V y Host de virtualización de escritorio remoto (Host de virtualización de escritorio remoto) en el mismo servidor para que estén disponibles los escritorios virtuales personales o grupos de escritorios virtuales a los usuarios.

- **Hacer que el desarrollo y pruebas más eficaz.** Reproducir diferentes entornos informáticos sin tener que comprar o mantener todo el hardware que tendría si solo usa los sistemas físicos.

## <a name="hyper-v-and-other-virtualization-products"></a>Hyper-V y otros productos de virtualización

Hyper-V en Windows y Windows Server reemplaza los productos de virtualización de hardware más antiguos, como Microsoft Virtual PC, Microsoft Virtual Server y Windows Virtual PC. Hyper-V ofrece características de seguridad, rendimiento, almacenamiento y redes no están disponibles en estos productos anteriores.

Hyper-V y la mayoría de las aplicaciones de virtualización de terceros que requieren las mismas características de procesador no es compatible. Eso es porque las características del procesador, conocidas como extensiones de virtualización de hardware, están diseñadas para que no se puede compartir. Para obtener más información, consulte [no funcionan las aplicaciones de virtualización con Hyper-V, Device Guard y Credential Guard](https://support.microsoft.com/kb/3204980).

## <a name="what-features-does-hyper-v-have"></a>¿Qué características tiene Hyper-V?

Hyper-V ofrece muchas características. Se trata de obtener información general, agrupado por lo que proporcionan las características o ayudarle a hacerlo.

**Entorno informático** -máquina virtual A Hyper-V incluye las mismas partes básicas, como un equipo físico, como memoria, procesador, almacenamiento y redes. Todas estas partes tienen características y opciones que puede configurar maneras diferentes para satisfacer distintas necesidades. Almacenamiento y redes de cada uno se pueden considerar las categorías de sus propios, debido a las numerosas formas en puede configurar.

**Copia de seguridad y recuperación ante desastres** -recuperación ante desastres, réplica de Hyper-V crea copias de las máquinas virtuales, diseñadas para almacenarse en otra ubicación física, por lo que puede restaurar la máquina virtual desde la copia. Para la copia de seguridad, Hyper-V ofrece dos tipos. Uno usa estados guardados y el otro usa el servicio de instantáneas de volumen (VSS) para que pueda realizar copias de seguridad coherentes con la aplicación para programas compatibles con VSS.

**Optimización** -cada sistema operativo invitado admitido tiene un conjunto personalizado de servicios y controladores, llamados *servicios de integración*, que facilitan más fácil de usar el sistema operativo en una máquina virtual de Hyper-V.

**Portabilidad** : características tales como migración en vivo, migración de almacenamiento, y import/export que sea más fácil mover o distribuir una máquina virtual.

**Conectividad remota** -Hyper-V incluye la conexión a máquina Virtual, una herramienta de conexión remota para su uso con Windows y Linux. A diferencia de escritorio remoto, esta herramienta le proporciona consola acceso, por lo que puede ver lo que sucede en el invitado incluso cuando el sistema operativo no se ha iniciado todavía.

**Seguridad** -arranque seguro y las máquinas virtuales blindadas ayudan a protegerse frente a malware y otro acceso no autorizado a una máquina virtual y sus datos.

Para obtener un resumen de las características introducidas en esta versión, consulte [cuáles son las novedades en Hyper-V en Windows Server](What-s-new-in-Hyper-V-on-Windows.md). Algunas características o elementos tienen un límite en cuántos puede configurarse. Para obtener más información, consulte [planear la escalabilidad de Hyper-V en Windows Server 2016](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).

## <a name="how-to-get-hyper-v"></a>Obtención de Hyper-V

Hyper-V está disponible en Windows Server y Windows, como un rol de servidor disponible para x64 versiones de Windows Server. Para obtener instrucciones de servidor, consulte [instalar el rol de Hyper-V en Windows Server](get-started/Install-the-Hyper-V-role-on-Windows-Server.md). En Windows, está disponible como [característica](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index) en algunas versiones de 64 bits de Windows. También está disponible como un producto descargable, de servidor independiente, [Microsoft Hyper-V Server](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2019).

## <a name="supported-operating-systems"></a>Sistemas operativos compatibles

Muchos sistemas operativos se ejecutará en las máquinas virtuales. En general, un sistema operativo que usa una arquitectura que se ejecutará en una máquina virtual de Hyper-V de x86. No todos los sistemas operativos que se pueden ejecutar son probados y compatible con Microsoft, sin embargo. Para listas de lo que se admite, consulte:

- [Linux y FreeBSD las máquinas virtuales compatibles para Hyper-V en Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

- [Windows invitado sistemas operativos compatibles con Hyper-V en Windows Server](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)

## <a name="how-hyper-v-works"></a>Cómo funciona el Hyper-V

Hyper-V es una tecnología de virtualización basada en hipervisor. Hyper-V usa el hipervisor de Windows, lo que requiere un procesador físico con características específicas. Para obtener detalles de hardware, consulte [requisitos del sistema para Hyper-V en Windows Server](System-requirements-for-Hyper-V-on-Windows.md).

En la mayoría de los casos, el hipervisor administra las interacciones entre el hardware y las máquinas virtuales. Este acceso controlado por el hipervisor en el hardware ofrece las máquinas virtuales en el entorno aislado en que se ejecutan. En algunas configuraciones, una máquina virtual o el sistema operativo que se ejecuta en la máquina virtual tiene acceso directo al hardware de gráficos, redes o almacenamiento.

## <a name="what-does-hyper-v-consist-of"></a>¿Qué constan de Hyper-V?

Hyper-V ha requerido de elementos que funcionan conjuntamente para que pueda crear y ejecutar máquinas virtuales. Juntas, estas partes se llama a la plataforma de virtualización. Cuando se instala el rol Hyper-V están instalados como un conjunto. Los elementos necesarios incluyen el hipervisor de Windows, servicio de administración de máquinas virtuales de Hyper-V, el proveedor WMI de virtualización, el bus de máquina virtual (VMbus), el proveedor de servicios de virtualización (VSP) y la infraestructura virtual (VID) del controlador.

Hyper-V también dispone de herramientas de administración y la conectividad. Puede instalarlos en el mismo equipo que el rol de Hyper-V está instalado en y en equipos sin el rol de Hyper-V instalado. Estas herramientas son:

- Administrador de Hyper-V
- [Módulo de Hyper-V para Windows PowerShell](https://docs.microsoft.com/powershell/module/hyper-v/index)
- [Conexión a máquina virtual](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/hyper-v-virtual-machine-connect) \(a veces denominado VMConnect\)
- [Windows PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)

## <a name="related-technologies"></a>Tecnologías relacionadas

Éstas son algunas tecnologías de Microsoft que se suelen usar con Hyper-V:

- [Agrupación en clústeres de conmutación por error](../../failover-clustering/whats-new-in-failover-clustering.md)
- [Servicios de escritorio remoto](../../remote/remote-desktop-services/Host-desktops-and-apps-in-Remote-Desktop-Services.md)
- [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)

Diversas tecnologías de almacenamiento:, SMB 3.0, los volúmenes compartidos de clúster de espacios de almacenamiento directo

Los contenedores de Windows ofrecen otro enfoque para la virtualización. Consulte la [contenedores Windows](https://docs.microsoft.com/virtualization/windowscontainers/index) library en MSDN.
