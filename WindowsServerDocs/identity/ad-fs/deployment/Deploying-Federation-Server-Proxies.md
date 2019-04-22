---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: Implementación de servidores proxy de federación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b914141a0445febd3961b688aadc2f444b2eee7b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816266"
---
# <a name="deploying-federation-server-proxies"></a>Implementación de servidores proxy de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para implementar servidores proxy de federación de Active Directory Federation Services \(AD FS\), realice cada una de las tareas de [lista de comprobación: Configurar un servidor Proxy de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
> [!NOTE]  
> Cuando se usa esta lista de comprobación, se recomienda que lea primero las referencias al servidor proxy de federación instrucciones de planeamiento la [Guía de diseño de AD FS en Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de comenzar los procedimientos para configurar los servidores. La lista de comprobación proporciona una mejor comprensión del proceso de diseño e implementación de la federación proxies de servidor.  
  
## <a name="about-federation-server-proxies"></a>Acerca de servidores proxy de federación  
Servidores proxy de federación son equipos que ejecutan Windows Server® 2012 y el software AD FS que se han configurado manualmente para que actúe en el rol de proxy. Puedes usar proxies de servidor de federación de la organización para ofrecer servicios intermediarios entre un cliente de Internet y un servidor de federación que está detrás de un firewall de la red corporativa.  
  
> [!NOTE]  
> Aunque el servidor de federación y los roles de servidor proxy de federación no pueden instalarse en el mismo equipo, un servidor de federación puede realizar funciones de proxy de servidor de federación. Para obtener más información, consulte [When to Create a Federation Server](https://technet.microsoft.com/library/dd807101.aspx).  
  
El hecho de instalar el software AD FS en un equipo de Windows Server® 2012 y configurarlo para que actúe como proxy de lo convierte en un servidor proxy de federación.  
  

