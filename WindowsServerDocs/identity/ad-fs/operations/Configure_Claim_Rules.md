---
description: Más información acerca de cómo configurar reglas de notificaciones
ms.assetid: 20d48afc-2623-43e9-8ed9-aeb9a0505630
title: Configurar reglas de notificaciones en AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 05affd2c3c0d78b4710db168ef2d3c36318fb0db
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048793"
---
# <a name="configure-claim-rules"></a>Configuración de regla de notificación

En un modelo de identidad basado en notificaciones \- , la función de servicios de Federación de Active Directory (AD FS) (AD FS) como servicios de Federación es emitir un token que contenga un conjunto de notificaciones. Las reglas de notificaciones rigen las decisiones con respecto a las notificaciones que AD FS problemas. Las reglas de notificaciones y todos los datos de configuración del servidor se almacenan en la base de datos de configuración de AD FS.

AD FS toma decisiones de emisión basadas en la información de identidad que se proporciona en forma de notificaciones y otra información contextual. En un nivel alto, AD FS funciona como un procesador de reglas tomando un conjunto de notificaciones como entrada, realiza una serie de transformaciones y, a continuación, devuelve un conjunto diferente de notificaciones como salida.

Los temas siguientes le ayudarán a crear las reglas que AD FS procesarán:

-   [Crear una regla para pasar a través o filtrar una demanda entrante](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)

-   [Crear una regla para permitir a todos los usuarios](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)

-   [Crear una regla para permitir o denegar usuarios según una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)

-   [Crear una regla para enviar atributos LDAP como notificaciones](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)

-   [Crear una regla para enviar la pertenencia a grupos como una notificación](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)

-   [Crear una regla para transformar una notificación entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)

-   [Crear una regla para enviar una notificación de método de autenticación](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)

-   [Crear una regla para enviar notificaciones mediante una regla personalizada](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)

## <a name="see-also"></a>Consulte también
[Operaciones de AD FS](../ad-fs-operations.md)
