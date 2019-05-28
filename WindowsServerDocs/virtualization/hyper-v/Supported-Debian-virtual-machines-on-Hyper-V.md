---
title: Admite máquinas virtuales de Debian en Hyper-V
description: Enumera los servicios de integración de Linux y características incluidas en cada versión
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cc62c10-02a3-4633-960c-23bf91a45bd5
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 129783dc980be6e471ecadb2cdbffee900e3396e
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222837"
---
# <a name="supported-debian-virtual-machines-on-hyper-v"></a>Admite máquinas virtuales de Debian en Hyper-V

>Se aplica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

El siguiente mapa de distribución de la característica indica las características que están presentes en cada versión. Después de la tabla se enumeran los problemas conocidos y soluciones alternativas para cada distribución.

## <a name="table-legend"></a>Leyenda de la tabla

* **Integrado** -LIS se incluye como parte de esta distribución de Linux. El paquete de descarga LIS proporcionada por Microsoft no funciona para esta distribución, por lo que no lo instale. Los números de versión del módulo de kernel para la compilación en LIS (tal como se muestra por **lsmod**, por ejemplo) son diferentes desde el número de versión del paquete de descarga LIS proporcionada por Microsoft. Un error de coincidencia no indica que la compilación en LIS está obsoleta.

* &#10004;-Característica disponible

* (*en blanco*)-característica no está disponible

|**Característica**|**Versión del sistema operativo Windows Server**|**9.0-9.6 (stretch)**|**8.0-8.11 (jessie)**|**7.0-7.11 (wheezy)**|
|-|-|-|-|-|
|**Disponibilidad**||Integrado|Integrado|Integrada (Nota 6)|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Hora exacta de Windows Server 2016|2019, 2016|&#10004;Tenga en cuenta 8||
|**[Funciones de red](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|
|Tramas gigantes|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Etiquetado de VLAN y enlace troncal|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Migración en vivo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Inserción de la dirección IP estática|2019, 2016, 2012 R2, 2012|||
|vRSS|2019, 2016, 2012 R2|&#10004;Tenga en cuenta 8|||
|Segmentación de TCP y las descargas de suma de comprobación|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;Tenga en cuenta 8|||
|SR-IOV|2019, 2016|&#10004;Tenga en cuenta 8||
|**[Storage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|
|Cambio de tamaño de VHDX|2019, 2016, 2012 R2|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|
|Canal de fibra virtual|2019, 2016, 2012 R2|||
|Copia de seguridad de máquina virtual en vivo|2019, 2016, 2012 R2|&#10004;Tenga en cuenta 4,5|&#10004;Tenga en cuenta 4,5|&#10004;Nota 4|
|RECORTAR el soporte técnico|2019, 2016, 2012 R2|&#10004;Tenga en cuenta 8|||
|SCSI WWN|2019, 2016, 2012 R2|&#10004;Tenga en cuenta 8||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|
|Soporte técnico de núcleo PAE|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Configuración de gap MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|
|Memoria dinámica - agregado en caliente|2019, 2016, 2012 R2, 2012|&#10004;Tenga en cuenta 8|||
|Memoria dinámica - incremento|2019, 2016, 2012 R2, 2012|&#10004;Tenga en cuenta 8|||
|Cambio de tamaño de memoria en tiempo de ejecución|2019, 2016|&#10004;Tenga en cuenta 8|||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|
|Dispositivo de vídeo específico de Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|**[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|
|Par clave-valor|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;Nota 4|&#10004;Nota 4||
|Interrupción no enmascarable|2019, 2016, 2012 R2|&#10004;|&#10004;|
|Copia de archivos de host a invitado|2019, 2016, 2012 R2|&#10004;Nota 4|&#10004;Nota 4||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|||
|Sockets de Hyper-V|2019, 2016|&#10004;Tenga en cuenta 8|||
|Acceso directo/DDA de PCI|2019, 2016|&#10004;Tenga en cuenta 8|||
|**[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|
|Arrancar con UEFI|2019, 2016, 2012 R2|&#10004;Nota 7|&#10004;Nota 7||
|Arranque seguro|2019, 2016|||

## <a name="BKMK_notes"></a>Notas de la

1. No se admite la creación de sistemas de archivos en discos duros virtuales mayores de 2TB.

2. En Windows Server 2008 R2 SCSI discos crean 8 entradas diferentes en/dev/sd *.

3. Windows Server 2012 R2 una máquina virtual con 8 núcleos o más tendrá todas las interrupciones que se enruta a una única vCPU.

4. A partir de Debian 8.3, el paquete de Debian instalados de forma manual "Hyper-v-demonios" contiene el par clave-valor, fcopy y los demonios de VSS. En Debian 7.x y 8.0 8.2 el paquete demonios de Hyper-v debe proceder de [backports Debian](https://wiki.debian.org/Backports).

5. Copia de seguridad de máquina virtual en vivo no funcionará con los sistemas de archivos ext2. El diseño predeterminado creado por el instalador de Debian incluye filesystems ext2, debe personalizar el diseño para no crear este tipo de sistema de archivos.

6. Aunque Debian 7.x está fuera del soporte técnico y usa una versión anterior del kernel, incluido el kernel en [backports Debian](https://wiki.debian.org/Backports) para Debian 7.x ha mejorado las capacidades de Hyper-V.

7. En Windows Server 2012 R2 generación 2 máquinas virtuales tienen el arranque seguro habilitado de forma predeterminada y algunas máquinas virtuales de Linux no se iniciará a menos que la opción de arranque seguro está deshabilitada. Puede deshabilitar arranque seguro en el **Firmware** sección de la configuración de la máquina virtual en **Administrador de Hyper-V** o se puede deshabilitar con Powershell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```
8. Las últimas funcionalidades de nivel superior de kernel solo están disponibles mediante el kernel incluido [backports Debian](https://wiki.debian.org/Backports).

Vea también

* [Admite CentOS y Red Hat Enterprise Linux virtual machines en Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales Oracle Linux compatibles en Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales SUSE compatibles en Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales de Ubuntu compatibles en Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales de FreeBSD compatibles en Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descripciones de características para las máquinas virtuales de Linux y FreeBSD en Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Procedimientos recomendados para ejecutar Linux en Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
