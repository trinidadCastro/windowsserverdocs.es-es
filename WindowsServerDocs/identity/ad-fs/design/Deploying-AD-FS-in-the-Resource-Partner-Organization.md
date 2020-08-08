---
ms.assetid: 41d6b897-1e72-4522-aad6-eece1154a154
title: Implementación de AD FS heredadas en la organización del asociado de recurso
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a129423c4d646549b0b569fb9920e0b550a1fdbb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942899"
---
# <a name="deploying-legacy-ad-fs-in-the-resource-partner-organization"></a>Implementación de AD FS heredadas en la organización del asociado de recurso

La organización del asociado de recurso de Servicios de federación de Active Directory (AD FS) \( AD FS \) representa la organización cuyos servidores Web pueden estar protegidos por un servidor de Federación de recursos \- . El servidor de Federación del asociado de recurso utiliza los tokens de seguridad generados por el asociado de cuenta para proporcionar notificaciones a los servidores web ubicados en el asociado de recurso.

En escenarios en los que es necesario proporcionar acceso a servicios o aplicaciones federados a muchos usuarios diferentes (cuando algunos usuarios residen en organizaciones diferentes), puede configurar el servidor de Federación de recursos para que pueda implementar varios asociados de cuenta.

Para obtener más información sobre cómo instalar y configurar una organización de asociado de cuenta, consulta [Checklist: Configuring the Resource Partner Organization](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md).

## <a name="in-this-section"></a>En esta sección

-   [Revisar el rol del servidor de federación en el asociado de recurso](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)

-   [Revisar el rol del servidor proxy de federación en el asociado de recurso](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)

-   [Determinar la estrategia de aplicación federada en el asociado de recurso](Determine-Your-Federated-Application-Strategy-in-the-Resource-Partner.md)


## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
