---
title: Límites de escalabilidad del servidor de destino iSCSI
TOCTitle: iSCSI Target Server Scalability Limits
description: 'Más información sobre: límites de escalabilidad del servidor de destino iSCSI'
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 657dfea08aba671883d0cca01f53854f48871544
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048733"
---
# <a name="iscsi-target-server-scalability-limits"></a>Límites de escalabilidad del servidor de destino iSCSI

Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se proporcionan los límites admitidos y probados del servidor de destino iSCSI de Microsoft en Windows Server. En las tablas siguientes se muestran los límites de compatibilidad probados y, si procede, si se aplican los límites.

## <a name="general-limits"></a>Límites generales

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Elemento</p></th>
<th><p>Límite de compatibilidad</p></th>
<th><p>Aplica?</p></th>
<th><p>Comentario</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>instancias de destino iSCSI por servidor de destino iSCSI</p></td>
<td><p>256</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>unidades lógicas (UGM) de iSCSI o discos virtuales por cada servidor de destino iSCSI</p></td>
<td><p>512</p></td>
<td><p>No</p></td>
<td><p>Configuraciones de pruebas incluidas: 8 UGM por instancia de destino con un promedio de 64 destinos y 256 instancias de destino con un LU por destino.</p></td>
</tr>
<tr class="odd">
<td><p>unidades de disco virtual o UGM iSCSI por instancia de destino iSCSI</p></td>
<td><p>256 (128 en Windows Server 2012)</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Sesiones que pueden conectarse simultáneamente a una instancia de destino iSCSI</p></td>
<td><p>544 (512 en Windows Server 2012)</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantáneas por LU</p></td>
<td><p>512</p></td>
<td><p>Yes</p></td>
<td><p>Hay un límite de 512 instantáneas por volumen de aplicación iSCSI independiente.</p></td>
</tr>
<tr class="even">
<td><p>Discos virtuales montados localmente o instantáneas por dispositivo de almacenamiento</p></td>
<td><p>32</p></td>
<td><p>Sí</p></td>
<td><p>Los discos virtuales montados localmente no&#39;t ofrecen ninguna funcionalidad específica de iSCSI y están en desuso. para obtener más información, consulte <a href="/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303411(v=ws.11)">características eliminadas o en desuso en Windows Server 2012 R2</a>.</p></td>
</tr>
</tbody>
</table>

## <a name="fault-tolerance-limits"></a>Límites de tolerancia a errores

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Elemento</p></th>
<th><p>Límite de compatibilidad</p></th>
<th><p>Aplica?</p></th>
<th><p>Comentario</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Nodos de clúster de conmutación por error</p></td>
<td><p>8 (5 en Windows Server 2012)</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Varios nodos de clúster activos</p></td>
<td><p>Compatible</p></td>
<td>
<p>N/D</p></td>
<td><p>Cada nodo activo del clúster de conmutación por error posee una instancia en clúster de servidor de destino iSCSI diferente con otros nodos que actúan como posibles nodos propietarios.</p></td>
</tr>
<tr class="odd">
<td><p>Nivel de recuperación de errores (ERL)</p></td>
<td><p>0</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Conexiones por sesión</p></td>
<td><p>1</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Sesiones que pueden conectarse simultáneamente a una instancia de destino iSCSI</p></td>
<td><p>544 (512 en Windows Server 2012)</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Entrada/salida de múltiples rutas (MPIO)</p></td>
<td><p>Compatible</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Rutas de acceso de MPIO</p></td>
<td><p>4</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Conversión de un servidor de destino iSCSI independiente en un servidor de destino iSCSI en clúster o viceversa</p></td>
<td><p>No compatible</p></td>
<td><p>No</p></td>
<td><p>La instancia de destino iSCSI y los datos de configuración de disco virtual, incluidos los metadatos de instantánea, se pierden durante la conversión.</p></td>
</tr>
</tbody>
</table>

## <a name="network-limits"></a>Límites de red

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Elemento</p></th>
<th><p>Límite de compatibilidad</p></th>
<th><p>Aplica?</p></th>
<th><p>Comentario</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Número máximo de adaptadores de red activos</p></td>
<td><p>8</p></td>
<td><p>No</p></td>
<td><p>Se aplica a los adaptadores de red que están dedicados al tráfico iSCSI, en lugar de al número total de adaptadores de red del dispositivo.</p></td>
</tr>
<tr class="even">
<td><p>Portal (direcciones IP) admitido</p></td>
<td><p>64</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Velocidad de puerto de red</p></td>
<td><p>1 Gbps, 10 Gbps, 40Gbps, 56 Gbps (solo Windows Server 2012 R2 y versiones más recientes)</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>IPv4</p></td>
<td><p>Compatible</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPv6</p></td>
<td><p>Compatible</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Descarga TCP</p></td>
<td><p>Compatible</p></td>
<td><p>N/D</p></td>
<td><p>Aproveche el envío de gran tamaño (segmentación), suma de comprobación, moderación de interrupciones y descarga RSS</p></td>
</tr>
<tr class="odd">
<td><p>descarga de iSCSI</p></td>
<td><p>No compatible</p></td>
<td><br/><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Tramas gigantes</p></td>
<td><p>Compatible</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPSec</p></td>
<td><p>Compatible</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Descarga de CRC</p></td>
<td><p>Compatible</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
</tbody>
</table>

## <a name="iscsi-virtual-disk-limits"></a>límites de disco virtual iSCSI

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Elemento</p></th>
<th><p>Límite de compatibilidad</p></th>
<th><p>Aplica?</p></th>
<th><p>Comentario</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Desde un iniciador iSCSI que convierte el disco virtual de un disco básico a un disco dinámico </p></td>
<td><p>Sí</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Formato del disco duro virtual</p></td>
<td><p>. vhdx (solo Windows Server 2012 R2 y versiones más recientes)</p>
<p>.vhd</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Tamaño de formato mínimo de VHD</p></td>
<td><p>. vhdx: 3 MB</p>
<p>. vhd: 8 MB</p></td>
<td><p>Yes</p></td>
<td><p>Se aplica a todos los tipos de VHD compatibles: primario, diferenciado y fijo.</p></td>
</tr>
<tr class="even">
<td><p>Tamaño máximo de VHD primario</p></td>
<td><p>. vhdx: 64 TB</p>
<p>. vhd: 2 TB</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Tamaño máximo de VHD fijo</p></td>
<td><p>. vhdx: 64 TB</p>
<p>. vhd: 16 TB</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Tamaño máximo de VHD de diferenciación</p></td>
<td><p>. vhdx: 64 TB</p>
<p>. vhd: 2 TB</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Formato fijo de VHD</p></td>
<td><p>Compatible</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Formato de diferenciación de VHD</p></td>
<td><p>Compatible</p></td>
<td><p>No</p></td>
<td><p>No se pueden tomar instantáneas de discos virtuales iSCSI basados en VHD de diferenciación.</p></td>
</tr>
<tr class="odd">
<td><p>Número de discos duros virtuales de diferenciación por VHD primario</p></td>
<td><p>256</p></td>
<td><p>No (sí en Windows Server 2012)</p></td>
<td><p>Dos niveles de profundidad (archivos descendientes. vhdx) es el máximo para los archivos. vhdx; un nivel de profundidad (archivos. vhd secundarios) es el máximo para los archivos. VHD.</p></td>
</tr>
<tr class="even">
<td><p>Formato dinámico de VHD</p></td>
<td><p>. vhdx: sí</p>
<p>. vhd: sí (no en Windows Server 2012)</p></td>
<td><p>Yes</p></td>
<td><p>No se puede desasignar el&#39;t.</p></td>
</tr>
<tr class="odd">
<td><p>exFAT/FAT32/FAT (volumen de hospedaje del VHD)</p></td>
<td><p>No compatible</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>CSV V2</p></td>
<td><p>No compatible</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>ReFS</p></td>
<td><p>Compatible</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>NTFS</p></td>
<td><p>Compatible</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CFS que no es de Microsoft</p></td>
<td><p>No compatible</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Aprovisionamiento fino</p></td>
<td><p>No</p></td>
<td><p>N/D</p></td>
<td><p>Se admiten los discos duros virtuales dinámicos, pero no se puede desasignar el&#39;t.</p></td>
</tr>
<tr class="odd">
<td><p>Reducción de unidad lógica</p></td>
<td><p>Sí (solo Windows Server 2012 R2 y versiones más recientes)</p></td>
<td><p>N/D</p></td>
<td><p>Use <a href="/powershell/module/iscsitarget/resize-iscsivirtualdisk">Resize-iSCSIVirtualDisk</a> para reducir un LUN.</p></td>
</tr>
<tr class="even">
<td><p>Clonación de unidad lógica</p></td>
<td><p>No compatible</p></td>
<td><p>N/D</p></td>
<td><p>Puede clonar rápidamente datos de disco mediante discos duros virtuales de diferenciación.</p></td>
</tr>
</tbody>
</table>

## <a name="snapshot-limits"></a>Límites de instantáneas

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Elemento</p></th>
<th><p>Límite de compatibilidad</p></th>
<th><p>Comentario</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Creación de instantáneas</p></td>
<td><p>Compatible</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Restauración de instantáneas</p></td>
<td><p>Compatible</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantáneas de escritura</p></td>
<td><p>No compatible</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Instantánea: convertir en completo</p></td>
<td><p>No compatible</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantánea: reversión en línea</p></td>
<td><p>No compatible</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Instantánea: convertir en grabable</p></td>
<td><p>No compatible</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantánea: redirección</p></td>
<td><p>No compatible</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Anclaje de instantáneas</p></td>
<td><p>No compatible</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Montaje local</p></td>
<td><p>Compatible</p></td>
<td><p>Los discos virtuales iSCSI montados localmente están en desuso. para obtener más información, consulte <a href="/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303411(v=ws.11)">características eliminadas o en desuso en Windows Server 2012 R2</a>. Las instantáneas de disco dinámico no se pueden montar localmente.</p></td>
</tr>
</tbody>
</table>

## <a name="iscsi-target-server-manageability-and-backup"></a>facilidad de administración y copia de seguridad del servidor de destino iSCSI

Si desea crear instantáneas de volumen (instantáneas de archivo abierto VSS) de datos en discos virtuales iSCSI desde un servidor de aplicaciones, o bien, desea administrar los discos virtuales iSCSI con una aplicación anterior (como el comando de Diskraid) que requiere un proveedor de hardware de servicio de disco virtual (VDS), instalar el proveedor de almacenamiento del destino iSCSI en el servidor desde el que desea realizar una instantánea o usar una aplicación de administración de VDS.

El proveedor de almacenamiento del destino iSCSI es un servicio de rol en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012; también puede descargar e instalar [proveedores de almacenamiento de destino iSCSI (VDS/VSS) para servidores de aplicaciones de nivel inferior](https://www.microsoft.com/download/details.aspx?id=34759) en los siguientes sistemas operativos, siempre que el servidor de destino iSCSI se ejecute en Windows Server 2012:

  - Windows Storage Server 2008 R2

  - Windows Server 2008 R2

  - Windows HPC Server 2008 R2

  - Windows HPC Server 2008

Tenga en cuenta que si el servidor de destino iSCSI está hospedado en un servidor que ejecuta Windows Server 2012 R2 o una versión más reciente y desea usar VSS o VDS desde un servidor remoto, el servidor remoto también debe ejecutar la misma versión de Windows Server y tener instalado el servicio de rol proveedor de almacenamiento del destino iSCSI. Tenga en cuenta también que en todas las versiones de Windows debe instalar solo una versión del servicio de rol proveedor de almacenamiento del destino iSCSI.

Para obtener más información sobre el proveedor de almacenamiento del destino iSCSI, consulte [proveedor de almacenamiento de destino iSCSI (VDS/VSS)](/powershell/module/iscsi/).

## <a name="tested-compatibility-with-iscsi-initiators"></a>Compatibilidad probada con iniciadores iSCSI

Hemos probado el software del servidor de destino iSCSI con los siguientes iniciadores iSCSI:

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Iniciador</p></td>
<td><p>Windows Server 2012 R2</p></td>
<td><p>Windows Server 2012</p></td>
<td><p>Comentarios</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
<td><p>Validar</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003</p></td>
<td><p>Validar</p></td>
<td><p>Validar</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare vSphere 5</p></td>
<td></td>
<td><p>Validar</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>VMWare ESXi 5,0</p></td>
<td><p>Validar</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare ESX 4,1</p></td>
<td><p>Validar</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CentOS 6.x</p></td>
<td><p>Validar</p></td>
<td></td>
<td><p>Debe cerrar sesión y volver a iniciar sesión para detectar un disco virtual cuyo tamaño se ha cambiado.</p></td>
</tr>
<tr class="even">
<td><p>Red Hat Enterprise Linux 6</p></td>
<td><p>Validar</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>RedHat Enterprise Linux 5 y 5</p></td>
<td><p>Validar</p></td>
<td><p>Validar</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>SUSE Linux Enterprise Server 10</p></td>
<td></td>
<td><p>Validar</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Oracle Solaris 11. x</p></td>
<td><p>Validar</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

También hemos probado los siguientes iniciadores iSCSI que realizan un arranque sin disco desde discos virtuales hospedados por el servidor de destino iSCSI:

  - Windows Server 2012 R2

  - Windows Server 2012

  - NIC PCIe con iPXE

  - Disco CD o USB con iPXE

## <a name="additional-references"></a>Referencias adicionales

En la siguiente lista se proporcionan recursos adicionales acerca del servidor de destino iSCSI y las tecnologías relacionadas.

- [Información general de almacenamiento de bloque de destino iSCSI](iscsi-target-server.md)

- [iSCSI Target Boot Overview](iscsi-boot-overview.md)

- [Almacenamiento en Windows Server](../storage.yml)
