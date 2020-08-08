---
title: Requisitos del sistema para Hyper-V en Windows Server
description: Enumera los requisitos de hardware y firmware para Hyper-V en Windows Server
manager: dongill
ms.topic: article
ms.assetid: bc4a4971-f727-40cd-91f5-2ee6d24b54cb
author: kbdazure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: d0cbbc79fe1dc942dfe79ca9dbe81769dd112730
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997616"
---
# <a name="system-requirements-for-hyper-v-on-windows-server"></a>Requisitos del sistema para Hyper-V en Windows Server

>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Hyper-V tiene requisitos de hardware específicos y algunas características de Hyper-V tienen requisitos adicionales. Use los detalles de este artículo para decidir qué requisitos debe cumplir el sistema para que pueda usar Hyper-V de la forma en que planea. A continuación, revise el [Catálogo de Windows Server](https://www.windowsservercatalog.com/). Tenga en cuenta que los requisitos de Hyper-V superan los requisitos mínimos generales para Windows Server 2016 porque un entorno de virtualización requiere más recursos informáticos.

Si ya está usando Hyper-V, es probable que pueda usar el hardware existente. Los requisitos generales de hardware no han cambiado significativamente respecto a Windows Server 2012 R2.  Sin embargo, necesitará hardware más reciente para usar máquinas virtuales blindadas o la asignación discreta de dispositivos. Estas características se basan en la compatibilidad de hardware específica, tal y como se describe a continuación. Aparte de eso, la principal diferencia en el hardware es que ahora se requiere la traducción de direcciones de segundo nivel (SLAT) en lugar de la recomendada.

Para obtener más información acerca de las configuraciones máximas admitidas para Hyper-V, como el número de máquinas virtuales en ejecución, consulte [planear la escalabilidad de Hyper-v en Windows Server 2016](./plan/plan-hyper-v-scalability-in-windows-server.md). La lista de sistemas operativos que se pueden ejecutar en las máquinas virtuales se trata en [sistemas operativos invitados de Windows admitidos para Hyper-V en Windows Server](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md).

## <a name="general-requirements"></a>Requisitos generales

Independientemente de las características de Hyper-V que quiera usar, necesitará lo siguiente:

- Un procesador de 64 bits con traducción de direcciones de segundo nivel (SLAT). Para instalar los componentes de virtualización de Hyper-V como el hipervisor de Windows, el procesador debe tener SLAT. Sin embargo, no es necesario instalar las herramientas de administración de Hyper-V como conexión a máquina virtual (VMConnect), el administrador de Hyper-V y los cmdlets de Hyper-V para Windows PowerShell. Consulte "cómo comprobar los requisitos de Hyper-V" a continuación para averiguar si el procesador tiene SLAT.

- Extensiones del modo de monitor de VM

- Suficiente memoria: plan para *al menos* 4 GB de RAM. Más memoria es mejor. Necesitará suficiente memoria para el host y todas las máquinas virtuales que desee ejecutar al mismo tiempo.

- Compatibilidad con la virtualización activada en el BIOS o UEFI:

  - Virtualización asistida por hardware. Está disponible en procesadores que incluyen una opción de virtualización, en concreto, procesadores con tecnología Intel Virtualization Technology (Intel VT) o AMD Virtualization (AMD-V).

  - La prevención de ejecución de datos (DEP) mediante hardware tiene que estar disponible y habilitada. En los sistemas Intel, es el bit XD (bit para deshabilitar la ejecución). En los sistemas AMD, es el bit NX (bit de no ejecución).

## <a name="how-to-check-for-hyper-v-requirements"></a>Comprobación de los requisitos de Hyper-V

Abra Windows PowerShell o un símbolo del sistema y escriba:

```cmd
Systeminfo.exe
```

Desplácese a la sección de requisitos de Hyper-V para revisar el informe.

## <a name="requirements-for-specific-features"></a>Requisitos para características específicas

Estos son los requisitos para la asignación discreta de dispositivos y las máquinas virtuales blindadas. Para obtener descripciones de estas características, consulte [novedades de Hyper-V en Windows Server](What-s-new-in-Hyper-V-on-Windows.md).

### <a name="discrete-device-assignment"></a>Asignación discreta de dispositivos

Los requisitos de **host** son similares a los requisitos existentes para la característica SR-IOV en Hyper-V.

- El procesador debe tener la tabla de páginas extendidas (EPT) de Intel o la tabla de páginas anidada (NPT) de AMD.

- El chipset debe tener:

  - Reasignación de interrupciones: VT-d de Intel con la capacidad de reasignación de interrupciones (VT-D2) o cualquier versión de la unidad de administración de memoria de e/s de AMD (e/s MMU).

  - Reasignación de DMA: Intel VT-d con invalidaciones en cola o cualquier MMU de e/s de AMD.

  - Servicios de control de acceso (ACS) en puertos raíz de PCI Express.

- Las tablas de firmware deben exponer la MMU de e/s al hipervisor de Windows. Tenga en cuenta que esta característica puede estar desactivada en UEFI o BIOS. Para obtener instrucciones, consulte la documentación del hardware o póngase en contacto con el fabricante del hardware.

Los **dispositivos** necesitan GPU o memoria no volátil (NVMe). En GPU, solo algunos dispositivos admiten la asignación discreta de dispositivos. Para comprobarlo, consulte la documentación del hardware o póngase en contacto con el fabricante del hardware. Para obtener más información acerca de esta característica, incluida la forma de usarla y las consideraciones, consulte la entrada "[asignación de dispositivos discretos: Descripción y fondo](https://blogs.technet.com/b/virtualization/archive/2015/11/19/discrete-device-assignment.aspx)" en el blog de virtualización.

### <a name="shielded-virtual-machines"></a>Máquinas virtuales blindadas

Estas máquinas virtuales se basan en la seguridad basada en la virtualización y están disponibles a partir de Windows Server 2016.

Los requisitos de **host** son:

- UEFI 2.3.1 c: admite el arranque medido y seguro

  Los dos siguientes son opcionales para la seguridad basada en la virtualización en general, pero se requieren para el host si se desea la protección que proporcionan estas características:

- TPM v 2.0: protege los recursos de seguridad de la plataforma
- IOMMU (Intel VT-D): para que el hipervisor pueda proporcionar protección de acceso directo a memoria (DMA)

Los requisitos de las **máquinas virtuales** son:

- Generación 2
- Windows Server 2012 o posterior como sistema operativo invitado