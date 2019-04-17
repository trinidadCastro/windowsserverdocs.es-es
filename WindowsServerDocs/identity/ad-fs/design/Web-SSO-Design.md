---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: "Diseño de inicio de sesión único Web"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1b8344594c9fc477ed8424c716ec8d7f7fd91ef3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="web-sso-design"></a>Diseño de inicio de sesión único Web

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En el diseño de Single\ Sign\ en \(SSO\) en los servicios de federación de Active Directory \(AD FS\) Web, los usuarios deben autenticarse una sola vez para acceder a varias aplicaciones seguras FS\ AD o servicios. En este diseño son externos todos los usuarios y no se confía federación existe porque no hay ningún organizaciones asociadas. Por lo general, implementación este diseño cuando desea proporcionar acceso individuales de consumidor o cliente a uno o más aplicaciones o servicios de AD FS seguro a través de Internet, como se muestra en la siguiente ilustración.  
  
![diseño de inicio de sesión único Web](media/adfs2_WebSSODesign.gif)  
  
Con el SSO Web diseñar una organización que hospeda normalmente una aplicación protegida por AD FS\ o servicio en una red perimetral puede mantener un almacén independiente de las cuentas de cliente en la red perimetral, lo cual facilita la aislar las cuentas de cliente de cuentas de los empleados.  
  
Puedes administrar las cuentas locales para los clientes en la red perimetral mediante los servicios de dominio de Active Directory \(AD DS\), SQL Server o un almacén de atributo personalizado.  
  
Este diseño coincide con el objetivo de implementación en [proporcionar tu Active Directory a los usuarios acceso a tus aplicaciones para notificaciones y los servicios](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Para obtener una lista de las tareas detalladas que puedes usar para planear e implementar el diseño Web SSO, consulta [lista de comprobación: implementar un diseño de inicio de sesión único Web](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
