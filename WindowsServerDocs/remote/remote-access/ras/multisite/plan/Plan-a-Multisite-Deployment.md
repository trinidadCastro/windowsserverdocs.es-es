---
title: Planear una implementación multisitio
description: Este tema forma parte de la Guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8387eabe-7363-4367-b5b1-03c67baa2933
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ba813fc5da53e9635e9ef1363f1a559a29419312
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282599"
---
# <a name="plan-a-multisite-deployment"></a>Planear una implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016, Windows Server 2012 combina DirectAccess y enrutamiento y servicio de acceso remoto (RRAS) VPN en un solo rol de acceso remoto. Esta información general proporciona una introducción a los pasos de planificación requeridos para implementar Windows Server 2016 o acceso remoto de Windows Server 2012 en una configuración multisitio.  
  
1.  [Implementar un único servidor de DirectAccess con configuración avanzada](https://technet.microsoft.com/library/hh831436(v=ws.11).aspx). Este paso incluye la planificación de la infraestructura necesaria para implementar un único servidor. Comprende la planificación de la red y configuración del servidor, los requisitos de certificados, configuración de DNS, implementación de servidor de ubicación de red, servidores de administración de DirectAccess, configuración de Active Directory y objetos de directiva de grupo (GPO).  
  
2.  [Paso 2 Planear la infraestructura de multisitio](Step-2-Plan-the-Multisite-Infrastructure.md). Este paso incluye Active Directory y GPO planificación y configuración de DNS.  
  
3.  [Paso 3 planear una implementación multisitio](Step-3-Plan-the-Multisite-Deployment.md). Este paso incluye la planificación de configuración de certificados, configuración del servidor de ubicación de red, configuración de punto de entrada de cliente, una configuración de prefijo IPv6 y configuración de equilibrio de carga global si lo desea.  
  
> [!NOTE]  
> Registre sus decisiones de planificación de implementación avanzada de acceso remoto. Este registro se puede usar como trabajo para todos los implicados en los pasos de implementación.  
  
Después de haber completado estos pasos de planificación, consulte [configurando una implementación multisitio](../configure/Configure-a-Multisite-Deployment.md).  
  


