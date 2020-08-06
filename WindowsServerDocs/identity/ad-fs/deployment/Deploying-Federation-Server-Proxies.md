---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: Implementación de servidores proxy de Federación en AD FS
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5f9ab6729b5cc5d0b4981f24cdc5a3ef48ee967e
ms.sourcegitcommit: de8fea497201d8f3d995e733dfec1d13a16cb8fa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87864193"
---
# <a name="deploying-legacy-ad-fs-federation-server-proxies"></a>Implementación de servidores proxy de Federación de AD FS heredados

Para implementar servidores proxy de Federación en Servicios de federación de Active Directory (AD FS) \( AD FS \) , complete cada una de las tareas de [lista de comprobación: configuración de un servidor proxy de Federación](Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
> [!NOTE]  
> Cuando use esta lista de comprobación, recomendamos que lea primero las referencias a la guía de planeamiento del servidor proxy de Federación en la [Guía de diseño de AD FS en Windows server 2012](../design/ad-fs-design-guide-in-windows-server-2012.md) antes de comenzar los procedimientos para configurar los servidores. La siguiente lista de comprobación proporciona una mejor comprensión del proceso de diseño e implementación de los servidores proxy de Federación.  
  
## <a name="about-federation-server-proxies"></a>Acerca de los servidores proxy de Federación  
Los servidores proxy de Federación son equipos que ejecutan Windows Server &reg; 2012 y AD FS software que se han configurado manualmente para que actúen en el rol de proxy. Puede usar servidores proxy de Federación en su organización para proporcionar servicios intermedios entre un cliente de Internet y un servidor de Federación que esté detrás de un firewall en la red corporativa.  
  
> [!NOTE]  
> Aunque el servidor de Federación y los roles de servidor proxy de Federación no se pueden instalar en el mismo equipo, un servidor de Federación puede realizar funciones de proxy de servidor de Federación. Para obtener más información, consulte [When to Create a Federation Server](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807101(v=ws.11)).  
  
La acción de instalar el software de AD FS en un equipo con Windows Server &reg; 2012 y configurarlo para que actúe en el rol de proxy hace que ese equipo sea un servidor proxy de Federación.  
  
