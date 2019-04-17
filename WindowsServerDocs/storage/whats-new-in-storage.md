---
ms.assetid: 0f2a7f7b-aca8-4e5d-ad67-4258e88bc52f
title: Novedades en el almacenamiento en Windows Server
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dongill
ms.technology: storage
ms.topic: article
author: kumudd
ms.date: 09/15/2016
ms.openlocfilehash: 9aab6246f7ddc86629834bf20a7d21cc4ce2ec8f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="whats-new-in-storage-in-windows-server-2016"></a>Novedades en el almacenamiento en Windows Server 2016

>Se aplica a: Windows Server2016

En este tema se explica la funcionalidad nueva y modificada en Almacenamiento en Windows Server 2016.

## <a name="s2d"></a>Espacios de almacenamiento directo  
Espacios de almacenamiento directo permite la creación de almacenamiento altamente disponible y escalable con servidores de almacenamiento local. Simplifica la implementación y administración de los sistemas de almacenamiento definidos por software y desbloquea el uso de las nuevas clases de dispositivos de disco, como SSD de SATA y dispositivos de disco NVMe, que no estaban disponibles con Espacios de almacenamiento de clúster con discos compartidos.  

**¿Qué valor aporta este cambio?**  
Espacios de almacenamiento directo permite a los proveedores de servicios y las empresas usar los servidores estándar del sector con almacenamiento local para generar almacenamiento definido por software altamente disponible y escalable. El uso de servidores con almacenamiento local disminuye la complejidad, aumenta la escalabilidad y permite el uso de dispositivos de almacenamiento que antes no se podían emplear, como los discos de estado sólido SATA para reducir el costo de almacenamiento flash o los discos de estado sólido NVMe para mejorar el rendimiento.  

Con Espacios de almacenamiento directo, ya no es necesario disponer un tejido de SAS compartido, simplificando la implementación y la configuración. En su lugar, utiliza la red como un tejido de almacenamiento, aprovechando SMB3 y SMB directo (RDMA) para el almacenamiento eficiente de CPU de alta velocidad y latencia baja. Para escalar horizontalmente, basta con agregar más servidores para aumentar la capacidad de almacenamiento y rendimiento de E/S.  
Para más información, consulte [Storage Spaces Direct in Windows Server 2016](storage-spaces/storage-spaces-direct-overview.md) (Espacios de almacenamiento directo en Windows Server 2016).  

**¿Qué funciona de manera diferente?**  
Esta funcionalidad es nueva en Windows Server 2016.  

## <a name="storage-replica"></a>Réplica de almacenamiento  
Réplica de almacenamiento (SR) permite la replicación sincrónica independiente del almacenamiento y a nivel de bloque entre servidores o clústeres para la recuperación ante desastres, así como la extensión de un clúster de conmutación por error entre sitios. La replicación sincrónica permite el reflejo de datos en sitios físicos con volúmenes coherentes frente a bloqueos para asegurar que no se produce absolutamente ninguna pérdida de datos en el nivel de sistema de archivos. La replicación asincrónica permite la extensión de sitios más allá del área metropolitana con la posibilidad de pérdida de datos.  

**¿Qué valor aporta este cambio?**  
Replicación de almacenamiento le permite hacer lo siguiente:  

* Especificar una sola solución de recuperación ante desastres de proveedores para las interrupciones planificadas y no planificadas de cargas de trabajo esenciales.
* Utilizar el transporte SMB3 con rendimiento, escalabilidad y confiabilidad probada.
* Extender los clústeres de conmutación por error de Windows a distancias metropolitanas.
* Usar software integral de Microsoft para el almacenamiento y la agrupación en clústeres, como Hyper-V, Réplica de almacenamiento, Espacios de almacenamiento, Clúster, Servidor de archivos de escalabilidad horizontal, SMB3, Desduplicación y ReFS/NTFS.
* Ayudar a reducir el costo y la complejidad como sigue: 
    * Es independiente del hardware, y no requiere una configuración del almacenamiento específica como SAN o DAS.
    * Permite las tecnologías de red y el almacenamiento de productos.
    * Presenta facilidad de administración gráfica para nodos y clústeres individuales mediante el Administrador de clústeres de conmutación por error.
    * Incluye opciones de scripting completas y a gran escala a través de Windows PowerShell. 
* Ayudar a reducir el tiempo de inactividad y aumentar la confiabilidad y productividad intrínseca de Windows.  
* Proporcionar funcionalidades de diagnóstico, métricas de rendimiento y compatibilidad.  

Para más información, consulte [Storage Replica in Windows Server 2016](storage-replica/storage-replica-overview.md) (Réplica de almacenamiento en Windows Server 2016).  

**¿Qué funciona de manera diferente?**  
Esta funcionalidad es nueva en Windows Server 2016.  

## <a name="storage-qos"></a>Calidad de servicio de almacenamiento  
Ahora puede usar la calidad de servicio de almacenamiento para supervisar de manera centralizada el rendimiento del almacenamiento de extremo a extremo y crear directivas de administración mediante Hyper-V y clústeres de CSV en Windows Server 2016.  

**¿Qué valor aporta este cambio?**  
Ahora puede crear directivas QoS de almacenamiento en un clúster de CSV y asignarlas a uno o varios discos virtuales en máquinas virtuales de Hyper-V. El rendimiento del almacenamiento se reajusta automáticamente para cumplir las directivas a medida que varían las cargas de trabajo y las cargas de almacenamiento.  

* Cada directiva puede especificar una reserva (mínima) y/o un límite (máximo) que se aplicará a una colección de flujos de datos, como un disco duro virtual, una sola máquina virtual o un grupo de máquinas virtuales, un servicio o un inquilino.  
* Con Windows PowerShell o WMI, puede realizar las siguientes tareas:  
    * Crear directivas en un clúster de CSV.
    * Enumerar directivas disponibles en un clúster de CSV.
    * Asignar una directiva a un disco duro virtual en una máquina virtual de Hyper-V. 
    * Supervisar el rendimiento de cada flujo y estado dentro de la directiva.  
* Si varios discos duros virtuales comparten la misma directiva, el rendimiento se distribuye equitativamente para satisfacer la demanda en la configuración mínima y máxima de la directiva. Por lo tanto, una directiva puede utilizarse para administrar un disco duro virtual, una máquina virtual, varias máquinas virtuales que componen un servicio o todas las máquinas virtuales que pertenecen a un inquilino.  

**¿Qué funciona de manera diferente?**  
Esta funcionalidad es nueva en Windows Server 2016. La administración de reservas mínimas, la supervisión de flujos de todos los discos virtuales en el clúster a través de un único comando y la administración centralizada basada en políticas no eran posibles en versiones anteriores de Windows Server.  

Para más información, consulte [Calidad de servicio de almacenamiento](storage-qos/storage-qos-overview.md).

## <a name="dedup"></a>Desduplicación de datos  
| Funcionalidad | Nueva o actualizada | Descripción |
|---------------|----------------|-------------|
| [Compatibilidad con volúmenes grandes](data-deduplication/whats-new.md#large-volume-support) | Actualizado | Antes de Windows Server 2016, los volúmenes debían tener un tamaño específico para la renovación esperada, y aquellos tamaños de volúmenes por encima de los 10 TB no eran buenos candidatos para la desduplicación. En Windows Server 2016, Desduplicación de datos admite tamaños de volúmenes de **hasta 64 TB**. |
| [Compatibilidad con archivos de gran tamaño](data-deduplication/whats-new.md#large-file-support) | Actualizado | Antes de Windows Server 2016, los archivos cuyo tamaño se aproximase a 1 TB no eran buenos candidatos para la desduplicación. En Windows Server 2016, los archivos de **hasta 1 TB** son totalmente compatibles. |
| [Compatibilidad con Nano Server](data-deduplication/whats-new.md#nano-server-support) | New | Desduplicación de datos está disponible y es totalmente compatible con la nueva opción de implementación de Nano Server para Windows Server 2016. |
| [Compatibilidad con copia de seguridad simplificada](data-deduplication/whats-new.md#simple-backup-support) | New | En Windows Server 2012 R2, las aplicaciones virtualizadas de copia de seguridad, como [Data Protection Manager](https://technet.microsoft.com/en-us/library/hh758173.aspx) de Microsoft, se admitían a través de una serie de pasos de configuración manual. En Windows Server 2016, se ha agregado un nuevo tipo de uso de "Copia de seguridad" predeterminado para una implementación fluida de Desduplicación de datos para aplicaciones virtualizadas de copia de seguridad. |
| [Compatibilidad con las actualizaciones graduales de sistema operativo de clúster](data-deduplication/whats-new.md#cluster-upgrade-support) | New | Desduplicación de datos es totalmente compatible con la nueva característica [Actualización gradual de sistema operativo de clúster](..//failover-clustering/cluster-operating-system-rolling-upgrade.md) de Windows Server 2016. |

## <a name="smb-hardening-improvements"></a>Mejoras de protección de SMB para conexiones de SYSVOL y NETLOGON  
En Windows 10 y Windows Server 2016, las conexiones de cliente a los recursos compartidos de archivos SYSVOL y NETLOGON predeterminados de Active Directory Domain Services en controladores de dominio ahora requieren firma SMB y autenticación mutua (como Kerberos).   

**¿Qué valor aporta este cambio?**  
Este cambio reduce la probabilidad de ataques de tipo "Man in the middle".   

**¿Qué funciona de manera diferente?**  
Si la firma SMB y la autenticación mutua no están disponibles, un equipo con Windows 10 o Windows Server 2016 no procesará las directivas de grupo y los scripts basados en dominio.  

> [!NOTE]  
> Los valores del Registro para esta configuración no están presentes de forma predeterminada, pero se siguen aplicando las reglas de protección hasta que se reemplacen por la Directiva de grupo u otros valores del registro.  

Para más información sobre estas mejoras de seguridad (también conocidas como protección UNC), vea el artículo de Microsoft Knowledge Base [3000483](http://support.microsoft.com/kb/3000483) y [MS15-011 & MS15-014: Hardening Group Policy](http://blogs.technet.microsoft.com/srd/2015/02/10/ms15-011-ms15-014-hardening-group-policy) (Protección de directiva de grupo).  

## <a name="work-folders"></a>Carpetas de trabajo
Notificación de cambio mejorada cuando el servidor de Carpetas de trabajo ejecuta Windows Server 2016 y el cliente de Carpetas de trabajo es Windows 10.

**¿Qué valor aporta este cambio?**<br>
En Windows Server 2012 R2, cuando los cambios efectuados en el archivo se sincronizan con el servidor de Carpetas de trabajo, no se notifica el cambio a los clientes y estos esperan hasta 10 minutos para obtener la actualización.  Cuando se utiliza Windows Server 2016, el servidor de Carpetas de trabajo notifica inmediatamente a los clientes de Windows 10 y los cambios del archivo se sincronizan inmediatamente.

**¿Qué funciona de manera diferente?**<br>
Esta funcionalidad es nueva en Windows Server 2016. Requiere un servidor de Carpetas de trabajo de Windows Server 2016 y el cliente debe ejecutar Windows 10.

Si usas un cliente anterior, o si el servidor de Carpetas de trabajo es Windows Server 2012 R2, el cliente seguirá buscando cambios cada 10minutos.

## <a name="refs"></a>ReFS 
La siguiente iteración del ReFS proporciona compatibilidad para implementaciones de almacenamiento a gran escala con diferentes cargas de trabajo, así como confiabilidad, resistencia y escalabilidad para los datos.     

**¿Qué valor aporta este cambio?**<br>
ReFS presenta las siguientes mejoras:

* ReFS implementa la nueva característica de niveles de almacenamiento, lo que ayuda a proporcionar un rendimiento más rápido y mayor capacidad de almacenamiento. Esta nueva característica permite:
    * Varios tipos de resistencia en el mismo disco virtual (mediante la creación de reflejos en el nivel de rendimiento y paridad en el nivel de capacidad, por ejemplo).
    * Mayor capacidad de respuesta ante el desfase de conjuntos de trabajo. 
    * Compatibilidad con medios SMR (grabación magnética de superposición). 
* La incorporación de la clonación de bloques mejora sustancialmente el rendimiento de las operaciones de máquina virtual, como las operaciones de fusión de punto de control .vhdx.
* La nueva herramienta de examen de ReFS permite la recuperación de almacenamiento perdido y ayuda a rescatar datos en caso de daños críticos. 

**¿Qué funciona de manera diferente?**<br>
Estas funcionalidades son nuevas en Windows Server 2016. 

## <a name="see-also"></a>Consulta también  
* [Novedades en Windows Server 2016](../get-started/what-s-new-in-windows-server-2016.md)  
