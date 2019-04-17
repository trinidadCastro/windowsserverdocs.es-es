---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: "Asignar los objetivos de la implementación a un diseño de AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 048bce75c52895b2d9e215bdccef9cb13dc23533
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>Asignar los objetivos de la implementación a un diseño de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una vez que termines de revisar los objetivos de implementación de \(AD FS\) de los servicios de federación de Active Directory existentes y determinar qué objetivos están relacionados con la implementación, puedes asignar esos objetivos de un diseño específico de AD FS. Para obtener más información acerca de AD FS predefinidas objetivos de la implementación, consulta [identificar los objetivos de implementación de AD FS](Identifying-Your-AD-FS-Deployment-Goals.md).  
  
Usa la tabla siguiente para determinar qué diseño de AD FS asigna a la combinación adecuada de AD FS objetivos de la implementación de la organización. En esta tabla hace referencia solamente a los dos diseños de AD FS principales, como se describe en esta guía. Sin embargo, puedes crear un diseño personalizado de AD FS o híbridas mediante cualquier combinación de los objetivos de la implementación de AD FS para satisfacer las necesidades de la organización.  
  
|Objetivo de implementación de AD FS|[Diseño de inicio de sesión único Web](Web-SSO-Design.md)|[Diseño SSO Web federado](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[Proporcionar el acceso a los usuarios de Active Directory a los servicios y las aplicaciones de notificaciones](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|No|Sí, en el partner de cuenta|  
|[Proporcionar el acceso a los usuarios de Active Directory para las aplicaciones y servicios de otras organizaciones](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|No|Sí, opcional en el partner de cuenta|  
|[Proporcionar a los usuarios en otra organización acceso a los servicios y las aplicaciones de notificaciones](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Sí|Sí|  

## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

