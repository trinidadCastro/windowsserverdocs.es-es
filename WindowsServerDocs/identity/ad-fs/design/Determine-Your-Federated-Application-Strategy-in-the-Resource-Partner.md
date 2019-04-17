---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: "Determinar la estrategia de aplicación federada en el Partner de recursos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aca47658cc5a20f63dbd59a26ebe135dd04def92
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>Determinar la estrategia de aplicación federada en el Partner de recursos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una parte importante del diseño de una nueva infraestructura \(AD FS\) de los servicios de federación de Active Directory en la organización de partner de recurso es determinar el conjunto completo de aplicaciones y servicios que se usará para participar en la federación y los partners de cuenta será los destinatarios de esos recursos. Antes de diseñar una aplicación federado y la estrategia de servicios, ten en cuenta las siguientes preguntas:  
  
-   ¿Se puede habilitar e implementar una aplicación ASP.NET o un servicio de Windows Communication Foundation \(WCF\) para federación?  
  
-   ¿Los usuarios de la red corporativa requiere acceso a la aplicación federado o servicio mediante la autenticación integrada de Windows?  
  
-   ¿La aplicación federado o servicio se usará por los usuarios en la red perimetral? ¿Si es así, autenticación integrada de Windows será necesaria?  
  
-   ¿Son todos los servidores Web que ejecute un sistema operativo Windows Server y \(IIS\) Internet Information Services host federado las aplicaciones?  
  
-   ¿Quién la aplicación federado o servicio proporcionará recursos para?  
  
Responder a estas preguntas te ayudará a planear un diseño de AD FS sólido. También le ayudará en la creación de una aplicación federada y estrategia de servicios rentable y recursos eficaz. Para obtener más información sobre cómo diseñar la estrategia de servicios y aplicaciones más adecuada federada para la organización, consulta los siguientes temas de esta guía:  
  
-   [Proporcionar el acceso a los usuarios de Active Directory a los servicios y las aplicaciones de notificaciones](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Proporcionar el acceso a los usuarios de Active Directory para las aplicaciones y servicios de otras organizaciones](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [Proporcionar a los usuarios en otra organización acceso a los servicios y las aplicaciones de notificaciones](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Para obtener más información sobre cómo crear una aplicación con reconocimiento de claims\ ASP.NET o el servicio de WCF, consulta [SDK de identidad de Windows Foundation](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

