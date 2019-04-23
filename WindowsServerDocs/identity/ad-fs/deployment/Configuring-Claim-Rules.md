---
ms.assetid: 46dce9d4-7293-4b1c-9710-78b04f2e347a
title: Configuración de reglas de notificación
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6494f584edd5f84a5987707953f79edbce15cc02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814666"
---
# <a name="configuring-claim-rules"></a>Configuración de reglas de notificación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En una de las reclamaciones\-modelo de identidad basado en, la función de los servicios de federación de Active Directory \(AD FS\) como federation services consiste en emitir un token que contiene un conjunto de notificaciones. Reglas de notificaciones rigen la decisión respecto de notificaciones que emite AD FS. Reglas de notificación y todos los datos de configuración del servidor se almacenan en la base de datos de configuración de AD FS.  
  
AD FS toma decisiones de emisión que se basan en información de identidad que se proporciona en forma de notificaciones y otra información contextual. En un nivel alto, AD FS funciona como un procesador de reglas por toma un conjunto de notificaciones como entrada, realiza una serie de transformaciones y, a continuación, devuelve un conjunto diferente de notificaciones como salida.  
  
-   [Crear una regla para pasar a través o filtrar una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Crear una regla para permitir que todos los usuarios](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  

-   [Crear una regla para enviar una notificación Compatible de AD FS 1.x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)
  
-   [Crear una regla para permitir o denegar a los usuarios según una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Cree una regla de pertenencia a grupo de envío como una notificación](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Crear una regla para transformar una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Crear una regla para enviar una notificación del método de autenticación](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Crear una regla para enviar notificaciones mediante una regla personalizada](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="additional-references"></a>Referencias adicionales  

[Operaciones de AD FS](../../ad-fs/AD-FS-2016-Operations.md)