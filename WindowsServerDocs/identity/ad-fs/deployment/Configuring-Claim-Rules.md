---
ms.assetid: 46dce9d4-7293-4b1c-9710-78b04f2e347a
title: "Configurar reglas de notificación"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6494f584edd5f84a5987707953f79edbce15cc02
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="configuring-claim-rules"></a>Configurar reglas de notificación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En un modelo basado en claims\ identidad, la función de los servicios de federación de Active Directory \(AD FS\) como servicios de federación es emitir un token que contiene un conjunto de notificaciones. La decisión respecto de las solicitudes que emite AD FS rigen por las reglas de reclamaciones. Reglas de notificación y todos los datos de configuración del servidor se almacenan en la base de datos de configuración de AD FS.  
  
AD FS toma decisiones de emisión que se basan en la información de identidad que se proporciona a ella en el formulario de una reclamación y otra información contextual. En un nivel alto, AD FS funciona como un procesador de reglas mediante uno de una reclamación como entrada, realiza una serie de transformaciones y, a continuación, devuelve un conjunto diferente de una reclamación como salida.  
  
-   [Crear una regla para atravesar o filtrar una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Crear una regla para permitir que todos los usuarios](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  

-   [Crear una regla para enviar una notificación de AD FS 1.x Compatible](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)
  
-   [Crear una regla para permitir o denegar a los usuarios en función de una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Crear una regla para enviar pertenencia a grupos como una notificación](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Crear una regla para transformar una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Crear una regla para enviar una notificación de método de autenticación](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Crear una regla para enviar notificaciones usando una regla personalizada](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="additional-references"></a>Referencias adicionales  

[AD FS operaciones](../../ad-fs/AD-FS-2016-Operations.md)
