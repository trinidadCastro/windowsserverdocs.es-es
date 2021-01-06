---
title: Planear una implementación multisitio
description: Obtenga información acerca de los pasos de planeación necesarios para implementar el acceso remoto de Windows Server 2016 o Windows Server 2012 en una configuración multisitio.
manager: brianlic
ms.topic: article
ms.assetid: 8387eabe-7363-4367-b5b1-03c67baa2933
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: a4a56e430b1b40627aaf0e387c8df8ec122b94fb
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97941641"
---
# <a name="plan-a-multisite-deployment"></a>Planear una implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016, Windows Server 2012 combinan DirectAccess y la VPN del servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto. Esta información general proporciona una introducción a los pasos de planeación necesarios para implementar el acceso remoto de Windows Server 2016 o Windows Server 2012 en una configuración multisitio.

1.  [Implementar un único servidor de DirectAccess con configuración avanzada](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831436(v=ws.11)). Este paso incluye la planificación de la infraestructura necesaria para implementar un solo servidor. Incluye la planeación de la configuración de red y del servidor, los requisitos de certificados, la configuración de DNS, la implementación del servidor de ubicación de red, los servidores de administración de DirectAccess, la configuración de Active Directory y los objetos de directiva de grupo (GPO).

2.  [Paso 2: planear la infraestructura multisitio](Step-2-Plan-the-Multisite-Infrastructure.md). Este paso incluye el planeamiento de Active Directory y GPO, y la configuración de DNS.

3.  [Paso 3 planear la implementación multisitio](Step-3-Plan-the-Multisite-Deployment.md). Este paso incluye la planeación de la configuración del certificado, la configuración del servidor de ubicación de red, la configuración del punto de entrada de cliente, la configuración de prefijo IPv6 y, opcionalmente, la configuración de equilibrio de

> [!NOTE]
> Registre sus decisiones de planeación para la implementación avanzada de acceso remoto. Este registro se puede usar como trabajo para todos los implicados en los pasos de implementación.

Una vez que haya completado estos pasos de planeación, consulte [configuración de una implementación multisitio](../configure/Configure-a-Multisite-Deployment.md).

