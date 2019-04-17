---
ms.assetid: 41d6b897-1e72-4522-aad6-eece1154a154
title: "Implementación de AD FS en la organización de Partner de recurso"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4a556c07e7d6e0bec4c947ea9d1a75eef9964cef
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-ad-fs-in-the-resource-partner-organization"></a>Implementación de AD FS en la organización de Partner de recurso

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

La organización de partner de recursos en los servicios de federación de Active Directory \(AD FS\) representa la organización cuyos servidores Web pueden estar protegidos por un servidor de federación de resource\ lado. El servidor de federación en el partner de recursos usa los tokens de seguridad que se presentan con el asociado de la cuenta para proporcionar notificaciones a los servidores Web que se encuentran en el partner de recursos.  
  
En escenarios en los que debes proporcionar acceso a los servicios federados o aplicaciones para muchos usuarios diferentes: cuando algunos usuarios residen en diferentes organizaciones, puedes configurar el servidor de federación de recursos, lo que puede implementar varios partners de cuenta.  
  
Para obtener más información acerca de cómo instalar y configurar una organización de partner de recursos, consulta [lista de comprobación: configuración de la organización de Partner de recurso](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md).  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Revisa el rol de servidor de federación de asociado de recurso](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)  
  
-   [Revisa el rol del servidor Proxy de servidor de federación en el Partner de recursos](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
-   [Determinar la estrategia de aplicación federada en el Partner de recursos](Determine-Your-Federated-Application-Strategy-in-the-Resource-Partner.md)  
  

## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
