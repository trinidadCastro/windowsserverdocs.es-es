---
title: Información general de Espacios de almacenamiento directos
ms.prod: windows-server
ms.author: cosdar
manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 07/24/2020
ms.assetid: 8bd0d09a-0421-40a4-b752-40ecb5350ffd
description: Información general de Espacios de almacenamiento directo, una característica de Windows Server y Azure Stack HCl que permite agrupar en clústeres servidores con almacenamiento interno en una solución de almacenamiento definida por software.
ms.localizationpriority: medium
ms.openlocfilehash: 3fd86a8465d2fef59ccce73fc473790682f0d180
ms.sourcegitcommit: de8fea497201d8f3d995e733dfec1d13a16cb8fa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87864326"
---
# <a name="storage-spaces-direct-overview"></a>Información general de Espacios de almacenamiento directos

>Se aplica a: Azure Stack HCl, Windows Server 2019, Windows Server 2016

Espacios de almacenamiento directo usa servidores estándar del sector con unidades conectadas localmente para crear almacenamiento definido por software de alta disponibilidad y escalabilidad por un porcentaje mínimo del costo de las matrices de SAN o NAS tradicionales. Su arquitectura convergente o hiperconvergida simplifica radicalmente el aprovisionamiento y la implementación, mientras que las características como el almacenamiento en caché, las capas de almacenamiento y la codificación de borrado, junto con las últimas innovaciones de hardware como redes RDMA y unidades de NVMe, ofrecen eficiencia y rendimiento inigualable.

Espacios de almacenamiento directo se incluye en [Azure Stack HCl](/azure-stack/hci/), windows Server 2019 Datacenter, windows Server 2016 Datacenter y las [compilaciones de Windows Server Insider Preview](https://insider.windows.com/for-business-getting-started-server/).

Para otras aplicaciones de espacios de almacenamiento, como clústeres de SAS compartidos y servidores independientes, consulte [Introducción a los espacios de almacenamiento](overview.md). Si está buscando información sobre el uso de espacios de almacenamiento en un equipo con Windows 10, consulte [espacios de almacenamiento en Windows 10](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

| Descripción | Documentación |
|--|--|
| **Descripción**<br><ul><li>Información general (está aquí)</li><li>[Descripción de la memoria caché](understand-the-cache.md)</li><li>[Tolerancia a errores y eficiencia del almacenamiento](storage-spaces-fault-tolerance.md)<li>[Consideraciones de simetría de unidad](drive-symmetry-considerations.md)</li><li>[Comprender y controlar la resincronización de almacenamiento](understand-storage-resync.md)</li><li>[Descripción del cuórum de clúster y grupo](understand-quorum.md)</li><li>[Conjuntos de clústeres](cluster-sets.md)</li> | **Plan**<br><ul><li>[Requisitos de hardware](storage-spaces-direct-hardware-requirements.md)</li><li>[Usar la caché de lectura en memoria CSV](csv-cache.md)</li><li>[Elegir las unidades](choosing-drives.md)</li><li>[Planificar los volúmenes](plan-volumes.md)</li><li>[Usar clústeres de VM invitados](storage-spaces-direct-in-vm.md)</li><li>[Recuperación ante desastres](storage-spaces-direct-disaster-recovery.md)</li> |
| **Implementar**<br><ul><li>[Implementar espacios de almacenamiento directo](deploy-storage-spaces-direct.md)</li><li>[Crear volúmenes](create-volumes.md)</li><li>[Resistencia anidada](nested-resiliency.md)</li><li>[Configurar cuórum](../../failover-clustering/manage-cluster-quorum.md)</li><li>[Actualizar un clúster de espacios de almacenamiento directo a Windows Server 2019](upgrade-storage-spaces-direct-to-windows-server-2019.md)</li><li>[Descripción e implementación de memoria persistente](deploy-pmem.md)</li> | **Administración**<br><ul><li>[Administrar con Windows Admin Center](../../manage/windows-admin-center/use/manage-hyper-converged.md)</li><li>[Agregar servidores o unidades](add-nodes.md)</li><li>[Desconectar un servidor para realizar labores de mantenimiento](maintain-servers.md)</li><li>[Quitar servidores](remove-servers.md)</li><li>[Ampliar volúmenes](resize-volumes.md)</li><li>[Eliminar volúmenes](delete-volumes.md)</li><li>[Actualizar el firmware de la unidad](../update-firmware.md)</li><li>[Historial de rendimiento](performance-history.md)</li><li>[Delimitar la asignación de volúmenes](delimit-volume-allocation.md)</li><li>[Usar Azure Monitor en un clúster hiperconvergido](configure-azure-monitor.md)</li> |
| **Solución de problemas**<br><ul><li>[Escenarios de solución de problemas](troubleshooting-storage-spaces.md)</li><li>[Solucionar problemas de Estados operativos y de estado](storage-spaces-states.md)</li><li>[Recopilación de datos de diagnóstico con Espacios de almacenamiento directo](data-collection.md)</li><li>[Administración del estado de memoria de clase de almacenamiento](Storage-class-memory-health.md)</li> | **Entradas de blog recientes**<br><ul><li>[13,7 millones IOPS con Espacios de almacenamiento directo: el nuevo registro del sector para la infraestructura hiperconvergida](https://techcommunity.microsoft.com/t5/storage-at-microsoft/the-new-hci-industry-record-13-7-million-iops-with-windows/ba-p/428314)</li><li>[Infraestructura hiperconvergida en Windows Server 2019: el reloj de la cuenta atrás comienza ahora.](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB)</li><li>[Cinco anuncios grandes de la Cumbre de Windows Server](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB)</li><li>[10.000 Espacios de almacenamiento directo clústeres y recuento...](https://techcommunity.microsoft.com/t5/storage-at-microsoft/storage-spaces-direct-10-000-clusters-and-counting/ba-p/428185)</li></ul> |

## <a name="videos"></a>Vídeos

**Introducción rápida al vídeo (5 minutos)**

> [!Video https://www.youtube-nocookie.com/embed/raeUiNtMk0E]

**Espacios de almacenamiento directo en Microsoft encendido 2018 (1 hora)**

> [!Video https://www.youtube-nocookie.com/embed/5kaUiW3qo30]

**Espacios de almacenamiento directo en Microsoft encendido 2017 (1 hora)**

> [!Video https://www.youtube-nocookie.com/embed/YDr2sqNB-3c]

**Evento Launch en Microsoft encendido 2016 (1 hora)**

> [!Video https://www.youtube-nocookie.com/embed/LK2ViRGbWs]

## <a name="key-benefits"></a>Ventajas principales

| Imagen | Descripción |
|--|--|
| ![Simplicidad](media/storage-spaces-direct-in-windows-server-2016/simplicity-icon.png) | **Proporcionar.** Pase de servidores estándar del sector que ejecutan Windows Server 2016 a su primer clúster de Espacios de almacenamiento directo en menos de 15 minutos. En el caso de los usuarios de System Center, la implementación consiste en una simple casilla. |
| ![Rendimiento inigualable](media/storage-spaces-direct-in-windows-server-2016/performance-icon.png) | **Rendimiento inigualable.** Tanto si son de memoria flash como híbridas, Espacios de almacenamiento directo supera fácilmente las [150 000 E/S por segundo aleatorias de 4K mixtas por servidor](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB) con una latencia baja constante gracias a su arquitectura incrustada de hipervisor, su caché de lectura y escritura integrada y su compatibilidad con las unidades de NVMe de vanguardia montadas directamente en el bus PCIe. |
| ![Tolerancia a errores](media/storage-spaces-direct-in-windows-server-2016/fault-tolerance-icon.png) | **Tolerancia a errores.** La resistencia integrada controla los errores de unidades, servidores o componentes con una disponibilidad continua. También es posible configurar implementaciones más grandes para la [tolerancia a errores de chasis y bastidor](../../failover-clustering/fault-domains.md). Cuando se produce un error de hardware, cámbielo. El software se repara automáticamente, sin pasos de administración complicados. |
| ![Eficacia de los recursos](media/storage-spaces-direct-in-windows-server-2016/efficiency-icon.png) | **Eficiencia de los recursos.** La codificación de borrado ofrece una eficiencia de almacenamiento de hasta 2,4 veces mayor, con innovaciones exclusivas como códigos de reconstrucción locales y niveles de ReFS en tiempo real para ampliar estas ganancias a unidades de disco duro y cargas de trabajo mixtas de acceso frecuente y frío, todo ello a la vez que se minimiza el consumo de CPU para dar recursos a donde son necesarias la mayoría de las máquinas virtuales. |
| ![Facilidad de uso](media/storage-spaces-direct-in-windows-server-2016/manageability-icon.png) | **Manejabilidad**. Use los [controles de QoS de almacenamiento](../storage-qos/storage-qos-overview.md) para comprobar las máquinas virtuales demasiado ocupadas con límites mínimos y máximos de E/S por segundo por máquina virtual. El [Servicio de mantenimiento](../../failover-clustering/health-service-overview.md) proporciona continuamente alertas y supervisión integrada, mientras que las nuevas API facilitan la recopilación de métricas enriquecidas de rendimiento y capacidad de todo el clúster. |
| ![Escalabilidad](media/storage-spaces-direct-in-windows-server-2016/scalability-icon.png) | **Escalabilidad**. Vaya hasta 16 servidores y más de 400 unidades, hasta 1 petabyte (1.000 terabytes) de almacenamiento por clúster. Para escalar horizontalmente, basta con agregar unidades o más servidores. Espacios de almacenamiento directo incorporará automáticamente las unidades nuevas y empezará a usarlas. La eficiencia de almacenamiento y el rendimiento mejoran de forma predecible a escala. |

## <a name="deployment-options"></a>Opciones de implementación

Espacios de almacenamiento directo está diseñado para dos opciones de implementación distintas:

### <a name="converged"></a>Convergido

**Almacenamiento y cálculo en clústeres independientes.** La opción de implementación convergida, también denominada "desagregada", separa en capas un Servidor de archivos de escalabilidad horizontal (SoFS) encima de Espacios de almacenamiento directo para proporcionar almacenamiento conectado a la red a través de recursos compartidos de archivos SMB3. Esto permite escalar el proceso y la carga de trabajo independientemente del clúster de almacenamiento, lo que es esencial para implementaciones a gran escala, como IaaS (infraestructura como servicio) de Hyper-V para empresas y proveedores de servicios.

![Espacios de almacenamiento directo proporciona almacenamiento mediante la característica de Servidor de archivos de escalabilidad horizontal a máquinas virtuales de Hyper-V en otro servidor o clúster](media/storage-spaces-direct-in-windows-server-2016/converged-minimal.png)

### <a name="hyper-converged"></a>Hiperconvergido

**Un clúster para cálculo y almacenamiento.** La opción de implementación hiperconvergida ejecuta máquinas virtuales de Hyper-V o bases de datos de SQL Server directamente en los servidores que proporcionan el almacenamiento y almacena sus archivos en los volúmenes locales. Esto elimina la necesidad de configurar los permisos y el acceso al servidor de archivos, y además reduce los costos de hardware para pequeñas y medianas empresas o implementaciones remotas de oficinas y sucursales. Consulte [implementar espacios de almacenamiento directo](deploy-storage-spaces-direct.md).

![Espacios de almacenamiento directo sirve almacenamiento a las máquinas virtuales de Hyper-V en el mismo clúster](media/storage-spaces-direct-in-windows-server-2016/hyper-converged-minimal.png)

## <a name="how-it-works"></a>Funcionamiento

Espacios de almacenamiento directo es la evolución de Espacios de almacenamiento, introducido por primera vez en Windows Server 2012. Aprovecha muchas de las características que se conocen hoy en día en Windows Server, como los clústeres de conmutación por error, el sistema de archivos del Volumen compartido de clúster (CSV), el Bloque de mensajes del servidor (SMB) 3 y, por supuesto, Espacios de almacenamiento. También introduce tecnologías nuevas, en especial el bus de almacenamiento de software.

A continuación presentamos una visión general de la pila de Espacios de almacenamiento directo:

![Pila de Espacios de almacenamiento directo](media/storage-spaces-direct-in-windows-server-2016/converged-full-stack.png)

**Hardware de red.** Espacios de almacenamiento directo usa SMB3 (incluido SMB directo y SMB multicanal) sobre Ethernet para la comunicación entre los servidores. Le recomendamos encarecidamente que disponga de más de 10 GbE con acceso directo a memoria remota (RDMA), ya sea iWARP o RoCE.

**Hardware de almacenamiento.** De 2 a 16 servidores con unidades de SATA, SAS o NVMe conectadas localmente. Cada servidor debe tener al menos dos unidades de estado sólido y al menos cuatro unidades adicionales. Los dispositivos SATA y SAS deben estar detrás de un adaptador de bus host (HBA) y un ampliador SAS. Le recomendamos encarecidamente las plataformas meticulosamente diseñadas y ampliamente validadas de nuestros socios (próximamente).

**Clústeres de conmutación por error.** La característica integrada de agrupación en clústeres de Windows Server se usa para conectar los servidores.

**Bus de almacenamiento de software.** El bus de almacenamiento de software es una novedad de Espacios de almacenamiento directo. Abarca el clúster y establece un tejido de almacenamiento definido por software en el que todos los servidores pueden ver todas las unidades locales de cada uno de ellos. Considérelo una manera de reemplazar el cableado costoso y restrictivo de Canal de fibra o SAS compartido.

**Caché de capa de bus de almacenamiento.** El bus de almacenamiento de software enlaza dinámicamente las unidades más rápidas presentes (por ejemplo, SSD) con unidades más lentas (por ejemplo, HDD) para proporcionar un almacenamiento en caché de lectura y escritura del lado servidor que acelera la E/S y aumenta el rendimiento.

**Bloque de almacenamiento.** La colección de unidades que forman la base de Espacios de almacenamiento se denomina grupo de almacenamiento. Se crea automáticamente, y todas las unidades válidas se detectan y se agregan automáticamente. Le recomendamos encarecidamente que use un grupo por cada clúster, con la configuración predeterminada. Lee nuestro blog sobre [profundización en el grupo de almacenamiento](https://techcommunity.microsoft.com/t5/storage-at-microsoft/deep-dive-the-storage-pool-in-storage-spaces-direct/ba-p/425959) para obtener más información.

**Espacios de almacenamiento.** Los espacios de almacenamiento proporcionan tolerancia a errores a "discos" virtuales mediante la [creación de reflejo, la codificación de borrado o ambos](storage-spaces-fault-tolerance.md). Considérelo un volumen RAID distribuido definido por software que usa las unidades del grupo. En Espacios de almacenamiento directo, estos discos virtuales suelen tener resistencia a dos errores de unidad o de servidor simultáneos (por ejemplo, creación de reflejo triple, con cada copia de datos en un servidor diferente) aunque la tolerancia a errores de chasis y bastidor también esté disponible.

**Sistema de archivos resistente (ReFS).** ReFS es un sistema de archivos Premier diseñado específicamente para la virtualización. Incluye aceleraciones considerables para operaciones de archivos .vhdx, como la creación, la expansión y la combinación de puntos de control, así como sumas de comprobación integradas para detectar y corregir errores de bit. También incluye niveles en tiempo real que giran los datos entre las capas de almacenamiento "en caliente" y "en frío" en tiempo real en función del uso.

**Volúmenes compartidos de clúster.** El sistema de archivos CSV unifica todos los volúmenes ReFS en un único espacio de nombres accesible a través de cualquier servidor, de modo que, en cada servidor, todos los volúmenes parecen y actúan como montados localmente.

**Servidor de archivos de escalabilidad horizontal.** Esta capa final solo es necesaria para las implementaciones convergidas. Proporciona acceso a archivos remotos con el protocolo de acceso SMB3 a clientes (como otro clúster que ejecuta Hyper-V) a través de la red, con lo que efectivamente convierte Espacios de almacenamiento directo en un almacenamiento conectado a la red (NAS).

## <a name="customer-stories"></a>Testimonios de clientes

Hay [más de 10.000 clústeres](https://techcommunity.microsoft.com/t5/storage-at-microsoft/storage-spaces-direct-10-000-clusters-and-counting/ba-p/428185) que se ejecutan en todo el mundo espacios de almacenamiento directo. Las organizaciones de todos los tamaños, desde pequeñas empresas que solo implementan dos nodos, hasta grandes empresas y gobiernos que implementan cientos de nodos, dependen de Espacios de almacenamiento directo para las aplicaciones y la infraestructura críticas.

Visite [Microsoft.com/HCI](https://www.microsoft.com/hci) para leer sus historias:

[![Cuadrícula de logotipos de cliente](media/storage-spaces-direct-in-windows-server-2016/customer-stories.png)](https://www.microsoft.com/hci)

## <a name="management-tools"></a>Herramientas de administración

Las herramientas siguientes se pueden usar para administrar y/o supervisar Espacios de almacenamiento directo:

| Nombre | ¿Gráfico o línea de comandos? | ¿Está pagado o incluido? |
|-----------------|----------------------------|-------------------|
| [Windows Admin Center](../../manage/windows-admin-center/overview.md)     | Gráfico    | Se incluye |
| Administrador del servidor & Administrador de clústeres de conmutación por error                                 | Gráfico    | Se incluye |
| Windows PowerShell                                                        | Línea de comandos | Se incluye |
| [System Center Virtual Machine Manager (SCVMM)](/system-center/vmm/s2d?view=sc-vmm-2019) <br>& [Operations Manager (SCOM)](https://www.microsoft.com/download/details.aspx?id=54700) | Gráfico    | Pagado     |

## <a name="get-started"></a>Primeros pasos

Intente Espacios de almacenamiento directo [en Microsoft Azure](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB)o descargue una copia de evaluación con licencia de 180 días de Windows Server de las [evaluaciones de Windows Server](https://go.microsoft.com/fwlink/?linkid=842602).

## <a name="additional-references"></a>Referencias adicionales

- [Tolerancia a errores y eficacia del almacenamiento](storage-spaces-fault-tolerance.md)
- [Réplica de almacenamiento](../storage-replica/storage-replica-overview.md)
- [Blog de Storage en Microsoft](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB)
- [Espacios de almacenamiento directo rendimiento con iWARP](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB) (blog de TechNet)
- [Novedades de los clústeres de conmutación por error de Windows Server](../../failover-clustering/whats-new-in-failover-clustering.md)
- [Calidad de servicio de almacenamiento](../storage-qos/storage-qos-overview.md)
- [Soporte técnico Windows IT Pro](https://www.microsoft.com/itpro/windows/support)
