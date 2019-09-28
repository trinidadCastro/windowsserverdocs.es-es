---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: Implementación de servidores proxy de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c6af319283de72963691ae3e91c3db5992bdec72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408404"
---
# <a name="deploying-federation-server-proxies"></a>Implementación de servidores proxy de federación

Para implementar los servidores proxy de Federación en Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1, complete cada una de las tareas en [Checklist: Configuración de un servidor proxy de Federación @ no__t-0.  
  
> [!NOTE]  
> Cuando use esta lista de comprobación, recomendamos que lea primero las referencias a la guía de planeamiento del servidor proxy de Federación en la [Guía de diseño de AD FS en Windows server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de comenzar los procedimientos para configurar los servidores. La siguiente lista de comprobación proporciona una mejor comprensión del proceso de diseño e implementación de los servidores proxy de Federación.  
  
## <a name="about-federation-server-proxies"></a>Acerca de los servidores proxy de Federación  
Los servidores proxy de Federación son equipos que ejecutan Windows Server® 2012 y AD FS software que se han configurado manualmente para que actúen en el rol de proxy. Puedes usar proxies de servidor de federación de la organización para ofrecer servicios intermediarios entre un cliente de Internet y un servidor de federación que está detrás de un firewall de la red corporativa.  
  
> [!NOTE]  
> Aunque el servidor de Federación y los roles de servidor proxy de Federación no se pueden instalar en el mismo equipo, un servidor de Federación puede realizar funciones de proxy de servidor de Federación. Para obtener más información, consulte [When to Create a Federation Server](https://technet.microsoft.com/library/dd807101.aspx).  
  
La acción de instalar el software de AD FS en un equipo con Windows Server® 2012 y configurarlo para que actúe en el rol de proxy hace que ese equipo sea un servidor proxy de Federación.  
  

