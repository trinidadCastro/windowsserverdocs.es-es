---
title: Máquinas virtuales Linux y FreeBSD compatibles con Hyper-V en Windows
description: Enumera las características y servicios de integración de Linux que se incluyen en cada versión
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 990ff94a-30fb-434b-b4a2-3804a5245ba6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 593068f4fc2015c7f8f94bfe49c5a11c23cb6599
ms.sourcegitcommit: 1bc3c229e9688ac741838005ec4b88e8f9533e8a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314977"
---
# <a name="supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows"></a>Máquinas virtuales Linux y FreeBSD compatibles con Hyper-V en Windows

>Se aplica a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Hyper-V admite dispositivos emulados y específicos de Hyper-V para máquinas virtuales Linux y FreeBSD. Cuando se ejecuta con dispositivos emulados, no es necesario instalar ningún software adicional. Sin embargo, los dispositivos emulados no proporcionan un alto rendimiento y no pueden aprovechar la completa infraestructura de administración de máquinas virtuales que ofrece la tecnología Hyper-V. Para hacer un uso completo de todas las ventajas que ofrece Hyper-V, es mejor usar dispositivos específicos de Hyper-V para Linux y FreeBSD. La colección de controladores necesarios para ejecutar dispositivos específicos de Hyper-V se conoce como Linux Integration Services (LIS) o FreeBSD Integration Services (BIS).

LIS se ha agregado al kernel de Linux y se ha actualizado para nuevas versiones. Sin embargo, es posible que las distribuciones de Linux basadas en kernels anteriores no tengan las últimas mejoras o correcciones. Microsoft proporciona una descarga que contiene controladores LIS instalables para algunas instalaciones de Linux basadas en estos kernels anteriores. Dado que los proveedores de distribución incluyen versiones de Linux Integration Services, es mejor instalar la última versión descargable de LIS, si procede, para su instalación.

Para otras distribuciones de Linux, los cambios de LIS se integran periódicamente en el kernel del sistema operativo y las aplicaciones, por lo que no se requiere ninguna descarga o instalación aparte.

En el caso de las versiones anteriores de FreeBSD (antes 10,0), Microsoft proporciona puertos que contienen los controladores de BIS instalables y los demonios correspondientes para máquinas virtuales de FreeBSD. En el caso de las versiones más recientes de FreeBSD, BIS está integrado en el sistema operativo FreeBSD, y no se requiere ninguna descarga o instalación independiente, salvo una descarga de puertos KVP necesaria para FreeBSD 10,0.

> [!TIP]
> - Descargue [Windows Server 2019](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019) desde el centro de evaluación.

El objetivo de este contenido es proporcionar información que ayude a facilitar la implementación de Linux o FreeBSD en Hyper-V. Los detalles específicos incluyen:

* Distribuciones de Linux o versiones de FreeBSD que requieren la descarga e instalación de controladores LIS o BIS.

* Distribuciones de Linux o versiones de FreeBSD que contienen controladores LIS o BIS integrados.

* Mapas de distribución de características que indican las características de las principales distribuciones de Linux o versiones de FreeBSD.

* Problemas conocidos y soluciones alternativas para cada distribución o versión.

* Descripción de la característica para cada característica de LIS o BIS.

**¿Desea hacer una sugerencia sobre las características y la funcionalidad?** ¿Hay algo que podríamos mejorar? Puede usar el sitio de [Windows Server User Voice](https://windowsserver.uservoice.com/forums/295062-linux-support) para sugerir nuevas características y capacidades para Linux y FreeBSD virtual machines en Hyper-V y para ver qué dicen otras personas.

## <a name="in-this-section"></a>En esta sección

* [Compatibilidad con máquinas virtuales de alta y Red Hat Enterprise Linux en Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales Debian admitidas en Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Oracle Linux compatibles con máquinas virtuales en Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de SUSE compatibles en Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de Ubuntu admitidas en Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de FreeBSD compatibles en Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descripciones de características de máquinas virtuales Linux y FreeBSD en Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Prácticas recomendadas para ejecutar Linux en Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Prácticas recomendadas para ejecutar FreeBSD en Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
