---
title: Máquinas de virtuales SUSE compatibles en Hyper-V
description: Enumera los servicios de integración de Linux y características incluidas en cada versión
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ec0e14c-4498-4bd9-8fe6-b94260198efc
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: d7b6d3adb4841ea827c56309307549c911a439ea
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222810"
---
# <a name="supported-suse-virtual-machines-on-hyper-v"></a>Máquinas de virtuales SUSE compatibles en Hyper-V

>Se aplica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

El siguiente es un mapa de la distribución de característica que indica las características de cada versión. Después de la tabla se enumeran los problemas conocidos y soluciones alternativas para cada distribución.

Los controladores de SUSE Linux Enterprise Service integrados para Hyper-V están certificados por SUSE. Un ejemplo de configuración se puede ver en este boletín: [Boletín de certificación de SUSE Sí](https://www.suse.com/nbswebapp/yesBulletin.jsp?bulletinNumber=144176).

## <a name="table-legend"></a>Leyenda de la tabla

* **Integrado** -LIS se incluye como parte de esta distribución de Linux. El paquete de descarga LIS proporcionada por Microsoft no funciona para esta distribución, por lo que no lo instale. Los números de versión del módulo de kernel para la compilación en LIS (tal como se muestra por **lsmod**, por ejemplo) son diferentes desde el número de versión del paquete de descarga LIS proporcionada por Microsoft. Un error de coincidencia no indica que la compilación en LIS está obsoleta.

* &#10004;-Característica disponible

* (*en blanco*)-característica no está disponible

SLES12 + es 64 bits sólo.

|**Característica**|**Versión del sistema operativo Windows Server**|**SLES 15**|**SLES 12 SP3 O SP4**|**SLES 12 SP2**|**SLES 12 SP1**|**SLES 11 SP4**|**SLES 11 SP3**|
|-|-|-|-|-|-|-|-|
|**Disponibilidad**||Integrados|Integrados|Integrados|Integrados|Integrados|Integrados|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Hora exacta de Windows Server 2016|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[Funciones de red](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||||
|Tramas gigantes|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Etiquetado de VLAN y enlace troncal|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migración en vivo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Inserción de la dirección IP estática|2019, 2016, 2012 R2, 2012|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Segmentación de TCP y las descargas de suma de comprobación|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[Storage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||||
|Cambio de tamaño de VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Canal de fibra virtual|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copia de seguridad de máquina virtual en vivo|2019, 2016, 2012 R2|&#10004;Nota 2, 3, 8|&#10004;Nota 2, 3, 8|&#10004;Nota 2, 3, 8|&#10004;Nota 2, 3, 8|&#10004;Nota 2, 3, 8|&#10004;Nota 2, 3, 8|
|RECORTAR el soporte técnico|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SCSI WWN|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;||||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||||
|Soporte técnico de núcleo PAE|2019, 2016, 2012 R2, 2012, 2008 R2|N/D|N/D|N/D|N/D|&#10004;|&#10004;|
|Configuración de gap MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memoria dinámica - agregado en caliente|2019, 2016, 2012 R2, 2012|&#10004;Nota 5, 6|&#10004;Nota 5, 6|&#10004;Nota 5, 6|&#10004;Nota 5, 6|&#10004;Tenga en cuenta la 4, 5, 6|&#10004;Tenga en cuenta la 4, 5, 6|
|Memoria dinámica - incremento|2019, 2016, 2012 R2, 2012|&#10004;Nota 5, 6|&#10004;Nota 5, 6|&#10004;Nota 5, 6|&#10004;Nota 5, 6|&#10004;Tenga en cuenta la 4, 5, 6|&#10004;Tenga en cuenta la 4, 5, 6|
|Cambio de tamaño de memoria en tiempo de ejecución|2019, 2016|&#10004;Nota 5, 6|&#10004;Nota 5, 6|&#10004;Nota 5, 6||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||||
|Dispositivo de vídeo específico de Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||||
|Par clave/valor|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;Nota 7|&#10004;Nota 7|
|Interrupción no enmascarable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copia de archivos de host a invitado|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||||
|Sockets de Hyper-V|2019, 2016|&#10004;|&#10004;|||||
|Acceso directo/DDA de PCI|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||
|**[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||||
|Arrancar con UEFI|2019, 2016, 2012 R2|&#10004;Tenga en cuenta 9|&#10004;Tenga en cuenta 9|&#10004;Tenga en cuenta 9|&#10004;Tenga en cuenta 9|&#10004;Tenga en cuenta 9||
|Arranque seguro|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||

## <a name="BKMK_notes"></a>Notas de la

1. Inserción de IP estática puede no funcionar si **Network Manager** se ha configurado para un determinado adaptador de red específico de Hyper-V en la máquina virtual. Para garantizar un funcionamiento de la dirección IP estática inyección Asegúrese de que el Administrador de red está completamente desactivada o se ha desactivado para un adaptador de red específico a través de su **ifcfg ethX** archivo.

2. Si hay identificadores de archivos abiertos durante una operación de copia de seguridad de máquina virtual activa, a continuación, en algunos casos excepcionales, podrían tener los VHD de copia de seguridad que debe someterse a una comprobación de coherencia del sistema de archivo (fsck) en la restauración.

3. Operaciones de copia de seguridad en directo pueden fallar en modo silencioso si la máquina virtual tiene un dispositivo iSCSI conectados o almacenamiento conectado directo (también conocido como un disco de acceso directo).

4. Las operaciones de memoria dinámica pueden producir un error si el sistema operativo invitado se ejecuta demasiado bajo en memoria. Éstas son algunas prácticas recomendadas:

   * Memoria mínima y memoria de inicio deben ser igual o mayor que la cantidad de memoria que recomienda el proveedor de distribución.

   * Las aplicaciones que tienden a consumir toda la memoria disponible en un sistema se limitan a consumir hasta el 80 por ciento de memoria RAM disponible.

5. Compatibilidad con memoria dinámica sólo está disponible en las máquinas virtuales de 64 bits.

6. Si usa memoria dinámica en sistemas operativos Windows Server 2016 o Windows Server 2012, especifique **memoria de inicio**, **cantidad mínima de memoria**, y **cantidad máxima de memoria** parámetros en múltiplos de 128 megabytes (MB). Si no lo hace puede provocar errores de adición sin interrupción, y puede que no vea toda la memoria aumenta en un sistema operativo invitado.

7. En Windows Server 2016 o Windows Server 2012 R2, la infraestructura de par clave/valor no podría funcionar correctamente sin una actualización de software de Linux. Póngase en contacto con su proveedor de distribución para obtener la actualización de software en caso de que vea problemas con esta característica.

8. Copia de seguridad VSS se producirá un error si una sola partición se ha montado varias veces.

9. En Windows Server 2012 R2, las máquinas virtuales tienen el arranque seguro habilitado de forma predeterminada y las máquinas virtuales Linux de generación 2 de generación 2 no arrancará, a menos que la opción de arranque seguro está deshabilitada. Puedes deshabilitar arranque seguro en la sección **Firmware** de la configuración de la máquina virtual en Administrador de Hyper-V, o bien mediante PowerShell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

## <a name="see-also"></a>Vea también

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Admite CentOS y Red Hat Enterprise Linux virtual machines en Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales Debian admitidas en Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales Oracle Linux compatibles en Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales de Ubuntu compatibles en Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuales de FreeBSD compatibles en Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descripciones de características para las máquinas virtuales de Linux y FreeBSD en Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Procedimientos recomendados para ejecutar Linux en Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
