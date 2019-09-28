---
title: Administrar clústeres de conmutación por error con el centro de administración de Windows
description: Administrar clústeres de conmutación por error con el centro de administración de Windows (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 16e758f0a8746d41adcdafb2bc1be2d91a3fc29c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406801"
---
# <a name="manage-failover-clusters-with-windows-admin-center"></a>Administrar clústeres de conmutación por error con el centro de administración de Windows

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

> [!Tip]
> ¿Novedad en Windows Admin Center?
> [Obtén más información acerca de Windows Admin Center](../understand/windows-admin-center.md) o [descárgalo ahora](https://aka.ms/windowsadmincenter).

## <a name="managing-failover-clusters"></a>Administración de clústeres de conmutación por error
Los [clústeres de conmutación por error](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview) son una característica de Windows Server que permite agrupar varios servidores en un clúster tolerante a errores para aumentar la disponibilidad y la escalabilidad de aplicaciones y servicios, como servidor de archivos de escalabilidad horizontal, Hyper-V y Microsoft SQL Server.

Aunque puede administrar los nodos de clúster de conmutación por error como servidores individuales agregándolos como [conexiones de servidor](manage-servers.md) en el centro de administración de Windows, también puede agregarlos como clústeres de conmutación por error para ver y administrar los recursos del clúster, almacenamiento, red, nodos, roles, virtual máquinas y conmutadores virtuales.

![Pantalla de introducción al clúster de conmutación por error](../media/manage-failover-clusters/fcm-overview.png)

## <a name="adding-a-failover-cluster-to-windows-admin-center"></a>Agregar un clúster de conmutación por error al centro de administración de Windows
Para agregar un clúster al centro de administración de Windows:

1. Haga clic en **+ Agregar** en todas las conexiones.
2. Elija Agregar una **conexión de conmutación por error**.
3. Escriba el nombre del clúster y, si se le solicita, las credenciales que se usarán.
4. Tendrá la opción de agregar los nodos de clúster como conexiones de servidor individuales en el centro de administración de Windows.
5. Haga clic en **Enviar** para finalizar.

El clúster se agregará a la lista de conexiones en la página información general. Haga clic en él para conectarse al clúster.

> [!NOTE]
> También puede administrar clúster hiperconvergido agregando el clúster como una [conexión de clúster hiperconvergida](manage-hyper-converged.md) en el centro de administración de Windows.

## <a name="tools"></a>Herramientas

Las siguientes herramientas están disponibles para las conexiones de clúster de conmutación por error:

| Herramienta | Descripción |
| ---- | ----------- |
| Información general | Ver detalles de clúster de conmutación por error y administrar recursos de clúster |
| Discos | Visualización de volúmenes y discos compartidos de clúster |
| Redes | Visualización de redes en el clúster |
| Nodos | Visualización y administración de nodos de clúster |
| Roles | Administrar roles de clúster o crear un rol vacío |
| Actualizaciones | Administrar las actualizaciones compatibles con clústeres (requiere [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)) |
| [Máquinas virtuales](manage-virtual-machines.md) | Ver y administrar máquinas virtuales |
| Conmutadores virtuales | Visualización y administración de conmutadores virtuales |

## <a name="more-coming"></a>Más próximamente

La administración del clúster de conmutación por error en el centro de administración de Windows está activamente en desarrollo y las nuevas características se agregarán en un futuro próximo. Puede ver el estado y votar para las características de UserVoice:

|Solicitud de característica|
|-------|
| [Mostrar más información de disco en clúster](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [Compatibilidad con acciones de clúster adicionales](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [Compatibilidad con clústeres convergentes que ejecutan Hyper-V y Servidor de archivos de escalabilidad horizontal en clústeres diferentes](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [Ver caché en bloque CSV](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [Ver toda o proponer nueva característica](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |