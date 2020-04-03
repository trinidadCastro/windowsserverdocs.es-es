---
title: Máquinas virtuales de Debian compatibles en Hyper-V
description: Enumera las características y servicios de integración de Linux que se incluyen en cada versión
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cc62c10-02a3-4633-960c-23bf91a45bd5
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 7a717acf5c132d68d6ee041aeb5af5a430aa171b
ms.sourcegitcommit: 9f7cc76b8c9add44dcbbd97f77b4f881d5a2c073
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80613006"
---
# <a name="supported-debian-virtual-machines-on-hyper-v"></a>Máquinas virtuales de Debian compatibles en Hyper-V

>Se aplica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

El siguiente mapa de distribución de características indica las características que se encuentran en cada versión. Los problemas conocidos y las soluciones alternativas para cada distribución se enumeran después de la tabla.

## <a name="table-legend"></a>Leyenda de tabla

* La **compilación en** LIS se incluye como parte de esta distribución de Linux. El paquete de descarga de LIS proporcionado por Microsoft no funciona para esta distribución, por lo que no se debe instalar. Los números de versión del módulo de kernel para el LIS integrado (como se muestra en **lsmod**, por ejemplo) son diferentes del número de versión del paquete de descarga de lis proporcionado por Microsoft. Un error de coincidencia no indica que el LIS integrado no está actualizado.

* &#10004;-Característica disponible

* (*en blanco*): característica no disponible

| **Ofrecen**                                                                                                                                  | **Versión del sistema operativo Windows Server** | **10.0-10.3 (validador)** | **9.0-9.12 (Stretch)** | **8.0-8.11 (Jessie)** | **7.0-7.11 (Wheezy)** |
|----------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|-----------------------|-----------------------|-----------------------|-----------------------|
| **Disponibilidad**                                                                                                                             |                                             | Integrado              | Integrado              | Integrado              | Integrado (Nota 6)     |
| **[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**                                                   | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Windows Server 2016 hora precisa                                                                                                            | 2019, 2016                                  | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| **[Soluciona](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**                                       |                                             |                       |                       |                       |                       |
| Tramas gigantes                                                                                                                                 | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Etiquetado y Troncalización de VLAN                                                                                                                    | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Migración en vivo                                                                                                                               | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Inyección de IP estática                                                                                                                          | 2019, 2016, 2012 R2, 2012                   |                       |                       |                       |                       |
| vRSS                                                                                                                                         | 2019, 2016, 2012 R2                         | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| Segmentación y descarga de sumas de comprobación TCP                                                                                                       | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| SR-IOV                                                                                                                                       | 2019, 2016                                  | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| **[Discos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**                                             |                                             |                       |                       |                       |                       |
| Cambiar el tamaño de VHDX                                                                                                                                  | 2019, 2016, 2012 R2                         | &#10004;Nota 1       | &#10004;Nota 1       | &#10004;Nota 1       | &#10004;Nota 1       |
| Canal de fibra virtual                                                                                                                        | 2019, 2016, 2012 R2                         |                       |                       |                       |                       |
| Copia de seguridad de máquinas virtuales en vivo                                                                                                                  | 2019, 2016, 2012 R2                         | &#10004;Nota 4, 5     | &#10004;Nota 4, 5     | &#10004;Nota 4, 5     | &#10004;Nota 4       |
| Compatibilidad con TRIM                                                                                                                                 | 2019, 2016, 2012 R2                         | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| WWN SCSI                                                                                                                                     | 2019, 2016, 2012 R2                         | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| **[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**                                               |                                             |                       |                       |                       |                       |
| Compatibilidad con el kernel PAE                                                                                                                           | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Configuración de la brecha de MMIO                                                                                                                    | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Memoria dinámica: agregar en caliente                                                                                                                     | 2019, 2016, 2012 R2, 2012                   | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| Memoria dinámica: globos                                                                                                                  | 2019, 2016, 2012 R2, 2012                   | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| Tamaño de memoria en tiempo de ejecución                                                                                                                        | 2019, 2016                                  | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| **[Cámara](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**                                                 |                                             |                       |                       |                       |                       |
| Dispositivo de vídeo específico de Hyper-V                                                                                                                | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;              | &#10004;              | &#10004;              |                       |
| **[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**                                 |                                             |                       |                       |                       |                       |
| Par clave-valor                                                                                                                               | 2019, 2016, 2012 R2, 2012, 2008 R2          | &#10004;Nota 4       | &#10004;Nota 4       | &#10004;Nota 4       |                       |
| Interrupción no enmascarable                                                                                                                       | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              |                       |
| Copia de archivos de host a invitado                                                                                                                 | 2019, 2016, 2012 R2                         | &#10004;Nota 4       | &#10004;Nota 4       | &#10004;Nota 4       |                       |
| comando lsvmbus                                                                                                                              | 2019, 2016, 2012 R2, 2012, 2008 R2          |                       |                       |                       |                       |
| Sockets de Hyper-V                                                                                                                              | 2019, 2016                                  | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| Acceso directo/DDA de PCI                                                                                                                          | 2019, 2016                                  | &#10004;Nota 8       | &#10004;Nota 8       |                       |                       |
| **[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** |                                             |                       |                       |                       |                       |
| Arranque mediante UEFI                                                                                                                              | 2019, 2016, 2012 R2                         | &#10004;Nota 7       | &#10004;Nota 7       | &#10004;Nota 7       |                       |
| Arranque seguro                                                                                                                                  | 2019, 2016                                  | &#10004;              |                       |                       |                       |


## <a name="notes"></a><a name="BKMK_notes"></a>Apunte

1. No se admite la creación de sistemas de archivos en VHD de más de 2 TB.

2. En los discos SCSI de Windows Server 2008 R2, cree 8 entradas diferentes en/dev/SD *.

3. Windows Server 2012 R2 una máquina virtual con 8 núcleos o más tendrá todas las interrupciones enrutadas a un único vCPU.

4. A partir de Debian 8,3, el paquete de Debian instalado manualmente "HyperV-daemons" contiene el par clave-valor, el fcopy y los demonios de VSS. En Debian 7. x y 8.0-8.2, el paquete de demonio de HyperV debe proviene de los [puertos de subpuertos de Debian](https://wiki.debian.org/Backports).

5. La copia de seguridad de máquinas virtuales en vivo no funcionará con sistemas de archivos ext2. El diseño predeterminado creado por el instalador de Debian incluye los sistemas de archivos de ext2, por lo que debe personalizar el diseño para no crear este tipo de sistema de archivos.

6. Aunque Debian 7. x no es compatible y usa un kernel anterior, el kernel incluido en los [puertos](https://wiki.debian.org/Backports) de salida de Debian para Debian 7. x ha mejorado las funciones de Hyper-V.

7. En las máquinas virtuales con Windows Server 2012 R2 de segunda generación 2, el arranque seguro está habilitado de forma predeterminada y algunas máquinas virtuales Linux no se iniciarán a menos que se deshabilite la opción de arranque seguro. Puede deshabilitar el arranque seguro en la sección **firmware** de la configuración de la máquina virtual en el **Administrador de Hyper-V** o puede deshabilitarla mediante PowerShell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```
8. Las funcionalidades más recientes del kernel de nivel superior solo están disponibles mediante el uso de los [puertos de subpuertos de Debian](https://wiki.debian.org/Backports)de kernel incluidos.

Vea también

* [Compatibilidad con máquinas virtuales de alta y Red Hat Enterprise Linux en Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Oracle Linux compatibles con máquinas virtuales en Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de SUSE compatibles en Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de Ubuntu admitidas en Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de FreeBSD compatibles en Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descripciones de características de máquinas virtuales Linux y FreeBSD en Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Prácticas recomendadas para ejecutar Linux en Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
