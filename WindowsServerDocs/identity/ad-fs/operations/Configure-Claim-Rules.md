---
ms.assetid: 9cafa3e1-8118-4a75-a7c2-1dbe40b1a444
title: Configuración de reglas de notificaciones
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6ceca4b76ba1744c3cc988fd840453f9391ce3d3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407677"
---
# <a name="configure-claim-rules"></a>Configuración de regla de notificación

En un modelo de identidad Claims @ no__t-0based, la función de Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-2 como servicios de Federación es emitir un token que contenga un conjunto de notificaciones. Las reglas de notificaciones rigen las decisiones con respecto a las notificaciones que AD FS problemas. Las reglas de notificaciones y todos los datos de configuración del servidor se almacenan en la base de datos de configuración de AD FS.  
  
AD FS toma decisiones de emisión basadas en la información de identidad que se proporciona en forma de notificaciones y otra información contextual. En un nivel alto, AD FS funciona como un procesador de reglas tomando un conjunto de notificaciones como entrada, realiza una serie de transformaciones y, a continuación, devuelve un conjunto diferente de notificaciones como salida. 

Los temas siguientes le ayudarán a crear las reglas que AD FS procesarán: 
  
-   [Crear una regla para pasar a través o filtrar una demanda entrante](Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Crear una regla para permitir a todos los usuarios](Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Crear una regla para permitir o denegar usuarios según una notificación entrante](Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Crear una regla para enviar atributos LDAP como notificaciones](Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Crear una regla para enviar la pertenencia a grupos como una notificación](Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Crear una regla para transformar una notificación entrante](Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Crear una regla para enviar una notificación de método de autenticación](Create-a-Rule-to-Send-an-Authentication-Method-Claim.md) 
-   [Creación de una regla para enviar una afirmación compatible con AD FS 1. x](Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md) 
  
-   [Crear una regla para enviar notificaciones mediante una regla personalizada](Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="see-also"></a>Vea también  
[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
