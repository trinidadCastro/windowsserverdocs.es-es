---
title: Límites de escalabilidad del servidor de destino iSCSI
TOCTitle: iSCSI Target Server Scalability Limits
ms.prod: windows-server-threshold
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: 9514392da133911c900f68fc8f1be260b6c91138
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873036"
---
# <a name="iscsi-target-server-scalability-limits"></a>Límites de escalabilidad del servidor de destino iSCSI

Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema proporciona a Microsoft iSCSI compatible y probado los límites del servidor de destino en Windows Server. Las siguientes tablas muestran los límites probados de soporte técnico y, si procede, si se aplican los límites.

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
<th><p>Límite de soporte técnico</p></th>
<th><p>¿Aplican?</p></th>
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
<td><p>unidades lógicas (lu) de iSCSI o discos virtuales por servidor de destino iSCSI</p></td>
<td><p>512</p></td>
<td><p>No</p></td>
<td><p>Configuraciones de pruebas incluidas: 8 lu por instancia de destino con un promedio destinos más de 64 y 256 instancias de destino con uno LU por cada destino.</p></td>
</tr>
<tr class="odd">
<td><p>instancia de destino iSCSI lu o discos virtuales por iSCSI</p></td>
<td><p>256 (128 en Windows Server 2012)</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Sesiones que pueden conectarse simultáneamente a una instancia de destino iSCSI</p></td>
<td><p>544 (512 en Windows Server 2012)</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantáneas por LU</p></td>
<td><p>512</p></td>
<td><p>Sí</p></td>
<td><p>Hay un límite de 512 instantáneas por volumen iSCSI independientes de la aplicación.</p></td>
</tr>
<tr class="even">
<td><p>Discos virtuales montados localmente o instantáneas por dispositivo de almacenamiento</p></td>
<td><p>32</p></td>
<td><p>Sí</p></td>
<td><p>Discos virtuales montados localmente no ofrecen ninguna funcionalidad específica de iSCSI y están en desuso: para obtener más información, consulte <a href="https://technet.microsoft.com/library/dn303411.aspx">características o funcionalidades desusadas en Windows Server 2012 R2</a>.</p></td>
</tr>
</tbody>
</table>

## <a name="fault-tolerance-limits"></a>Limita la tolerancia a errores

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
<th><p>Límite de soporte técnico</p></th>
<th><p>¿Aplican?</p></th>
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
<td><p>Varios nodos de clúster activo</p></td>
<td><p>Se admite</p></td>
<td> 
<p>N/D</p></td>
<td><p>Cada nodo activo del clúster de conmutación por error es el propietario de una instancia en clúster del servidor de destino iSCSI diferentes con otros nodos que actúa como posibles nodos propietarios.</p></td>
</tr>
<tr class="odd">
<td><p>Nivel de error de recuperación (ERL)</p></td>
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
<td><p>Se admite</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Rutas de acceso MPIO</p></td>
<td><p>4</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Conversión de un servidor de destino de iSCSI independiente a un servidor de destino o viceversa de iSCSI en clúster</p></td>
<td><p>No se admite</p></td>
<td><p>No</p></td>
<td><p>La instancia de destino iSCSI y el disco virtual de datos de configuración, incluidos los metadatos de instantánea, se pierde durante la conversión.</p></td>
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
<th><p>Límite de soporte técnico</p></th>
<th><p>¿Aplican?</p></th>
<th><p>Comentario</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Número máximo de adaptadores de red activos</p></td>
<td><p>8</p></td>
<td><p>No</p></td>
<td><p>Se aplica a los adaptadores de red que se dedican al tráfico de iSCSI, en lugar de con el número total de adaptadores de red en el dispositivo.</p></td>
</tr>
<tr class="even">
<td><p>Portal (direcciones IP) compatible</p></td>
<td><p>64</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Velocidad de puerto de red</p></td>
<td><p>40Gbps de 1 Gbps, 10 Gbps, 56 Gbps (Windows Server 2012 R2 y versiones posteriores únicamente)</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>IPv4</p></td>
<td><p>Se admite</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPv6</p></td>
<td><p>Se admite</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Descarga de TCP</p></td>
<td><p>Se admite</p></td>
<td><p>N/D</p></td>
<td><p>Sacar provecho de envío grande (segmentación), suma de comprobación, moderación de interrupciones y descarga RSS</p></td>
</tr>
<tr class="odd">
<td><p>descarga de iSCSI</p></td>
<td><p>No se admite</p></td>
<td>              
<p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Tramas gigantes</p></td>
<td><p>Se admite</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPSec</p></td>
<td><p>Se admite</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Descarga CRC</p></td>
<td><p>Se admite</p></td>
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
<th><p>Límite de soporte técnico</p></th>
<th><p>¿Aplican?</p></th>
<th><p>Comentario</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Desde un iniciador iSCSI de convertir el disco virtual de un disco básico a un disco dinámico </p></td>
<td><p>Sí</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Formato del disco duro virtual</p></td>
<td><p>.vhdx (Windows Server 2012 R2 y versiones posteriores únicamente)</p>
<p>.vhd</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Tamaño mínimo del formato de disco duro virtual</p></td>
<td><p>.vhdx: 3 MB</p>
<p>.vhd: 8 MB</p></td>
<td><p>Sí</p></td>
<td><p>Se aplica a todos los tipos de disco duro virtual compatibles: primario, de diferenciación y se ha corregido.</p></td>
</tr>
<tr class="even">
<td><p>Tamaño máximo de disco duro virtual primario</p></td>
<td><p>.vhdx: 64 TB</p>
<p>.vhd: 2 TB</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Tamaño máximo de disco duro virtual fijo</p></td>
<td><p>.vhdx: 64 TB</p>
<p>.vhd: 16 TB</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Tamaño máximo de disco duro virtual de diferenciación</p></td>
<td><p>.vhdx: 64 TB</p>
<p>.vhd: 2 TB</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>VHD con formato fijo</p></td>
<td><p>Se admite</p></td>
<td><p>No</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Formato de diferenciación de VHD</p></td>
<td><p>Se admite</p></td>
<td><p>No</p></td>
<td><p>No se pueden tomar instantáneas de discos virtuales iSCSI basadas en disco duro virtual de diferenciación.</p></td>
</tr>
<tr class="odd">
<td><p>Número de discos duros virtuales de diferenciación por VHD primario</p></td>
<td><p>256</p></td>
<td><p>No (Sí, en Windows Server 2012)</p></td>
<td><p>Dos niveles de profundidad (archivos .vhdx de sus nietos) es el número máximo de los archivos .vhdx; un nivel de profundidad (archivos .vhd de secundarios) es el máximo para archivos .vhd.</p></td>
</tr>
<tr class="even">
<td><p>Formato de disco duro virtual dinámico</p></td>
<td><p>.vhdx: Sí</p>
<p>.vhd: Sí (No en Windows Server 2012)</p></td>
<td><p>Sí</p></td>
<td><p>Anular la asignación no se admite.</p></td>
</tr>
<tr class="odd">
<td><p>exFAT/FAT32 o FAT (hospeda el volumen del VHD)</p></td>
<td><p>No se admite</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>CSV v2</p></td>
<td><p>No se admite</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>ReFS</p></td>
<td><p>Se admite</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>NTFS</p></td>
<td><p>Se admite</p></td>
<td><p>N/D</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CFS que no son de Microsoft</p></td>
<td><p>No se admite</p></td>
<td><p>Sí</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Aprovisionamiento fino</p></td>
<td><p>No</p></td>
<td><p>N/D</p></td>
<td><p>Se admiten discos duros virtuales dinámicos, pero no se admite la asignación.</p></td>
</tr>
<tr class="odd">
<td><p>Reducción de unidad lógica</p></td>
<td><p>Sí (Windows Server 2012 R2 y versiones posteriores únicamente)</p></td>
<td><p>N/D</p></td>
<td><p>Use <a href="https://docs.microsoft.com/powershell/module/iscsitarget/resize-iscsivirtualdisk">Resize-iscsivirtualdisk, han</a> para reducir un LUN.</p></td>
</tr>
<tr class="even">
<td><p>Clonación de unidad lógica</p></td>
<td><p>No se admite</p></td>
<td><p>N/D</p></td>
<td><p>Rápidamente puede clonar los datos del disco mediante el uso de discos duros virtuales de diferenciación.</p></td>
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
<th><p>Límite de soporte técnico</p></th>
<th><p>Comentario</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Crear instantánea</p></td>
<td><p>Se admite</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Restauración de instantáneas</p></td>
<td><p>Se admite</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantáneas de escritura</p></td>
<td><p>No se admite</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Instantánea: convertir en completa</p></td>
<td><p>No se admite</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantánea: restauración en línea</p></td>
<td><p>No se admite</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Instantánea: convertir en grabable</p></td>
<td><p>No se admite</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Instantánea - redirección</p></td>
<td><p>No se admite</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Instantánea - asignación</p></td>
<td><p>No se admite</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Montaje local</p></td>
<td><p>Se admite</p></td>
<td><p>Discos virtuales iSCSI montado localmente están en desuso: para obtener más información, consulte <a href="https://technet.microsoft.com/library/dn303411.aspx">características o funcionalidades desusadas en Windows Server 2012 R2</a>. Las instantáneas de disco dinámico no pueden montarse localmente.</p></td>
</tr>
</tbody>
</table>

## <a name="iscsi-target-server-manageability-and-backup"></a>copia de seguridad y capacidad de administración de servidor de destino iSCSI

Si desea crear instantáneas (instantáneas VSS open file) de datos de volumen en los discos virtuales iSCSI desde un servidor de aplicaciones, o que desea administrar los discos virtuales iSCSI con una aplicación anterior (por ejemplo, el comando Diskraid) que requiere un hardware de servicio de disco Virtual (VDS) proveedor, instale el proveedor de almacenamiento de destino iSCSI en el servidor desde el que desea tomar una instantánea o usar una aplicación de administración de VDS.

El proveedor de almacenamiento de destino iSCSI es un servicio de rol de Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012; También puede descargar e instalar [proveedores de almacenamiento de destino (VDS y VSS) de iSCSI para servidores de aplicaciones de nivel inferior](http://www.microsoft.com/download/details.aspx?id=34759) en los siguientes sistemas operativos, siempre que se está ejecutando el servidor de destino iSCSI en Windows Server 2012:

  - Windows Storage Server 2008 R2

  - Windows Server 2008 R2

  - Windows HPC Server 2008 R2

  - Windows HPC Server 2008

Tenga en cuenta que si el servidor de destino iSCSI está hospedado por un servidor que ejecuta Windows Server 2012 R2 o versiones posteriores y quiere usar VSS o VDS desde un servidor remoto, el servidor remoto tiene también de ejecutar la misma versión de Windows Server y tener el rol de proveedor de almacenamiento de destino iSCSI Service e instalado. Tenga en cuenta también que en todas las versiones de Windows debe instalar solo una versión de servicio de rol de proveedor de almacenamiento de destino iSCSI.

Para obtener más información sobre el proveedor de almacenamiento de destino iSCSI, consulte [proveedor de almacenamiento de destino (VDS y VSS) iSCSI](http://blogs.technet.com/b/filecab/archive/2012/10/08/iscsi-target-storage-vds-vss-provider.aspx).

## <a name="tested-compatibility-with-iscsi-initiators"></a>Compatibilidad probado con los iniciadores iSCSI

Hemos probado el software del servidor de destino con los siguientes iniciadores iSCSI iSCSI:

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
<td><p>Windows Server 2012 R2</p></td>
<td><p>Windows Server 2012</p></td>
<td><p>Observaciones</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
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
<td><p>VMWare ESXi 5.0</p></td>
<td><p>Validar</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare ESX 4.1</p></td>
<td><p>Validar</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CentOS 6.x</p></td>
<td><p>Validar</p></td>
<td></td>
<td><p>Debe cerrar una sesión y volver a iniciar sesión para detectar un disco virtual cuyo tamaño ha cambiado.</p></td>
</tr>
<tr class="even">
<td><p>Red Hat Enterprise Linux 6</p></td>
<td><p>Validar</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>RedHat Enterprise Linux 5 and 5</p></td>
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
<td><p>Oracle Solaris 11.x</p></td>
<td><p>Validar</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

También hemos probado los siguientes iniciadores de iSCSI que se realiza un arranque sin disco desde los discos virtuales hospedados por el servidor de destino iSCSI:

  - Windows Server 2012 R2

  - Windows Server 2012

  - PCIe NIC con iPXE

  - Disco CD o USB con iPXE

## <a name="see-also"></a>Vea también

En la siguiente lista se proporcionan recursos adicionales acerca del servidor de destino iSCSI y las tecnologías relacionadas.

  - [Introducción al almacenamiento de bloque de destino iSCSI](iscsi-target-server.md)

  - [información general de arranque de destino iSCSI](iscsi-boot-overview.md)

  - [Almacenamiento en Windows Server](..\storage.md)

