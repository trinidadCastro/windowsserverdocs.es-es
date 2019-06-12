---
ms.assetid: 0f2a7f7b-aca8-4e5d-ad67-4258e88bc52f
title: Novedades de Espacios de almacenamiento en Windows Server
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dongill
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 05/29/2019
ms.openlocfilehash: f72156b050aa943cfafaf1fa2539911d6d1e089e
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501480"
---
# <a name="whats-new-in-storage-in-windows-server"></a>Novedades de almacenamiento en Windows Server

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

En este tema se explica la funcionalidad nueva y modificada en el almacenamiento en Windows Server 2019, Windows Server 2016, y libera el canal semianual de Windows Server.

## <a name="whats-new-in-storage-in-windows-server-version-1903"></a>Novedades de almacenamiento en Windows Server, versión 1903

Esta versión de Windows Server agrega los siguientes cambios y tecnologías.

### <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>Servicio de migración de almacenamiento ahora migra las cuentas locales, clústeres y servidores Linux

Servicio de migración de almacenamiento facilita la migración de servidores a una versión más reciente de Windows Server. Proporciona una herramienta gráfica que hará un inventario de datos en los servidores y, a continuación, transfiere los datos y la configuración a los servidores más recientes, todo ello sin tener que cambiar nada de usuarios o aplicaciones.

Al usar esta versión de Windows Server para organizar las migraciones, hemos agregado las siguientes capacidades:

- Migrar usuarios y grupos locales en el nuevo servidor
- Migrar el almacenamiento de clústeres de conmutación por error
- Migrar el almacenamiento desde un servidor de Linux que usa Samba
- Sincronizar más fácilmente recursos compartidos migrados en Azure mediante el uso de Azure File Sync
- Migrar a nuevas redes, como Azure

Para obtener más información acerca del servicio de migración de almacenamiento, consulte [información general del servicio de migración de almacenamiento](storage-migration-service/overview.md).

### <a name="system-insights-disk-anomaly-detection"></a>Detección de anomalías de disco de sistema Insights

[Información del sistema](../manage/system-insights/overview.md) es una característica de análisis predictivo que analiza los datos del sistema de Windows Server localmente y proporciona información detallada sobre el funcionamiento del servidor. Incluye una serie de funcionalidades integradas, pero hemos agregado la capacidad para instalar funciones adicionales a través de Windows Admin Center, a partir de la detección de anomalías de disco.

Detección de anomalías de disco es una nueva funcionalidad que resalta cuando se comportan discos *diferente* de lo habitual. Aunque diferentes no es necesariamente algo malo, ver estos instantes anómalas puede resultar útil al solucionar problemas en sus sistemas.

Esta funcionalidad también está disponible para los servidores que ejecutan Windows Server 2019.

### <a name="windows-admin-center-enhancements"></a>Mejoras de Windows Admin Center

Una nueva versión de Windows Admin Center es horizontal, agregar nueva funcionalidad a Windows Server. Para obtener información sobre las características más recientes, consulte [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md).

## <a name="whats-new-in-storage-in-windows-server-2019-and-windows-server-version-1809"></a>Novedades de almacenamiento en Windows Server 2019 y Windows Server, versión 1809

Esta versión de Windows Server agrega los siguientes cambios y tecnologías.

### <a name="manage-storage-with-windows-admin-center"></a>Administrar el almacenamiento con Windows Admin Center

[Windows Admin Center](../manage/windows-admin-center/overview.md) es una nueva aplicación implementada localmente, basada en explorador para administrar servidores, clústeres, infraestructura hiperconvergida con espacios de almacenamiento directo y equipos con Windows 10. Se incluye sin costo adicional más allá de Windows y está listo para su uso en producción.

Para ser justos, Windows Admin Center es una descarga independiente que se ejecuta en Windows Server 2019 y otras versiones de Windows, pero es nueva y no queremos que se lo pierda...

### <a name="storage-migration-service"></a>Servicio de migración de almacenamiento

Servicio de migración de almacenamiento es una nueva tecnología que facilita migrar servidores a una versión más reciente de Windows Server. Proporciona una herramienta gráfica que inventaría los datos en los servidores, transfiere los datos y la configuración a servidores más recientes y entonces, opcionalmente, pasa las identidades de los servidores antiguos a los nuevos servidores, para que las aplicaciones y los usuarios no tengan que cambiar nada. Para obtener más información, consulta [Servicio de migración de almacenamiento](storage-migration-service/overview.md).

### <a id="storage-spaces-direct"></a>Espacios de almacenamiento directo (sólo en Windows Server 2019)

Hay una serie de mejoras en espacios de almacenamiento directo en Windows Server 2019 (espacios de almacenamiento directo no se incluye en Windows Server, el canal semianual):

- **Desduplicación y compresión de volúmenes de ReFS**

    Store hasta diez veces más datos en el mismo volumen con desduplicación y compresión para el sistema de archivos ReFS. (Tiene [un solo clic](https://www.youtube.com/watch?v=PRibTacyKko&feature=youtu.be) para activar con Windows Admin Center.) El almacén de fragmentos de tamaño variable con compresión opcional maximiza las tasas de ahorro, mientras que la arquitectura de procesamiento posterior multiproceso mantiene mínimo impacto en el rendimiento. Es compatible con volúmenes de hasta 64 TB y se desduplicar los primeros 4 TB de cada archivo.

- **Compatibilidad nativa con memoria persistente**

    Desbloquea un rendimiento sin precedentes con el soporte del Espacios de almacenamiento directo nativo para los módulos de memoria persistente, como Intel® Optane™ DC PM y NVDIMM-N. Usa la memoria persistente como memoria caché para acelerar el conjunto de trabajo activo o como capacidad para garantizar una latencia baja coherente del orden de microsegundos. Administra la memoria persistente como lo harías con cualquier otra unidad en PowerShell o Windows Admin Center.

- **Resistencia anidada para las infraestructuras hiperconvergidas de dos nodos en el perímetro**

    Sobrevive a dos errores de hardware a la vez con una opción de resistencia de software totalmente nueva opción inspirada por RAID 5+1. Con la resistencia anidada, un clúster de Espacios de almacenamiento directo de dos nodos puede proporcionar almacenamiento continuamente accesible para las aplicaciones y máquinas virtuales incluso si un nodo de servidor deja de funcionar y se produce un error en el otro nodo del servidor.

- **Unidad flash de clústeres de dos servidores con un puerto USB como un testigo**

    Use una unidad flash de USB de bajo costo conectada a su enrutador para que actúe como un testigo en clústeres de dos servidores. Si un servidor deja de funcionar y, a continuación, copia de seguridad, el clúster de la unidad USB sabe qué servidor tiene los datos más actualizados. Para obtener más información, consulte el [almacenamiento en el blog de Microsoft](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/).

- **Windows Admin Center**

    Administra y supervisa Espacios de almacenamiento directo con el nuevo [panel diseñado específicamente](../manage/windows-admin-center/use/manage-hyper-converged.md) y la experiencia en Windows Admin Center. Crea, abre, amplía o elimina volúmenes con tan solo unos clics. Supervisa el rendimiento, como la latencia E/S e IOPS del clúster general hasta el HDD o el SSD individual. Disponible sin ningún coste adicional para Windows Server 2016 y Windows Server 2019.

- **Historial de rendimiento**

    Obtén visibilidad sin esfuerzo de la utilización de recursos y del rendimiento con el [historial integrado](storage-spaces/performance-history.md). Más de 50 contadores esenciales que abarcan cálculo, memoria, red y almacenamiento se recopilan y almacenan automáticamente en el clúster hasta un máximo de un año. Lo mejor de todo, no hay nada que instalar, configurar o iniciar: funciona por sí solo. Visualiza en Windows Admin Center o consulta y procesa en PowerShell.

- **Escalar hasta 4 PB por clúster**

    Lograr escala de varios petabytes: excelente para medios, copias de seguridad y casos de uso de archivado. En Windows Server 2019, Espacios de almacenamiento directo admite hasta 4 petabytes (PB) = 4000 terabytes de capacidad sin procesar por grupo de almacenamiento. También aumentan las directrices de capacidad relacionadas: por ejemplo, puedes crear el doble de volúmenes (64 en lugar de 32), cada uno de ellos el doble de grande que antes (64 TB en lugar de 32 TB). Unir varios clústeres en un [configuración del clúster](storage-spaces/cluster-sets.md) incluso mayor escala en el almacenamiento de un espacio de nombres. Para obtener más información, consulte el [almacenamiento en el blog de Microsoft](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/).

- **Paridad acelerada reflejado es 2 X más rápido**

    Con la paridad acelerada por reflejos puedes crear volúmenes de Espacios de almacenamiento directo con paridad y reflejo de partes, como la mezcla RAID-1 y RAID-5/6 para obtener lo mejor de ambos. (Tiene [sea más fácil de lo que piensas](https://www.youtube.com/watch?v=R72QHudqWpE) en Windows Admin Center.) En Windows Server 2019, el rendimiento de paridad acelerada reflejado es más del doble en relación con Windows Server 2016 gracias a las optimizaciones.

- **Unidad de detección de valores atípicos de latencia**

    Identifica con facilidad las unidades con una latencia anómala con supervisión proactiva y detección de valores atípicos integrada, inspirada por el enfoque correcto y tradicional de Microsoft Azure. Ya sea una latencia promedio o algo más sutil como latencia de percentil 99 que destaque, las unidades lentas se etiquetan automáticamente en PowerShell y Windows Admin Center con el estado "Latencia anómala".

- **Delimitar manualmente la asignación de volúmenes para aumentar la tolerancia a errores**

    Esto permite a los administradores delimitar manualmente la asignación de volúmenes en espacios de almacenamiento directo. Si lo hace por lo que puede aumentar considerablemente la tolerancia a errores en determinadas condiciones, pero impone algunas consideraciones sobre la administración se ha agregado y la complejidad. Para obtener más información, consulte [delimitar la asignación de volúmenes](storage-spaces/delimit-volume-allocation.md).

### <a name="storage-replica2019"></a>Réplica de almacenamiento

Hay una serie de mejoras en [réplica de almacenamiento](storage-replica/storage-replica-overview.md) en esta versión:

#### <a name="storage-replica-in-windows-server-standard-edition"></a>Réplica de almacenamiento en Windows Server, Standard Edition

Ahora puede usar réplica de almacenamiento con Windows Server, Standard Edition, además de Datacenter Edition. Réplica de almacenamiento que se ejecuta en Windows Server, Standard Edition, tiene las siguientes limitaciones:

- Réplica de almacenamiento replica un único volumen en lugar de un número ilimitado de volúmenes.
- Los volúmenes pueden tener un tamaño de hasta 2 TB en lugar de un tamaño ilimitado.

#### <a name="storage-replica-log-performance-improvements"></a>Mejoras de rendimiento de registro de Réplica de almacenamiento

También hemos realizado mejoras en cómo el registro de réplica de almacenamiento realiza un seguimiento de la replicación, mejorar el rendimiento de replicación y la latencia, especialmente en el almacenamiento de memoria flash, así como los clústeres de espacios de almacenamiento directo que se replican entre sí.

Para obtener el mayor rendimiento, todos los miembros del grupo de replicación deben ejecutar Windows Server 2019.

#### <a name="test-failover"></a>Conmutaciones por error de prueba

Ahora puede temporalmente montar una instantánea de almacenamiento replicado en un servidor de destino para las pruebas o de copia de seguridad con fines. Para obtener más información, consulta [Preguntas frecuentes acerca de Réplica de almacenamiento](https://aka.ms/srfaq).

#### <a name="windows-admin-center-support"></a>Soporte técnico de Windows Admin Center

Compatibilidad con la administración gráfica de replicación ya está disponible en Windows Admin Center a través de la herramienta Administrador del servidor. Esto incluye la replicación de servidor a servidor, replicación de clúster del clúster a clúster, así como stretch.

#### <a name="miscellaneous-improvements"></a>Mejoras varias

Réplica de almacenamiento también contiene las siguientes mejoras:

-   Modifica asincrónica estirar los comportamientos de clúster para que ahora se produzca la conmutación por error automática
-   Varias correcciones de errores

### <a name="smb"></a>SMB

- **Eliminación de autenticación de invitado y el bloque de mensajes 1**: Windows Server ya no instala el cliente de bloque de mensajes 1 y el servidor de forma predeterminada. Además, la capacidad de autenticar como invitado en SMB2 y versiones posteriores está desactivada de forma predeterminada. Para obtener más información, revisa [SMBv1 no está instalado de forma predeterminada en Windows 10, versión 1709 y Windows Server, versión 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server). 

- **Compatibilidad y seguridad SMB2/SMB3**: Se han agregado opciones adicionales para la seguridad y compatibilidad de aplicaciones, incluida la capacidad de deshabilitar oplocks en SMB2 + para las aplicaciones heredadas, así como para requerir firma o cifrado en función de la conexión desde un cliente. Para obtener más información, revisa la Ayuda del módulo de SMBShare PowerShell.

### <a name="data-deduplication"></a>Desduplicación de datos

- **Ahora la desduplicación de datos admite ReFS**: Ya no debe elegir entre las ventajas de un sistema de archivos modernos con ReFS y desduplicación de datos: ahora, puede habilitar la desduplicación de datos siempre que se puede habilitar ReFS. Aumenta la eficacia del almacenamiento hasta un 95 % con ReFS.
- **API de comunicaciones de entrada/salida optimizada para los volúmenes desduplicados**: Los desarrolladores ahora pueden aprovechar el conocimiento de desduplicación de datos tiene acerca de cómo almacenar los datos de forma eficaz para mover datos entre servidores, volúmenes y clústeres de manera eficaz.

### <a name="file-server-resource-manager"></a>Administrador de recursos del servidor de archivos

Windows Server 2019 incluye la capacidad para evitar que el servicio Administrador de recursos del servidor de archivos desde la creación de un diario de cambios (también conocido como un diario USN) en todos los volúmenes cuando se inicia el servicio. Esto puede ahorrar espacio en cada volumen, pero deshabilitará la clasificación de archivos en tiempo real. Para obtener más información, consulta [Información general sobre el Administrador de recursos del servidor de archivos](fsrm/fsrm-overview.md).

## <a name="whats-new-in-storage-in-windows-server-version-1803"></a>Novedades de almacenamiento en Windows Server, versión 1803

### <a name="file-server-resource-manager"></a>Administrador de recursos del servidor de archivos

Windows Server, versión 1803 incluye la capacidad para evitar que el servicio Administrador de recursos del servidor de archivos desde la creación de un diario de cambios (también conocido como un diario USN) en todos los volúmenes cuando se inicia el servicio. Esto puede ahorrar espacio en cada volumen, pero deshabilitará la clasificación de archivos en tiempo real. Para obtener más información, consulta [Información general sobre el Administrador de recursos del servidor de archivos](fsrm/fsrm-overview.md).

## <a name="whats-new-in-storage-in-windows-server-version-1709"></a>Novedades de almacenamiento en Windows Server, versión 1709

Windows Server, versión 1709 es la primera versión de Windows Server en el canal semianual. El canal semianual es una ventaja de Software Assurance y es totalmente compatible en producción durante 18 meses, con una nueva versión cada seis meses.

Para obtener más información, consulta [Introducción al Canal semianual de Windows Server](../get-started/semi-annual-channel-overview.md).

### <a name="storage-replica"></a>Réplica de almacenamiento

La protección de recuperación ante desastres agregada por la réplica de almacenamiento se ha ampliado para incluir:

- **Conmutación por error de prueba**: la opción para montar el almacenamiento de destino ya es posible a través de la característica Conmutación por error de prueba. Puedes montar una instantánea del almacenamiento replicado en los nodos de destino temporalmente para realizar pruebas o copias de seguridad. Para obtener más información, consulta [Preguntas frecuentes acerca de Réplica de almacenamiento](https://aka.ms/srfaq).
- **Soporte técnico de Windows Admin Center**: Compatibilidad con la administración gráfica de replicación ya está disponible en Windows Admin Center a través de la herramienta Administrador del servidor. Esto incluye la replicación de servidor a servidor, replicación de clúster del clúster a clúster, así como stretch.

Réplica de almacenamiento también contiene las siguientes mejoras:

-   Modifica asincrónica estirar los comportamientos de clúster para que ahora se produzca la conmutación por error automática
-   Varias correcciones de errores

### <a name="smb"></a>SMB

- **Eliminación de autenticación de invitado y el bloque de mensajes 1**: Windows Server, versión 1709 ya no instala el cliente de bloque de mensajes 1 y el servidor de forma predeterminada. Además, la capacidad de autenticar como invitado en SMB2 y versiones posteriores está desactivada de forma predeterminada. Para obtener más información, revisa [SMBv1 no está instalado de forma predeterminada en Windows 10, versión 1709 y Windows Server, versión 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server). 

- **Compatibilidad y seguridad SMB2/SMB3**: Se han agregado opciones adicionales para la seguridad y compatibilidad de aplicaciones, incluida la capacidad de deshabilitar oplocks en SMB2 + para las aplicaciones heredadas, así como para requerir firma o cifrado en función de la conexión desde un cliente. Para obtener más información, revisa la Ayuda del módulo de SMBShare PowerShell.

### <a name="data-deduplication"></a>Desduplicación de datos

- **Ahora la desduplicación de datos admite ReFS**: Ya no debe elegir entre las ventajas de un sistema de archivos modernos con ReFS y desduplicación de datos: ahora, puede habilitar la desduplicación de datos siempre que se puede habilitar ReFS. Aumenta la eficacia del almacenamiento hasta un 95 % con ReFS.
- **API de comunicaciones de entrada/salida optimizada para los volúmenes desduplicados**: Los desarrolladores ahora pueden aprovechar el conocimiento de desduplicación de datos tiene acerca de cómo almacenar los datos de forma eficaz para mover datos entre servidores, volúmenes y clústeres de manera eficaz.

## <a name="whats-new-in-storage-in-windows-server-2016"></a>Novedades de Espacios de almacenamiento en Windows Server 2016

### <a name="s2d"></a>Espacios de almacenamiento directo  
Espacios de almacenamiento directo permite la creación de almacenamiento altamente disponible y escalable con servidores de almacenamiento local. Simplifica la implementación y administración de los sistemas de almacenamiento definidos por software y desbloquea el uso de las nuevas clases de dispositivos de disco, como SSD de SATA y dispositivos de disco NVMe, que no estaban disponibles con Espacios de almacenamiento de clúster con discos compartidos.  

**¿Qué valor aporta este cambio?**  
Espacios de almacenamiento directo permite a los proveedores de servicios y las empresas usar los servidores estándar del sector con almacenamiento local para generar almacenamiento definido por software altamente disponible y escalable. El uso de servidores con almacenamiento local disminuye la complejidad, aumenta la escalabilidad y permite el uso de dispositivos de almacenamiento que antes no se podían emplear, como los discos de estado sólido SATA para reducir el costo de almacenamiento flash o los discos de estado sólido NVMe para mejorar el rendimiento.  

Con Espacios de almacenamiento directo, ya no es necesario disponer un tejido de SAS compartido, simplificando la implementación y la configuración. En su lugar, utiliza la red como un tejido de almacenamiento, aprovechando SMB3 y SMB directo (RDMA) para el almacenamiento eficiente de CPU de alta velocidad y latencia baja. Para escalar horizontalmente, basta con agregar más servidores para aumentar la capacidad de almacenamiento y rendimiento de E/S.  
Para más información, consulte [Storage Spaces Direct in Windows Server 2016](storage-spaces/storage-spaces-direct-overview.md) (Espacios de almacenamiento directo en Windows Server 2016).  

**¿Qué funciona de manera diferente?**  
Esta funcionalidad es nueva en Windows Server 2016.  

### <a name="storage-replica"></a>Réplica de almacenamiento

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

### <a name="storage-qos"></a>Calidad de servicio de almacenamiento  
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

### <a name="dedup"></a>Desduplicación de datos  
| Funcionalidad | Nueva o actualizada | Descripción |
|---------------|----------------|-------------|
| [Compatibilidad con volúmenes grandes](data-deduplication/whats-new.md#large-volume-support) | Actualizado | Antes de Windows Server 2016, los volúmenes debían tener un tamaño específico para la renovación esperada, y aquellos tamaños de volúmenes por encima de los 10 TB no eran buenos candidatos para la desduplicación. En Windows Server 2016, Desduplicación de datos admite tamaños de volúmenes de **hasta 64 TB**. |
| [Compatibilidad con archivos de gran tamaño](data-deduplication/whats-new.md#large-file-support) | Actualizado | Antes de Windows Server 2016, los archivos cuyo tamaño se aproximase a 1 TB no eran buenos candidatos para la desduplicación. En Windows Server 2016, los archivos de **hasta 1 TB** son totalmente compatibles. |
| [Compatibilidad con Nano Server](data-deduplication/whats-new.md#nano-server-support) | Nuevo | Desduplicación de datos está disponible y es totalmente compatible con la nueva opción de implementación de Nano Server para Windows Server 2016. |
| [Compatibilidad con copia de seguridad simplificada](data-deduplication/whats-new.md#simple-backup-support) | Nuevo | En Windows Server 2012 R2, las aplicaciones virtualizadas de copia de seguridad, como [Data Protection Manager](https://technet.microsoft.com/library/hh758173.aspx) de Microsoft, se admitían a través de una serie de pasos de configuración manual. En Windows Server 2016, se ha agregado un nuevo tipo de uso de "Copia de seguridad" predeterminado para una implementación fluida de Desduplicación de datos para aplicaciones virtualizadas de copia de seguridad. |
| [Compatibilidad con las actualizaciones graduales de SO del clúster](data-deduplication/whats-new.md#cluster-upgrade-support) | Nuevo | Desduplicación de datos es totalmente compatible con la nueva característica [Actualización gradual de sistema operativo de clúster](..//failover-clustering/cluster-operating-system-rolling-upgrade.md) de Windows Server 2016. |

### <a name="smb-hardening-improvements"></a>Mejoras para las conexiones de SYSVOL y NETLOGON de protección de SMB  
En Windows 10 y Windows Server 2016, las conexiones de cliente a los recursos compartidos de archivos SYSVOL y NETLOGON predeterminados de Active Directory Domain Services en controladores de dominio ahora requieren firma SMB y autenticación mutua (como Kerberos).   

**¿Qué valor aporta este cambio?**  
Este cambio reduce la probabilidad de ataques de tipo "Man in the middle".   

**¿Qué funciona de manera diferente?**  
Si la firma SMB y la autenticación mutua no están disponibles, un equipo con Windows 10 o Windows Server 2016 no procesará las directivas de grupo y los scripts basados en dominio.  

> [!NOTE]  
> Los valores del Registro para esta configuración no están presentes de forma predeterminada, pero se siguen aplicando las reglas de protección hasta que se reemplacen por la Directiva de grupo u otros valores del registro.  

Para obtener más información sobre estas mejoras de seguridad - también se conoce como protección UNC, vea el artículo de Microsoft Knowledge Base [3000483](https://support.microsoft.com/kb/3000483) y [MS15-011 & MS15-014: Directiva de grupo de protección](https://blogs.technet.microsoft.com/srd/2015/02/10/ms15-011-ms15-014-hardening-group-policy).  

### <a name="work-folders"></a>Carpetas de trabajo
Notificación de cambio mejorada cuando el servidor de carpetas de trabajo se está ejecutando Windows Server 2016 y el cliente de carpetas de trabajo es Windows 10.

**¿Qué valor aporta este cambio?**<br>
En Windows Server 2012 R2, cuando los cambios efectuados en el archivo se sincronizan con el servidor de Carpetas de trabajo, no se notifica el cambio a los clientes y estos esperan hasta 10 minutos para obtener la actualización.  Cuando se usa Windows Server 2016, el servidor de carpetas de trabajo notifica inmediatamente a los clientes de Windows 10 y los cambios del archivo se sincronizan inmediatamente.

**¿Qué funciona de manera diferente?**<br>
Esta funcionalidad es nueva en Windows Server 2016. Requiere un servidor de Carpetas de trabajo Windows Server 2016 y el cliente debe ser Windows 10.

Si usas un cliente anterior, o si el servidor de Carpetas de trabajo es Windows Server 2012 R2, el cliente seguirá buscando cambios cada 10 minutos.

### <a name="refs"></a>ReFS 
La siguiente iteración del ReFS proporciona compatibilidad para implementaciones de almacenamiento a gran escala con diferentes cargas de trabajo, así como confiabilidad, resistencia y escalabilidad para los datos.     

**¿Qué valor aporta este cambio?**<br>
ReFS presenta las siguientes mejoras:

* ReFS implementa la nueva característica de niveles de almacenamiento, lo que ayuda a proporcionar un rendimiento más rápido y mayor capacidad de almacenamiento. Esta nueva característica permite:
    * Varios tipos de resistencia en el mismo disco virtual (mediante la creación de reflejos en el nivel de rendimiento y paridad en el nivel de capacidad, por ejemplo).
    * Mayor capacidad de respuesta ante el desfase de conjuntos de trabajo.  
* La incorporación de la clonación de bloques mejora sustancialmente el rendimiento de las operaciones de máquina virtual, como las operaciones de fusión de punto de control .vhdx.
* La nueva herramienta de examen de ReFS permite la recuperación de almacenamiento perdido y ayuda a rescatar datos en caso de daños críticos. 

**¿Qué funciona de manera diferente?**<br>
Estas funcionalidades son nuevas en Windows Server 2016. 

## <a name="see-also"></a>Vea también  
* [Novedades en Windows Server 2016](../get-started/what-s-new-in-windows-server-2016.md)  
