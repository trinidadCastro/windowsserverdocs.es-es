---
title: Conexión de estaciones adicionales a MultiPoint Server
description: Agregar más estaciones a la implementación de Multipoint Services
ms.topic: article
ms.assetid: d78ebf4e-0968-4014-9a42-9f75cc50cb52
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: a73c0c9fe077da5baa850bb0c4a8f6d6a018397e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955412"
---
# <a name="attach-additional-stations-to-multipoint-services"></a>Conexión de estaciones adicionales a multipoint Services
En el entorno de Multipoint Services, los usuarios usan estaciones para conectarse a multipoint Services y realizar su trabajo. Las estaciones son los puntos de conexión de usuario para conectarse al equipo que ejecuta Multipoint Services.

Multipoint Services admite tres tipos de estación:

-   Estaciones conectadas a vídeo directo

-   Estaciones conectadas por el cliente USB a través de las estaciones conectadas al cliente USB a través de Ethernet

-   Estaciones conectadas de RDP a través de LAN

Las clasificaciones se basan en el hardware de una estación y en el tipo de conexión que usa. Puede mezclar y hacer coincidir los tipos de conexión de las estaciones. El único requisito es que la estación primaria (que instaló anteriormente) debe ser una estación conectada directamente a vídeo. Para obtener más información sobre las configuraciones de la estación, consulte [estaciones Multipoint](MultiPoint-services-Stations.md).

Para obtener instrucciones que explican cómo configurar cada tipo de estación, consulte lo siguiente:

-   [Configurar una estación conectada mediante de vídeo en directo](Set-up-a-direct-video-connected-station-in-MultiPoint-services.md)

-   [Configurar una estación conectada a un cliente cero mediante USB](Set-up-a-USB-zero-client-connected-station-in-MultiPoint-services.md)

-   [Configurar una estación conectada mediante RDP a través de LAN](Set-up-an-RDP-over-LAN-connected-station-in-MultiPoint-services.md)

Para obtener una comparación detallada de los tipos de estación, vea comparación de tipos de [estación](multipoint-services-stations.md#BKMK_StationTypeComparison).

> [!NOTE]
> -   Los procedimientos para asociar estaciones no describen cómo configurar concentradores intermedios o concentradores de bajada. Para obtener información sobre dónde instalar estos centros, consulte [estaciones Multipoint](MultiPoint-services-Stations.md).
> -   En algunos casos, es posible que tenga que crear escritorios virtuales de estación, que se ejecutan en máquinas virtuales. Por ejemplo, se usan aplicaciones que no se pueden instalar en Windows Server o en aplicaciones que no ejecutan varias instancias en el mismo equipo host. Para obtener más información, consulte [crear escritorios virtuales de Windows 10 Enterprise para estaciones](Create-Windows-10-Enterprise-virtual-desktops-for-stations.md).

> [!TIP]
> Resulta útil para crear sus estaciones en el orden de sus ubicaciones físicas, de modo que se identifiquen secuencialmente en MultiPoint Server. Si más adelante desea cambiar el nombre de una estación, puede hacerlo en Multipoint Manager. Para obtener más información, consulte reasignación de todas las estaciones en ayuda y soporte técnico de MultiPoint Server.