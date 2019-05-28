---
ms.assetid: 20d48afc-2623-43e9-8ed9-aeb9a0505630
title: Configurar reglas de notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7eedd907c07c2aaef1670c5db3a6892ca3e650d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189631"
---
# <a name="configure-claim-rules"></a>Configuración de regla de notificación

En una de las reclamaciones\-identidades basado en modelo, la función de servicios de federación de Active Directory (AD FS) como servicios de federación consiste en emitir un token que contiene un conjunto de notificaciones. Reglas de notificaciones controlan las decisiones con respecto a las notificaciones que emite AD FS. Reglas de notificación y todos los datos de configuración del servidor se almacenan en la base de datos de configuración de AD FS.  
  
AD FS toma decisiones de emisión que se basan en información de identidad que se proporciona en forma de notificaciones y otra información contextual. En un nivel alto, AD FS funciona como un procesador de reglas por toma un conjunto de notificaciones como entrada, realiza una serie de transformaciones y, a continuación, devuelve un conjunto diferente de notificaciones como salida. 

Los siguientes temas le ayudarán a crear las reglas que va a procesar AD FS: 
  
-   [Crear una regla para pasar a través o filtrar una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Crear una regla para permitir a todos los usuarios](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Crear una regla para permitir o denegar usuarios según una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Crear una regla para enviar la pertenencia a grupos como una notificación](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Crear una regla para transformar una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Crear una regla para enviar una notificación de método de autenticación](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Crear una regla para enviar notificaciones mediante una regla personalizada](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)  

## <a name="see-also"></a>Vea también  
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
