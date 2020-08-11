---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: Clúster de conmutación por error
ms.topic: landing-page
manager: lizross
author: JasonGerend
ms.author: jgerend
ms.date: 06/06/2019
ms.localizationpriority: high
ms.openlocfilehash: 41f5eef75e20a4da740141620493d2daa254b0a1
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992859"
---
# <a name="failover-clustering-in-windows-server"></a>Conmutación de clústeres por error en Windows Server

> Se aplica a: Windows Server 2019, Windows Server 2016

Un clúster de conmutación por error es un grupo de equipos independientes que trabajan juntos para aumentar la disponibilidad y la escalabilidad de los roles en clúster (anteriormente denominados aplicaciones y servicios en clúster). Los servidores agrupados (denominados nodos) están conectados mediante cables físicos y mediante software. Si se produce un error en uno o más de los nodos del clúster, otro nodo comienza a dar servicio (proceso que se denomina conmutación por error). Además, los roles en clúster se supervisan proactivamente para comprobar que estén funcionando correctamente. Si no están funcionando, se reinician o se mueven a otro nodo.

Los clústeres de conmutación por error también proporcionan la funcionalidad Volúmenes compartidos de clúster (CSV), que ofrece un espacio de nombres distribuido y uniforme que los roles en clúster pueden usar para acceder al almacenamiento compartido de todos los nodos. Con la característica Clústeres de conmutación por error, los usuarios experimentan una cantidad mínima de interrupciones del servicio.

La Conmutación de clústeres por error tiene muchas aplicaciones prácticas, incluyendo:

* Almacenamiento de recursos compartidos de archivos disponible continuamente o altamente disponible para aplicaciones como Microsoft SQL Server y máquinas virtuales de Hyper-V.
* Roles en clúster de alta disponibilidad que se ejecutan en servidores físicos o en máquinas virtuales instaladas en servidores que ejecutan Hyper-V.

| **Descripción**                                                               |  **Planeamiento**                          |  **Implementación**       |
| -------------                                                                |  --------------                        | --------------------- |
| [Novedades de los clústeres de conmutación por error](whats-new-in-failover-clustering.md)    | [Planeamiento de los requisitos de hardware de los clústeres de conmutación por error y opciones de almacenamiento](clustering-requirements.md)  | [Creación de clústeres de conmutación por error](create-failover-cluster.md) |
| [Servidor de archivos de escalabilidad horizontal para datos de aplicación](sofs-overview.md)               | [Usar volúmenes compartidos de clúster (CSV)](failover-cluster-csvs.md) | [Implementar un servidor de archivos de dos nodos](deploy-two-node-clustered-file-server.md) |
|  [Cuórum de clúster y grupo](../storage/storage-spaces/understand-quorum.md)   |  [Usar clústeres de máquina virtual de invitado con espacios de almacenamiento directo](../storage/storage-spaces/storage-spaces-direct-in-vm.md)       | [Preconfigurar objetos de equipo de clúster en Active Directory Domain Services](prestage-cluster-adds.md) |
| [Reconocimiento de dominio de error](fault-domains.md)                                 |                                 | [Configuración de cuentas de clúster en Active Directory](configure-ad-accounts.md) |
| [SMB multicanal simplificada y redes de clústeres de varias NIC](smb-multichannel.md) |                       | [Administrar el cuórum y los testigos](manage-cluster-quorum.md) |
| [Equilibrio de carga de VM](vm-load-balancing-overview.md)                         |                             | [Implementar un testigo en la nube](deploy-cloud-witness.md) |
| [Conjuntos de clústeres](../storage/storage-spaces/cluster-sets.md)                  |                             |[Implementar un testigo del recurso compartido de archivos](file-share-witness.md) |
| [Afinidad de clústeres](cluster-affinity.md)                                     |                            | [Actualizaciones graduales del sistema operativo del clúster](cluster-operating-system-rolling-upgrade.md) |
|                                                                             |                            | [Actualizar un clúster de conmutación por error en el mismo hardware](upgrade-option-same-hardware.md) |
|                                                                            |                             | [Implementar un clúster desconectado de Active Directory](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\))

|**Administrar**  |  **Herramientas y configuración**  |  **Recursos de la comunidad**       |
| ------------- |  -------------- | --------------------- |
| [Actualización compatible con clústeres](cluster-aware-updating.md)    |   [Cmdlets de Windows PowerShell de clúster de conmutación por error](/powershell/module/failoverclusters/?view=win10-ps)      |  [Foro sobre alta disponibilidad (clúster)](https://go.microsoft.com/fwlink/p/?LinkId=230641)       |
|  [Servicio de mantenimiento](health-service-overview.md)   |   [Cmdlets de PowerShell de actualización compatible con clústeres](/powershell/module/clusterawareupdating/?view=win10-ps)      | [Blog del equipo de Clústeres de conmutación por error y Equilibrio de carga de red](https://blogs.msdn.com/b/clustering/)        |
|  [Migración del dominio del clúster](cluster-domain-migration.md)   |         |         |
|  [Solución de problemas de uso del Informe de errores de Windows](troubleshooting-using-wer-reports.md)   |         |         |