---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: Diseño de SSO web
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1b8344594c9fc477ed8424c716ec8d7f7fd91ef3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852806"
---
# <a name="web-sso-design"></a>Diseño de SSO web

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En la Web único\-sesión\-en \(SSO\) diseño en Active Directory Federation Services \(AD FS\), los usuarios deberán autenticarse una sola vez para tener acceso a varios AD FS\- servicios o aplicaciones protegidas. En este diseño todos los usuarios son externos y no existe ninguna confianza de federación porque no hay organizaciones asociadas. Normalmente, implementas este diseño cuando desea proporcionar acceso de cliente o consumidor individual a uno o varios servicios de AD FS protegida o aplicaciones a través de Internet, como se muestra en la siguiente ilustración.  
  
![diseño de sso Web](media/adfs2_WebSSODesign.gif)  
  
Con el diseño SSO Web, una organización que normalmente hospeda una instancia de AD FS\-segura aplicación o servicio en una red perimetral puede mantener un almacén de cuentas de clientes en la red perimetral, lo que resulta más fácil aislar cliente independiente cuentas de empleados.  
  
Puede administrar las cuentas locales para los clientes de la red perimetral mediante el uso de ambos servicios de dominio de Active Directory \(AD DS\), SQL Server o un almacén de atributos personalizados.  
  
Este diseño coincide con el objetivo de implementación de [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Para obtener una lista de tareas detalladas que puede usar para planear e implementar el diseño SSO Web, consulte [lista de comprobación: Implementar un diseño SSO Web](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
