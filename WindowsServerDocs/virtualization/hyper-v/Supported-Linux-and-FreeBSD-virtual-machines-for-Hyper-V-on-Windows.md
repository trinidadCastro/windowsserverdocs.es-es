---
title: Linux y FreeBSD las máquinas virtuales compatibles para Hyper-V en Windows
description: Enumera los servicios de integración de Linux y características incluidas en cada versión
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
ms.openlocfilehash: 9df495bdc67b06a675fec050fb4c2960337ce8ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832906"
---
# <a name="supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows"></a>Linux y FreeBSD las máquinas virtuales compatibles para Hyper-V en Windows

>Se aplica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Hyper-V es compatible con dispositivos emulados y específicos de Hyper-V para máquinas virtuales Linux y FreeBSD. Cuando se ejecuta con dispositivos emulados, no es necesario instalar ningún software adicional. Sin embargo, los dispositivos emulados no proporcionan un alto rendimiento y no pueden aprovechar la infraestructura de administración de la máquina virtual enriquecido que ofrece la tecnología Hyper-V. Para hacer un uso completo de todas las ventajas que proporciona Hyper-V, es mejor usar dispositivos específicos de Hyper-V para Linux y FreeBSD. La colección de controladores que son necesarios para ejecutar los dispositivos específicos de Hyper-V se conocen como servicios de integración de Linux (LIS) o servicios de integración de FreeBSD (BIS).

LIS se ha agregado al kernel de Linux y se actualiza para las nuevas versiones. Pero las distribuciones de Linux en función de los kernels anteriores no pueden tener las últimas mejoras o correcciones. Microsoft ofrece una descarga que contiene los controladores LIS instalables de algunas instalaciones de Linux según estos kernels anteriores. Dado que los proveedores de distribución incluyen las versiones de Linux Integration Services, es mejor instalar la última versión descargable de LIS, si corresponde, para la instalación.

Para otras distribuciones de Linux LIS los cambios se integran con regularidad en el núcleo del sistema operativo y las aplicaciones por lo que no es necesaria ninguna instalación o una descarga independiente.

Para las versiones anteriores de FreeBSD (antes 10.0), Microsoft proporciona los puertos que contienen los controladores instalables de BIS y daemons correspondientes para las máquinas virtuales de FreeBSD. Para las versiones más recientes de FreeBSD, BIS está integrado en el sistema operativo FreeBSD y es necesaria excepto una descarga KVP puertos que se necesita para FreeBSD 10.0 ninguna descarga independiente o instalación.

> [!TIP]
> - Descargar [Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016) desde el centro de evaluación.
> - Descargar [Microsoft Hyper-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016) desde el centro de evaluación.

El objetivo de este contenido es proporcionar información que ayuda a facilitar la implementación de Linux ni de FreeBSD en Hyper-V. Incluyen detalles específicos:

* Versiones de FreeBSD que requieren la descarga e instalación de controladores LIS o BIS o las distribuciones de Linux.

* Distribuciones de Linux o versiones de FreeBSD que contienen controladores LIS o BIS integrados.

* Versiones de FreeBSD o mapas de distribución de característica que indican las características de las principales distribuciones de Linux.

* Problemas conocidos y soluciones alternativas para cada distribución o lanzamiento.

* Descripción de la característica para cada característica LIS o BIS.

**¿Desea hacer una sugerencia sobre características y funcionalidades?** ¿Hay algo que podíamos hacer mejor? Puede usar el [Windows Server User Voice](https://windowsserver.uservoice.com/forums/295062-linux-support) sitio para sugerir nuevas características y capacidades para Linux y FreeBSD máquinas virtuales Hyper-V y para ver lo que dicen otros usuarios.

## <a name="in-this-section"></a>En esta sección

* [Admite CentOS y Red Hat Enterprise Linux virtual machines en Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Admite máquinas virtuales de Debian en Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales Oracle Linux compatibles en Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales SUSE compatibles en Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales de Ubuntu compatibles en Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales de FreeBSD compatibles en Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descripciones de características para las máquinas virtuales de Linux y FreeBSD en Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Procedimientos recomendados para ejecutar Linux en Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Procedimientos recomendados para ejecutar FreeBSD en Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
