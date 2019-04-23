---
title: Admite CentOS y Red Hat Enterprise Linux virtual machines en Hyper-V
description: Enumera las versiones de Linux integration services para compatibles CentOS y Red Hat Enterprise distribuciones
ms.prod: windows-server-threshold
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4bf8783d-dee5-4b3e-8cce-2b11b117c189
author: danihalfin
ms.author: daniha
ms.date: 12/20/2017
ms.openlocfilehash: 6bf15e5bfff4b875c4debd3c682bbdccd81a7bb6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869436"
---
# <a name="supported-centos-and-red-hat-enterprise-linux-virtual-machines-on-hyper-v"></a>Admite CentOS y Red Hat Enterprise Linux virtual machines en Hyper-V

>Se aplica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

La característica distribución asignaciones indican las características que están presentes en las versiones integradas y descargables de Linux Integration Services. Después de las tablas se enumeran los problemas conocidos y soluciones alternativas para cada distribución.

Los controladores de Red Hat Enterprise Linux Integration Services integrados para Hyper-V (disponible desde la Red Hat Enterprise Linux 6.4) son suficientes para los invitados de Red Hat Enterprise Linux ejecutar con los dispositivos sintéticos de alto rendimiento en los hosts de Hyper-V. Estos controladores integrados están certificados por Red Hat para este uso. Las configuraciones de certificado se pueden ver en esta página web de Red Hat: [Catálogo de certificación de Red Hat](https://access.redhat.com/ecosystem/search/#/ecosystem/Red%20Hat%20Enterprise%20Linux?sort=sortTitle%20asc&vendors=Microsoft&category=Server). No es necesario descargar e instalar paquetes de Linux Integration Services desde Microsoft Download Center, y si lo hace por lo que puede limitar el soporte técnico de Red Hat como se describe en el artículo de Knowledgebase de Red Hat 1067: [Base de conocimiento de Red Hat 1067](https://access.redhat.com/articles/1067).

Debido a posibles conflictos entre la compatibilidad integrada de LIS y la compatibilidad LIS descargable cuando se actualiza el kernel, deshabilitar las actualizaciones automáticas, desinstale los paquetes descargables de LIS, actualice el kernel, reinicio y después instalar el LIS más reciente de versión, y reiniciar de nuevo.

>[!NOTE]
>Información de certificación de Red Hat Enterprise Linux oficial está disponible a través de la [Red Hat Customer Portal](https://access.redhat.com/ecosystem/search/#/category/Server?sort=sortTitle%20asc&query=windows%20server&ecosystem=Red%20Hat%20Enterprise%20Linux).

En esta sección:

* [RHEL/CentOS 7.x Series](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_7x)

* [RHEL/CentOS 6.x Series](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_6x)

* [RHEL/CentOS 5.x Series](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_5x)

* [Notas de la](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_notes)

## <a name="table-legend"></a>Leyenda de la tabla

* **Integrado** -LIS se incluye como parte de esta distribución de Linux. Los números de versión del módulo de kernel para la compilación en LIS (tal como se muestra por **lsmod**, por ejemplo) son diferentes desde el número de versión del paquete de descarga LIS proporcionada por Microsoft. Un error de coincidencia no indica que la compilación en LIS está obsoleta.

* &#10004;-Característica disponible

* (*en blanco*)-característica no está disponible

## <a name="BKMK_7x"></a>RHEL/CentOS 7.x Series

Esta serie tiene sólo los kernels de 64 bits.

|**Característica**|**Versión de Windows Server**|**7.5-7.6**|**7.3-7.4**|**7.0-7.2**|**7.5-7.6**|**7.4**|**7.3**|**7.2**|**7.1**|**7.0**|
|-|-|-|-|-|-|-|-|-|-|-|
|**Disponibilidad**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|Integrado|Integrado|Integrado|Integrado|Integrado|Integrado||
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Hora exacta de Windows Server 2016|2019, 2016|&#10004;|&#10004;|||||||
|**[Funciones de red](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**|||||||
|Tramas gigantes|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Etiquetado de VLAN y enlace troncal|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migración en vivo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Inserción de la dirección IP estática|2019, 2016, 2012 R2, 2012|&#10004;Nota 2|&#10004;Nota 2|&#10004;Nota 2|&#10004;Nota 2|&#10004;Nota 2|&#10004;Nota 2|&#10004;Nota 2|&#10004;Nota 2|&#10004;Nota 2|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|Segmentación de TCP y las descargas de suma de comprobación|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;||&#10004;|&#10004;|&#10004;|||
|**[Storage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**||||
|Cambio de tamaño de VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Canal de fibra virtual|2019, 2016, 2012 R2|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|
|Copia de seguridad de máquina virtual en vivo|2019, 2016, 2012 R2|&#10004;Nota 5|&#10004;Nota 5|&#10004;Nota 5|&#10004;Tenga en cuenta 4,5|&#10004;Nota 4, 5|&#10004;Nota 4, 5|&#10004;Nota 4, 5|&#10004;Nota 4, 5|&#10004;Nota 4, 5|
|RECORTAR el soporte técnico|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||
|SCSI WWN|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**||||
|Soporte técnico de núcleo PAE|2019, 2016, 2012 R2, 2012, 2008 R2|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Configuración de gap MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memoria dinámica - agregado en caliente|2019, 2016, 2012 R2, 2012|&#10004;Tenga en cuenta 8, 9, 10|&#10004;Tenga en cuenta 8, 9, 10|&#10004;Tenga en cuenta 8, 9, 10|&#10004;Tenga en cuenta 9, 10|&#10004;Tenga en cuenta 9, 10|&#10004;Tenga en cuenta 9, 10|&#10004;Tenga en cuenta 9, 10|&#10004;Tenga en cuenta 9, 10|&#10004;Tenga en cuenta 8, 9, 10|
|Memoria dinámica - incremento|2019, 2016, 2012 R2, 2012|&#10004;Tenga en cuenta 8, 9, 10|&#10004;Tenga en cuenta 8, 9, 10|&#10004;Tenga en cuenta 8, 9, 10|&#10004;Tenga en cuenta 9, 10|&#10004;Tenga en cuenta 9, 10|&#10004;Tenga en cuenta 9, 10|&#10004;Tenga en cuenta 9, 10|&#10004;Tenga en cuenta 9, 10|&#10004;Tenga en cuenta 8, 9, 10|
|Cambio de tamaño de memoria en tiempo de ejecución|2019, 2016|&#10004;|&#10004;|&#10004;|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**||||
|Dispositivo de vídeo específico de Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**||||
|Par clave-valor|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Interrupción no enmascarable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copia de archivos de host a invitado|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|||||||
|Sockets de Hyper-V|2019, 2016|&#10004;|&#10004;|&#10004;|||||||
|Acceso directo/DDA de PCI|2019, 2016|&#10004;|&#10004;||&#10004;|&#10004;|&#10004;||||
|**[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**|||||
|Arrancar con UEFI|2019, 2016, 2012 R2|&#10004;Tenga en cuenta 14|&#10004;Tenga en cuenta 14|&#10004;Tenga en cuenta 14|&#10004;Tenga en cuenta 14|&#10004;Tenga en cuenta 14|&#10004;Tenga en cuenta 14|&#10004;Tenga en cuenta 14|&#10004;Tenga en cuenta 14|&#10004;Tenga en cuenta 14|
|Arranque seguro|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="BKMK_6x"></a>RHEL/CentOS 6.x Series

El kernel de 32 bits para esta serie es la característica PAE habilitada. No hay ninguna compatibilidad integrada de LIS para RHEL/CentOS 6.0-6.3.

|**Característica**|**Versión de Windows Server**|**6.4-6.10**|**6.0-6.3**|**6.10, 6.9, 6.8**|**6.6, 6.7**|**6.5**|**6.4**|
|-|-|-|-|-|-|-|-|
|**Disponibilidad**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|Integrado|Integrado|Integrado|Integrado|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Hora exacta de Windows Server 2016|2019, 2016||||||
|**[Funciones de red](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**|
|Tramas gigantes|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Etiquetado de VLAN y enlace troncal|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|
|Migración en vivo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Inserción de la dirección IP estática|2019, 2016, 2012 R2, 2012|&#10004;Nota 2|&#10004;Nota 2|&#10004;Nota 2|&#10004;Nota 2|&#10004;Nota 2|&#10004;Nota 2|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Segmentación de TCP y las descargas de suma de comprobación|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|SR-IOV|2019, 2016|||||||
|**[Storage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**|
|Cambio de tamaño de VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|Canal de fibra virtual|2019, 2016, 2012 R2|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3||
|Copia de seguridad de máquina virtual en vivo|2019, 2016, 2012 R2|&#10004;Nota 5|&#10004;Nota 5|&#10004;Nota 4, 5|&#10004;Nota 4, 5|&#10004;Tenga en cuenta la 4, 5, 6|&#10004;Tenga en cuenta la 4, 5, 6|
|RECORTAR el soporte técnico|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;||||
|SCSI WWN|2019, 2016, 2012 R2|&#10004;|&#10004;|||||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**|
|Soporte técnico de núcleo PAE|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Configuración de gap MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memoria dinámica - agregado en caliente|2019, 2016, 2012 R2, 2012|&#10004;Tenga en cuenta 7, 9, 10|&#10004;Tenga en cuenta 7, 9, 10|&#10004;Tenga en cuenta 7, 9, 10|&#10004;Tenga en cuenta 7, 8, 9, 10|&#10004;Tenga en cuenta 7, 8, 9, 10||
|Memoria dinámica - incremento|2019, 2016, 2012 R2, 2012|&#10004;Tenga en cuenta 7, 9, 10|&#10004;Tenga en cuenta 7, 9, 10|&#10004;Tenga en cuenta 7, 9, 10|&#10004;Tenga en cuenta 7, 9, 10|&#10004;Tenga en cuenta 7, 9, 10|&#10004;Tenga en cuenta 7, 9, 10, 11|
|Cambio de tamaño de memoria en tiempo de ejecución|2019, 2016|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**|
|Dispositivo de vídeo específico de Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**|
|Par clave-valor|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;Nota 12|&#10004;Nota 12|&#10004;Nota 12, 13|&#10004;Nota 12, 13|
|Interrupción no enmascarable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copia de archivos de host a invitado|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||||||
|Sockets de Hyper-V|2019, 2016|&#10004;|&#10004;|||||
|Acceso directo/DDA de PCI|2019, 2016|||||||
|**[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**|
|Arrancar con UEFI|2012 R2|||||||
||2019, 2016|&#10004;Tenga en cuenta 14|&#10004;Tenga en cuenta 14|&#10004;Tenga en cuenta 14||||
|Arranque seguro|2019, 2016||||||

## <a name="BKMK_5x"></a>RHEL/CentOS 5.x Series

Esta serie tiene un núcleo de PAE de 32 bits compatible. No hay ninguna compatibilidad integrada de LIS para CentOS o RHEL antes 5.9.

|**Característica**|**Versión de Windows Server**|5.2 -5.11|**5.2-5.11**|**5.9 - 5.11**|
|-|-|-|-|-|
|**Disponibilidad**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.1](https://www.microsoft.com/download/details.aspx?id=51612)|Integrado|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Hora exacta de Windows Server 2016|2019, 2016||||
|**[Funciones de red](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**|
|Tramas gigantes|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Etiquetado de VLAN y enlace troncal|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|
|Migración en vivo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Inserción de la dirección IP estática|2019, 2016, 2012 R2, 2012|&#10004;Nota 2|&#10004;Nota 2|&#10004;Nota 2|
|vRSS|2019, 2016, 2012 R2||||
|Segmentación de TCP y las descargas de suma de comprobación|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|SR-IOV|2019, 2016||||||
|**[Storage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**|
|Cambio de tamaño de VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;||
|Canal de fibra virtual|2019, 2016, 2012 R2|&#10004;Nota 3|&#10004;Nota 3||
|Copia de seguridad de máquina virtual en vivo|2019, 2016, 2012 R2|&#10004;Nota 5, 15|&#10004;Nota 5|&#10004;Tenga en cuenta la 4, 5, 6|
|RECORTAR el soporte técnico|2019, 2016, 2012 R2||||
|SCSI WWN|2019, 2016, 2012 R2||||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**|
|Soporte técnico de núcleo PAE|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Configuración de gap MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|
|Memoria dinámica - agregado en caliente|2019, 2016, 2012 R2, 2012||||
|Memoria dinámica - incremento|2019, 2016, 2012 R2, 2012|&#10004;Tenga en cuenta 7, 9, 10, 11|&#10004;Tenga en cuenta 7, 9, 10, 11||
|Cambio de tamaño de memoria en tiempo de ejecución|2019, 2016||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**|
|Dispositivo de vídeo específico de Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|**[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**|
|Par clave-valor|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|Interrupción no enmascarable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|
|Copia de archivos de host a invitado|2019, 2016, 2012 R2|&#10004;|&#10004;||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2||||
|Sockets de Hyper-V|2019, 2016||||
|Acceso directo/DDA de PCI|2019, 2016||||||
|**[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**|
|Arrancar con UEFI|2019, 2016, 2012 R2||||
|Arranque seguro|2019, 2016||||

## <a name="BKMK_notes"></a>Notas de la

1. Para esta versión de RHEL/CentOS, funciona el etiquetado de VLAN, pero no enlace troncal VLAN.

2. Inserción de IP estática puede no funcionar si se ha configurado el Administrador de red para un adaptador de red sintético determinado en la máquina virtual. Para el correcto funcionamiento de la dirección IP estática inyección Asegúrese de que el Administrador de red está apagado completamente o se ha desactivado para un adaptador de red específico a través de su archivo ifcfg-ethX.

3. En Windows Server 2012 R2 al usar dispositivos de canal de fibra virtual, asegúrese de que se ha rellenado el número de unidad lógica (LUN 0) de 0. Si no se ha rellenado el LUN 0, es posible que una máquina virtual Linux no pueda montar de forma nativa los dispositivos de canal de fibra.

4. Para LIS integrados, debe instalarse el paquete "los demonios de Hyper-v" para esta funcionalidad.

5. Si hay identificadores de archivos abiertos durante una operación de copia de seguridad de máquina virtual activa, a continuación, en algunos casos excepcionales, podrían tener los VHD de copia de seguridad que debe someterse a una comprobación de coherencia del sistema de archivo (fsck) en la restauración. Operaciones de copia de seguridad en directo pueden fallar en modo silencioso si la máquina virtual tiene un dispositivo iSCSI conectados o almacenamiento conectado directo (también conocido como un disco de acceso directo).

6. Aunque se prefiere la descarga de Linux Integration Services, compatibilidad con copia de seguridad para RHEL/CentOS 5.9 en vivo: 5.11/6.4/6.5 también está disponible a través de [aspectos básicos de copia de seguridad de Hyper-V para Linux](https://github.com/LIS/backupessentials/tree/1.0).

7. Compatibilidad con memoria dinámica sólo está disponible en las máquinas virtuales de 64 bits.

8. Soporte técnico en caliente no está habilitado de forma predeterminada en esta distribución. Para habilitar la compatibilidad de adición sin interrupción que tiene que agregar una regla udev en /etc/udev/rules.d/ como sigue:

   1. Cree un archivo **/etc/udev/rules.d/100-balloon.rules**. Puede usar cualquier otro nombre para el archivo deseado.

   2. En el archivo, agregue el siguiente contenido: `SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. Reinicie el sistema para habilitar la compatibilidad de adición sin interrupción.

   Mientras la descarga de Linux Integration Services crea esta regla en la instalación, la regla también se quita cuando se desinstala LIS, por lo que la regla se debe volver a crearse si se necesita memoria dinámica después de la desinstalación.

9. Las operaciones de memoria dinámica pueden producir un error si el sistema operativo invitado se ejecuta demasiado bajo en memoria. Éstas son algunas prácticas recomendadas:

   * Memoria mínima y memoria de inicio deben ser igual o mayor que la cantidad de memoria que recomienda el proveedor de distribución.

   * Las aplicaciones que tienden a consumir toda la memoria disponible en un sistema se limitan a consumir hasta el 80 por ciento de memoria RAM disponible.

10. Si usas la memoria dinámica en un sistema operativo Windows Server 2016 o Windows Server 2012 R2, especifique **memoria de inicio**, **cantidad mínima de memoria**, y **cantidad máxima de memoria** parámetros en múltiplos de 128 megabytes (MB). Si no lo hace puede dar lugar a errores en caliente y puede que no vea toda la memoria aumenta en un sistema operativo invitado.

11. Solo ciertas distribuciones, incluidas aquellas que usan LIS 4.0 y 4.1, proporcionan soporte técnico de incremento y no proporcionan compatibilidad de adición sin interrupción. En este escenario, se puede usar la característica memoria dinámica estableciendo el parámetro de inicio de memoria en un valor que es igual que el parámetro de memoria máximo. Este resultado en toda la memoria necesaria está agregado en caliente a la máquina virtual en tiempo de arranque y, posteriormente, dependiendo de los requisitos de memoria del host, Hyper-V libremente puede asignar o desasignar memoria desde el invitado con el incremento. Configure **memoria de inicio** y **cantidad mínima de memoria** o mayor que el valor recomendado para la distribución.

12. Para habilitar la infraestructura de clave/valor par (KVP), instale el paquete rpm hypervkvpd o demonios de Hyper-v desde el ISO de RHEL. También puede instalarse el paquete directamente desde los repositorios RHEL.

13. La infraestructura de clave/valor par (KVP) podría no funcionar correctamente sin una actualización de software de Linux. Póngase en contacto con su proveedor de distribución para obtener la actualización de software en caso de que vea problemas con esta característica.

14. En Windows Server 2012 R2 generación 2 máquinas virtuales tienen el arranque seguro habilitado de forma predeterminada y algunas máquinas virtuales de Linux no se iniciará a menos que la opción de arranque seguro está deshabilitada. Puede deshabilitar arranque seguro en el **Firmware** sección de la configuración de la máquina virtual en **Administrador de Hyper-V** o se puede deshabilitar con Powershell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

   La descarga de Linux Integration Services se puede aplicar a las máquinas virtuales existentes generación 2 pero impartir la funcionalidad de generación 2.

15. En Red Hat Enterprise Linux o CentOS 5.2, 5.3, 5.4 el sistema de archivos inmovilización de funcionalidad y no está disponible, por lo que la copia de seguridad de máquina Virtual en vivo no está disponible.

Vea también

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Admite máquinas virtuales de Debian en Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales Oracle Linux compatibles en Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales SUSE compatibles en Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales de Ubuntu compatibles en Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales de FreeBSD compatibles en Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descripciones de características para las máquinas virtuales de Linux y FreeBSD en Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Procedimientos recomendados para ejecutar Linux en Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Certificación de Hardware de Red Hat](https://hardware.redhat.com/&quicksearch=Microsoft)
