---
title: Arquitectura de Servicios de Escritorio remoto
description: Diagramas de arquitectura de RDS
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 02/10/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f73bb0a-ce98-48a4-9d9f-cf7438936ca1
author: lizap
manager: dongill
ms.openlocfilehash: ba597318cdaf1d4659a72905eeb4e252c9020e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887876"
---
# <a name="remote-desktop-services-architecture"></a>Arquitectura de Servicios de Escritorio remoto

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

A continuación se muestran diversas configuraciones para la implementación de servicios de escritorio remoto para hospedar aplicaciones de Windows y equipos de escritorio para los usuarios finales.

>[!NOTE]
> Los diagramas de arquitectura siguientes muestran el uso de RDS en Azure. Sin embargo, puede implementar servicios de escritorio remoto en el entorno local y en otras nubes. Estos diagramas están pensados principalmente para ilustrar cómo las funciones RDS se colocan y utilizar otros servicios.

## <a name="standard-rds-deployment-architectures"></a>Arquitecturas de implementación de RDS estándares

Servicios de escritorio remoto consta de dos arquitecturas estándares:
-   Implementación básica: contiene el número mínimo de servidores para crear un entorno totalmente efectivo de RDS
-   Implementación de alta disponibilidad: contiene todos los componentes necesarios para que tenga el mayor tiempo de actividad garantizado para su entorno de RDS

### <a name="basic-deployment"></a>Implementación básica

![Implementación de RDS básica](./media/basic-rds.png)

### <a name="highly-available-deployment"></a>Implementación de alta disponibilidad

![Implementación de RDS de alta disponibilidad](./media/ha-rds.png)

## <a name="rds-architectures-with-unique-azure-paas-roles"></a>Arquitecturas RDS con funciones exclusivas de PaaS de Azure

Aunque la mayoría de escenarios se ajusten a las arquitecturas de implementación de RDS estándares, Azure sigue invirtiendo en soluciones de PaaS de la primera parte de ese valor de cliente de la unidad. A continuación se muestran algunas arquitecturas que muestra cómo incorporan con RDS.

### <a name="rds-deployment-with-azure-ad-domain-services"></a>Implementación de RDS con Azure AD Domain Services

Los dos diagramas de arquitectura estándar anteriores se basan en un tradicional Active Directory (AD) implementada en una máquina virtual de Windows Server. Sin embargo, si no tiene un anuncio tradicional y solo tiene un inquilino de Azure AD, a través de servicios como Office 365, pero todavía desea aprovechar RDS, puede usar [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) para crear un dominio administrado totalmente en la IaaS de Azure entorno que usa los mismos usuarios que existen en su inquilino de Azure AD. Esto elimina la complejidad de la sincronización de los usuarios y administrar varias máquinas virtuales manualmente. Azure AD Domain Services puede trabajar en cualquier implementación: básica o de alta disponibilidad.

![Azure AD y la implementación de RDS](./media/aadds-rds.png)

### <a name="rds-deployment-with-azure-ad-application-proxy"></a>Implementación de RDS con Azure AD Application Proxy

Los diagramas de arquitectura estándar dos anteriores utilizan los servidores Web de escritorio remoto/puerta de enlace como punto de entrada a través de Internet en el sistema RDS. En algunos entornos, los administradores prefieren quitar sus propios servidores del perímetro y en su lugar, use las tecnologías que también proporcionan seguridad adicional a través de tecnologías de proxy inverso. El [Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) rol PaaS encaja adecuadamente en este escenario.

Para que las configuraciones admitidas y cómo crear esta configuración, vea cómo [publicación de escritorio remoto con Azure AD Application Proxy](/azure/active-directory/application-proxy-publish-remote-desktop).

![RDS con el Proxy de aplicación de Azure AD](./media/aadappproxy-rds.png)
