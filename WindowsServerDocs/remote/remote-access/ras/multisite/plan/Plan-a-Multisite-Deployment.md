---
title: Planear una implementación multisitio
description: Este tema forma parte de la guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 8387eabe-7363-4367-b5b1-03c67baa2933
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 6a0679ba548aabd9ea83f886e5e8db79480c819e
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958717"
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
  
