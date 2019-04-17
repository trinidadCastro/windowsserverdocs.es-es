---
ms.assetid: 9cafa3e1-8118-4a75-a7c2-1dbe40b1a444
title: Configurar las reglas de notificaciones
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f9e0509e2f870fd0edc7f0c6a241d789945e7ccb
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="configure-claim-rules"></a>Configurar reglas de notificación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

En un modelo basado en claims\ identidad, la función de los servicios de federación de Active Directory \(AD FS\) como servicios de federación es emitir un token que contiene un conjunto de notificaciones. Reglas de notificaciones rigen por las decisiones en relación con notificaciones de problemas de AD FS. Reglas de notificación y todos los datos de configuración del servidor se almacenan en la base de datos de configuración de AD FS.  
  
AD FS toma decisiones de emisión que se basan en la información de identidad que se proporciona a ella en el formulario de una reclamación y otra información contextual. En un nivel alto, AD FS funciona como un procesador de reglas mediante uno de una reclamación como entrada, realiza una serie de transformaciones y, a continuación, devuelve un conjunto diferente de una reclamación como salida. 

Los temas siguientes le ayudará a crear las reglas que procesará AD FS: 
  
-   [Crear una regla para atravesar o filtrar una notificación entrante](Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Crear una regla para permitir que todos los usuarios](Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Crear una regla para permitir o denegar a los usuarios en función de una notificación entrante](Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Crear una regla para enviar atributos LDAP como notificaciones](Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Crear una regla para enviar pertenencia a grupos como una notificación](Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Crear una regla para transformar una notificación entrante](Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Crear una regla para enviar una notificación de método de autenticación](Create-a-Rule-to-Send-an-Authentication-Method-Claim.md) 
-   [Crear una regla para enviar una notificación de AD FS 1.x Compatible](Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md) 
  
-   [Crear una regla para enviar notificaciones usando una regla personalizada](Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="see-also"></a>Consulta también  
[AD FS operaciones](../../ad-fs/AD-FS-2016-Operations.md) 
