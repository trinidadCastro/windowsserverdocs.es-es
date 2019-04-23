---
title: Ejecución del modo de réplica de WSUS
description: 'Tema de Windows Server Update Service (WSUS): cómo configurar el modo de réplica '
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d218cd6b-3b6b-4429-913b-31d412ce3356
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b4139354a3f0f7b1f1a97107d2f6b28db2b02c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878746"
---
# <a name="running-wsus-replica-mode"></a>Ejecución del modo de réplica de WSUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un servidor WSUS que se ejecuta en modo de réplica hereda las aprobaciones de actualización y los grupos de equipos creados en un servidor de administración. En un escenario que usa el modo de réplica, suelen tener un servidor de administración único y uno o varios servidores WSUS de réplica subordinados se reparten a lo largo de la organización, en función de la topografía organizativa o el sitio. Aprobar actualizaciones y crear grupos de equipos en el servidor de administración, que, a continuación, se reflejará los servidores de modo de réplica. Servidores de modo de réplica se pueden establecer únicamente durante la instalación de WSUS y, si implementa este escenario, es probable, dado que es importante en su organización que aprobaciones de actualización y los grupos de equipos se administran de forma centralizada.

Si su servidor WSUS se está ejecutando en modo de réplica, podrá realizar solo las funcionalidades de administración limitada en el servidor, que principalmente se compone de:

-   Agregar y quitar equipos de grupos de equipos. No se distribuye la pertenencia a grupo de equipos a servidores de réplica, sólo el equipo grupos propios. Por lo tanto, en un servidor de modo de réplica, heredarán los grupos de equipos que ha creado en el servidor de administración. Sin embargo, los grupos de equipos estará vacíos. A continuación, debe asignar al cliente en equipos que se conectan al servidor de réplica para los grupos de equipos.

-   Configurar un programa de sincronización

-   Especifica la configuración de servidor proxy

-   Especificar el origen de actualización. Esto puede ser un servidor distinto del servidor de administración

-   Ver las actualizaciones disponibles

-   Actualización, sincronización, el estado de equipo y configuración de WSUS en el servidor de supervisión

-   Ejecuta WSUS estándares todos los informes disponibles en los servidores de modo de réplica



