---
title: Máquinas virtuales de SUSE compatibles en Hyper-V
description: Enumera las características y servicios de integración de Linux que se incluyen en cada versión
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ec0e14c-4498-4bd9-8fe6-b94260198efc
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 45517c1d381ba55c819b09b53ae563092e161b1e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366729"
---
# <a name="supported-suse-virtual-machines-on-hyper-v"></a>Máquinas virtuales de SUSE compatibles en Hyper-V

>Se aplica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

A continuación se muestra un mapa de distribución de características que indica las características de cada versión. Los problemas conocidos y las soluciones alternativas para cada distribución se enumeran después de la tabla.

Los Controladores integrados de SUSE Linux Enterprise Service para Hyper-V están certificados por SUSE. En este boletín se puede ver una configuración de ejemplo: [Boletín de certificación de SUSE sí](https://www.suse.com/nbswebapp/yesBulletin.jsp?bulletinNumber=144176).

## <a name="table-legend"></a>Leyenda de tabla

* La **compilación en** LIS se incluye como parte de esta distribución de Linux. El paquete de descarga de LIS proporcionado por Microsoft no funciona para esta distribución, por lo que no se debe instalar. Los números de versión del módulo de kernel para el LIS integrado (como se muestra en **lsmod**, por ejemplo) son diferentes del número de versión del paquete de descarga de lis proporcionado por Microsoft. Una desigualdad no indica que el LIS integrado no está actualizado.

* &#10004;-Característica disponible

* (*en blanco*): característica no disponible

SLES12 + es solo 64 bits.

|**Característica**|**Versión del sistema operativo Windows Server**|**SLES 15**|**SLES 12 SP3/SP4**|**SLES 12 SP2**|**SLES 12 SP1**|**SLES 11 SP4**|**SLES 11 SP3**|
|-|-|-|-|-|-|-|-|
|**Disponibilidad**||Integrados|Integrados|Integrados|Integrados|Integrados|Integrados|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 hora precisa|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[Soluciona](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||||
|Tramas gigantes|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Etiquetado y Troncalización de VLAN|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migración en vivo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Inyección de IP estática|2019, 2016, 2012 R2, 2012|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|
|vRSS|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Segmentación y descarga de sumas de comprobación TCP|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[Discos](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||||
|Cambiar el tamaño de VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Canal de fibra virtual|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copia de seguridad de máquinas virtuales en vivo|2019, 2016, 2012 R2|&#10004;Nota 2, 3, 8|&#10004;Nota 2, 3, 8|&#10004;Nota 2, 3, 8|&#10004;Nota 2, 3, 8|&#10004;Nota 2, 3, 8|&#10004;Nota 2, 3, 8|
|Compatibilidad con TRIM|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|WWN SCSI|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;||||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||||
|Compatibilidad con el kernel PAE|2019, 2016, 2012 R2, 2012, 2008 R2|N/D|N/D|N/D|N/D|&#10004;|&#10004;|
|Configuración de la brecha de MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memoria dinámica: agregar en caliente|2019, 2016, 2012 R2, 2012|&#10004;Nota 5, 6|&#10004;Nota 5, 6|&#10004;Nota 5, 6|&#10004;Nota 5, 6|&#10004;Nota 4, 5, 6|&#10004;Nota 4, 5, 6|
|Memoria dinámica: globos|2019, 2016, 2012 R2, 2012|&#10004;Nota 5, 6|&#10004;Nota 5, 6|&#10004;Nota 5, 6|&#10004;Nota 5, 6|&#10004;Nota 4, 5, 6|&#10004;Nota 4, 5, 6|
|Tamaño de memoria en tiempo de ejecución|2019, 2016|&#10004;Nota 5, 6|&#10004;Nota 5, 6|&#10004;Nota 5, 6||||
|**[Cámara](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||||
|Dispositivo de vídeo específico de Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||||
|Par clave-valor|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;Nota 7|&#10004;Nota 7|
|Interrupción no enmascarable|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copia de archivos de host a invitado|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||||
|Sockets de Hyper-V|2019, 2016|&#10004;|&#10004;|||||
|Acceso directo/DDA de PCI|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||
|**[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||||
|Arranque mediante UEFI|2019, 2016, 2012 R2|&#10004;Nota 9|&#10004;Nota 9|&#10004;Nota 9|&#10004;Nota 9|&#10004;Nota 9||
|Arranque seguro|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||

## <a name="BKMK_notes"></a>Apunte

1. Es posible que la inyección de direcciones IP estáticas no funcione si se ha configurado el **Administrador de red** para un adaptador de red específico de Hyper-V determinado en la máquina virtual. Para garantizar el correcto funcionamiento de la inyección de IP estática, asegúrese de que el administrador de red esté desactivado completamente o se haya desactivado para un adaptador de red específico a través de su archivo **ifcfg-ethX** .

2. Si hay identificadores de archivo abiertos durante una operación de copia de seguridad de una máquina virtual activa, en algunos casos, los VHD de copia de seguridad podrían tener que someterse a una comprobación de coherencia del sistema de archivos (fsck) en la restauración.

3. Las operaciones de copia de seguridad en directo pueden producir errores silenciosamente si la máquina virtual tiene un dispositivo iSCSI conectado o un almacenamiento conectado directamente (también conocido como disco de acceso directo).

4. Se puede producir un error en las operaciones de memoria dinámica si el sistema operativo invitado se está ejecutando demasiado bajo en la memoria. A continuación se indican algunas prácticas recomendadas:

   * La memoria de inicio y la memoria mínima deben ser iguales o mayores que la cantidad de memoria que el proveedor de distribución recomienda.

   * Las aplicaciones que tienden a consumir toda la memoria disponible en un sistema se limitan a consumir hasta el 80 por ciento de la RAM disponible.

5. La compatibilidad con memoria dinámica solo está disponible en las máquinas virtuales de 64 bits.

6. Si usa Memoria dinámica en sistemas operativos Windows Server 2016 o Windows Server 2012, especifique la **memoria de inicio**, la **memoria mínima**y los parámetros de **memoria máxima** en múltiplos de 128 megabytes (MB). Si no lo hace, pueden producirse errores de adición en caliente y es posible que no vea ningún aumento de la memoria en un sistema operativo invitado.

7. En Windows Server 2016 o Windows Server 2012 R2, es posible que la infraestructura de pares clave-valor no funcione correctamente sin una actualización de software de Linux. Póngase en contacto con el proveedor de distribución para obtener la actualización de software en caso de que vea problemas con esta característica.

8. La copia de seguridad de VSS producirá un error si una sola partición se monta varias veces.

9. En Windows Server 2012 R2, las máquinas virtuales de generación 2 tienen el arranque seguro habilitado de forma predeterminada y las máquinas virtuales Linux de generación 2 no se iniciarán a menos que se deshabilite la opción de arranque seguro. Puedes deshabilitar arranque seguro en la sección **Firmware** de la configuración de la máquina virtual en Administrador de Hyper-V, o bien mediante PowerShell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

## <a name="see-also"></a>Vea también

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Compatibilidad con máquinas virtuales de alta y Red Hat Enterprise Linux en Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales Debian admitidas en Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Oracle Linux compatibles con máquinas virtuales en Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de Ubuntu admitidas en Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de FreeBSD compatibles en Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descripciones de características de máquinas virtuales Linux y FreeBSD en Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Prácticas recomendadas para ejecutar Linux en Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
