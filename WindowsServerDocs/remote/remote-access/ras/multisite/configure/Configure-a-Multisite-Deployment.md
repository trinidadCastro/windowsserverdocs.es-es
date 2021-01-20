---
title: Configurar una implementación multisitio
description: Obtenga información acerca de los pasos de configuración necesarios para implementar una única implementación multisitio de acceso remoto de Windows Server 2016 o Windows Server 2012.
manager: brianlic
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 20a475b7dd48a20f216173ee5a6b4e07dad6d3c3
ms.sourcegitcommit: 7674bbe49517bbfe0e2c00160e08240b60329fd9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98603381"
---
# <a name="configure-a-multisite-deployment"></a>Configurar una implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016 combina VPN de servicio de acceso remoto (RAS) y DirectAccess en un único rol de acceso remoto. Esta información general proporciona una introducción a los pasos de configuración necesarios para implementar una única implementación de multisitio de acceso remoto de Windows Server 2016 o Windows Server 2012.

-   Paso 1: [implementar un único servidor de DirectAccess con configuración avanzada](../../../directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings.md). Instalar y configurar un solo servidor de acceso remoto. La implementación multisitio requiere la instalación de un solo servidor antes de configurar una implementación multisitio.

-   [Paso 2: configurar la infraestructura multisitio](Step-2-Configure-the-Multisite-Infrastructure.md). Para una implementación multisitio, debe configurar sitios de Active Directory adicionales y controladores de dominio. También se requieren grupos de seguridad y objetos de directiva de grupo (GPO) adicionales si no se usan GPO configurados automáticamente.

-   [Paso 3: configurar la implementación multisitio](Step-3-Configure-the-Multisite-Deployment.md): instalar el rol de acceso remoto en servidores de acceso remoto adicionales, habilitar la implementación multisitio y configurar los servidores adicionales como puntos de entrada para la implementación.

-   [Paso 4: comprobar la implementación multisitio](Step-4-Verify-the-Multisite-Deployment.md)

