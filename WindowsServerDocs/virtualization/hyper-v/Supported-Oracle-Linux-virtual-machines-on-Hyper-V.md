---
title: Oracle Linux compatibles con máquinas virtuales en Hyper-V
description: Enumera las características y servicios de integración de Linux que se incluyen en cada versión
manager: dongill
ms.topic: article
ms.assetid: c02fdb5b-62f3-43cb-a190-ab74b3ebcf77
author: shirgall
ms.author: kathydav
ms.date: 06/05/2020
ms.openlocfilehash: 0e9a11fbff5015037bffa1cad14e70d629fef94b
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989308"
---
# <a name="supported-oracle-linux-virtual-machines-on-hyper-v"></a>Oracle Linux compatibles con máquinas virtuales en Hyper-V

>Se aplica a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows 10, Windows 8.1

El siguiente mapa de distribución de características indica las características que se encuentran en cada versión. Los problemas conocidos y las soluciones alternativas para cada distribución se enumeran después de la tabla.

En esta sección:

* [Serie Oracle Linux 8. x](#oracle-linux-8x-series)
* [Serie Oracle Linux 7. x](#oracle-linux-7x-series)
* [Serie Oracle Linux 6. x](#oracle-linux-6x-series)


## <a name="table-legend"></a>Leyenda de tabla

* La **compilación en** LIS se incluye como parte de esta distribución de Linux. Los números de versión del módulo de kernel para el LIS integrado (como se muestra en **lsmod**, por ejemplo) son diferentes del número de versión del paquete de descarga de lis proporcionado por Microsoft. Una desigualdad no indica que el LIS integrado no está actualizado.

* &#10004;: característica disponible
* (*en blanco*): característica no disponible
* **RHCK** : kernel compatible con Red Hat
* **UEK** -Unbreakable Enterprise kernel (UEK)
   * UEK4: basado en la versión del kernel de Linux de nivel superior 4.1.12
   * UEK5: basado en la versión del kernel de Linux de nivel superior 4,14
   * UEK6: basado en la versión del kernel de Linux de nivel superior 5,4

## <a name="oracle-linux-8x-series"></a>Serie Oracle Linux 8. x

|       **Característica**     |       **Versión de Windows Server**      |       **8.0-8.1 (RHCK)** |
|-----------------------|---------------------------------------|-------------------|
|       **Disponibilidad**        |   |
|       **[Principal](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**      | 2019, 2016, 2012 R2 | &#10004; |
|       Windows Server 2016 hora precisa       | 2019, 2016 | &#10004; |
|       **[Redes](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**      |   |
|       Tramas gigantes        | 2019, 2016, 2012 R2 | &#10004; |
|       Etiquetado y Troncalización de VLAN       | 2019, 2016, 2012 R2 | &#10004;  |
|       Migración en vivo      | 2019, 2016, 2012 R2 | &#10004; |
|       Inyección de IP estática     |  2019, 2016, 2012 R2 | &#10004; Nota 2 |
|       vRSS     | 2019, 2016, 2012 R2 | &#10004; |
|       Segmentación y descarga de sumas de comprobación TCP | 2019, 2016, 2012 R2 | &#10004;|
|       SR-IOV  | 2019, 2016 |  &#10004;   |
|       **[Storage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)** |  |
|       Cambiar el tamaño de VHDX  | 2019, 2016, 2012 R2 | &#10004; |
|       Canal de fibra virtual | 2019, 2016, 2012 R2 | &#10004; Nota 3  |
|       Copia de seguridad de máquinas virtuales en vivo  | 2019, 2016, 2012 R2 | Nota &#10004; 5 |
|       Compatibilidad con TRIM | 2019, 2016, 2012 R2 | &#10004;  |
|       WWN SCSI | 2019, 2016, 2012 R2 | &#10004;  |
|       **[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)** | |
|       Compatibilidad con el kernel PAE  | 2019, 2016, 2012 R2 |  N/D |
|       Configuración de la brecha de MMIO  | 2019, 2016, 2012 R2 | &#10004; |
|       Memoria dinámica: agregar en caliente | 2019, 2016, 2012 R2  | &#10004; Nota 7, 8, 9 |
|       Memoria dinámica: globos | 2019, 2016, 2012 R2 | &#10004; Nota 7, 8, 9 |
|       Tamaño de memoria en tiempo de ejecución | 2019, 2016  | &#10004;  |
|       **[Cámara](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)** | |
|       Dispositivo de vídeo específico de Hyper-V | 2019, 2016, 2012 R2 | &#10004;   |
|       **[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)** | |
|       Par clave-valor  | 2019, 2016, 2012 R2 | &#10004;   |
|       Interrupción no enmascarable | 2019, 2016, 2012 R2 | &#10004;  |
|       Copia de archivos de host a invitado | 2019, 2016, 2012 R2 | &#10004;  |
|       comando lsvmbus | 2019, 2016, 2012 R2 | &#10004;  |
|       Sockets de Hyper-V | 2019, 2016 | &#10004;  |
|       Acceso directo/DDA de PCI | 2019, 2016 | &#10004; |
| **[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** | |  |
|       Arranque mediante UEFI | 2019, 2016, 2012 R2 |  &#10004; Nota 12  |
|       Arranque seguro | 2019, 2016 |  &#10004; |

## <a name="oracle-linux-7x-series"></a>Serie Oracle Linux 7. x

Esta serie solo tiene kernels de 64 bits.

<table width="100%">
<tr height="50px">
<td width="20%" rowspan="2">

Característica
</td>
<td width="20%" rowspan="2">

Versión de Windows Server
</td>
<td width="30%" colspan="3">

7,5-7.8
</td>
<td width="30%" colspan="3">

7.3-7.4
</td>
</tr>
<tr>
<td width="20%" colspan="2">

RHCK
</td>
<td width="10%">

UEK5
</td>
<td width="20%" colspan="2">

RHCK
</td>
<td width="10%">

UEK4
</td>
</tr>
<tr>
<td width="20%">

Disponibilidad
</td>
<td width="20%">


</td>
<td width="10%">

LIS 4,3
</td>
<td width="10%">

Integrado
</td>
<td width="10%">

Integrado
</td>
<td width="10%">

LIS 4,3
</td>
<td width="10%">

Integrado
</td>
<td width="10%">

Integrado
</td>
</tr>
<tr height="50px">
<td width="20%">

**[Principal](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Windows Server 2016 hora precisa
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

 **[Redes](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Tramas gigantes
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">
Etiquetado y Troncalización de VLAN
</td>
<td width="20%">
2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Migración en vivo
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Inyección de IP estática
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004; Nota 2
</td>
<td width="10%">

&#10004; Nota 2
</td>
<td width="10%">

&#10004; Nota 2
</td>
<td width="10%">

&#10004; Nota 2
</td>
<td width="10%">

&#10004; Nota 2
</td>
<td width="10%">

&#10004; Nota 2
</td>
</tr>
<tr height="50px">
<td width="20%">

vRSS
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Segmentación y descarga de sumas de comprobación TCP
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

SR-IOV
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

**[Storage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Cambiar el tamaño de VHDX
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Canal de fibra virtual
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004; Nota 3
</td>
<td width="10%">

&#10004; Nota 3
</td>
<td width="10%">

&#10004; Nota 3
</td>
<td width="10%">

&#10004; Nota 3
</td>
<td width="10%">

&#10004; Nota 3
</td>
<td width="10%">

&#10004; Nota 3
</td>
</tr>
<tr height="50px">
<td width="20%">

Copia de seguridad de máquinas virtuales en vivo
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

Nota &#10004; 5
</td>
<td width="10%">

&#10004; Nota 4, 5
</td>
<td width="10%">

Nota &#10004; 5
</td>
<td width="10%">

Nota &#10004; 5
</td>
<td width="10%">

&#10004; Nota 4, 5
</td>
<td width="10%">

Nota &#10004; 5
</td>
</tr>
<tr height="50px">
<td width="20%">

Compatibilidad con TRIM
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

WWN SCSI
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**
</td>
<td width="20%">


</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Compatibilidad con el kernel PAE
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
<td width="10%">

N/D
</td>
</tr>
<tr height="50px">
<td width="20%">

Configuración de la brecha de MMIO
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Memoria dinámica agregar en caliente
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004; Nota 7, 8, 9
</td>
<td width="10%">

&#10004; Nota 8, 9
</td>
<td width="10%">

&#10004; Nota 8, 9
</td>
<td width="10%">

&#10004; Nota 8, 9
</td>
<td width="10%">

&#10004; Nota 8, 9
</td>
<td width="10%">

&#10004; Nota 8, 9
</td>
</tr>
<tr height="50px">
<td width="20%">

Memoria dinámica globos
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004; Nota 7, 8, 9
</td>
<td width="10%">

&#10004; Nota 8, 9
</td>
<td width="10%">

&#10004; Nota 8, 9
</td>
<td width="10%">

&#10004; Nota 8, 9
</td>
<td width="10%">

&#10004; Nota 8, 9
</td>
<td width="10%">

&#10004; Nota 8, 9
</td>
</tr>
<tr height="50px">
<td width="20%">

Tamaño de memoria en tiempo de ejecución
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

**[Cámara](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Vídeo específico de Hyper-V
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

**[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Par clave-valor
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Interrupción no enmascarable
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Copia de archivos de host a invitado
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

comando lsvmbus
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Sockets de Hyper-V
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">


</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

Acceso directo/DDA de PCI
</td>
<td width="20%">

2019, 2016
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
<tr height="50px">
<td width="20%">

**[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**
</td>
<td width="20%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
<td width="10%">

</td>
</tr>
<tr height="50px">
<td width="20%">

Arranque mediante UEFI
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004; Nota 12
</td>
<td width="10%">

&#10004; Nota 12
</td>
<td width="10%">

&#10004; Nota 12
</td>
<td width="10%">

&#10004; Nota 12
</td>
<td width="10%">

&#10004; Nota 12
</td>
<td width="10%">

&#10004; Nota 12
</td>
</tr>
<tr height="50px">
<td width="20%">

Arranque seguro
</td>
<td width="20%">

2019, 2016, 2012 R2
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
<td width="10%">

&#10004;
</td>
</tr>
</table>


## <a name="oracle-linux-6x-series"></a>Serie Oracle Linux 6. x

Esta serie solo tiene kernels de 64 bits.

|       **Característica**     |       **Versión de Windows Server**      |       **6.8-6.10 (RHCK)** |       **6.8-6.10 (UEK4)**     |
|-----------------------|---------------------------------------|-------------------|-------------------|
|       **Disponibilidad**     |   | LIS 4,3  | Integrado  |
|       **[Principal](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**      | 2019, 2016, 2012 R2 | &#10004; | &#10004;
|       Windows Server 2016 hora precisa       | 2019, 2016 | |
|       **[Redes](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**      |   |  |
|       Tramas gigantes        | 2019, 2016, 2012 R2 | &#10004; | &#10004;|
|       Etiquetado y Troncalización de VLAN       | 2019, 2016, 2012 R2 | &#10004; Nota 1 | &#10004; Nota 1 |
|       Migración en vivo      | 2019, 2016, 2012 R2 | &#10004; | &#10004;|
|       Inyección de IP estática     |  2019, 2016, 2012 R2 | &#10004; Nota 2 | &#10004;|
|       vRSS     | 2019, 2016, 2012 R2 | &#10004; | &#10004;|
|       Segmentación y descarga de sumas de comprobación TCP | 2019, 2016, 2012 R2 | &#10004;|  &#10004; |
|       SR-IOV  | 2019, 2016 |    |  |
|       **[Storage](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)** |  |  |
|       Cambiar el tamaño de VHDX  | 2019, 2016, 2012 R2 | &#10004; | &#10004; |
|       Canal de fibra virtual | 2019, 2016, 2012 R2 | &#10004; Nota 3  | &#10004; Nota 3 |
|       Copia de seguridad de máquinas virtuales en vivo  | 2019, 2016, 2012 R2 | Nota &#10004; 5 | Nota &#10004; 5|
|       Compatibilidad con TRIM | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       WWN SCSI | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       **[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)** | |  |
|       Compatibilidad con el kernel PAE  | 2019, 2016, 2012 R2 |  N/D | N/D
|       Configuración de la brecha de MMIO  | 2019, 2016, 2012 R2 | &#10004; | &#10004;  |
|       Memoria dinámica: agregar en caliente | 2019, 2016, 2012 R2  | &#10004; Nota 6, 8, 9 | &#10004; Nota 6, 8, 9 |
|       Memoria dinámica: globos | 2019, 2016, 2012 R2 | &#10004; Nota 6, 8, 9 | &#10004; Nota 6, 8, 9 |
|       Tamaño de memoria en tiempo de ejecución | 2019, 2016  |  | |
|       **[Cámara](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)** | | |
|       Dispositivo de vídeo específico de Hyper-V | 2019, 2016, 2012 R2 | &#10004;   | &#10004; |
|       **[Varios](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)** | | |
|       Par clave-valor  | 2019, 2016, 2012 R2 | &#10004; Nota 10, 11   | &#10004; Nota 10, 11  |
|       Interrupción no enmascarable | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       Copia de archivos de host a invitado | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       comando lsvmbus | 2019, 2016, 2012 R2 | &#10004;  | &#10004; |
|       Sockets de Hyper-V | 2019, 2016 | &#10004;  | &#10004; |
|       Acceso directo/DDA de PCI | 2019, 2016 | &#10004; | &#10004; |
| **[Máquinas virtuales de generación 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** | |  |
|       Arranque mediante UEFI | 2019, 2016, 2012 R2 |  &#10004; Nota 12  | &#10004; Nota 12
|       Arranque seguro | 2019, 2016 |  |  |



## <a name="notes"></a><a name="BKMK_notes"></a>Notas

1. En esta versión de Oracle Linux, el etiquetado de VLAN funciona pero no la Troncalización de VLAN.

2. Es posible que la inyección de direcciones IP estáticas no funcione si se ha configurado el administrador de red para un adaptador de red sintético determinado en la máquina virtual. Para un funcionamiento sin problemas de la inyección de direcciones IP estáticas, asegúrese de que el administrador de red esté desactivado completamente o se haya desactivado para un adaptador de red específico a través de su archivo ifcfg-ethX.

3.  En Windows Server 2012 R2 al usar dispositivos de canal de fibra virtual, asegúrese de que se ha rellenado el número de unidad lógica 0 (LUN 0). Si LUN 0 no se ha rellenado, es posible que una máquina virtual de Linux no pueda montar los dispositivos de canal de fibra de forma nativa.

4. En el caso de LIS integrado, debe instalarse el paquete de "los demonios de HyperV" para esta funcionalidad.

5.  Si hay identificadores de archivo abiertos durante una operación de copia de seguridad de una máquina virtual activa, en algunos casos, los VHD de copia de seguridad podrían tener que someterse a una comprobación de coherencia del sistema de archivos (fsck) en la restauración. Las operaciones de copia de seguridad en directo pueden producir errores silenciosamente si la máquina virtual tiene un dispositivo iSCSI conectado o un almacenamiento conectado directamente (también conocido como disco de acceso directo).

6. La compatibilidad con memoria dinámica solo está disponible en las máquinas virtuales de 64 bits.

7. La compatibilidad con la adición en caliente no está habilitada de forma predeterminada en esta distribución. Para habilitar la compatibilidad con la adición en caliente, debe agregar una regla de udev en/etc/udev/rules.d/como se indica a continuación:

   1. Cree un archivo **/etc/udev/rules.d/100-Balloon.rules**. Puede usar cualquier otro nombre que desee para el archivo.

   2. Agregue el siguiente contenido al archivo:`SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. Reinicie el sistema para habilitar la compatibilidad con la adición en caliente.

   Mientras que la descarga de Linux Integration Services crea esta regla en la instalación, la regla también se quita cuando se desinstala LIS, por lo que la regla debe volver a crearse si se necesita memoria dinámica después de la desinstalación.

8. Se puede producir un error en las operaciones de memoria dinámica si el sistema operativo invitado se está ejecutando demasiado bajo en la memoria. A continuación se indican algunas prácticas recomendadas:

   * La memoria de inicio y la memoria mínima deben ser iguales o mayores que la cantidad de memoria que el proveedor de distribución recomienda.

   * Las aplicaciones que tienden a consumir toda la memoria disponible en un sistema se limitan a consumir hasta el 80 por ciento de la RAM disponible.

9. Si utiliza Memoria dinámica en un sistema operativo Windows Server 2016 o Windows Server 2012 R2, especifique la **memoria de inicio**, la **memoria mínima**y los parámetros de **memoria máxima** en múltiplos de 128 megabytes (MB). Si no lo hace, pueden producirse errores de adición en caliente y es posible que no vea ningún aumento de la memoria en un sistema operativo invitado.

10. Para habilitar la infraestructura de pares clave-valor (KVP), instale el paquete RPM de hypervkvpd o HyperV-daemons en el Oracle Linux ISO. También puede instalar el paquete directamente desde Oracle Linux repositorios de yum.

11. Es posible que la infraestructura de pares clave-valor (KVP) no funcione correctamente sin una actualización de software de Linux. Póngase en contacto con el proveedor de distribución para obtener la actualización de software en caso de que vea problemas con esta característica.

12. En las máquinas virtuales con Windows Server 2012 R2 de segunda generación 2, el arranque seguro está habilitado de forma predeterminada y algunas máquinas virtuales Linux no se iniciarán a menos que se deshabilite la opción de arranque seguro. Puede deshabilitar el arranque seguro en la sección **firmware** de la configuración de la máquina virtual en el **Administrador de Hyper-V** o puede deshabilitarla mediante PowerShell:

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

    La descarga de Integration Services de Linux se puede aplicar a las máquinas virtuales de generación 2 existentes, pero no a la capacidad de generación 2.


Consulte también

* [Set-VMFirmware](/powershell/module/hyper-v/set-vmfirmware?view=win10-ps)

* [Compatibilidad con máquinas virtuales de alta y Red Hat Enterprise Linux en Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales Debian admitidas en Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de SUSE compatibles en Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de Ubuntu admitidas en Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuales de FreeBSD compatibles en Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descripciones de características de máquinas virtuales Linux y FreeBSD en Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Prácticas recomendadas para ejecutar Linux en Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)