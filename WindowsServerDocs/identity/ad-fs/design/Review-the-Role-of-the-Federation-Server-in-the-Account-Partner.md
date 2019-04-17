---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: "Revisa el rol de servidor de federación de asociado de cuenta"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0914d32e8f24d5e7db0a25c733342c1bde3e0329
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Revisa el rol de servidor de federación de asociado de cuenta

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un servidor de federación de los servicios de federación de Active Directory \(AD FS\) funciona como un emisor de tokens de seguridad. Un servidor de federación genera reclamaciones basadas en la cuenta almacenan valores que residen en un atributo local y empaqueta en tokens de seguridad para que los usuarios puedan acceder sin problemas Web\ browser\-aplicaciones basadas en \ (usando solo \(SSO\)\) sign\ en que se hospedan en una organización de partner de recursos.  
  
> [!NOTE]  
> Cuando el acceso a los usuarios federados aplicaciones usando un explorador Web, un servidor de federación emite automáticamente las cookies a los usuarios a mantener su estado de inicio de sesión para esa aplicación Web\ basados en browser\. Estas cookies contienen reclamaciones por los usuarios. Las cookies habilitar las funcionalidades de SSO para que los usuarios no tienen que escribir credenciales cada vez que visitan diferentes Web\ browser\-aplicaciones basadas en el partner de recursos.  
  
En el diseño Web SSO, las organizaciones con una red perimetral que quieren que los usuarios tengan acceso a aplicaciones de Internet deben instalar a un proxy de federación de servidor en la red perimetral. En el diseño SSO Web federado, debe haber al menos un servidor de federación instalado en la red corporativa de la organización de partner de la cuenta y al menos un servidor de federación instalado en la red corporativa de la organización de partner de recursos.  
  
> [!NOTE]  
> Antes de configurar un equipo de servidor de federación de la organización de partner de cuenta, primero debes unir el equipo a ningún dominio en el bosque de Active Directory que se usará el servidor de federación para autenticar usuarios de ese bosque. Para obtener más información, consulta [lista de comprobación: configurar un servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
