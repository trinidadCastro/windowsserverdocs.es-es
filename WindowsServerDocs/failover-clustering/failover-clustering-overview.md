---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: Clústeres de conmutación por error
ms.prod: windows-server-threshold
layout: LandingPage
ms.topic: landing-page
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 03/08/2019
ms.localizationpriority: high
ms.openlocfilehash: 9e4184e52ef48e758ebc80e63d3d6f952a09cc2c
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222452"
---
# <a name="failover-clustering-in-windows-server"></a>Conmutación de clústeres por error en Windows Server

> Se aplica a: Windows Server 2019, Windows Server 2016

>[!TIP]
> ¿Buscas información sobre versiones anteriores de Windows Server? Eche un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puedes [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

<hr />

Un clúster de conmutación por error es un grupo de equipos independientes que trabajan juntos para aumentar la disponibilidad y la escalabilidad de los roles en clúster (anteriormente denominados aplicaciones y servicios en clúster). Los servidores agrupados (denominados nodos) están conectados mediante cables físicos y mediante software. Si se produce un error en uno o más de los nodos del clúster, otro nodo comienza a dar servicio (proceso que se denomina conmutación por error). Además, los roles en clúster se supervisan proactivamente para comprobar que estén funcionando correctamente. Si no están funcionando, se reinician o se mueven a otro nodo.

Los clústeres de conmutación por error también proporcionan la funcionalidad Volúmenes compartidos de clúster (CSV), que ofrece un espacio de nombres distribuido y uniforme que los roles en clúster pueden usar para acceder al almacenamiento compartido de todos los nodos. Con la característica Clústeres de conmutación por error, los usuarios experimentan una cantidad mínima de interrupciones del servicio.

La Conmutación de clústeres por error tiene muchas aplicaciones prácticas, incluyendo:
* Almacenamiento de recursos compartidos de archivos disponible continuamente o altamente disponible para aplicaciones como Microsoft SQL Server y máquinas virtuales de Hyper-V.
* Roles en clúster de alta disponibilidad que se ejecutan en servidores físicos o en máquinas virtuales instaladas en servidores que ejecutan Hyper-V.


|  |  |
|---------|---------|
|![Novedades](../media/i-whats-new.svg)  | [**Novedades de agrupación en clústeres de conmutación por error**](whats-new-in-failover-clustering.md) |


|  |  |  |
|---------|---------|---------|
|![Comprender](../media/i-cluster.svg)**comprender**  |  ![Planeación](../media/i-cluster.svg)**planeación**  |  ![Implementación](../media/i-cluster.svg)**implementación**       |
| [Servidor de archivos de escalabilidad horizontal para datos de aplicación](sofs-overview.md)    |   [Planear los requisitos de Hardware de clústeres de conmutación por error y opciones de almacenamiento](clustering-requirements.md)      |  [Objetos de equipo de clúster preconfigurado de implementación en servicios de dominio de Active Directory](prestage-cluster-adds.md)  |
|  [Cuórum de clúster y grupo](../storage/storage-spaces/understand-quorum.md)   |   [Usar volúmenes compartidos de clúster (CSV)](failover-cluster-csvs.md)      | [Creación de un clúster de conmutación por error](create-failover-cluster.md)        |
|  [Reconocimiento de dominio de error](fault-domains.md)   |  [Usar clústeres invitados de máquina virtual con espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-in-vm.md)       | [Implementar un servidor de archivos de dos nodos](../storage/storage-spaces/storage-spaces-direct-in-vm.md)        |
| [SMB multicanal simplificada y redes de clústeres de varias NIC](smb-multichannel.md)    |         |  [Administrar el quórum y testigos](manage-cluster-quorum.md)       |
|   [Equilibrio de carga de VM](vm-load-balancing-overview.md)  |         |   [Implementación de un testigo en la nube](deploy-cloud-witness.md)      |
|   [Conjuntos de clústeres](../storage/storage-spaces/cluster-sets.md)  |         |     [Implementar un testigo del recurso compartido de archivos](file-share-witness.md)    |
|   [Afinidad de clústeres](cluster-affinity.md)  |         |    [Actualizaciones graduales del sistema operativo del clúster]()     |
|     |         |     [Actualizar un clúster de conmutación por error en el mismo hardware](upgrade-option-same-hardware.md)    |
|     |         |     [Implementar un clúster desasociado de Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\))    |


|  |  |  |
|---------|---------|---------|
|![Administrar](../media/i-cluster.svg)**administrar**  |  ![Configuración y herramientas](../media/i-cluster.svg)**herramientas y configuración**  |  ![Recursos de la Comunidad](../media/i-cluster.svg)**recursos de la Comunidad**       |
| [Actualización compatible con clústeres](cluster-aware-updating.md)    |   [Cmdlets de PowerShell de clústeres de conmutación por error](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)      |  [Foro High Availability (Clustering)](https://go.microsoft.com/fwlink/p/?LinkId=230641)       |
|  [Servicio de mantenimiento](health-service-overview.md)   |   [Tenga en cuenta los Cmdlets de PowerShell de actualización del clúster](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps)      | [Blog del equipo de equilibrio de carga de agrupación en clústeres de conmutación por error y de red](http://blogs.msdn.com/b/clustering/)        |
|  [Migración del dominio del clúster](cluster-domain-migration.md)   |         |         |
|  [Solución de problemas de uso del Informe de errores de Windows](troubleshooting-using-wer-reports.md)   |         |         |
