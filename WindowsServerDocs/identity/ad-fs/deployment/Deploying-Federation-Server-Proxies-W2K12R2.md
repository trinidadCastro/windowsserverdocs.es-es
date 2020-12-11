---
description: Más información acerca de cómo implementar servidores proxy de Federación
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: Implementación de servidores proxy de Federación en AD FS para Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 6554cd206c459d5b0eacb8bf375d5fcd132c8530
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048893"
---
# <a name="deploying-federation-server-proxies"></a>Implementación de servidores proxy de federación

En Servicios de federación de Active Directory (AD FS) \( AD FS \) en Windows Server 2012 R2, el rol de un servidor proxy de Federación se administra mediante un nuevo servicio de rol de acceso remoto denominado proxy de aplicación Web. Para habilitar la AD FS para la accesibilidad desde fuera de la red corporativa, que era el propósito de implementar un servidor proxy de Federación en versiones heredadas de AD FS, como AD FS 2,0 y AD FS en Windows Server 2012, puede implementar uno o varios proxy de aplicación web para AD FS en Windows Server 2012 R2.

En el contexto de AD FS, el proxy de aplicación web funciona como un proxy de servidor de Federación AD FS. Además, el servicio Proxy de aplicación web ofrece funcionalidad de proxy inverso para las aplicaciones web dentro de la red corporativa para permitir a los usuarios de cualquier dispositivo acceso a estas desde fuera de la red corporativa. Para obtener información general sobre el servicio de rol Proxy de aplicación web, consulte la introducción a Proxy de aplicación web.

Para planear la implementación de Proxy de aplicación web, puede consultar la información de los temas siguientes:

-   [Planear la infraestructura del proxy de aplicación web (WAP)](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11))

-   [Planear el servidor de Proxy de aplicación web](/previous-versions/orphan-topics/ws.11/dn383647(v=ws.11))

Para implementar Web Application Proxy, puede seguir los procedimientos de los temas siguientes:

-   [Configurar la infraestructura del Proxy de aplicación web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383644(v=ws.11))

-   [Instalar y configurar el servidor de Proxy de aplicación web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383662(v=ws.11))


## <a name="see-also"></a>Consulte también

[Implementación de AD FS](../../ad-fs/AD-FS-Deployment.md)

[Guía de implementación de AD FS en Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

[Implementar una granja de servidores de federación](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)

