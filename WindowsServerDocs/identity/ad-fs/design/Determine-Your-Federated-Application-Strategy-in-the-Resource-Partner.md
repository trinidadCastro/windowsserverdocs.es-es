---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: Determinar la estrategia de aplicación federada en el asociado de recurso
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0cc7a9202813cd3f8d45a72305d13ad197f5b04d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359165"
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>Determinar la estrategia de aplicación federada en el asociado de recurso

Una parte importante del diseño de una nueva infraestructura de Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 en la organización del asociado de recurso es determinar el conjunto completo de aplicaciones y servicios que se usarán para participar en la Federación. los asociados de cuenta serán los destinatarios de esos recursos. Antes de diseñar una estrategia de servicios y aplicación federada, plantéese las siguientes cuestiones:  
  
-   ¿Habilitará e implementará una aplicación ASP.NET o un servicio Windows Communication Foundation \(WCF @ no__t-1 para la Federación?  
  
-   ¿Necesitarán los usuarios de la red corporativa obtener acceso al servicio o la aplicación federada mediante la autenticación integrada de Windows?  
  
-   ¿Utilizarán los usuarios de la red perimetral el servicio o la aplicación federada? Si es así, ¿será necesaria la autenticación integrada de Windows?  
  
-   ¿Todos los servidores web que hospedan aplicaciones federadas ejecutan un sistema operativo Windows Server y Internet Information Services \(IIS @ no__t-1?  
  
-   ¿A quién proporcionará recursos el servicio o la aplicación federada?  
  
Responder a estas preguntas le ayudará a planear un diseño sólido de AD FS. También le ayudará en la creación de una estrategia de servicios y aplicación federada que sea rentable y eficaz con los recursos. Para más información sobre el diseño de la estrategia de servicios y aplicación federada más adecuada para su organización, consulte los temas siguientes de esta guía:  
  
-   [Proporcionar a los usuarios de Active Directory acceso a aplicaciones y servicios habilitados para notificaciones](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Proporcionar a los usuarios de Active Directory acceso a aplicaciones y servicios de otras organizaciones](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [Proporcionar a los usuarios de otra organización acceso a aplicaciones y servicios habilitados para notificaciones](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Para obtener más información sobre cómo crear una aplicación de notificaciones @ no__t-0aware ASP.NET o un servicio WCF, consulte el [SDK de Windows Identity Foundation](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

