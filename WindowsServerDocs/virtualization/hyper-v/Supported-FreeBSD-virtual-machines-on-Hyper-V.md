---
title: Máquinas de virtuales de FreeBSD compatibles en Hyper-V
description: Enumera los servicios de integración de Linux y características incluidas en cada versión
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 930e758f-bd50-46b4-a3a4-9857110f17b4
author: shirgall
ms.author: kathydav
ms.date: 08/30/2017
ms.openlocfilehash: 013328953321bc66b3fd30759e5be321eea32dde
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824236"
---
# <a name="supported-freebsd-virtual-machines-on-hyper-v"></a>Máquinas de virtuales de FreeBSD compatibles en Hyper-V

>Se aplica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

El siguiente mapa de distribución de la característica indica las características de cada versión. Después de la tabla se enumeran los problemas conocidos y soluciones alternativas para cada distribución.

## <a name="table-legend"></a>Leyenda de la tabla

* **Integrado** -BIS (servicio de integración de FreeBSD) se incluyen como parte de esta versión de FreeBSD.

* &#10004;-Característica disponible

* (*en blanco*)-característica no está disponible

|**Característica**|**Versión del sistema operativo Windows Server**|**11.1/11.2**|**11.0**|**10.3**|**10.2**|**10.0 - 10.1**|**9.1 - 9.3, 8.4**|
|-|-|-|-|-|-|-|-|
|**Disponibilidad**||Integrado|Integrado|Integrado|Integrado|Integrado|[Puertos](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) |
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004; |
|Hora exacta de Windows Server 2016|2016|&#10004;||||||
|**[Funciones de red](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**||||||||
|Tramas gigantes|2016, 2012 R2, 2012, 2008 R2|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|&#10004;Nota 3|
|Etiquetado de VLAN y enlace troncal|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migración en vivo|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Inserción de la dirección IP estática|2016, 2012 R2, 2012|&#10004;Nota 4|&#10004;Nota 4|&#10004;Nota 4|&#10004;Nota 4|&#10004;Nota 4|&#10004;|
|vRSS|2016, 2012 R2|&#10004;|&#10004;|||||
|Segmentación de TCP y las descargas de suma de comprobación|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Grandes recibir descarga (LRO)|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||||
|SR-IOV|2016|||||||
|**[Storage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**||Nota 1|Nota 1|Nota 1|Nota 1|Nota 1, 2|Nota 1, 2|
|Cambio de tamaño de VHDX|2016, 2012 R2|&#10004;Nota 7|&#10004;Nota 7|||||
|Canal de fibra virtual|2016, 2012 R2|||||||
|Copia de seguridad de máquina virtual en vivo|2016, 2012 R2|&#10004;||||||
|RECORTAR el soporte técnico|2016, 2012 R2|&#10004;||||||
|SCSI WWN|2016, 2012 R2|||||||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**||||||||
|Soporte técnico de núcleo PAE|2016, 2012 R2, 2012, 2008 R2|||||||
|Configuración de gap MMIO|2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memoria dinámica - agregado en caliente|2016, 2012 R2, 2012|||||||
|Memoria dinámica - incremento|2016, 2012 R2, 2012|||||||
|Cambio de tamaño de memoria en tiempo de ejecución|2016|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**||||||||
|Dispositivo de vídeo específico de Hyper-V|2016, 2012 R2, 2012, 2008 R2|||||||
|**[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**||||||||
|Par clave/valor|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;Nota 6|&#10004;Nota 5, 6|&#10004;Nota 6|
|Interrupción no enmascarable|2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copia de archivos de host a invitado|2016, 2012 R2|||||||
|comando lsvmbus|2016, 2012 R2, 2012, 2008 R2|||||||
|Sockets de Hyper-V|2016|||||||
|Acceso directo/DDA de PCI|2016|&#10004;||||||
|**[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**||||||||
|Arrancar con UEFI|2016, 2012 R2|&#10004;||||||
|Arranque seguro|2016|||||||

## <a name="BKMK_notes"></a>Notas de la

1. Sugerir a [dispositivos de disco de la etiqueta]( https://www.freebsd.org/doc/handbook/geom-glabel.html) para evitar el ERROR de montaje de raíz durante el inicio.

2. La unidad de DVD virtual puede no ser reconocida cuando se cargan los controladores de BIS en FreeBSD 8.x y 9.x a menos que habilite el controlador ATA heredado mediante el siguiente comando.
    ```sh
    # echo ‘hw.ata.disk_enable=1’ >> /boot/loader.conf
    # shutdown -r now
    ```

3. 9126 es que el número máximo admitido de tamaño de MTU.

4. En un escenario de conmutación por error, no se puede establecer una dirección IPv6 estática en el servidor de réplica. Use una dirección IPv4 en su lugar.

5. KVP se proporciona mediante los puertos en FreeBSD 10.0. Consulte la [10.0 FreeBSD puertos](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) en FreeBSD.org para obtener más información.

6. KVP no funcionen en Windows Server 2008 R2.

7. Para realizar el trabajo de cambio de tamaño en línea de VHDX correctamente en la versión 11.0 de FreeBSD, un paso manual especial es necesario para solucionar un error GEOM que se ha corregido en 11.0 + después de que el host cambia el tamaño del disco VHDX: abrir el disco para escribir y ejecutar "recover gpart" como el siguiente.
    ```sh
    # dd if=/dev/da1 of=/dev/da1 count=0
    # gpart recover da1
    ```
**Notas adicionales**: La matriz de características de estable 10 y 11 estable es la misma versión de FreeBSD 11.1. Además, FreeBSD 10.2 y versiones anteriores (10.1, 10.0, 9.x, 8.x) son el final del ciclo de vida. Consulte [aquí](https://security.freebsd.org/) para obtener una lista actualizada de las versiones admitidas y los avisos de seguridad más recientes.

**Notas adicionales**: La matriz de características de estable 10 y 11 estable es la misma versión de FreeBSD 11.1. Además, FreeBSD 10.2 y versiones anteriores (10.1, 10.0, 9.x, 8.x) son el final del ciclo de vida. Consulte [aquí](https://security.freebsd.org/) para obtener una lista actualizada de las versiones admitidas y los avisos de seguridad más recientes.

## <a name="see-also"></a>Vea también

* [Descripciones de características para las máquinas virtuales de Linux y FreeBSD en Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)
* [Procedimientos recomendados para ejecutar FreeBSD en Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
