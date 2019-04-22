---
title: Administración de clústeres de conmutación por error con Windows Admin Center
description: Administración de clústeres de conmutación por error con Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f7e14581f7f6b14b0cf39308de236b68a07e8c9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824076"
---
# <a name="manage-failover-clusters-with-windows-admin-center"></a>Administración de clústeres de conmutación por error con Windows Admin Center

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

> [!Tip]
> ¿Novedad en Windows Admin Center?
> [Obtén más información acerca de Windows Admin Center](../understand/windows-admin-center.md) o [descárgalo ahora](https://aka.ms/windowsadmincenter).

## <a name="managing-failover-clusters"></a>Administración de clústeres de conmutación por error
[Agrupación en clústeres de conmutación por error](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview) es una característica de Windows Server que le permite agrupar varios servidores en un clúster con tolerancia a aumentar la disponibilidad y escalabilidad de aplicaciones y servicios como servidor de archivos de escalabilidad horizontal, Hyper-V y Microsoft SQL Server.

Aunque puede administrar nodos de clúster de conmutación por error como servidores individuales mediante la adición de ellos como [las conexiones de servidor](manage-servers.md) en Windows Admin Center, también puede agregarlos como clústeres de conmutación por error para ver y administrar recursos de clúster, almacenamiento, red, nodos, roles, las máquinas virtuales y conmutadores virtuales.

![Pantalla de información general del clúster de conmutación por error](../media/manage-failover-clusters/fcm-overview.png)

## <a name="adding-a-failover-cluster-to-windows-admin-center"></a>Agregar un clúster de conmutación por error a Windows Admin Center
Para agregar un clúster a Windows Admin Center:

1. Haga clic en **+ agregar** en todas las conexiones.
2. Optar por agregar una **conmutación por error de conexión**.
3. Escriba el nombre del clúster y, si se le pide las credenciales para usar.
4. Tendrá la opción para agregar los nodos del clúster como conexiones de servidor individuales en Windows Admin Center.
5. Haga clic en **enviar** para finalizar.

El clúster se agregarán a la lista de conexiones en la página información general. Haga clic en él para conectarse al clúster.

> [!NOTE]
> También puede administrar hiperconvergido en el clúster mediante la adición del clúster como un [conexión del clúster Hyper-Converged](manage-hyper-converged.md) en Windows Admin Center.

## <a name="tools"></a>Herramientas

Las siguientes herramientas están disponibles para las conexiones de clúster de conmutación por error:

| Herramienta | Descripción |
| ---- | ----------- |
| Información general | Ver los detalles del clúster de conmutación por error y administrar recursos de clúster |
| Discos | Compartido del clúster de la vista discos y volúmenes |
| Redes | Ver redes del clúster |
| Nodos | Ver y administrar nodos de clúster |
| Roles | Administrar roles de clúster o crear un rol vacío |
| Actualizaciones | Administrar las actualizaciones de clústeres |
| [Máquinas virtuales](manage-virtual-machines.md) | Ver y administrar máquinas virtuales |
| Conmutadores virtuales | Ver y administrar los conmutadores virtuales |

## <a name="more-coming"></a>Pero pronto habrá más

Administración de clúster de conmutación por error en Windows Admin Center activamente está en desarrollo y se agregarán nuevas características en un futuro próximo. Puede ver el estado y vote por características en UserVoice:

|Solicitud de característica|
|-------|
| [Mostrar más información de disco en clúster](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [Admitir acciones de clúster adicional](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [Admite convergentes clústeres que ejecutan Hyper-V y Scale-Out File Server en clústeres diferentes](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [Caché de bloques de vista CSV](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [Ver todos o Proponer nueva característica](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |