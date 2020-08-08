---
title: Información general sobre la tecnología Hyper-V
description: Describe qué es Hyper-V, cómo obtenerlo, las características clave y los usos comunes.
manager: dongill
ms.topic: article
ms.assetid: ac069fed-7bf5-4cc3-aff5-25a2766040b8
author: kbdazure
ms.author: kathydav
ms.date: 11/29/2016
ms.openlocfilehash: 5fd4c0199cea04d6697b593ad70b4f31b55afad0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960768"
---
# <a name="hyper-v-technology-overview"></a>Información general sobre la tecnología Hyper-V

>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Hyper-V es el producto de virtualización de hardware de Microsoft. Permite crear y ejecutar una versión de software de un equipo, denominada *máquina virtual*. Cada máquina virtual actúa como un equipo completo en el que se ejecuta un sistema operativo y programas. Cuando necesite recursos informáticos, las máquinas virtuales le ofrecen mayor flexibilidad, le ayudan a ahorrar tiempo y dinero, y son una manera más eficaz de usar hardware que simplemente ejecutar un sistema operativo en hardware físico.

Hyper-V ejecuta cada máquina virtual en su propio espacio aislado, lo que significa que puede ejecutar más de una máquina virtual en el mismo hardware al mismo tiempo. Es posible que desee hacer esto para evitar problemas, como un bloqueo que afecte a otras cargas de trabajo, o para dar acceso a diferentes usuarios, grupos o servicios a distintos sistemas.

## <a name="some-ways-hyper-v-can-help-you"></a>Algunas formas de que Hyper-V le ayude

Hyper-V puede ayudarle a:

- **Establecer o ampliar un entorno de nube privado.** Proporcione servicios de TI más flexibles y a petición al migrar o ampliar el uso de recursos compartidos y ajustar el uso a medida que cambia la demanda.

- **Use el hardware de forma más eficaz.** Consolide servidores y cargas de trabajo en menos equipos físicos más eficaces para usar menos energía y espacio físico.

- **Mejorar la continuidad empresarial.** Minimice el impacto de los tiempos de inactividad programados y no programados de las cargas de trabajo.

- **Establecer o ampliar una infraestructura de escritorio virtual (VDI).** Usar una estrategia de escritorio centralizada con VDI puede ayudarle a aumentar la agilidad empresarial y la seguridad de los datos, así como a simplificar el cumplimiento normativo y administrar sistemas operativos y aplicaciones de escritorio. Implemente Hyper-V y Host de virtualización de Escritorio remoto (host de virtualización de escritorio remoto) en el mismo servidor para que los escritorios virtuales personales o los grupos de escritorios virtuales estén disponibles para los usuarios.

- **Haga que el desarrollo y las pruebas sean más eficaces.** Reproduzca diferentes entornos informáticos sin tener que comprar o mantener todo el hardware que necesita si solo usa sistemas físicos.

## <a name="hyper-v-and-other-virtualization-products"></a>Hyper-V y otros productos de virtualización

Hyper-V en Windows y Windows Server reemplaza los productos de virtualización de hardware más antiguos, como Microsoft Virtual PC, Microsoft Virtual Server y Windows Virtual PC. Hyper-V ofrece funciones de red, rendimiento, almacenamiento y seguridad que no están disponibles en estos productos anteriores.

Hyper-V y la mayoría de las aplicaciones de virtualización de terceros que requieren las mismas características de procesador no son compatibles. Esto se debe a que las características del procesador, conocidas como extensiones de virtualización de hardware, están diseñadas para no ser compartidas. Para obtener más información, consulte [las aplicaciones de virtualización no funcionan en conjunto con Hyper-V, Device Guard y Credential Guard](https://support.microsoft.com/kb/3204980).

## <a name="what-features-does-hyper-v-have"></a>¿Qué características tiene Hyper-V?

Hyper-V ofrece muchas características. Se trata de una introducción, agrupada por lo que proporcionan las características o ayudarle.

**Entorno informático** : una máquina virtual de Hyper-V incluye las mismas partes básicas que un equipo físico, como la memoria, el procesador, el almacenamiento y las redes. Todas estas partes tienen características y opciones que se pueden configurar de diferentes maneras para satisfacer diferentes necesidades. El almacenamiento y las redes pueden considerarse como categorías propias, debido a las muchas maneras en las que puede configurarlas.

**Recuperación ante desastres y copia de seguridad** : para la recuperación ante desastres, la réplica de Hyper-V crea copias de máquinas virtuales, diseñadas para almacenarse en otra ubicación física, por lo que puede restaurar la máquina virtual a partir de la copia. Para la copia de seguridad de, Hyper-V ofrece dos tipos. Uno utiliza los Estados guardados y el otro USA Servicio de instantáneas de volumen (VSS) para que pueda realizar copias de seguridad coherentes con la aplicación para programas compatibles con VSS.

**Optimización** : cada sistema operativo invitado compatible tiene un conjunto personalizado de servicios y controladores, denominados *Integration Services*, que facilitan el uso del sistema operativo en una máquina virtual de Hyper-V.

**Portabilidad** : características como migración en vivo, migración de almacenamiento e importación y exportación facilitan el traslado o la distribución de una máquina virtual.

**Conectividad remota** : Hyper-V incluye conexión a máquina virtual, una herramienta de conexión remota para su uso con Windows y Linux. A diferencia de Escritorio remoto, esta herramienta le proporciona acceso a la consola, por lo que puede ver lo que sucede en el invitado incluso cuando el sistema operativo no se ha iniciado todavía.

Las máquinas virtuales blindadas y de arranque seguro de **seguridad** ayudan a protegerse contra malware y otro acceso no autorizado a una máquina virtual y sus datos.

Para obtener un resumen de las características introducidas en esta versión, consulte [novedades de Hyper-V en Windows Server](What-s-new-in-Hyper-V-on-Windows.md). Algunas características o partes tienen un límite de número de elementos que se pueden configurar. Para obtener más información, vea [planear la escalabilidad de Hyper-V en Windows Server 2016](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).

## <a name="how-to-get-hyper-v"></a>Cómo obtener Hyper-V

Hyper-V está disponible en Windows Server y Windows, como un rol de servidor disponible para las versiones x64 de Windows Server. Para obtener instrucciones sobre el servidor, consulte [instalar el rol Hyper-V en Windows Server](get-started/Install-the-Hyper-V-role-on-Windows-Server.md). En Windows, está disponible como [característica](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index) en algunas versiones de 64 bits de Windows. También está disponible como un producto de servidor independiente descargable, [Microsoft Hyper-V Server](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2019).

## <a name="supported-operating-systems"></a>Sistemas operativos admitidos

Muchos sistemas operativos se ejecutarán en máquinas virtuales. En general, un sistema operativo que usa una arquitectura x86 se ejecutará en una máquina virtual de Hyper-V. No obstante, Microsoft no admite todos los sistemas operativos que se pueden ejecutar. Para obtener listas de lo que se admite, consulte:

- [Máquinas virtuales Linux y FreeBSD compatibles con Hyper-V en Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

- [Sistemas operativos invitados de Windows admitidos para Hyper-V en Windows Server](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)

## <a name="how-hyper-v-works"></a>Funcionamiento de Hyper-V

Hyper-V es una tecnología de virtualización basada en hipervisor. Hyper-V usa el hipervisor de Windows, que requiere un procesador físico con características específicas. Para obtener información detallada sobre el hardware, consulte [requisitos del sistema para Hyper-V en Windows Server](System-requirements-for-Hyper-V-on-Windows.md).

En la mayoría de los casos, el hipervisor administra las interacciones entre el hardware y las máquinas virtuales. Este acceso controlado por hipervisor al hardware proporciona a las máquinas virtuales el entorno aislado en el que se ejecutan. En algunas configuraciones, una máquina virtual o el sistema operativo que se ejecuta en la máquina virtual tiene acceso directo a gráficos, redes o hardware de almacenamiento.

## <a name="what-does-hyper-v-consist-of"></a>¿Qué consiste en Hyper-V?

Hyper-V ha requerido partes que funcionan conjuntamente para que pueda crear y ejecutar máquinas virtuales. Juntas, estas partes se denominan la plataforma de virtualización. Se instalan como un conjunto al instalar el rol de Hyper-V. Entre las partes necesarias se incluyen el hipervisor de Windows, el servicio de administración de máquinas virtuales de Hyper-V, el proveedor de WMI de virtualización, el bus de máquina virtual (VMbus), el proveedor de servicios de virtualización (VSP) y el controlador de infraestructura virtual (VID).

Hyper-V también tiene herramientas para la administración y la conectividad. Puede instalarlos en el mismo equipo en el que está instalado el rol de Hyper-V y en los equipos sin el rol de Hyper-V instalado. Estas herramientas son:

- Administrador de Hyper-V
- [Módulo de Hyper-V para Windows PowerShell](https://docs.microsoft.com/powershell/module/hyper-v/index)
- Conexión de máquina [virtual](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/hyper-v-virtual-machine-connect) \( a veces denominado VMConnect\)
- [Windows PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)

## <a name="related-technologies"></a>Tecnologías relacionadas

Estas son algunas tecnologías de Microsoft que se suelen usar con Hyper-V:

- [Clústeres de conmutación por error](../../failover-clustering/whats-new-in-failover-clustering.md)
- [Servicios de Escritorio remoto](../../remote/remote-desktop-services/Host-desktops-and-apps-in-Remote-Desktop-Services.md)
- [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)

Varias tecnologías de almacenamiento: volúmenes compartidos de clúster, SMB 3,0, espacios de almacenamiento directo

Los contenedores de Windows ofrecen otro enfoque para la virtualización. Vea la biblioteca de [contenedores de Windows](https://docs.microsoft.com/virtualization/windowscontainers/index) en MSDN.
