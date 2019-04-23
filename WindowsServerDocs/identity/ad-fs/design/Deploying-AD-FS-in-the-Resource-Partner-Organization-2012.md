---
ms.assetid: 39acccd9-0402-49ca-8ce1-b239e1e7e455
title: Implementación de AD FS en la organización del asociado de recurso
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a20fab1cca4c33485fd599de5525c7a718e9598e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882326"
---
# <a name="deploying-ad-fs-in-the-resource-partner-organization"></a>Implementación de AD FS en la organización del asociado de recurso

>Se aplica a: Windows Server 2012

La organización del asociado de recurso en Active Directory Federation Services \(AD FS\) representa la organización cuyos servidores Web pueden estar protegidos por un recurso\-servidor de federación de lado. El servidor de federación en el asociado de recurso usa los tokens de seguridad generados por el asociado de cuenta para proporcionar notificaciones a los servidores Web que se encuentran en el asociado de recurso.  
  
En escenarios en los que es necesario proporcionar acceso a los servicios federados o aplicaciones a distintos usuarios: cuando algunos usuarios residen en distintas organizaciones, puede configurar el servidor de federación de recursos para que pueda implementar varios asociados de cuenta.  
  
Para obtener más información acerca de cómo instalar y configurar una organización del asociado de recurso, consulte [lista de comprobación: Configuración de la organización del asociado de recurso](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md).  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Revise el rol del servidor de federación del asociado de recurso](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)  
  
-   [Revise el rol de servidor Proxy de federación del asociado de recurso](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
-   [Determinar la estrategia de aplicación federada en el asociado de recurso](Determine-Your-Federated-Application-Strategy-in-the-Resource-Partner.md)  
  

## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
