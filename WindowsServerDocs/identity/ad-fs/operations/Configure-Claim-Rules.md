---
ms.assetid: 9cafa3e1-8118-4a75-a7c2-1dbe40b1a444
title: Configurar reglas de notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d0dd8528e5fbd6829b313a3e6bc47f5a17f6a12f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189716"
---
# <a name="configure-claim-rules"></a>Configuración de regla de notificación

En una de las reclamaciones\-modelo de identidad basado en, la función de los servicios de federación de Active Directory \(AD FS\) como federation services consiste en emitir un token que contiene un conjunto de notificaciones. Reglas de notificaciones controlan las decisiones con respecto a las notificaciones que emite AD FS. Reglas de notificación y todos los datos de configuración del servidor se almacenan en la base de datos de configuración de AD FS.  
  
AD FS toma decisiones de emisión que se basan en información de identidad que se proporciona en forma de notificaciones y otra información contextual. En un nivel alto, AD FS funciona como un procesador de reglas por toma un conjunto de notificaciones como entrada, realiza una serie de transformaciones y, a continuación, devuelve un conjunto diferente de notificaciones como salida. 

Los siguientes temas le ayudarán a crear las reglas que va a procesar AD FS: 
  
-   [Crear una regla para pasar a través o filtrar una notificación entrante](Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Crear una regla para permitir a todos los usuarios](Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Crear una regla para permitir o denegar usuarios según una notificación entrante](Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Crear una regla para enviar atributos LDAP como notificaciones](Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Crear una regla para enviar la pertenencia a grupos como una notificación](Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Crear una regla para transformar una notificación entrante](Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Crear una regla para enviar una notificación de método de autenticación](Create-a-Rule-to-Send-an-Authentication-Method-Claim.md) 
-   [Crear una regla para enviar una notificación Compatible de AD FS 1.x](Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md) 
  
-   [Crear una regla para enviar notificaciones mediante una regla personalizada](Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="see-also"></a>Vea también  
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
