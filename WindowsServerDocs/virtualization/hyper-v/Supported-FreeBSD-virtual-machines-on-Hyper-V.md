---
title: Máquinas virtuales de FreeBSD compatibles en Hyper-V
description: Enumera las características y servicios de integración de Linux que se incluyen en cada versión
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 930e758f-bd50-46b4-a3a4-9857110f17b4
author: shirgall
ms.author: kathydav
ms.date: 04/07/2020
ms.openlocfilehash: 9394ff04b32ab34cbdad6a46573fd3674051db36
ms.sourcegitcommit: 7b1ebc4934998af2472962ca8cce1c872f39946f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80994528"
---
# <a name="supported-freebsd-virtual-machines-on-hyper-v"></a>Máquinas virtuales de FreeBSD compatibles en Hyper-V

>Se aplica a: Windows Server 2019, Hyper-V Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows 10, Windows 8.1

El siguiente mapa de distribución de características indica las características de cada versión. Los problemas conocidos y las soluciones alternativas para cada distribución se enumeran después de la tabla.

## <a name="table-legend"></a>Leyenda de tabla

* **Integrado en** -bis (servicio de integración de FreeBSD) se incluye como parte de esta versión de FreeBSD.

* &#10004;-Característica disponible

* (*en blanco*): característica no disponible

|**Ofrecen**|**Versión del sistema operativo Windows Server**|**12-12,1**|**11.1 a 11.3**|**11,0**|**10,3**|**10,2**|**10,0-10,1**|**9,1-9,3, 8,4**|
|-|-|-|-|-|-|-|-|-|
|**Disponibilidad**||Integrado|Integrado|Integrado|Integrado|Integrado|Integrado|[Puerto](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) |
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 hora precisa|2019, 2016|&#10004;|&#10004;||||||
|**[Soluciona](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||||
|Tramas gigantes|2019, 2016, 2012 R2|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|
|Etiquetado y Troncalización de VLAN|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migración en vivo|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Inyección de IP estática|2019, 2016, 2012 R2|&#10004;Nota 4|&#10004;Nota 4|&#10004;Nota 4|&#10004;Nota 4|&#10004;Nota 4|&#10004;Nota 4|&#10004;|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|||||
|Segmentación y descarga de sumas de comprobación TCP|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||
|Descarga de recepción de gran tamaño (LRO)|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;|||||
|**[Discos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||Note1|Nota 1:|Nota 1:|Nota 1:|Nota 1:|Nota 1, 2|Nota 1, 2|
|Cambiar el tamaño de VHDX|2019, 2016, 2012 R2|&#10004;Nota 6|&#10004;Nota 6|&#10004;Nota 6|||||
|Canal de fibra virtual|2019, 2016, 2012 R2||||||||
|Copia de seguridad de máquinas virtuales en vivo|2019, 2016, 2012 R2|&#10004;|&#10004;||||||
|Compatibilidad con TRIM|2019, 2016, 2012 R2|&#10004;|&#10004;||||||
|WWN SCSI|2019, 2016, 2012 R2||||||||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|||||||||
|Compatibilidad con el kernel PAE|2019, 2016, 2012 R2||||||||
|Configuración de la brecha de MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memoria dinámica: agregar en caliente|2019, 2016, 2012 R2||||||||
|Memoria dinámica: globos|2019, 2016, 2012 R2||||||||
|Tamaño de memoria en tiempo de ejecución|2019, 2016||||||||
|**[Cámara](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|||||||||
|Dispositivo de vídeo específico de Hyper-V|2019, 2016, 2012 R2||||||||
|**[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|||||||||
|Par clave-valor|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;Nota 5|&#10004;|
|Interrupción no enmascarable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copia de archivos de host a invitado|2019, 2016, 2012 R2||||||||
|comando lsvmbus|2019, 2016, 2012 R2||||||||
|Sockets de Hyper-V|2019, 2016||||||||
|Acceso directo/DDA de PCI|2019, 2016|&#10004;|&#10004;||||||
|**[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|||||||||
|Arranque mediante UEFI|2019, 2016, 2012 R2|&#10004;|&#10004;||||||
|Arranque seguro|2019, 2016||||||||

## <a name="notes"></a><a name="BKMK_notes"></a>Apunte

1. Sugerimos que [etiquete los dispositivos de disco]( https://www.freebsd.org/doc/handbook/geom-glabel.html) para evitar un error de montaje raíz durante el inicio.

2. Es posible que la unidad de DVD virtual no se reconozca cuando se carguen controladores de BIS en FreeBSD 8. x y 9. x a menos que habilite el controlador ATA heredado mediante el siguiente comando.
    ```sh
    # echo ‘hw.ata.disk_enable=1' >> /boot/loader.conf
    # shutdown -r now
    ```

3. 9126 es el tamaño máximo admitido de MTU.

4. En un escenario de conmutación por error, no se puede establecer una dirección IPv6 estática en el servidor réplica. En su lugar, use una dirección IPv4.

5. KVP lo proporcionan los puertos en FreeBSD 10,0. Consulte los [puertos FreeBSD 10,0](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) en FreeBSD.org para obtener más información.

6. Para que el cambio de tamaño en línea de VHDX funcione correctamente en FreeBSD 11,0, se requiere un paso manual especial para solucionar un error de GEOM, que se corrige en 11.0 +, después de que el host cambie el tamaño del disco VHDX: Abra el disco para escritura y ejecute "gpart Recover" como sigue.
    ```sh
    # dd if=/dev/da1 of=/dev/da1 count=0
    # gpart recover da1
    ```
   
**Notas adicionales**: la matriz de características de 10 Stable y 11 stable es la misma que la versión de FreeBSD 11,1. Además, FreeBSD 10,2 y las versiones anteriores (10,1, 10,0, 9. x, 8. x) están al final del ciclo de vida. Consulte [aquí](https://security.freebsd.org/) para obtener una lista actualizada de las versiones admitidas y los avisos de seguridad más recientes.

## <a name="see-also"></a>Vea también

* [Descripciones de características de máquinas virtuales Linux y FreeBSD en Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)
* [Prácticas recomendadas para ejecutar FreeBSD en Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
