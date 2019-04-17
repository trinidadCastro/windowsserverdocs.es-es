---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: "Implementar el proxy de servidor de federación"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b914141a0445febd3961b688aadc2f444b2eee7b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-federation-server-proxies"></a>Implementar el proxy de servidor de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para implementar el proxy de servidor de federación en los servicios de federación de Active Directory \(AD FS\), complete cada una de las tareas en [lista de comprobación: configuración de un Proxy de servidor de federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
> [!NOTE]  
> Al usar esta lista de comprobación, te recomendamos leer primero las referencias a proxy del servidor de federación planeación instrucciones en el [Guía de diseño de AD FS en Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de comenzar los procedimientos para configurar los servidores. La lista de comprobación proporciona una mejor comprensión del proceso de diseño e implementación de federación proxies de servidor.  
  
## <a name="about-federation-server-proxies"></a>Acerca de proxies de servidor de federación  
Proxies de servidor de federación son equipos que ejecutan Windows Server® 2012 y el software de AD FS y que se han configurado manualmente para que actúe en la función de proxy. Puedes usar a servidores proxy de servidor de federación de la organización para proporcionar servicios intermediarios entre un cliente de Internet y un servidor de federación está protegido por un firewall en la red corporativa.  
  
> [!NOTE]  
> Aunque el servidor de federación y las funciones de proxy del servidor de federación no se puede instalar en el mismo equipo, un servidor de federación puede realizar funciones de proxy del servidor de federación. Para obtener más información, consulta [cuándo se debe crear un servidor de federación](https://technet.microsoft.com/library/dd807101.aspx).  
  
La acción de instalación del software de AD FS en un equipo de Windows Server® 2012 y configurarla para que actúe en la función de proxy hace que ese equipo un proxy de federación de servidor.  
  

