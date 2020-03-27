---
title: Configurar una implementación multisitio
description: Este tema forma parte de la guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4a0229d5605271876f89e8e0ae75f8612e3f5762
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314016"
---
# <a name="configure-a-multisite-deployment"></a>Configurar una implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016 combina VPN de servicio de acceso remoto (RAS) y DirectAccess en un único rol de acceso remoto. Esta información general proporciona una introducción a los pasos de configuración necesarios para implementar una única implementación de multisitio de acceso remoto de Windows Server 2016 o Windows Server 2012.  
  
-   Paso 1: [implementar un único servidor de DirectAccess con configuración avanzada](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Instalar y configurar un solo servidor de acceso remoto. La implementación multisitio requiere la instalación de un solo servidor antes de configurar una implementación multisitio.  
  
-   [Paso 2: configurar la infraestructura multisitio](Step-2-Configure-the-Multisite-Infrastructure.md). Para una implementación multisitio, debe configurar sitios de Active Directory adicionales y controladores de dominio. También se requieren grupos de seguridad y objetos de directiva de grupo (GPO) adicionales si no se usan GPO configurados automáticamente.  
  
-   [Paso 3: configurar la implementación multisitio](Step-3-Configure-the-Multisite-Deployment.md): instalar el rol de acceso remoto en servidores de acceso remoto adicionales, habilitar la implementación multisitio y configurar los servidores adicionales como puntos de entrada para la implementación.  
  
-   [Paso 4: comprobar la implementación multisitio](Step-4-Verify-the-Multisite-Deployment.md) 
  


