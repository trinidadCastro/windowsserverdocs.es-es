---
description: Más información acerca de cómo configurar reglas de notificaciones
ms.assetid: 46dce9d4-7293-4b1c-9710-78b04f2e347a
title: Configuración de reglas de notificación
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 70d3c1f562d4faf9cea6608be9e2825dd39e8744
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044175"
---
# <a name="configuring-claim-rules"></a>Configuración de reglas de notificación

En un modelo de identidad basado en notificaciones \- , la función de Servicios de federación de Active Directory (AD FS) \( AD FS \) como servicios de Federación es emitir un token que contenga un conjunto de notificaciones. Las reglas de notificaciones rigen la decisión con respecto a las notificaciones que AD FS problemas. Las reglas de notificaciones y todos los datos de configuración del servidor se almacenan en la base de datos de configuración de AD FS.

AD FS toma decisiones de emisión basadas en la información de identidad que se proporciona en forma de notificaciones y otra información contextual. En un nivel alto, AD FS funciona como un procesador de reglas tomando un conjunto de notificaciones como entrada, realiza una serie de transformaciones y, a continuación, devuelve un conjunto diferente de notificaciones como salida.

-   [Crear una regla para pasar a través o filtrar una demanda entrante](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)

-   [Crear una regla para permitir a todos los usuarios](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)

-   [Creación de una regla para enviar una afirmación compatible con AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)

-   [Crear una regla para permitir o denegar usuarios según una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)

-   [Crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)

-   [Crear una regla para enviar la pertenencia a grupos como una notificación](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)

-   [Crear una regla para transformar una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)

-   [Crear una regla para enviar una notificación de método de autenticación](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)

-   [Crear una regla para enviar notificaciones mediante una regla personalizada](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)

## <a name="additional-references"></a>Referencias adicionales

[Operaciones de AD FS](../ad-fs-operations.md)
