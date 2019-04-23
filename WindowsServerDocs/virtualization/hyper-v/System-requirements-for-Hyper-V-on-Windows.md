---
title: Requisitos del sistema para Hyper-V en Windows Server
description: Enumera los requisitos de hardware y firmware para Hyper-V en Windows Server
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bc4a4971-f727-40cd-91f5-2ee6d24b54cb
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 55114821b5ac2f1cc028c662217f4bee6980c923
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845206"
---
# <a name="system-requirements-for-hyper-v-on-windows-server"></a>Requisitos del sistema para Hyper-V en Windows Server

>Se aplica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server, Microsoft Hyper-V Server 2019 de 2019

Hyper-V tiene requisitos de hardware específicos, y algunas características de Hyper-V tienen requisitos adicionales. Use los detalles en este artículo para decidir los requisitos que el sistema debe cumplir para que pueda usar Hyper-V de la manera en que piensa. A continuación, revise el [catálogo de Windows Server](https://www.windowsservercatalog.com/). Tenga en cuenta que los requisitos para Hyper-V superan los requisitos mínimos generales para Windows Server 2016 porque un entorno de virtualización requiere más recursos informáticos.

Si ya está utilizando Hyper-V, es probable que puede usar el hardware existente. Los requisitos de hardware general no han cambiado significativamente desde Windows Server 2012 R2.  Sin embargo, necesitará un hardware más reciente para las máquinas virtuales de uso blindada o asignación discreta de dispositivos. Estas características dependen de compatibilidad de hardware específico, como se describe a continuación. Aparte de eso, la principal diferencia en el hardware es esa dirección de segundo nivel traducción (SLAT) ahora es obligatorio en lugar de recomendado.

Para obtener más información sobre configuraciones admitidas máximas para Hyper-V, como el número de máquinas virtuales en funcionamiento, consulte [planear la escalabilidad de Hyper-V en Windows Server 2016](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md). La lista de sistemas operativos que puede ejecutar en las máquinas virtuales se trata en [Windows admite sistemas operativos invitados para Hyper-V en Windows Server](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md).

## <a name="general-requirements"></a>Requisitos generales

Independientemente de las características de Hyper-V que desea usar, necesitará:

- Un procesador de 64 bits con traducción de direcciones de segundo nivel (SLAT). Para instalar los componentes de virtualización de Hyper-V como el hipervisor de Windows, el procesador debe SLAT. Sin embargo, no es necesario para instalar las herramientas de administración de Hyper-V como conexión a máquina Virtual (VMConnect), el Administrador de Hyper-V y los cmdlets de Hyper-V para Windows PowerShell. Consulte "Cómo comprobar los requisitos de Hyper-V," a continuación averiguar si su procesador tiene SLAT.

- Extensiones de modo de supervisión de la máquina virtual

- Memoria insuficiente: plan para *al menos* 4 GB de RAM. Es mejor más memoria. Necesitará suficiente memoria para el host y todas las máquinas virtuales que se va a ejecutar al mismo tiempo.

- Compatibilidad con la virtualización activado en el BIOS o UEFI:

  - Virtualización asistida por hardware. Esto está disponible en procesadores que incluyen una opción de virtualización - específicamente los procesadores con Intel Virtualization Technology (Intel VT) o la tecnología AMD Virtualization (AMD-V).

  - La Prevención de ejecución de datos (DEP) implementada por hardware debe estar disponible y habilitada. Para los sistemas de Intel, esto es el bit XD (execute disable bit). Para los sistemas AMD, esto no es el NX (bit ejecutar).

## <a name="bkmk_CheckReq"></a>Cómo comprobar los requisitos de Hyper-V

Abra Windows PowerShell o un símbolo del sistema y escriba:

```cmd
Systeminfo.exe
```

Desplácese hasta la sección de requisitos de Hyper-V para revisar el informe.

## <a name="requirements-for-specific-features"></a>Requisitos de características específicas

Estos son los requisitos para la asignación discreta de dispositivos y las máquinas virtuales blindadas. Para obtener descripciones de estas características, consulte [cuáles son las novedades en Hyper-V en Windows Server](What-s-new-in-Hyper-V-on-Windows.md).

### <a name="discrete-device-assignment"></a>Asignación discreta de dispositivos

**Host** requisitos son similares a los requisitos para la característica de SR-IOV en Hyper-V existentes.

- El procesador debe tener ya sea Intel Extended página tabla (EPT) o tabla de página anidadas (NPT de AMD).

- Debe tener el conjunto de chips:

  - Reasignación - de interrupciones VT-d de Intel con la capacidad de reasignación de interrupciones (VT-d2) o cualquier versión de la unidad de administración de memoria (MMU E/S) de AMD E/S.

  - Reasignación de DMA - Intel VT-d con invalidaciones en cola o cualquier MMU de E/S de AMD.

  - Servicios de control de acceso (ACS) en los puertos de raíz de PCI Express.

- Las tablas de firmware deben exponer la E/S MMU el hipervisor de Windows. Tenga en cuenta que esta característica podría estar desactivada en el UEFI o BIOS. Para obtener instrucciones, consulte la documentación del hardware o póngase en contacto con el fabricante del hardware.

**Dispositivos** necesita express GPU o memoria no volátil (NVMe). Para GPU, sólo ciertos dispositivos admiten la asignación discreta de dispositivos. Para comprobar, consulte la documentación del hardware o póngase en contacto con el fabricante del hardware. Para obtener más información sobre esta característica, incluido cómo usarla y consideraciones, consulte la entrada "[discretos asignación del dispositivo: descripción y en segundo plano](https://blogs.technet.com/b/virtualization/archive/2015/11/19/discrete-device-assignment.aspx)" en el blog de virtualización.

### <a name="shielded-virtual-machines"></a>Máquinas virtuales blindadas

Estas máquinas virtuales se basan en la seguridad basada en virtualización y están disponibles a partir de Windows Server 2016.

**Host** requisitos son:

- UEFI 2.3. 1C - admite el arranque seguro, medido

  Los siguientes dos son opcionales para la seguridad basada en virtualización en general, pero necesario para el host si desea que la protección de que estas características ofrecen:

- Versión 2.0 TPM - protege los activos de seguridad de plataforma
- IOMMU (Intel VT-D): por lo que el hipervisor puede proporcionar protección de memoria directa (DMA) de acceso

**Máquina virtual** requisitos son:

- Generación 2
- Windows Server 2012 o posterior como sistema operativo invitado

