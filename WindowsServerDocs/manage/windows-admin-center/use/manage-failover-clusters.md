---
title: Administrar clústeres de conmutación por error con Windows Admin Center
description: Administrar clústeres de conmutación por error con Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 63f5fca8e7ef63200e01cfc6e00a979e7f045b51
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296677"
---
# Administrar clústeres de conmutación por error con Windows Admin Center

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

> [!Tip]
> ¿Novedad en Windows Admin Center?
> [Obtén más información acerca de Windows Admin Center](../understand/windows-admin-center.md) o [descárgalo ahora](https://aka.ms/windowsadmincenter).

## Administrar clústeres de conmutación por error
[Clústeres de conmutación por error](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview) es una característica de Windows Server que te permite agrupar varios servidores en un clúster tolerante a aumentar la disponibilidad y escalabilidad de aplicaciones y servicios como servidor de archivos de escalabilidad horizontal, Hyper-V y Microsoft SQL Server.

Si bien puedes administrar los nodos de clúster de conmutación por error como servidores individuales agregándolos como [conexiones de servidor](manage-servers.md) en Windows Admin Center, también puedes agregar ellos como clústeres de conmutación por error para ver y administrar recursos de clúster, almacenamiento, red, nodos, roles, virtuales las máquinas y conmutadores virtuales.

![Pantalla de información general del clúster de conmutación por error](../media/manage-failover-clusters/fcm-overview.png)

## Agregar un clúster de conmutación por error al centro de administración de Windows
Para agregar un clúster a Windows Admin Center:

1. Haga clic en **+ Agregar** todas las conexiones.
2. Elegir esta opción para agregar una **Conexión de conmutación por error**.
3. Escribe el nombre del clúster y, si se te solicite, las credenciales que se usan.
4. Tienes la opción para agregar los nodos del clúster como conexiones de servidor individual en Windows Admin Center.
5. Haz clic en **Enviar** para finalizar.

El clúster se agregará a tu lista de conexión en la página información general. Haz clic en él para conectarse al clúster.

> [!NOTE]
> También puedes administrar hiperconvergida agrupado agregando el clúster como una [conexión de clúster hiperconvergido](manage-hyper-converged.md) en Windows Admin Center.

## Herramientas

Las siguientes herramientas están disponibles para las conexiones de clúster de conmutación por error:

| Herramienta | Descripción |
| ---- | ----------- |
| Introducción | Ver los detalles de clúster de conmutación por error y administrar recursos de clúster |
| Discos | Compartidos de clúster de vista volúmenes y discos |
| Redes | Ver las redes del clúster |
| Nodos | Ver y administrar los nodos de clúster |
| Roles | Administrar roles de clúster o crear un rol vacío |
| Actualizaciones | Administrar las actualizaciones compatible con clústeres (requiere [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)) |
| [Máquinas virtuales](manage-virtual-machines.md) | Ver y administrar las máquinas virtuales |
| Conmutadores virtuales | Ver y administrar los conmutadores virtuales |

## Más próximas

Administración de clústeres de conmutación por error en Windows Admin Center está activamente en desarrollo y se agregarán nuevas características en un futuro cercano. Puedes ver el estado y el voto de características en UserVoice:

|Solicitud de característica|
|-------|
| [Mostrar más información de disco de clúster](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [Acciones de clúster adicionales de soporte técnico](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [Compatible con clústeres convergentes que se ejecutan Hyper-V y el servidor de archivos de escalabilidad horizontal en clústeres diferentes](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [Memoria caché de bloque CSV de vista](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [Ver todos o Proponer nueva característica](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |