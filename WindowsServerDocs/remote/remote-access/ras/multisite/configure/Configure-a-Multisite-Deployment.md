---
title: Configurar una implementación multisitio
description: Este tema forma parte de la Guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b602855db271348ac48ee0a5691424a7321c7370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849756"
---
# <a name="configure-a-multisite-deployment"></a>Configurar una implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016 combina DirectAccess y el servicio de acceso remoto (RAS) VPN en un solo rol de acceso remoto. Esta información general proporciona una introducción a los pasos de configuración necesarios para implementar una única implementación de multisitio de acceso remoto de Windows Server 2012 o de Windows Server 2016.  
  
-   Paso 1: [Implementar un único servidor de DirectAccess con configuración avanzada](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Instalar y configurar un solo servidor de acceso remoto. La implementación multisitio, deberá instalar a un único servidor antes de configurar una implementación multisitio.  
  
-   [Paso 2: Configurar la infraestructura de multisitio](Step-2-Configure-the-Multisite-Infrastructure.md). Para una implementación multisitio debe configurar sitios de Active Directory adicionales y controladores de dominio. Grupos de seguridad adicional y objetos de directiva de grupo (GPO) también son necesarios si no usas GPO configurados automáticamente.  
  
-   [Paso 3: Configurar la implementación multisitio](Step-3-Configure-the-Multisite-Deployment.md)-instalar el rol de acceso remoto en otros servidores de acceso remoto, la implementación multisitio de habilitar y configurar los servidores adicionales como puntos de entrada para la implementación.  
  
-   [Paso 4: Comprobar la implementación multisitio](Step-4-Verify-the-Multisite-Deployment.md) 
  


