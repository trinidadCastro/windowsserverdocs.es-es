---
title: Ejecución del modo de réplica de WSUS
description: 'Tema de Windows Server Update Service (WSUS): Cómo configurar el modo de réplica '
ms.prod: windows-server
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
ms.openlocfilehash: b7da68fa9cbe71f8a67e74671d64d11908ae4654
ms.sourcegitcommit: 9687d3eb221b89061a48bf1e73fb3b25bee69f9a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2020
ms.locfileid: "78169565"
---
# <a name="running-wsus-replica-mode"></a>Ejecución del modo de réplica de WSUS

>Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Un servidor WSUS que se ejecuta en modo de réplica hereda las aprobaciones de actualización y los grupos de equipos creados en un servidor de administración. En un escenario en el que se usa el modo de réplica, normalmente tiene un único servidor de administración y uno o varios servidores WSUS de réplica subordinada se propagan por toda la organización, en función de la topografía del sitio o de la organización. Se aprueban las actualizaciones y se crean grupos de equipos en el servidor de administración, que a continuación se reflejarán los servidores del modo de réplica. Los servidores en modo de réplica solo se pueden configurar durante la instalación de WSUS y, si ha implementado este escenario, es probable que sea importante en su organización que las aprobaciones de actualización y los grupos de equipos se administren de forma centralizada.

Si el servidor WSUS se ejecuta en modo de réplica, solo podrá realizar funciones de administración limitadas en el servidor, que se compondrá principalmente de:

-   Agregar y quitar equipos de grupos de equipos. La pertenencia a grupos de equipos no se distribuye a los servidores de réplica, solo a los grupos de equipos. Por lo tanto, en un servidor de modo de réplica, heredará los grupos de equipos que ha creado en el servidor de administración. Sin embargo, los grupos de equipos estarán vacíos. A continuación, debe asignar los equipos cliente que se conectan al servidor réplica a los grupos de equipos.

-   Configurar un programa de sincronización

-   Especificación de la configuración del servidor proxy

-   Especificar el origen de la actualización. Puede ser un servidor distinto del servidor de administración

-   Ver las actualizaciones disponibles

-   Supervisión de la actualización, la sincronización, el estado del equipo y la configuración de WSUS en el servidor

-   Ejecución de todos los informes de WSUS estándar disponibles en servidores en modo de réplica



