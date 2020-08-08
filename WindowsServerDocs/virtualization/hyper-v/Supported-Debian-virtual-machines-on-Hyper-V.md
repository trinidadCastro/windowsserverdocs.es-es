---
title: Máquinas virtuales Debian admitidas en Hyper-V
description: Enumera las características y servicios de integración de Linux que se incluyen en cada versión
manager: dongill
ms.topic: article
ms.assetid: 3cc62c10-02a3-4633-960c-23bf91a45bd5
author: shirgall
ms.author: kathydav
ms.date: 04/07/2020
ms.openlocfilehash: da96f78c9886ea392ccb2834f4b245a2422dc17e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87965752"
---
# <a name="supported-debian-virtual-machines-on-hyper-v"></a>Máquinas virtuales Debian admitidas en Hyper-V

>Se aplica a: Windows Server 2019, Hyper-V Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows 10, Windows 8.1

El siguiente mapa de distribución de características indica las características que se encuentran en cada versión. Los problemas conocidos y las soluciones alternativas para cada distribución se enumeran después de la tabla.

## <a name="table-legend"></a>Leyenda de tabla

* La **compilación en** LIS se incluye como parte de esta distribución de Linux. El paquete de descarga de LIS proporcionado por Microsoft no funciona para esta distribución, por lo que no se debe instalar. Los números de versión del módulo de kernel para el LIS integrado (como se muestra en **lsmod**, por ejemplo) son diferentes del número de versión del paquete de descarga de lis proporcionado por Microsoft. Un error de coincidencia no indica que el LIS integrado no está actualizado.

* &#10004;: característica disponible

* (*en blanco*): característica no disponible

| **Característica**                                                                                                                                  | **Versión del sistema operativo Windows Server** | **10.0-10.3 (validador)** | **9.0-9.12 (Stretch)** | **8.0-8.11 (Jessie)** | **7.0-7.11 (Wheezy)** |
|----------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|-----------------------|-----------------------|-----------------------|-----------------------|
| **Disponibilidad**                                                                                                                             |                                             | Integrado              | Integrado              | Integrado              | Integrado (Nota 5)     |
| **[Principal](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**                                                   | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Windows Server 2016 hora precisa                                                                                                            | 2019, 2016                                  | &#10004; Nota 4       | &#10004; Nota 4       |                       |                       |
| **[Redes](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**                                       |                                             |                       |                       |                       |                       |
| Tramas gigantes                                                                                                                                 | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Etiquetado y Troncalización de VLAN                                                                                                                    | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Migración en vivo                                                                                                                               | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Inyección de IP estática                                                                                                                          | 2019, 2016, 2012 R2                   |                       |                       |                       |                       |
| vRSS                                                                                                                                         | 2019, 2016, 2012 R2                         | &#10004; Nota 4       | &#10004; Nota 4       |                       |                       |
| Segmentación y descarga de sumas de comprobación TCP                                                                                                       | 2019, 2016, 2012 R2          | &#10004; Nota 4       | &#10004; Nota 4       |                       |                       |
| SR-IOV                                                                                                                                       | 2019, 2016                                  | &#10004; Nota 4       | &#10004; Nota 4       |                       |                       |
| **[Storage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**                                             |                                             |                       |                       |                       |                       |
| Cambiar el tamaño de VHDX                                                                                                                                  | 2019, 2016, 2012 R2                         | &#10004; Nota 1       | &#10004; Nota 1       | &#10004; Nota 1       | &#10004; Nota 1       |
| Canal de fibra virtual                                                                                                                        | 2019, 2016, 2012 R2                         |                       |                       |                       |                       |
| Copia de seguridad de máquinas virtuales en vivo                                                                                                                  | 2019, 2016, 2012 R2                         | &#10004; Note2 | &#10004; Note2 | &#10004; Note2 | &#10004; Note2 |
| Compatibilidad con TRIM                                                                                                                                 | 2019, 2016, 2012 R2                         | &#10004; Nota 4       | &#10004; Nota 4       |                       |                       |
| WWN SCSI                                                                                                                                     | 2019, 2016, 2012 R2                         | &#10004; Nota 4       | &#10004; Nota 4       |                       |                       |
| **[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**                                               |                                             |                       |                       |                       |                       |
| Compatibilidad con el kernel PAE                                                                                                                           | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Configuración de la brecha de MMIO                                                                                                                    | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Memoria dinámica: agregar en caliente                                                                                                                     | 2019, 2016, 2012 R2                   | &#10004; Nota 4       | &#10004; Nota 4       |                       |                       |
| Memoria dinámica: globos                                                                                                                  | 2019, 2016, 2012 R2                   | &#10004; Nota 4       | &#10004; Nota 4       |                       |                       |
| Tamaño de memoria en tiempo de ejecución                                                                                                                        | 2019, 2016                                  | &#10004; Nota 4       | &#10004; Nota 4       |                       |                       |
| **[Cámara](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**                                                 |                                             |                       |                       |                       |                       |
| Dispositivo de vídeo específico de Hyper-V                                                                                                                | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              |                       |
| **[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**                                 |                                             |                       |                       |                       |                       |
| Par clave-valor                                                                                                                               | 2019, 2016, 2012 R2          | &#10004; Nota 2       | &#10004; Nota 2       | &#10004; Nota 2       |                       |
| Interrupción no enmascarable                                                                                                                       | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              |                       |
| Copia de archivos de host a invitado                                                                                                                 | 2019, 2016, 2012 R2                         | &#10004; Nota 2       | &#10004; Nota 2       | &#10004; Nota 2       |                       |
| comando lsvmbus                                                                                                                              | 2019, 2016, 2012 R2          |                       |                       |                       |                       |
| Sockets de Hyper-V                                                                                                                              | 2019, 2016                                  | &#10004; Nota 4       | &#10004; Nota 4       |                       |                       |
| Acceso directo/DDA de PCI                                                                                                                          | 2019, 2016                                  | &#10004; Nota 4       | &#10004; Nota 4       |                       |                       |
| **[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** |                                             |                       |                       |                       |                       |
| Arranque mediante UEFI                                                                                                                              | 2019, 2016, 2012 R2                         | &#10004; Nota 3       | &#10004; Nota 3       | &#10004; Nota 3       |                       |
| Arranque seguro                                                                                                                                  | 2019, 2016                                  | &#10004;              |                       |                       |                       |


## <a name="notes"></a>Notas

1. No se admite la creación de sistemas de archivos en VHD de más de 2 TB.

2. A partir de Debian 8,3, el paquete de Debian instalado manualmente "HyperV-daemons" contiene el par clave-valor, el fcopy y los demonios de VSS. En Debian 7. x y 8.0-8.2, el paquete de demonio de HyperV debe proviene de los [puertos de subpuertos de Debian](https://wiki.debian.org/Backports).

3. En las máquinas virtuales con Windows Server 2012 R2 de segunda generación 2, el arranque seguro está habilitado de forma predeterminada y algunas máquinas virtuales Linux no se iniciarán a menos que se deshabilite la opción de arranque seguro. Puede deshabilitar el arranque seguro en la sección **firmware** de la configuración de la máquina virtual en el **Administrador de Hyper-V** o puede deshabilitarla mediante PowerShell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
   ```
4. Las funcionalidades más recientes del kernel de nivel superior solo están disponibles mediante el uso de los [puertos de subpuertos de Debian](https://wiki.debian.org/Backports)de kernel incluidos.

5. Aunque Debian 7. x no es compatible y usa un kernel anterior, el kernel incluido en los puertos de salida de Debian para Debian 7. x ha mejorado las funciones de Hyper-V.

Consulte también

* [Compatibilidad con máquinas virtuales de alta y Red Hat Enterprise Linux en Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Oracle Linux compatibles con máquinas virtuales en Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de SUSE compatibles en Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de Ubuntu admitidas en Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de FreeBSD compatibles en Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descripciones de características de máquinas virtuales Linux y FreeBSD en Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Prácticas recomendadas para ejecutar Linux en Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
